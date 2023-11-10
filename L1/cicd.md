# CICD using CodePipeline

CICD is continuous integration and continuous deployment/ delivery.

# CodePipeline basics:

## stages of CICD 
source->build->test->deploy (stages of cicd)
## Feature
	-> rapid deployement
	-> maintaing high qaulity 
	-> Approval and innovations
	-> deplotment management
## sources satge 
 We will collect all code of our application and keep it it on repo and developer then do changes in code and it transfer to next stage that is build stage
##  build stage
In this we will convert our software from source code that we used in the source stage such as  c++ or java  into executable code that can be run on a computer this is also know as compiling. this is take place in build stage.
##  test stage 
  After compiling we can't server app to our customer. So we introduce test stage. in this it involves checking the software for any error that could be prevented before offering it our customer or deploying it to our prod env. it can be done by creating automated checks that we have created to look for any preventable or known issues that will occur. or to look know error that happen in our past.
## deploy stage
 After all this stages finally we can deploy our code and can maintain high quality.
## deployment can be done on following:
	-> server
	-> serverless 
	-> container
# Use Case of CICD (CodePipeline):
it has the ability to integrate with several services and options both within the aws cloud and no aws (externally).
## Source
It can be from AWS CodeCommit  or AWS s3 or AWS ECR or anywhere you want to store your file like GITHUB(third party tool) as repo or bitbucket.
## Build
AWS CodeBuild or AWS artifact or Jenkins(third party tool) 
## Deploy
AWS CodeDeploy
-> we can also use third party tool and aws resources all together like github for repo(source) and jenkins to build our file and then aws CodeDeploy for deployment. 
-> we can also choices where we can deploy using AWS CodeDeploy like AWS ECS , AWS Elastic Beanstalk, AWS EC2, AWS lambda
-> AWS CodePipeline isn't just AWS services can only use with AWS Codestar, AWS codeCommit, AWS codebuild and aws CodeDeploy , but can also use third party tool  and this thing only happen because of integration feature of codepipeline and make it possible to mix and matching services with stages
# Integration With CodePipeline:
## Additional Action
	-> Approve
	-> invoke
		-> Action can be done by using AWS SNS (simple notification serices) 
		-> Invocation can be done by usign AWS Lambda to invoke CodePipeline
		-> AWS steps Function services:
			-> it give options to allow to organize your application into workflow and tasks for easy management.
		-> Third party tool:
			-> we can use (snyk) tool and integrate with aws codePipeline.
## Source 
#### S3
Feature is (versioning and monitoring) and integrate with lambda and monitoring services and it can be used as repository .
#### AWS Code Commit
It is actually code repo of AWS which offers VCS(Version control service) and source branch.
#### AWS ECS
when we work on container and then this can support containers, Docker Compatible, and push and pull command to upload docker images in docker image in repo and also if we are using AWS ECS (it can integrate with it).
#### Third Party tool
Git and bitbucket to do that we have to set connection.
## Build
#### 	AWS CodeBuild
It is Full managed services and it will take care of everything i.e., (run test, compile and produce artifacts).
#### third party tool
Jenkins or team city or cloud bees. And for using Jenkins we have install CodePieline plugin and it supports by per project basis and additional security requirement to access Jenkins project.

	->  these tool not only used to build but also to test our products. build and test are so tightly conected that some  product allow to do both and if we want to work sepratly on test then we can work to test specifically by using tool
## Test
#### AWS Devices Farm
#### Third party tool
Run scope, Blaze Meter, Microfocus, Ghost Inspector.
## Deploy
while deploying we have several option like serverless , server and container.
 ### AWS S3
 We can use in sources stage but can also work here also because it has feature like version and monitoring activity on our files and place to deploy to.
 ### AWS CodeDeploy
 to deploy code on server or on premise server or both
 ### AWS Elastic BeanStalk
 it will quickly deploy application and automatically setup your web server, infrastructure, scaling, load balancing, monitoring and many more with in aws cloud by configuring.
 ### AWS Opsworks
 if we know how chef work and it good services to use because it use chef cookbook and allow to define an application's architecture and components such as packages, software configuration and resources like storage and other
 ### Other aws Services to build are
 AWS AppConfig, AWS CloudFormation, AWS services Cat log aur Alexa For business.
 ### Third party tool
 (Xebia lab) and many more options 
# CodePipeline security:
security is necessary because we using so many services.
## Main Areas of sercurity in codepipeline is
### AWS Identity and Access management (IAM)
With IAM we can authenticate and give authorization after sign in to use codepipeline services
### Authentication
For the source repositories and other components it can be done by:
1) **users** and **groups**:-> it use to provide long term access.
2) **role**:-> it is used for short-term access to complete a specific task and role is use to assign short-term permission to same account or different AWS account.
	 1.  Federated users i.e., with accounts outside aws.
	 2. Application running in EC2 instances(virtual servers)
### Authorization
it is through identity and resources-based polices.
**policies**:-> it's written in json file and give authorization identity to have access and perform  what action on what resources and under which conditions and it can be applied to identity i.e., (user and group) or resource based like S3
### Encryption
It includes both encryption of data at rest(in code repo but it's not secure between communication) as well as encrypted communications between entities(between resources) i.e., encryption in transit.
-> in every stage resources produce artifacts i.e.,(data) and to secure that we have to apply encryption
-> eg:-> if AWS CodeBuild is using S3 to store code artifacts from the build stage the we can use s3 sources-side data encryption to protect the data.
-> It is done by keys and in aws it is done by (Managed keys) created though create (pipeline wizard) in codepipeline. it is of two type :
1) AWS Managed key -> can't be deleted or change ( we have to just allow but can't manage.)
2) Customer Managed key -> it can deleted, change, and rotate. ( it's complex)
### Secrets
In this we have to manage secretes within the pipeline.
->any information like (password, server credentials, api keys, Hardcoded information)
->to manage this we can use (AWS Secrets Manager), In this we can store, rotate, manage, and retrieve secrets.

    we can call by using api call to secrets manager for info securely and programmatically.
## Cost Structure
**free tier** -> in this we can have one active pipeline per month(30 day) and active pipeline means one code changes should be happen in one month.
we can configure 10 pipeline and 1 will be free  and other will be charged by $1 per pipeline per month
**AWS CodeCommit** -> we free 5 active user, 1000 repositories , 50 gb storage, 10,000 git request and after that we have to pay $1 per additional active user and $0.06 per additional GB storage and even $0.001 per additional git request.
**AWS CodeBuild** -> in this we can have up to 100 build minutes per month with compute type (genral1.small or arm1.small) instance type, We can deploy it on AWS EC2 with no charges but if deploy  On-premise server then it will charge $0.02. 
## Monitoring
How we can know whether code build  failed or not for that what  can be monitored?
Tool to use to monitored is:
### AWS EventBridge:
-> publisher/subscriber model concept
-> Default events published free of cost but if it been customized then it will charged.
-> cross-account access ($1 dollar per 1,000,000 event)
-> third-party access ($1 dollar per 1,000,000 event)
### Event:
 - started->stopped->Succeeded->Failed
 - started->stopped->Succeeded->Abandoned
### Amazon CloudTrail:
-> logs user and role activity
-> logs api calls to other services
 -  these api call been monitored 
 - to store log we use Amazon s3.
### Amazon SNS:
to get notification
# Steps to create code pipeline 
![image](https://github.com/amanmallsops/road-to-devops/assets/146840696/eb05c21d-f4fd-4426-b05e-680e804f95e4)

 ## Step 1: create source 
In this we are choosing git repo and with our application code we added two files (**appspec.yml ** and ** buildspec.yml**) 
and one folder for script which is used by appspec at time of deployment .
Appspec.yml is used by code deploy.
Buildspec.yml is used by codebuild and its not imp to put that file name you can also define that name.
## Step2 : create IAM  role and attached to their following recourse  
It will create few  role automatically but few role for codepipeline have to created by yourself. and for secret manager we added please check images below

![Screenshot (22)](https://github.com/amanmallsops/road-to-devops/assets/146840696/0effd037-57a0-4b44-bf6d-25b330fdb694)
![Screenshot (23)](https://github.com/amanmallsops/road-to-devops/assets/146840696/61cbc64a-ff1f-4720-8d88-b60e9e3285f3)
![Screenshot (24)](https://github.com/amanmallsops/road-to-devops/assets/146840696/63aa6b94-d6fc-4248-ae2b-9f73a5231273)
![Screenshot (25)](https://github.com/amanmallsops/road-to-devops/assets/146840696/6f2530e1-412d-4af8-8ebe-120caad76f17)
![Screenshot (26)](https://github.com/amanmallsops/road-to-devops/assets/146840696/53b7bbff-996a-492e-8911-10d4e3c38e65)
![Screenshot (27)](https://github.com/amanmallsops/road-to-devops/assets/146840696/0ce6787d-6322-464e-b8e4-07e8e0f875b1)


