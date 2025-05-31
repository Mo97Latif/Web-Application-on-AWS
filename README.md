# Web-Application-on-AWS
Deployment of a Scalable Web Application on AWS

## Table of Contents
 - [Overview](#overview)
 - [Archeticture Diagram](#archeticture-diagram)
 - [Steps](#steps)


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

9.  ## Steps
    ### 1. The network environment:
    1. create the required subnets and the internet gateway.
    2. create the route tables and associate them accordingly.![subnets](https://github.com/user-attachments/assets/ec303bdc-effa-4b3e-a5ee-350d04e2cab0)
    ### 2. Security:
    1. create the ALB security group with the inbound rules: allow http traffic and allow https traffic from anywhere.
    2. create the EC2 security group with the inbound rules: allow http traffic and allow https traffic from the source (ALB-SG).
    3. create the DB security group with the inbound rules: allow http traffic and allow https traffic and mysql/aurora from the source(EC2-SG).
    4. Create an IAM role with the following permissions: SecretsManagerReadWrite - AmazonEC2ReadOnlyAccess - CloudWatchAgentServerPolicy - AmazonSSMManagedInstanceCore![security groups](https://github.com/user-attachments/assets/1993d505-afaf-479c-823f-d726785941b1)
    ### 3. Create launch template with the following:
    1. AMI: Amazon Linux 2
    2. Instance Type: t2.micro
    3. security group: EC2-SG
    4. Iam profile: the role we created in the previous step.
    5. user data: install apache and deploy the application files.![LT](https://github.com/user-attachments/assets/35d9c92e-2f75-431b-8279-395fa0f04823)
    ### 4. Create the load balancer:
    1. choose Application load balancer
    2. select "internet facing" and for subnets select "private subnet 1" and private subnet 2"
    3. select the ALB-SG.
    4. for listeners choose "create target group" and create the "application TG" then select it and the load balancer creation page then choose create.![ELB](https://github.com/user-attachments/assets/d32db238-3cec-4ffc-ac8a-e84dbb4e7f91)
    ### 5. Create the auto scaling group:
    1. select the launch template we created.
    2. select private subnet 1 and private subnet 2.
    3. select "Attach to an existing load balancer" and choose the one we created.
    4. configure group size and scaling, i configured it as follows![policy](https://github.com/user-attachments/assets/2b324a79-d312-4744-bcf7-363d7f42575e)
    5. Select enable group metrics collection within CloudWatch.
    6. continue and create te ASG, and we see that two instances were created according to our policy.![instances](https://github.com/user-attachments/assets/7e6f40a6-26ef-4e51-8434-2d80c33e5fd2)
    ### 6. Create the DB instance:
    1. create a subnet group including private subnet 3 and private subnet 4.
    2. choose mysql Db.
    3. select Multi-AZ DB instance deployment (2 instances).
    4. keep the default secretsmanager choice selected.
    5. choose the subnet group we created in the first step and the DB-SG.![DB](https://github.com/user-attachments/assets/44d040b0-76ef-4441-8c1a-f75de7fb75cb)
    ### 7. monitoring:
    1. go to SNS and create the "alarm" topic.
    2. create a new subscription and select E-mail and enter your Email.
    3. confirm the subscription by clicking the URL sent to your mail.
    4. go to cloudwatch and choose create alarm.
    5. select the source from the metrics (autoscaling group>>>>GroupTotalInstances).
    6. select the number to be greater than or equal to 6.
    7. For "Send a notification to the following SNS topic" select the topic we just created.
    8. for alarm name we write "maximum capacity", then review the settings and create.
    9. that alarm sends us an email notification whenever the application hits the maximum capacity instances.![monitor](https://github.com/user-attachments/assets/7dadc2a9-5c05-4ee2-babf-06f96b3f951b)

We are now ready to host a dynamic application and its database.
 
    



       



