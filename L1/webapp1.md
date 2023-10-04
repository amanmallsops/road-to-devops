# Building a web App using AWS console in single instance.
**Contents**
  - [1. Deploy a networking stack](#2-deploy-a-networking-stack)
  - [2. Create an Instance and SSH into your Instance](#3-create-an-instance-and-ssh-into-your-instance)
  - [3. Install the Apache Web Server to run PHP](#5-install-the-apache-web-server-to-run-php)
  - [4. Install PHP to run WordPress](#6-install-php-to-run-wordpress)
  - [5. Install MySQL for adding database](#7-install-mysql-for-adding-database)
  - [6. Install WordPress](#8-install-wordpress)
  - [7. Map IP Address and Domain Name](#9-map-ip-address-and-domain-name)
    - [Other Method: To change your WordPress site URL with the wp-cli](#other-method-to-change-your-wordpress-site-url-with-the-wp-cli)

## Deploying a neteworking stack
In this we are setting up an isolated network stack in AWS and while doing this we will going to use aws console. We are going to deploy minimal netwoking resourses for the sandbox enviroment.

note: it's only for learning practice

resource using:
- VPC 
- Public Subnets
- Public Route Tables
- IGW
- security group

### VPC
 VPC is an isolated portion of the AWS Cloud populated by AWS objects, such as Amazon EC2 instances.
 
While creating VPC we taking these parameter
Name: AppVPC
IPv4 CIDR block:(select) IPv4 CIDR manual input 
- to give CIDR manually 
IPv4 CIDR: 10.10.0.0/16
IPv6 CIDF block: (select) No IPv6 CIDR block
- it's totally depend upon weather you want IPv6 ip or not   
Tenancy:(select) default

(optional)
add tag :
Give key and value to distinguish.
then click on create vpc

### Subnet
A subnet, or subnetwork, is a network inside a network. Subnets make networks more efficient

To add a new subnet to your VPC, you must specify an IPv4 CIDR block for the subnet from the range of your VPC. You can specify the Availability Zone in which you want the subnet to reside. You can have multiple subnets in the same Availability Zone.

You can optionally specify an IPv6 CIDR block for your subnet if an IPv6 CIDR block is associated with your VPC.

To create the subnet in a Local Zone, or a Wavelength Zone, you must enable the Zone.

While creating Subnet we have to chose VPC Select the same VPC we created earlier(AppVPC)
Then give following paramenter:
subnet name: PublicSubnet1 
- note: just by give name publicsubnet in subnet it does not make it public
Availability Zone: (do not leave it on no prefernece its a good practice )
IPv4 VPC CIDR block: 10.10.0.0/16 (it will be automatically dsiplay)
Pv4 subnet CIDR block: 10.10.1.0/24

(optional)
add tag :
Give key and value to distinguish.(put same key and value which we use in vpc)
then click on create subnet

### Route table
A route table specifies how packets are forwarded between the subnets within your VPC, the internet, and your VPN connection.

when we create a vpc then a default route table created we don't have to use that route table beacuse that route table have access to only local intances.
So while creating a custom Route table we use these paramenter:
Name: PublicRoute1
- note: just by give name PublicRoute in Route table it does not make it public
VPC - (select the same the smae vpc we created ) AppVPC

(optional)
add tag :
Give key and value to distinguish.(put same key and value which we use in vpc)
then click on create route table

### Internet Gateway(IGW)
An internet gateway is a virtual router that connects a VPC to the internet. To create a new internet gateway specify the name for the gateway below.


