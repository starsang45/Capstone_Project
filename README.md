# Capstone_Project_sqs
My could bootcamp capstone project on multi cloud


# Scenario
A B2B web application deployed in Azure platform has been observing decreased number of leads/enquires being generated on their website. Upon investigating the logs generated in Azure Monitor service, they found that the app servers (backend) are going down for few minutes on a daily basis. Since they are on a tight budget, they are looking for a solution which can help them in retaining the forms filled via the front-end so that even if the backend servers are down the forms data submitted by the end users are not lost. They want to apply this solution part on AWS to retain the high availability of their app.  


# Objective
To set up decoupling of a multi-timer web application deployed on Azure platform using the SQS service offered by AWS
The scenario involves deploying a B2B web application in Azure, where the backend servers frequently go down, leading to a potential loss of form data submitted by end users. The goal is to create an architecture that ensures form data is retained, even when the backend is temporarily down, by using AWS services.

# Step 1
Deploy the Frontend and Backend on Azure VMs
Frontend VM:

Create a Virtual Machine (VM) in Azure for hosting the frontend of the web application (which is responsible for displaying the form).
Install necessary software such as a web server (e.g., Nginx or Apache) to host the frontend application.
Ensure that Python and any other required dependencies are installed for running the frontend scripts.
Backend VM:

Create another Azure VM for the backend server that processes the form submissions.
Install Python and any dependencies required for processing the form data.
The backend will act as a consumer of the messages queued in AWS SQS.

# Step 2
Setup Frontend Script (Producer of a Message)
Python Script for the Frontend:

Write a Python script that collects the form data submitted by users.
The script should package the form data into a message format (e.g., JSON).
Instead of sending the data directly to the backend, the script will send the message to AWS SQS, ensuring the data is retained even when the backend is down.
Use the Boto3 SDK (AWS SDK for Python) to interact with AWS SQS.
Example Python code to send a message:

python

import boto3
import json

-Initialize SQS client
sqs = boto3.client('sqs', region_name='us-east-1')

-SQS Queue URL
queue_url = 'https://sqs.us-east-1.amazonaws.com/123456789012/MyQueue'

def send_message(form_data):
    # Send message to SQS queue
    response = sqs.send_message(
        QueueUrl=queue_url,
        MessageBody=json.dumps(form_data)
    )
    print(f'Message sent to SQS: {response["MessageId"]}')
    
# Step 3
Setup Backend Script (Consumer of a Message)
Python Script for the Backend:

Write a Python script that acts as a consumer, retrieving the form data from AWS SQS.
This script will periodically check the queue for new messages and process the form data.
Once the data is successfully processed (e.g., saved to a database), the message will be deleted from the SQS queue to prevent reprocessing.
Example Python code to consume messages:

python

import boto3
import json

- Initialize SQS client
sqs = boto3.client('sqs', region_name='us-east-1')

- SQS Queue URL
queue_url = 'https://sqs.us-east-1.amazonaws.com/123456789012/MyQueue'

def receive_messages():
    # Poll for messages
    response = sqs.receive_message(
        QueueUrl=queue_url,
        MaxNumberOfMessages=10,  # Adjust as needed
        WaitTimeSeconds=10  # Long polling
    )

    messages = response.get('Messages', [])
    for message in messages:
        # Process the message
        form_data = json.loads(message['Body'])
        print(f'Processing form data: {form_data}')

        # Delete message from queue after processing
        sqs.delete_message(
            QueueUrl=queue_url,
            ReceiptHandle=message['ReceiptHandle']
        )
        print(f'Message deleted: {message["MessageId"]}')
        
# Step 4
Set Up Boto3 SDK for Both Frontend and Backend
Install Boto3 SDK:

On both the frontend and backend Azure VMs, install the Boto3 SDK to interact with AWS SQS.
bash

pip install boto3
Configure AWS Credentials:

Configure the AWS credentials (Access Key ID and Secret Access Key) on both VMs by using the AWS CLI or manually setting up the credentials in ~/.aws/credentials.

# Step 5
Set Up AWS SQS to Ingest Messages
Create an SQS Queue:

In the AWS Management Console, navigate to the SQS service and create a new standard queue (e.g., "FormSubmissionQueue").
Configure the queue with a retention period that allows messages to be kept for a sufficient duration (e.g., 4 days).
Take note of the queue URL as it will be used by the frontend and backend scripts.
Configure SQS Permissions:

Ensure that the frontend and backend VMs have appropriate IAM roles or access keys that allow them to send and receive messages from the SQS queue.

# Step 6
Set Up the Backend Server to Read and Delete Messages from SQS

Polling the Queue:

The backend server will run a script (as shown above) that continuously polls AWS SQS for new messages.
When messages (form submissions) are retrieved, they are processed, and once successfully stored (e.g., in a database), the message is deleted from the queue.
Ensure High Availability:

Ensure that the backend VM is configured to automatically restart or recover in case of downtime.
Optionally, consider using Azure Autoscaling or Load Balancers for both frontend and backend VMs to ensure high availability and prevent downtime.
