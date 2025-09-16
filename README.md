# AWS Production-Grade Web Application Deployment

## Project Overview

This project demonstrates the deployment of a real-time, production-grade web application on Amazon Web Services (AWS) using a highly available and secure architecture. The primary goal was to showcase an understanding of foundational AWS services, network design, and best practices for creating a scalable and resilient environment.

The architecture is designed to be highly available by distributing resources across two different **Availability Zones** within a single **Virtual Private Cloud (VPC)**. The application instances are placed in private subnets for enhanced security, while public-facing components like the load balancer and bastion host reside in public subnets.

### Key AWS Services Used

- **Amazon VPC:** To create a custom, isolated network for the application.
- **EC2:** To provision virtual servers (instances) for the application and the bastion host.
- **Application Load Balancer (ALB):** To distribute incoming traffic across multiple application instances.
- **Auto Scaling Group (ASG):** To automatically scale the number of application instances based on demand.
- **NAT Gateway:** To allow instances in private subnets to access the internet securely.
- **Security Groups:** To control network traffic to and from instances.

### Project Architecture

The following diagram illustrates the network and component layout for this project.

[[vpc-example-private-subnets.png]]

### Step-by-Step Implementation Guide

#### 1. VPC Creation

- I started by creating a new **VPC** in the AWS Console using the "VPC and more" wizard.
- I configured the VPC to span **two Availability Zones** and included two public subnets and two private subnets to ensure high availability.
- A **NAT Gateway** was deployed in each Availability Zone within the public subnets to provide internet access to the private instances.

[[vpc resource map.png]]

#### 2. Auto Scaling Group and Launch Template

- Next, I created a **Launch Template** for the EC2 instances. The template specified a simple EC2 instance running a basic Python web application.
- A new **Security Group** was created for the instances, allowing inbound traffic on **Port 8000** for the application and **Port 22** for SSH access.
- I then created an **Auto Scaling Group**, associating it with the Launch Template. The ASG was configured to maintain a desired capacity of **two instances** within the private subnets.


[[auto scaling group.png]]
#### 3. Bastion Host and Application Deployment

- To securely access the private instances, I launched a **Bastion Host** (a standard EC2 instance) in one of the public subnets.
- I used `SCP` to transfer my SSH key to the bastion host.
- From the bastion host, I was able to securely SSH into the private instances using their private IP addresses.
- I manually installed a simple Python web application on one of the instances to demonstrate how the load balancer would route traffic to a healthy target.

#### 4. Application Load Balancer

- I provisioned an **Application Load Balancer (ALB)** and placed it in the public subnets.
- A **Target Group** was created to define the application's health checks and to register the two EC2 instances as targets on **Port 8000**.
- The ALB was configured to forward all incoming traffic from **Port 80** to the new target group.
- After the ALB was provisioned, I had to update its security group to allow inbound traffic on Port 80 to ensure it could receive requests from the internet.

[[load balancer.png]]
### Conclusion

This project provided valuable hands-on experience in building a robust cloud infrastructure from the ground up. It reinforced my understanding of VPC networking, high availability, and the importance of security best practices in a production environment.