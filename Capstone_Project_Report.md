# Multi-Cloud Architecture for Fintech Firm
## Overview

This capstone project was completed independently as part of my cloud engineering bootcamp. The objective was to design and implement a secure multi-cloud architecture using **Azure** and **AWS**, allowing secure communication between resources without exposing them to the public internet. The project aimed to demonstrate my proficiency in cloud infrastructure, network security, and multi-cloud solutions.
## Role and Responsibilities

As the sole contributor to this project, I was responsible for the entire lifecycle of the project, including the design, implementation, testing, and deployment of the multi-cloud architecture. My key responsibilities included:

- **Requirement Analysis**: Identifying the requirements for secure cross-cloud communication.
- **Network Design**: Designing the architecture for secure communication between Azure and AWS using site-to-site VPN.
- **Configuration and Implementation**: 
    - Setting up **Azure Virtual Machines (VMs)** and **AWS EC2 instances**.
    - Configuring **Azure VPN Gateway**, **AWS VPN Gateway**, **Customer Gateway**, and **Local Network Gateway**.
- **Network Security**: Implementing security rules and managing routing tables to ensure the confidentiality of data.
- **Testing and Validation**: Testing the network setup to ensure secure, encrypted communication between Azure and AWS.
## Technical Approach

### Phase 1: Infrastructure Setup
Deployed **Azure Virtual Machines (VMs)** in a Virtual Network (VNet) and **AWS EC2 instances** in a Virtual Private Cloud (VPC), configured with internal security protocols to ensure no external exposure.

### Phase 2: VPN Gateway Configuration
Configured **Azure VPN Gateway** and **AWS VPN Gateway** for a secure site-to-site VPN. Defined external networks using **Azure Local Network Gateway** and **AWS Customer Gateway**, which allowed both cloud environments to establish communication.

### Phase 3: Network Security
Applied security rules and updated routing tables to ensure that only encrypted traffic was allowed between the two environments. Configured Network Security Groups (NSGs) to block public access and secure internal traffic.

### Phase 4: Testing and Validation
Conducted thorough testing to validate the security of the VPN connection. Verified that data traffic between the Azure and AWS resources was encrypted and met security standards.
## Challenges and Solutions

- **Challenge**: Configuring the VPN gateways between Azure and AWS.
    - **Solution**: Followed detailed documentation for each cloud provider, ensuring proper configuration of IP addresses, subnets, and routes.
- **Challenge**: Ensuring seamless communication without public exposure.
    - **Solution**: Applied strict security group rules and conducted multiple tests to validate secure communication.
## Conclusion

This project successfully demonstrated the implementation of a secure multi-cloud architecture that allows confidential data transfer between **Azure** and **AWS**. As the sole contributor, I independently applied a range of cloud engineering skills, from network design and VPN configuration to testing and validation. The project helped solidify my understanding of multi-cloud environments and secure communication.
## Technologies Used

- **Azure**: VPN Gateway, Virtual Machines, Local Network Gateway
- **AWS**: EC2 Instances, VPN Gateway, Customer Gateway
- **Networking**: Site-to-Site VPN, Routing Tables, Security Groups
