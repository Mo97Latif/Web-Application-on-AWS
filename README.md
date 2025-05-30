# Web-Application-on-AWS
Deployment of a Scalable Web Application on AWS

## Table of Contents
 - [Overview](#overview)
 - [Archeticture Diagram](archeticture-diagram)
 - [](deployment)


## Overview:
This example deploys a highly available, scalable, and monitored web application infrastructure on AWS. It leverages core AWS services like EC2, Application Load Balancer (ALB), Auto Scaling, RDS, IAM, CloudWatch, and SNS to ensure performance, resilience, and operational visibility.

## Archeticture Diagram:
The architecture is designed for high availability, scalability, and resilience using core AWS services. 
1. It begins with an Application Load Balancer (ALB) deployed in a public subnet that receives incoming traffic and distributes it across EC2 instances that host the dynamic web application.
2. the EC2 instances deployed in two Availability Zones to ensure fault tolerance, are part of an Auto Scaling Group (ASG) that automatically adjusts capacity based on demand according to its target tracking policy, helping maintain performance during traffic spikes and optimize costs during low usage.
3. A security group is configured for the ALB to allow inbound http traffic from the internet.
4. For an extra security layer a security group is configured for the EC2 instances to allow inbound traffic only from the ALB security group.
5. For the backend, an Amazon RDS database is deployed with Multi-AZ configuration, ensuring database availability and failover protection.
6. A DB security group is configured to allow inbound traffic only from the EC@ instances security group.
7. AN IAM role is configured to manage secure, role-based access for the EC2 instances to access and use the DB credentials that we securely save in secrets manager.
8.  Finally, CloudWatch monitors system metrics like CPU and memory usage that we specify in the target tracking policy of the ASG, It triggers an SNS topic to send alert notifications when predefined thresholds are breached, enabling proactive incident management.![Diagram](https://github.com/user-attachments/assets/8902748b-d460-416b-90b6-0612299436e0)

