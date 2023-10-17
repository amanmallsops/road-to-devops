# step 1: Create traget group:
Go to the target group and create a target group in which define the Target group Name and mention the VPC that we created under TG and after that do register the instance in which we have deployed the wordpress as shown below:
<![image](https://github.com/amanmallsops/road-to-devops/assets/146840696/a0e54616-71d4-4d39-bd8a-7ef954145516)>

# step 2: create ALB(application load balancer):
Now we need to create an Application Load Balancer in the public subnet in the VPC that we created earlier and in the listener add the HTTP port 80 and HTTPS port 443 (ACM) created it used so that we can add the SSL termination certificate for the Target Group we already created. Just need to add it in the Load Balancer. Make sure you add the Security Group of port 80 and 443 open for all.Do fill all the required details and create the Application Load Balancer.
<![image](https://github.com/amanmallsops/road-to-devops/assets/146840696/8b27e522-2091-4361-a295-25d906ed8458)>

# step 3: add dns name to R53: 
After creating ALB we get DNS and we will create a record in which we will add dns in R53 with domain name and select A type.
<![image](https://github.com/amanmallsops/road-to-devops/assets/146840696/b4b942d9-ad59-46aa-868e-bf13c8229cd5)>
