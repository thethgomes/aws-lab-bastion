# aws-lab-bastion
Creating a Bastion Host to access ec2 instances in a private Subnet
##

### 1  - Creating EC2 Instances - Bastion/Private

#### 1.Creating Bastion Host
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

#### 2.Creating Private Instance 
Accessing https://console.aws.amazon.com search for EC2, click on **"Launch Instances"**, set following options:

- **Choose an AMI**: Amazon Linux 2 AMI
- **Instance Type**: t2 micro (free tier)
- **Configure Instance Details:** 
![image](https://user-images.githubusercontent.com/48591555/153867140-ac4cc06b-0d3b-4ca4-b1eb-a04f3d5fb982.png)
- **Add Tags:** Name: Private-Instance
- **Configure Security Group**:  Create following Security Group

_Secrutiy Group Name: SG-Private-Instance_
Type  | Protocol  | Port Range  | Source  | Description
:----:| :--------:| :----------:| :------:|:----------:| 
SSH   | TCP       | 22          | "sg-Bastion Group"| SSH from Bastion Host

![image](https://user-images.githubusercontent.com/48591555/153867645-e33a5a69-aeca-440b-b107-c041c8eaafaf.png)

- **Create a new key-pair**: 


![image](https://user-images.githubusercontent.com/48591555/153867819-11154bfe-0d91-4831-8d64-256f859844fe.png)


##

### 2 - Accessing Private Instance from Bastion Host
Get Bastion Host public IP Address from Instances Console on aws:

![image](https://user-images.githubusercontent.com/48591555/153868102-cc6eb7ca-969b-461b-8fe7-704c6c31b2b8.png)

Search for ssh key pair to access bastion host from a terminal, on first access you have to change ssh permissions:

```
## !!Here in this example AWS ssh key pair is "lab-ssh-internet.pem" you can chage this file name to yours *.pem file used on instance creation!!

sudo chmod 400 lab-ssh-internet.pem
``` 
then you can connect from your terminal to Aws Bastion host through following command:
``` 
# Sintax : sudo ssh -i "yourkeypair.pem" ec2-user@"your-aws-bastion-host-public-ip"

sudo ssh -i lab-ssh-internet.pem ec2-user@44.202.12.190

```
From Bastion Host you need to create pem file to access private instance. Copy the content of _private-instance_ PEM file and create a new pem file from Bastion Host SSH access.

![image](https://user-images.githubusercontent.com/48591555/153871852-0f2e3c9a-3327-496b-b8c4-19cad3fc546f.png)

paste _private-instance.pem_ content and save file from vim: (_keys ESC then ":wq!" to save)

![image](https://user-images.githubusercontent.com/48591555/153871988-e2e27615-f1f2-46fa-be4d-085fb4983892.png)

change file permission:
```
sudo chmod 400 private-instances.pem
```
From EC2 instances console, find out Private Instance Private IPv4 Address:

![image](https://user-images.githubusercontent.com/48591555/153872496-1f2cf17d-8bd9-48f7-bf33-a2f7cb04d8a4.png)

Go back to bastion terminal and access Private-Instance with following command:
```
 sudo ssh -i private-instances.pem ec2-user@10.0.47.5
 ```
 
 ![image](https://user-images.githubusercontent.com/48591555/153873097-1cf77554-fa76-4436-b59f-73872ca91273.png)


It is the safest way to connect from your computer to a Private Instance without exposing it to internet.


