# Obeservabilty and monitring :
## monitring 
it enables failure detection
## oberservabilty
observability helps in gaining a better understanding of the system.
->The goal of observability is not only to detect problems, but also to understand where the issue is and what is causing it
**Obeservabilty**
-> (Metrics give an abstract overview of the system, and logging gives a record of events that occurred.)
-> Logs:-> they are record of activities performed by a sevices during its run time with a corresponding timestamp.
-> Metrics:-> It gives abstract information about degradations in system. (and logs give a detailed view of what is causing these degradations).
-> Traces:-> If a slow downstream microservice is leading to increased response times, you need to have detailed visibility across all involved microservices to identify such microservice then they need a request tracing machnism. So, a trace is a log of client-request serving derived from various microservices across different physical machines
->cloud watch 

	->alrams
	-> logs
	-> Metrics
	-> X-ray traces
	-> Events
	-> Application monitring
	-> insights

## cloudWatch agent?
AWS EC2 Monitoring — 5 min metrics intervals
AWS EC2 Detailed Monitoring — 1 min metrics intervals
CloudWatch Agent running on an EC2 Instance— Detailed metrics

## You can install CloudWatch Agent using three ways:
-> via Command Line
-> via SSM Agent (For a small handful of servers, you can probably manage to do this manually, but what you’ll soon realize is that for any form of consistency and simplifying things as they scale, you need to automate this process. This is where AWS Systems Manager comes into play.) it's like using ansible but ansible access using ssh and ssm (simple system manager) use agent 
-> via AWS CloudFormation


# Cloud watch and alarm 
#  Configuring CloudWatch Agent on linux instance with paramter store
