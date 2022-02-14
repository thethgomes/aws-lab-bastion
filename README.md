# aws-lab-bastion
Creating a Bastion Host to access ec2 instances in a private Subnet
##

### 1  - Creating EC2 Instances - Bastion/Private

Accessing https://console.aws.amazon.com search for EC2, click on **"Launch Instances"**, set following options:

- **Choose an AMI**: Amazon Linux 2 AMI
- **Instance Type**: t2 micro (free tier)
- **Configure Instance Details:** 

![image](https://user-images.githubusercontent.com/48591555/153864770-6504fa6f-1d2f-42da-a0f8-b5dfb55089eb.png)
- **Add Tags:** Name: BastionHost
- **Configure Security Group**:  Create following Security Group

_Secrutiy Group Name: SG-Bastion_

Type  | Protocol  | Port Range  | Source  | Description
:----:| :--------:| :----------:| :------:|:----------:| 
SSH   | TCP       | 22          | 0.0.0.0/0 | SSH from internet
All ICMP-IPv4 | All | N/A | 0.0.0.0/0 | PING from internet

