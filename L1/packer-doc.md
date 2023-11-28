# what is pakcer?
Packer is an OpenSource Image Provisioning tool developed by Hashicorp.

It creates machine images and makes the Configuration Management and Server provisioning an easy Job.

It can create Images for multiple platforms like Docker, AWS, Oracle Virtual Box, Digital Ocean, OpenStack, Linode, Azure etc.

you can write the Instructions to the packer as JSON files they are called as  Templates in Packer Parlance.

The technology Packer facilitates is widely known Infrastructure as a Code  (IAC).

# Steps to Create Packer Image and Create EC2 with Terraform
These are the steps we are going to perform to create a packer image which install apache httpd webserver using a shell provisioner and prepare the image.

Once the image is ready we are going to use Terraform create an instance based on the AMI or the image we have created using packer.
## Step0: Get your Programmatic Access / Create Access Key and Secret from AWS
1.)Login to AWS Console
2.)In the services go to IAM
3.)Create a User and Click on map existing Policies
4.)Choose UserName and Select the Policy (Administrator Access Policy)
5.)Create user
6.)Final Stage would present the AccessKEY and Secret Access like given below.
## Step1: Install and Setup Packer in linux (ubuntu)

-> wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
-> echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
-> sudo apt update && sudo apt install packer
## What is a Packer Template file and  How to Write a Packer Template?
The Packer template file uses JSON format and it consists of three parts

1.) provider
2.) scource
3.) build

save it in .pkr.hcl or .json but then the format will be in json file 
