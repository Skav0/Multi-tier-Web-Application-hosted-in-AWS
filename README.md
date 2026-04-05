# Multi-tier-Web-Application-hosted-in-AWS
This is my first AWS project showcasing how i built, designed and deployed a custom VPC for a Multi-tier Web Application hosted in AWS.


-I'm gonna clone the project locally and make changes using git and then commit so i can use git.
-The App idea is "Simple Todo/Notes App"
-the "Architecture.png" is just a sample, i made some changes such as:
    -modified the security group rules.
    -used different CIDR scope.
    -changed labels.
-See the Project_photos folder to see the project resources.
Read the section below to understand what i did exactly:
###
IN THE VPC:

Started by creating a vpc that points to 6 subnets(2 public, 4 private[2 excplicitly reserved for a database]). Each subnet is associated with its route table(2 private rts point to the private subnet, and 1 public rt points to two subnet[has the igw route so the vpc can reach the internet through it]).
created the 2 security groups(1 for the load balancer and 1 for the internal instance) and configured their inbound/outbound rules.
To be able for the clients to reach the servers i creatd two nat gws in different AZs for high availability. 
When i created the NAT GWS and the ALBs, Elastic IP addresses where automatically associated to them.

IN THE EC2 SERVICE:

created 2 instances and downloaded the apache server on them and added the web app html file in the /var/www/html/ folder. 
I enabled and start the service so it can be available for the client on port 80.
created an application load balancer so it can split the load "round robin" between the two instances.

IN THE BROWSER:
I copied the URL of the ALB pasted it on the my browser to see if the app works. and as you see in the "app_pic.png" photo on in the "web_app" folder the app works fine.


CONFIGURING AUTO SCALING:

For the autoscaling is started by making a bash script "User data script" so i can include user data at instances launched by the auto scaling group(ASG).
I created a launch template so the ASG can use it to scale out/in instances.
Then i created the ASG.
For the sake of simplicity i didn't include any scaling policy.

I assigned the ALB with the ASG for the high availability and scalability. For that each new created instance by the ASG will be handled automatically to the ALB to do its load balancing.



###
