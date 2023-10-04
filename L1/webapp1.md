# Building a web App using AWS console in single instance.
**Contents**
  - [1. Deploy a networking stack](#2-deploy-a-networking-stack)
  - [2. Create an Instance](#3-create-an-instance)
  - [3. SSH into your Instance](#4-ssh-into-your-instance)
  - [4. Install the Apache Web Server to run PHP](#5-install-the-apache-web-server-to-run-php)
  - [5. Install PHP to run WordPress](#6-install-php-to-run-wordpress)
  - [6. Install MySQL for adding database](#7-install-mysql-for-adding-database)
  - [7. Install WordPress](#8-install-wordpress)
  - [8. Map IP Address and Domain Name](#9-map-ip-address-and-domain-name)
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

While creating VPC we taking these parameter 
Name: AppVPC
CIDR: 10.10.0.0
