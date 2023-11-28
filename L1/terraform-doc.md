# Introduction of IAC(infrastructure as a code)
## Traditional IAC 

->Slow Deployment
->Expensive
->Limited Automation
->Human error
->Wasted Resourse 
->Inconsistency

but due to cloud provider like aws -> they remove all these 

So, it is ok to use managment console when we are dealing with **limited** no. of resourses, but in a **larger** organization with elastic and highly scalable cloud env. with immutable infrastucture, this approach is not feasible. beacuse with lots of process overhead that increase the delivery time and even have human error and resulting in inconsistent env.

So, to automate. every one try to use shell, python, ruby, perl, powershell and come up with common solution and it is (Infrastucre as code) like (docker, ansible, terraform, cloudformation, vagrant, packer, puppet, salt stack)


# Types of IAC

To write a script to provision ec2 is not eassy it require prgramming skills and devlopment knowlege and also not reuseable, so to do that we use tools like Terraform and Ansible helps with code and it is wasy to learn and human readable and maintain. 

Beacuse of terraform now we convert large script into simple terraform config file.

## Configuration Management 

-> Ansible
-> chef
-> Puppet
-> SALTStack

these tools are designed to install and manage software, maintain standard stucture, version control, Idempotent(imp ->it means that we run code multiple times and it will only make necessary changes every time we run and it bring the env into a define state and leave anything laready in place as it is without us having to write any additional code).

## Server templating 

-> Docker
-> Packer
-> Vagrant

These are used to create a custon image of a virtual machine or a conatiner and these images already contain all pre installed software and dependencies and virtual machine or docker images and immutable infrastucture.
Most common eg for server templated images are  VM images such as thoes that are offered on Osboxer.org, custom AMI in amazon aws and docker images on dockerHub and other container registries. server templting tools also promote immutable infrastructure unlike configuration managemt tools, 
It means once any vm deployed to remain unchanged. If there are Changes to be made to the image, insted of updating the running instance,  like in case of configuration managemt tools such as ansible, We update the image and then re-deploy a new instance using a new updated image

## Provision Tools

-> Terraform
-> CloudFormation

These tools are used to provision infrastrucre components (or deploy Immutable infrastructure resources) using a simple declerative code, these infrastruce commonents can range from servers such as VM, Databases, VPCs, Subnets, SG and storage and any services provider we choose.

Whereas CloudFormation is specifialy for AWS.

Terraform is **vendor agnostic** and supports plugins for almost all major cloud providers. 

## Why terraform

It is a popular tools and used specifially as infrasrtructe provisioning tool. It is open source tool developed by harshicorp

It installs as a single binary which can be set up very quickly, allowing us to build, manage, destory infrastruce in a matter of minutes. 

One of the biggest advantages of terraform is it's ability to deploy infrastructure across multuple platforms including private and public cloud, such as on-premise, vSphere clusture or cloud solutions such as aws, gcp, azure 

terraform can do this by providers, they helps terraform manage third-party platforms though their API, they enable terraform manage cloud platform like AWS etc, and as well as network infrastructure like BigIP, CloudFlare, DNS, Palo Alto, Infoblox as well as monitring and data management tools like datadog, grafana, auth0, Wavefront, Sumo Logic and even database like InfluxDB, MongoDB, MYSQL, PostgreSQL. version contol system like github, bitbucket or git lab etc,. So it can work with almost every infrastruce platform. 

Terraform uses HCL know as HarshiCorp Configuration language. which is a simple declarative language to define the infrasture resources to be provisoned as blocks of code. 

All infrastructure resources can be define within configuartion files that has a **.tf** file extension.

Code of terraform is declarative and can be maintained in a VCS which allowing it to be distributed to other teams. 

### Declarative

HCL is a declarative language and it's file have a .tf extention and can be created or edit on any editor
The code we defined in the state that we want our infrastucture to be in that's the desired state.
And terraform will take care of what is required to go from the current state to desired state without us having to worry about to get there. 

Internally Terraform work in three phases:

#### init
During this phase, Terraform initializes the project and identifies the providers to be used for the taret env. 
#### Plan
During this phase, terraform drafts a plan to get to the target state.  
#### Apply
Durring this phase, terraform make the necessary changes required on the target env  to bring it to the desired state. 

If for some reasone the env was to shift from the desired state, then a subsequent terraform apply will bring it back to the desired state, by only fixing the missing component. 

### Resource

Every object that terraform manages is called a **resource**.
resource can be a file in local, compute instance, a database sevrver in the cloud or in a physical server (on premise) the terraform manages. 

Terraform manages the lifecycle of the resources from its provisioning to configuration to decommissioning.  

### Terraform state

 Terraform records the state of the infrastructre as it is seen in the real world and based on this, It can determine what actions to take when updating resources for a particular platform. 

 terraform can ensure that the entire infrastructure is always in the defined state at all times. 

The **state** is a buleprint of the infrastruce deployed by terraform.

### Terraform Import


Terraform can read attributes of existing infrastructure components by configuring data sources. This can later be used for configuring other resources within terraform.

Terrafrom can also import other resources outside of terraform that were either created manually or by the means of othe IAC tools, and bring it under it's control, so that it can manage those resources going forward 


### Terrafrom cloud and Terraform enterprise

they both provide additional features thta allow simplifies collaboration between teams managing infrastructure, improved security and a centralized UI to manage terraform deployments. 

All these feature make terraform an excellent enterprise-grade infrastrucre provisioning tool. 


# Getting started with Terraform 

## installing terraform

-> wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

->echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

-> sudo apt update && sudo apt install terraform

-> terraform version 


in this we will work on simple resource and special type of resource that random pit
we will study the basics of terrafrom such as the lifecycle of resources, the HCL format etc.. 

## HCL Basic (Harshcrop Configuration Language)

### syntax
<block> <parameters> {
	Key1 = value1
	Key2 = value2
}

block is defined with in curly braces, and it contains a set of arguments in keys value pair fromat representing the confuguration data.

in simple language, it contains information about the infrastruce platform and set of resources within that platform that we want to create. 

#### task1, create a file where terrafrom is installed 

Steps are:
1.) create a directory 
mkdir /root/terrafrom-local-file
cd /root/terrafrom-local-file
2.) this is where we will create configuration file called local.tf and with in this we will define our resource block and inside resource block we specify the file name to be created as well as its contents using the block arguments.
3.) local.tf -> config file 
resource "locak_file" "pet"{
	filename = "/root/pets.txt"
	content = "we love pets!"
}
4.) run (terraform init) command
It will check the config file and initializinf the working directory containing the (.tf) file.
First thing this command does the it will check resource type declared in the resource blocks.
Then it will download the plugin to be able to work on the resource declared in .tf file
5.) run (terraform plan) command 
It will review the execution plan.
6.) run (terraform apply) command 
To apply changes use apply command
7.) run (terrafrom show) command (optinal)
to see deatils of the resource that we just created and this command inspects the current state file and displays the resource details

Note: Their are many resource type and argument of it 

## Update and destroy infrastructure 

updating local.tf file

file_permission = "0700"

above line added

and run plan and then apply 

To destory infrastructure completely run command -> terraform destory

# Terraform Basics
## using terraform providers 
offical, partner, community

# Configuration Directory
we can create many config file and terrafrom will initialize it all but 
we should use one config file add resource block in it 
the comman naming convention is **maine.tf**
there are other configuration files that can be created within the directory such as:

=> Main.tf ->main configuration file containing resource definition
=> variables.tf ->contains variable declaration
=> ouputs.tf -> contains outputs from rsources
=> provider.tf -> contains provider definition


## Multiple providers
In config file we can include multi provider by just giving resource name 

## Using Input Variables

we want to make sure that the same code can be used again and again, to deploy resources based on a set of input varibales that can be provided during the execution, that is where input variables come in to the picture.
Just as in any genral-purpose programming language such as bash scripting or powershell we can make use of input variables in terrafrom. 

to assign variables, we have to create a new configuration file called **varibles.tf** and define values like given below:

variable "filename" {
	default = "/root/pets.txt"
}
variable "content" {
	default = "Mrs"
}


varibles.tf file is like the main.tf file, it consist of block and arguments. To create a variable, use the keyword called variable.
This is followed by varible name and in argument value in this we are using default value it is optional and their other methods too.

### how to use varibles 
in main config file in argument in place of value we have to assign as (var.filename)
eg:
resource "local_file" "pet"{
	filename = var.filename
	content = var.content
}

now we can check value are correct or not we can use terraform plan


## understanding the varible block 
### diffrent types of arguments of varible block 
varible block access three parameter such as:
1.) default parameter
2.) type -> tell default parameter types (string("/root/pets.txt"), number, bool(true/false), any(default value))
	beside these three simple type their are other such as (list(["cat","dog"]), map({"pet1"="cat" "pet2"="dog"}), set(["mr", "kl", "12"]), object(complex data structure), tuple(complex data structure))
3.) description  (it is optional but it good practice to use)

## using varibles in  terraform 

if in varible.tf file we don't specificy any default value, then when we use terraform apply then it will work interactivly and  even given command as 

->terraform apply -var "filename=/root/pets.txt" -var "content=we love"

even we can also use as Env variables like:
export TF_VAR_filename="/root/pets.txt"
export TF_VAR_content="mrs"
terraform apply


if we dealing with lots of varibles then we can use file name **terraform.tfvars or .tfvars.json** or ending by **.auto.tfvars or auto.tfvars.json** will automatically loaded by terraform 
and inside that file we can give values 
eg:
filename ="/root/pets'txt"
content ="we love pets!"
if you give other filename such abc.tfvars then 
we have to use terraform apply -var-file varibles.tfvars

### variable definition precedence

1.) env varible -> export method
2.) terraform.tfvars
3.) *.autotfvars (alphabetical order)
4.) -var or -var-file (command line flag)

## Resource Attributes

if we want to use the output of one resource to another resource

resource "local_file" "pet"{
	filename = var.filename
	content = "my fav pet is ${random_pet.my-pet.id}"
}
resource "random_pet" "my-pet" {
	prefix = var.prefix
	sparator = var.separator
	length = var.length
}

## resource dependencies

### implicit dependency 
in this resource automativally knows which resource is depend upon which resources, and terraform do that, however, there are another way to specify dependency within the configuration file
by adding 
depend_on -> arguments
eg:

resource "local_file" "pet"{
	filename = var.filename
	content = "my fav pet is mr.cat"
	depends_on=[
	random_pet.my-pet
	]
}
resource "random_pet" "my-pet" {
	prefix = var.prefix
	sparator = var.separator
	length = var.length
}

and this is knows as **explicit dependency**

## output variables
if we want to save the output of terraform then we can add following line
output pet-name{
	value = random_pet.my-pet.id
	description = "record the value of pet ID generated by the random_pet resource"
}

#### syntax
output "<varible_name>"{
	value = "<variable_value>"
	<arguments>
}

we can also use **terrafrom output** command to get out to terraform apply and we can also use as terraform ouput pet-name


best use of terraform output variables is when you wnat to quickly display details about a provison resource on the screen or to feed the output varibles to other iac tools such as ad hoc scipts or answerable playbook

# terraform state
## introduction to terraform state
terraform.tfstate file only created after using terraform apply command at least one time and state file is a json data structure that maps the real-world infrastucture resources to the resource definition in the configuring files. 
it has every single detail to create infrastructer as a single source of truth when using plan and apply

when we do changes in config file and paln or apply then terraform by default refreshes the state again  and compares it against the configuration file. 
it now knows that the resource argument called content has a diffrent value in the configuration file as compared to the terraform state and the real world . 

## purpose of state
state file  can be considered to be blueprint of all the resource that are for managers out there in the real world.
when terraform created a resource, it records its identity in the state and be it the local file resource that creates a file in the machine. A logical resource such as the random pet, which just thorws out the random pet name or the resources in the cloud.
each resources created by terraform have unique id which is used to identity the resources in the real world
Besides the mapping between resurces and the configrarion and the real world, the state file also tracks metadeta details such as resource dependencies. 

eg. r1 is depend upon r2 and r3 is independed so, after apply terrafrom then irst r2 and r3 will be created parallely and after that r1 
and if we remove r1 and r2 and if we apply then how terraform will know whichto delete first so at that place state file is imp

To improve to performace significantly  and to that terraform state is used as satament of truth 
terraform stores a caches of attributes values for all resources in the state. And we can specifically make terraform to refer to the sate file alone.
to do that we can use -refresh=false with command

->tarraform plan --refresh=false


## terrafrom state considerations 
It contain sensitive data every little deatil of our infrastructure 
so, we have store terraform config file in vcs and to store state file in remote state backend such as AWS S3 or Terraform cloud

Notes:
if we want to changes in state file then we have to do that by state commands.

# working with terraform
## terraform commands
### terraform validate
it use to check the config file of terrform and tell the error if is there any
### terraform fmt
this command scans the config files in the current working directory
and formats the code into a canonical format
it used to to improve the readability of terraform config file
### terraform show
it prints out the current state of the infrastructure as seen by the terraform 
we can also print output in json format using

-> terrafrom show -json

### terraform providers
it will tell the list of all provider in the config directory

we can also use mirror command to copy provider into new folder

->terraform providers mirror /root/terraform/new_local_file

### terraform output
if you want to put all output variables in the configuraion directory

-> terraform output

output-> 
content = we love pets
pet-name = huge-owl

we can append the name of the variable to the end of the output command like this it show a specific command

-> terraform output pet-name

output->
pet-name = huge-owl

### terraform refresh
it will pick it up and update the state file, this reconciliation is used to determine what action to take during the next apply
this command will not modify any infrastructure resource, but it will modify the state file

terraform refresh automatically run by using terraform apply and plan
, this is done priror to terraform generating an execution plan.
and it can be bypassed by using the ** -refresh=false** option  with the command
eg: tarraform plan --refresh=false

### terraform graph
it used to create a visual representation of the dependencies on a terrform configuration or an execution plan.
The output after using this command is hard to comprehend as it is but it is a graph genrated in a format called DOT

to make more sense of this graph we can pass it though a graph visualization software such as graphviz we can install into ubuntu using apt install graphviz -y
after that we can pass the output of the terraform graph to the DOT command.

-> terraform graph | dot -Tsvg > graph.svg

and open this file in browser
### terrafrom destory
### terraform init
### terraform plan
### terraform apply
## Mutable vs immutbale infrastructure
### Mutable infrastructure 
if we use the same software upgrade lifecycle for each of the server as we do in one server (using ad-hoc scripts or configuration managements tools like ansible) this type of update is know as in-palce update.
And this is because the underlying infra reamian the same, but the software and the configuration on these server are changes as part of the update. This is example of mutable infrastructure.

updateing software on a system can be complex task, and in almost all cases, there are bound to be set of dependencies that have to be met before an upgrade can be carried out successfully. 
while updating issue can be like network issues impacting the connectivity to the software repository, file system full, or diffrent version of operating system running on web server
### configruration drift
web server1, 2 have same nginx version but 3 have diffrent version and maybe 2 have diffrent version of OS it is know as congigration drift.
and each will act slighltly diffrently sue to configration drift

so to correct that we have to remove that 3 webserver and replcae it new server similar to 2nd webserver and we keep on updating that server but removing old server with new and updated onces so it is know as immutable infrastructure.
it goof also by chanches it update fail thn old server will be intact and new server will be removed and because of it we don't leave any room for configration drift, and it make easy to roll back and roll forward

and terraform use this approch , but by default the terraform destory old resource first before creating a new in it's place. 
We can change to first create or ignore deletion completely by lifecycle rule.
## LifeCycle Rules
it uses the same block syntax that we use in config file, go directly to ressorce block whoes behaviour we want to change
### create_before_destory
lifecycle{
	create_before_destory = true
}
after using this it will create and after successfull creation it will destory old once.
### prevent_destroy
lifecycle{
	prevent_destroy= true
}
after using this it will create and will not destory old one
and this only cappable untill we update and change something it don'y prevent from terrafrom destory.
### ignore_changes
lifecycle{
	ignore_chnages= [tags] -> [] it is list , in this we can give all keyword 
}
in this if we have create a instances with tag name but by chances we change tag name manually or by other mean then terraform will try to change that to the same 
So, after this, it will stop terrarom from changing to what was mentioned in config file.
## Datasources variables
if some thing created not from terraform but manually or by script or ansible then terraform can read that data that is created outside the terraform and also can use as a **datasource**.
it is similer to resource block just in place of resource we have to write data
eg:
data "local_file" "dog"{
	filename = "/root/dog.txt"
}
and use this conntent we can simple write 
resource "local_file" "pet"{
	filename = "/root/pet.txt"
	content = data.local_file.dog.content
}

data source is called as data resource, and resource called managed resource.
## Meta Arguments
If we want to create multiple resource using same config file for that,
we can write a script, but in terraform we can use specific meta arguments in resource block 
we already work with two meta argument and other are below:
### depends_on
eg:
depends_on = [random_pet.my-pet]
### lifecycle
eg:
lifecycle{
	create_before_destory = true
}
### count
in this we use define no. of resurce we want to create
eg:
#### main.tf
resource "local_file" "pet"{
	filename = var.filename
	count = 3 
}
#### variables.tf
variable "filename"{
	default = "/root/pets.txt"
}
in this count = 3, means it will create three file, and it can be denoted as pet[0], pet[1], pet[2].
But their is a issue it create resource with same so, for that a better way to do this and make sure that all the three resource have unique filename is to make use of a list variable for filename.
to do this in **varible.tf** in default we give name in list fromat
eg:
default=[
	"/root/pets.txt",
	"/root/dog.txt",
	"/root/cats.txt"
]

and in main.tf we can give expresion like this
eg: 
filename = var.filename[count.index]

Case study:
If we want to add more file name in default then run apply then it agin only create 3 file and rest new file will not be created , to do that we use built in function **length function** in this it will count that automatically:
count =  length(var.filename) 
after using this it will automatically create new file and as mauch as file we give default value.

Case study:
if we remove one file name from defaut then run apply it will destory and replace first two resource and destory last resource but we have to reomve first then how we can do that, for that we use for_each:
### for-each
it uses resources block, in place of count we use for_each 
eg:
resource "local_file" "pet"{
	filename = each.value   <-
	for_each = var.filename  <-
}
but there is an issue in this code for_each use and read only set and map so, we have two method:
1.) 
either we change add **type** in vairaible.tf 
eg:  
type=set(string)
2.)
either we can give use built in function in for_each line 
eg:
for_each=toset(var.filename)

so, now if we remove first name in default then after running apply it will destory first file

to see working of it add output:
output "pets"{
	value = local_file.pet
}
and afte running terraform output command we see resource stored as a map not as a list.
so, this means thar the resource are no longer identified by index, thereby bypassing the issues that we saw we use count, they are now identified by the keys. we can also check the diffrence of output of count and for_each output. 

## version constraints
if want to use a specific version of provider but by default it depend upon terraform version and according to that the pull that provider version so, to do that we go to terraform documentory of provider and select the desire provide version and then click on use provider 

and use that code in main.tf config file
in this terraform in a resource and will use argument  as required_providers and pass value to the key

eg: 
terraform{
	repuired_providers{
		local={
			source ="harshicorp/local"
			version = "1.4.0"
		}
	}
}

other ways to use version constraint is:
eg:
terraform{
	repuired_providers{
		local={
			source ="harshicorp/local"
			version = "!=2.0.0"  or  "< 2.0.0" or "> 2.0.0"
			-or-
			version = "!=2.0.0, < 2.0.0, > 2.0.0" <- add comparison
			-or-
			version = "~> 1.2.0" -> specific version or incremental (1.2.0 to 1.2.9) version

		}
	}
}
in this case we have specifically asked terraform to not use ther version 2.0.0 

# Terraform with AWS
## Introduction of IAM
Administor -> AdministorAccess
Billing -> Billing
Database Administrator -> DatabaseAdministor
Network Administrator -> NetworkAdministrator
View-only user -> ViewOnlyAccess
### Programitiacl access
### AWS CLI 
It interact to aws using terminal
### AWS CLI Command
aws <command> <subcommand> [option and parameters]
## AWS IAM with terraform
while working with terraform we have give provider,
and where we have to give sensitive inforamtion.
So, we have to these aws credential on .aws/config/credentials
in local system where we are using terraform 

after we save credential in credential file then we don't have tp put cred in provider in config file

we can also export aws cresdential on termianl 

## how to give command in terraform 
### syntax -> Heredoc Syntax
[COMMAND] <<DELIMITER
	line1
	line2
	line3
DELIMITER
1.)
it used to give many lines of  command in single line 
eg:

resource "aws_iam_user" "admin-user"{
	name = "lucy"
	tag = {
	description = "techinal"
	}
}
resource "aws_iam_policy" "adminUser"{
	name ="AdminUsers"
	policy = <<EOF
	{
		"version" : "2012-10-17"
		"statement" : [
			{
				"Effect" : "Allow",
				"Action" : "*",
				"Resource" : "*"
			}
		]

	}
	EOF
}

2.)
in this we store commad in a file and call that file in terraform
eg:
##### mian.tf
resource "aws_iam_user" "admin-user"{
	name = "lucy"
	tag = {
	description = "techinal"
	}
}
resource "aws_iam_policy" "adminUser"{
	name ="AdminUsers"
	policy = file("admin-policy.json")
}
resource "aws_iam_user_policy_attachment" "lucy_admin_access"{
	user = aws_iam_user.admin-user.name
	policy_arn = aws_iam_policy.adminUser.arn
}

##### admin-policy.json
	{
		"version" : "2012-10-17"
		"statement" : [
			{
				"Effect" : "Allow",
				"Action" : "*",
				"Resource" : "*"
			}
		]

	}
# Remote state
terraform state is created when we run terraform apply first time 
terraform state benifits are:

-> Mapping configuration to real world
-> tracking metadata such as dependencies
-> improving the performance of terraform operations
-> collabrarion to provision resources as a team

## state locking
if we run terraform indivial on diffrent  system the it will give a unintended consequences such as corruption of the state file

to deal with it while the first operation is in progree terraform locks the state file. As a consequence, we will not be able to run any operation from the second terminal until the first operation finishes 

this the very important feature called state locking, it ensure that the stae file does not get corrupted by multiple opreations trying to update it at the same time.

and github like repositry do not support state locking whcih we result in issue like conflicts and data loss within the state file. And even if developer forget to refresh the state file then it will lead to error and it will point to roll back or even destorying there resources because of that we use **remote state backend** to store terraform state
so it is imp to store state file in shared storage solution such AWS S3, hashiCorp Consul, terraform cloud etc.. 

when a reomote backend is configured the terraform automatically load and upload state file every tume is required by a terraform operation 

It will also upload the updated terraform state file after every apply. And most importantly many of these remote backend options such as the AWS S3 allows for state locking this ensure that the integrity of the state file is maintained 

finally, the remote backend proide diffrent ways to secure the storage such as encryption at rest and in transit to make sure that all the sensitive information stored inside is secured.

## Remote Backend with s3
prerequsiet to save state file in s3 is:
S3 bucket and inside it a folder to store Terraform state in S3 and state locking table in DynamoDB table which will be used to implement state locking and consistency check

keep a note of :
bucket name, key to be used(eg: finance/terraform.tfstate), the region, region, dynamoDB Table

to configure remote backend state we must define additional setting by making use of the terraform block (we already use this to get a speciphic provider) but now will use to configure remote backend state and for that block will look like as follows:
terraform{
	backend "s3" {
		bucket = "bucket_name"
		key = "folder_name/terraform.tfstate"
		region = "us-west-1"
		dynamodb_table = "state_locking" -> it is optinal  use to achive state locking this table should have a primary or hash key with name lockID  
	}
}

as a standard practice, let's not store the terraform block inside main.tf, instead, we move it ot separate file called terraform.tf
then run terraform init to initilaize backend to s3

and we already have a state file and while we initilazie it will upload state file to s3.

## terraform state commands 

we see how we mainpulate the terraform state, beacuse we cannot edit terrafrom state file using vi or vim editor

terraform provides a set of commands that allows us to list, pull and manipulate state. and these is used insted of manually editing these files.
#### syntax of terraform state command are:

->terraform state <subcommand> [option] [args]

eg: 

-> terraform state show aws_s3_bucket.finance

### sub-command are
#### list
it will list all the resources recorded within the terraform state  file
eg: it only print the state-addres
terrafrom state list
terrafrom state list aws_s3_bucket.finance-2020922
#### mv
it used to move item in a terraform state file
**terraform state mv [option] source destination**
eg: for to change name of db in state file
terraform state mv aws_dynamodb_table.state.locking aws_dynamodb_table.state-locking-db
#### pull
we don't have terraform in local so to see the state we use this command
eg: 
terraform state pull
and even we use filter like if we want to filter the lockID from state file then we ca do :

->terraform state pull | jq '.resorces[] | select(.name =="state-locking-db")|.instances.attributes.hash_key'

#### rm
it used to delete items from the terrafrom state file.
it used to when you no loner wish to manage one or more resources 
via the current terrafrom configuraton and state.
terrafrom state address rm address
after the removing it make sure to reomve the associated resource block from the configuration file as well
note:
resource removed in the state file don't delete the resorce in real world
infrastructure.
#### show
it will give detail information about a resource from the state file. We can make use of the terraform state show command
eg:
terrafrom state show aws_s3_bucket.finance-2020992

# Terraform provissioners
## AWS EC2 with terraform
eg:
resource "aws_instance" "webserver"{
	ami = "ami-j0mkcceckjcjkoecmeo0009"
	instance_type = "t2.micro"
	tag={
		Name = "websrver"
		Description = "An Nginx webserver on ubuntu"
	}
	user_data = <<-EOF
			"sudo apt update",
			"sudo apt install nginx -y",
			"sudo systemctl enable nginx",
			"sudo systemctl start nginx
				EOF
	key_name = aws_key_pair.web.id
	vpc_security_group_ids = [aws_security_group.ssh-access.id] -> need to give [ ] because it is in list format
}
resource "aws_key_pair" "web"{
	public_key = file("/root/.ssh.web.pub")
}
resource "aws_security_group" "ssh-access"{
	Name = "ssh-access"
	description = "Allow SSH Access form the internet"
	ingress{
		from port = 22
		to_port = 22
		protocol = "tcp"
		cicd_block = ["0.0.0.0/0"]
	}
}
output publicip{
	value = aws_instance.webserver.public_ip
}

Terraform provissioners
### remote exec
Provisioners provide a way for us to carry out tasks such as running commands or script on remote resources or locally on the machine where terraform is installed. 
eg: to run a bash script after a resource is created, we can make use of the remote exec provisioner, using the same web server example that we have seen many times so far, we can pass in an line script like this.
eg:

resource "aws_instance" "webserver"{
	ami = "ami-j0mkcceckjcjkoecmeo0009"
	instance_type = "t2.micro"
	tag={
		Name = "websrver"
		Description = "An Nginx webserver on ubuntu"
	}
	user_data = <<-EOF
				script
				EOF
	provisioner "remote-exec"{  -> it used when resource is created
		inline = [
			"sudo apt update",
			"sudo apt install nginx -y",
			"sudo systemctl enable nginx",
			"sudo systemctl start nginx"
		]
	}
	connection{  		-> used for remote exec provisoner
		type = "ssh"
		host = self.public_ip
		user = "ubuntu"
		private_key = file("/root/.ssh/web")
	}
	key_name = aws_key_pair.web.id
	vpc_security_group_ids = [aws_security_group.ssh-access.id] -> need to give [ ] because it is in list format
}
resource "aws_key_pai" "web"{
	public_key = file("/root/.ssh.web.pub")
}
resource "aws_security_group" "ssh-access"{
	Name = "ssh-access"
	description = "Allow SSH Access form the internet"
	ingress{
		from port = 22
		to_port = 22
		protocol = "tcp"
		cicd_block = ["0.0.0.0/0"]
	}
}
output publicip{
	value = aws_instance.webserver.public_ip
}

### Local exec
it know as create time provisoiner


resource "aws_instance" "webserver"{
	ami = "ami-j0mkcceckjcjkoecmeo0009"
	instance_type = "t2.micro"
	tag={
		Name = "websrver"
		Description = "An Nginx webserver on ubuntu"
	}
	user_data = <<-EOF
				script
				EOF
	provisioner "local-exec" {  -> it create a file and put that data which you ask
		command = "echo ${aws_instance.webserver .public_ip} >> /tmp/ips.txt" 
	}
	connection{  		-> used for remote exec provisoner
		type = "ssh"
		host = self.public_ip
		user = "ubuntu"
		private_key = file("/root/.ssh/web")
	}
	key_name = aws_key_pair.web.id
	vpc_security_group_ids = [aws_security_group.ssh-access.id] -> need to give [ ] because it is in list format
}
resource "aws_key_pai" "web"{
	public_key = file("/root/.ssh.web.pub")
}
resource "aws_security_group" "ssh-access"{
	Name = "ssh-access"
	description = "Allow SSH Access form the internet"
	ingress{
		from port = 22
		to_port = 22
		protocol = "tcp"
		cicd_block = ["0.0.0.0/0"]
	}
}
output publicip{
	value = aws_instance.webserver.public_ip
}


note: by default provisioner are run after resource are created.


### destory time provisioner
we can make provisioner run before a resource is destoryed

resource "aws_instance" "webserver"{
	ami = "ami-j0mkcceckjcjkoecmeo0009"
	instance_type = "t2.micro"
	tag={
		Name = "websrver"
		Description = "An Nginx webserver on ubuntu"
	}
	user_data = <<-EOF
				script
				EOF
	provisioner "local-exec" {  -> it create a file and put that data which you ask
		when = destory -> destory time provisioner
		command = "echo ${aws_instance.webserver .public_ip} >> /tmp/ips.txt" 
	}
	connection{  		-> used for remote exec provisoner
		type = "ssh"
		host = self.public_ip
		user = "ubuntu"
		private_key = file("/root/.ssh/web")
	}
	key_name = aws_key_pair.web.id
	vpc_security_group_ids = [aws_security_group.ssh-access.id] -> need to give [ ] because it is in list format
}
resource "aws_key_pai" "web"{
	public_key = file("/root/.ssh.web.pub")
}
resource "aws_security_group" "ssh-access"{
	Name = "ssh-access"
	description = "Allow SSH Access form the internet"
	ingress{
		from port = 22
		to_port = 22
		protocol = "tcp"
		cicd_block = ["0.0.0.0/0"]
	} 
}
output publicip{
	value = aws_instance.webserver.public_ip
}

notes: 

if provisioner get fail while apply it will stop it know as -> failuer behavior.

### failuer behavior 
we have add line 
on_failure = continue  -> by deafult its fail



as a best practice, use terraform provisonal as a last resort and instead use option natively available for resource types for the provider used. 
eg:
use 
	user_data = <<-EOF
				script
				EOF

## consideration with provisioner 
to use provisioner in terraform it require to setup connection first between local to server and it not feesbel
so it best to use native feature like **user_data** and better to prebuild image or custom image by server templating (packer) after using this there is no need to use user_data or remote or local exec provisioiner

# terraform taint
when we try  to apply changes the resource creation failed because the part specified in the command of the local exit provisioner is invalid. When this happens and a resource creation fails for any reason. terraform marks the resource as tainted 
 
this can be seen when we run the terraform plan command again, the resource called web server is tainted, as aresult terraform will attempt to recreate it during the subsequent apply.

now there would be cases when we want to force a particular resource to be recreated. 

consider a case where manual changes to an AWS instance which is managed by terraform such as changing the verson of nginx running on it by manual methods. 
to revert this, we can either destory the resource using the command terraform destory, and then run apply again, **or** the better way would be taint the resource using the terraform taint command like this:

-> terraform taint aws_instance.webserver

when we run terraform plan next we will see that teh ressource is set to be recreated 

to undo a taint that was applied for a specific resource, use thr terraform untaint command like this:

-> terraform untaint aws_instance.webserver

now this resource will not be created now during a subsequent terraform apply. 

# debugging
wehn things go wrong in terraform, the obvious place to look for the cause is in the logs, although terraform apply often provides a good indicaton of what may caused an during the provisioning 

to do that we can use and enable:

->  export TF_LOG=TRACE 

types of log level are:

-> INFO
-> WARNING
-> ERROR
-> DEBUG
-> TRACE

type follwing coammandd before using apply

to save log in a file use:

-> export TF_LOG_FILE=/tmp/file_name.log

and to unset use:

-> unset TF_LOG_FILE

# terraform import

if any the resource allready made or made by consule then terraform can't manage it automatically
so for that we use data resource but though that we can use their value and result, stillc can't edit so

we run terraform import <resource_type> <resource_name> <attribute>

eg:

-> terraform import aws_instance.webserver-2 i-0233232k32k93en3

but it still show an error that it didn't exist in terraform config file
for that we write a empty resource block 
like this:

resource "aws_insatance" "webserver-2"{
	# (resource arguments)
}

and now we run the same command it will run without any error and put that resource in tstate it's means it will update the state ,  but still we can't edit aur manage that resource 


to get we can inspect the instance from the management consoel aur alternatively, since it's already been imported,  we can also inspecct the state file instead and look for the attributes. 

once we have all the details fill in the arguments in the resource block for the webserver 2.

 for eg:

 resouce "aws_instacne" "websever-2"{
 ami = "ami-81922n381813"
 instance_type = "t2.micro"
 key_name = "ws"
 vpc_security_group_ids = ["sg-9832239"]
 }

Here, we have added the values for AMI instacne type, the key name and the security group ID used by this insatance. Once all the arguments and the correct values have been proided 

then we run terraform plan will refresh the state and teh understand that the ec2 instance already exists will carry out no action 

now this resource is under control of terraform

# terrafrom module

for eg:
their are many resource using in terraform main.tf and it can be posible that the two ec2 instance can be quuite similar with much of thier code being duplicate between the two configuration block of resource, we can still opt to write the configuraton with an a single file. and terraform not impose any limit on the number of resource per config file. however, this would mean that our config file will contain hundreds to thousand of lines of code within no time. 

alternatvily we split our config file into multiple files within the config directory and things will work the same way 

terraform use every config as same way, when it have .tf extention 

disadvantages of making in same file:

-> it does not fix complexcity of configuration of files 
-> the problem of duplicate code
-> due to complexity it become incresingly difficult to update a resource. (imcreased risk)
-> and this config is serverly limits the reusability 

and all can give error it code given to another teammate 

to get over all these problem we can  use modules in terraform any confguration containing config file know as module 

Any directory conatining main.tf file will be consider the root module 

to use module in another directory we use a resource block:
eg:
module "dev-webserver"{
	source = "../aws-instance"
}

and scince we using current directory to run terrafrom command will be root module then that other directroy within our main.tf file is  called child module. 

in source we have to give relative path with respect to  config directory or the root module. 

## creating and using a module

advantages to use module is:

-> simpler config files
-> lower risk
-> re-usability
-> standardized configuration

## using modules from registry

custom module we are using are local module which reside it avaible in local directory and advantages is that it can be shared 

in terrafrom provide provider which we use terraform resource and terraform also give terraform registry 

**The modules enter from registry are group  based on the provider for which they are created and they are available for anyone to use to provison infrastructure**

they can be categorized into two types 

-> A verified module 
-> the community module 


for eg: if search module like Security group then that module here are tested and maintined by hashicorp and have blue tech bar next to it's name and other modules which don't check mark or blur thik are made by community, but they are not validaet by hashicorp. 

A terraform module that is available in the resgistry should be usussly contain destils about the moudule such as the publisher information of the module that is diffrent version available in the registry and instruction on how to use the module with examples. 

and also conatins several sub modules that can be used for diffrent use case 

to make use of module configration we can make as we done proviosly, but the value for the source argument would changes as the module is no longer local 

if provider is already there then we just have to **terraform get** command and it will only download the module from the registry 

it usually a good idea to specify the version of the module as there can be many revision that are made  to each module over time, not specifying the version will always download the latest version of the module from the registry 

this may have introduce changes to the way it works which may not always be desirable  

# terraform functions

## function

-> file("") ->file fucntion to read data from the file
-> length("") -> to determine the no. of elements in a given list or a map. 
-> toset() -> it is used to convert a list to a set 

to  test function as well as interpoations and so to get into this interartive console we use the command:

-> terraform console 

it loads the state associated with the configuration directory by deafult allowing us to laod any values that is currently stored in it. 

it also loads variables that are store within configuratin files, this will allow us to experiment with the function and interpolations that can later be made use of within the configuration files 

common funtion are 

-> numeric fucntion
-> string function
-> collection function
-> type conversion function 

