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

While creating a IGW we are using these paramenter:
Name: AppIGW

(optional)
add tag :
Give key and value to distinguish.(put same key and value which we use in vpc)
then click on create IGW

After completing these steps: 
- We  will attach this IGW( AppIGW ) with VPC(AppVPC) and then we go to route table and then we will select PublicRoute1 table.
- Then in bottom we can see routes 
- Selcet that and then click edit route
- Add IGW (AppIGW) into that route table  while giving destination 0.0.0.0/0(means everywhere)
- Go to subnet and select that subnet which we created (PublicSubnet1) and then click on route table and then in route we select are Subnet(Publicsubnet1)

### Security Group(SG)

A security group acts as a virtual firewall that controls the traffic for one or more instances. When you launch an instance, you can specify one or more security groups

While we create a SG we will give these paramenter:
Security group name: AppSG
Description: what we are using for
VPC: slect your VPC(AppVPC)
add Inbound rule and Outbond rule accrding to your prefrences 

and then create this SG


## Create instance using the same networking stack 

#installing (lamp) dependencies:

##Apache2
>> cat /etc/*release*  (checking version of ubuntu -> 22.04)

>> sudo apt update

>> sudo apt install apache2

note: check SG  port 80 and 443 

>> http://your_server_ip

##Mysql 8.0 (latest)
>>sudo apt install mysql-server-8.0

>> sudo mysql  -> it will not ask for password

>> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Database';  (change password according to you (Database))

>> exit

 >> mysql -u root -p  (enter password )

 >> exit

 ##PHP
>> sudo apt install php libapache2-mod-php php-mysql

note: maybe it will ask for reboot so do that

>> php -v

*****************************************************************



#Insatlling wordpress

##Creating a Mysql database and user for wordpress

>> mysql -u root -p

>> CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;

note: UTF-8 is a variable-width encoding that can represent every character in the Unicode character set and it support a maximum of 3bytes and utf8_unicode_ci means it will supports contractions and ignorable characters.


>> CREATE USER 'wordpressuser'@'%' IDENTIFIED WITH mysql_native_password BY 'Wordpress';

>> GRANT ALL ON wordpress.* TO 'wordpressuser'@'%';

note: % means all around acces. 

>> FLUSH PRIVILEGES;

>> SHOW GRANTS FOR 'wordpressuser'@'%';

note: to check your user you created

>>exit;




##Installing additional php extention

>> sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip

>> sudo systemctl restart apache2

note: restart to load thoes additional plugin

>> sudo systemctl status apache2



##comment don't impliment##
	##Adjusting Apache's config to allow for .htaccess Overrides and Rewrite

	note: work with .htaccess -> so that Apache can handle configuration changes on a per-directory basis. and it is by default disable

	To allow .htaccess files, you need to set the AllowOverride directive within a Directory block pointing to your document root. Add the following content inside the VirtualHost block in your configuration file, making sure to use the correct web root directory:




	###Steps to allow it 

	>> sudo vim /etc/apache2/sites-available/wordpress.conf

	****add these line****

	<VirtualHost *:80>
	. . .
	    <Directory /var/www/wordpress/>
	        AllowOverride All
	    </Directory>
	. . .
	</VirtualHost>

	********
#####

>> cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-default.conf_bkp
>> rm -rf /etc/apache2/sites-available/000-default.conf
>> cp /etc/apache2/sites-available/000-default.conf_bkp /etc/apache2/sites-available/wordpress.conf



##Enabling the Rewrite module
it will give wordpress to use permalink feature like for eg:
http://example.com/2012/post-name/
http://example.com/2012/12/30/post-name

note: The a2enmod command calls a script that enables the specified module within the Apache configuration.

>> sudo a2enmod rewrite

>> sudo systemctl restart apache2 





## Enabling the Changes

>> sudo apache2ctl configtest  -> testing to make sure that it's not showing any  syntax error

**optinal**
to surpass warning we can add ServerName  directive to your main (global) Apache configuration file at /etc/apache2/apache2.conf

 The ServerName can be your server’s domain or IP address.

****







##Downloading Wordpress

>> cd /tmp

>> curl -O https://wordpress.org/latest.tar.gz  -> downloading  wordpress file

>> tar xzvf latest.tar.gz -> unzip the wordpress file

	##comment_don't impliment#>> touch /tmp/wordpress/.htaccess -> creating dummy .htaccess file so that this will be available for WordPress to use later.##

>> cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php  -> Creating wp-config.php and copying content from sample file

>> mkdir /tmp/wordpress/wp-content/upgrade -> create the upgrade directory so that WordPress won’t run into permissions issues when trying to do this on its own following an update to its software

>> sudo cp -a /tmp/wordpress/. /var/www/wordpress





##Configuring the WordPress Directory



### adjusting the Ownership and permission to www-data

www-data is the user that apache server run as and  will need to be able to read and write WordPress files in order to serve the website and perform automatic updates. 

>> sudo chown -R www-data:www-data /var/www/wordpress  -> sets every directory in wordpress with permissions to 750

>> sudo find /var/www/wordpress/ -type d -exec chmod 750 {} \; 

>> sudo find /var/www/wordpress/ -type f -exec chmod 640 {} \; -> finds each file within the directory and sets their permissions to 640



- To adjust some secret keys to provide a level of security for your installation. 
- WordPress provides a secure generator for these values so that you do not have to try to come up with good values on your own. 
- These are only used internally, so it won’t hurt usability to have complex, secure values here.

>> curl -s https://api.wordpress.org/secret-key/1.1/salt/ -> secure values from the WordPress secret key generator

>> sudo vim var/www/wordpress/wp-config.php -> config file cange screte key and data base, data base user and passowrd.
****************************
