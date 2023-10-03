# Week 1 Tasks 
- [ ]  100 - Inventory and Patch Management
- [ ] AWS Account Setup and Root User
- [ ] Level 100: Monitoring with CloudWatch Dashboards
- [ ] Level 100: Deploy a Reliable Multi-tier Infrastructure using CloudFormation
- [ ] Level 100: AWS Account Setup: Lab Guide

In order to complete the Labs, I decided to go with the AWS sandboxes in [AcloudGuru](https://learn.acloud.guru/cloud-playground/cloud-sandboxes) in order to not incur any additional costs.
## Creating User and IAM Roles
- [x] Created new user
- [x] Created New User Group

In order to **set a password for a new user** I had to select the option to grant access to the IAM console and then a password was generated automatically or can be input manually. Details can tbe sent to the user through Email or excel sheet.
![image](https://github.com/Makinates/Faks-Infastructure/assets/32597117/6e08e191-14a3-4643-844b-93a5b35abc78)

### EC2 Key Pair
Ec2 uses a key pair to prove identity when connecting to the instance. The public key is stored on the instance(EC2)  whike the user stores the private key. On Linux the private key is used to SSH into the instance
- Alternative to using key pairs is the **AWS Systems Manager session**. This provides more security and monitoring 
A key pair can the generated when creating an EC2 Instance or in the network and security Tab

![image](https://github.com/Makinates/Faks-Infastructure/assets/32597117/68eca81b-94e8-4257-88df-49e9d1496fcf)

### Tagging
Tags help assign metaData to our resources. Aws has a **Resource groups tagging API** that programmatically controls tags.
To use the API,  it requires adding the following to your IAM policy
```
tag:GetResources

tag:TagResources

tag:UntagResources

tag:GetTagKeys

tag:GetTagValues
```
This was done by adding the policy to the Amdmin user group ( Although I believe the Administrator access already has the policy)
![image](https://github.com/Makinates/Faks-Infastructure/assets/32597117/3a02e427-1f1f-4ac8-9813-008de7d5aa4c)


### CloudFormation
This helps manage our AWS resources using templates. Templates are then used to create/ Destroy resources as a stack. 
Stacks can be created from
- Already existing templates which you have(usuallly in json or YAML format)
- Sample templates (provided by AWS)
- Template created by user in the designer. 

Image of what a template in the designer looks like
![image](https://github.com/Makinates/Faks-Infastructure/assets/32597117/219bfda5-6e5b-4305-aafe-8efe69dbe5b7)

CloudFormation is extremely useful in deploying multiple environments of the same type eg. (Prod, Test)  
N.B- On complete creation of the stack, I noticed most of the status of resources were still in progress
![image](https://github.com/Makinates/Faks-Infastructure/assets/32597117/a909e949-46b7-425f-890c-370d67eb342b)


## Systems Manager
Systems Manger in short is a centralized hub for managing and automating all aws resources both in AWS and outside AWS.
Steps to manage Resources with systems Manager after the first pre-requisites [pre-requisites](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html)
- Create an IAM Role for Systems manager ( the ec2 can call systems manger on our behalf)
- Add the Role to the EC2 instances
- The resources should then start to show under the Systems manager fleet

#### Issues getting inventory
![image](https://github.com/Makinates/Faks-Infastructure/assets/32597117/aa961519-9dbb-4935-9a9e-5e3cec3dc4b6)

![image](https://github.com/Makinates/Faks-Infastructure/assets/32597117/20694c73-5938-4b6e-a0b9-669c9a32b640)
This is as a result of a Service control policy 

```
A Service Control Policy (SCP) in AWS is a JSON policy statement that defines the maximum permissions for an AWS Organizations entity (such as an AWS account or an organizational unit).
SCPs are a type of policy that you can use to set permission guardrails across the AWS Organization.

User: arn:aws:sts::728643735238:assumed-role/managedInstancces/i-0429843dcdf465520 is not authorized to perform: ssm:PutInventory on resource: arn:aws:ec2:us-east-1:728643735238:instance/i-0429843dcdf465520 with an explicit deny in a service control policy

```


## Patching Instances using Patch Manager
Patch manager sends patch updates to instances under the fleet and under the same operating system
