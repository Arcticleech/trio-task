# Trio-task AWS Challenge

In this AWS Challenge, our task was to set up a flask application running through a EC2 Instance with an RDS database. The set up of this architecture in AWS would comply with as minimal viable product:<br><br>

* A custom VPC
* Public and Private Subnet
* Security group for public and private subnet
* Route Tables for connection to public from private
* Internet Gateway for public subnet
* EC2 Instance
* RDS Database

The AWS architecture would allow access to the public subnet from the web, but access to the database would only be allowed via the public subnet. In this case for security the subnets where set up with their own security groups to allow traffic only from the selected locations and ports.

## VPC

The VPC will act as the virtual network which i have defined to contain all elements of my architecture.<br>
![vpc]<br>
This will have the CIDR Block ip with a range of /16 to give allowance to a large quantity of avaliable ip addresses within.

## Subnets

The VPC will contain 2 subnets that act as seperate IPs and containing the 2 instances on seperate security groups. <br>
One will act as the public front to the application holding my EC2 Instance, whilst the other will be private, containing the RDS database. The IP range on these subnets at the private being seperate to public needs its own range within /16.
Public being 10.0.1.0/24 and private being 10.0.2.0/24<br>
![subnets]

## Security Groups

Within each subnet will be a security group which will act as a firewall to allow and disallow access.<br> 
[sg]
The private subnet will contain only access to the public subnet, so it is set as a port access on 3306 (mysql/aurora port) to the ip adress of the public subnet. Therefore the only access to this subnet is via the public subnet.<br>
![privsg]
The public subnet security group will allow access on the outbound to all ports/ip, and the inbout being the access to the specific ports we need. Port 22 for ssh, 80-443 for Internet access and port 5000 for flask.<br>
![pubsg]

## Internet Gateway

The internet gateway will allow access from my VPC and the resources within to the open web.<br>
![internetgw]

## EC2 Instance

The EC2 Instance was then made attached to the VPC and Public Subnet and then assigned to the Public Security Group, for access to our flask app on the web.<br>
![EC2]

## RDS Database

The RDS instance was set up to collect the data made by the flask application within the tables created. Ready for use on the flask app front end.This being made within our VPC and Private Subnet, with the Private Security Group assigned. This was made on two avaliablility zones, for optimal avaliability.<br>
![RDS]

## Flask App within Ubuntu

Following all of the above, then we had to access our EC2 Instances via SSH to clone down the resources for the Flask Application, then connecting this app to the RDS database via URI env. Then access to the flask app via public IP on EC2 would show the data stored in the RDS on our front end.<br>
![EC2flapp]

# Stretch goals

To make our architecture more efficient, secure and faster deployed. We have the options to implement more aspects to our project.

* IAM user/roles/policies
* Dockerfile and image mounting
* Load balancing and scalability, additional avaliability zones
* YAML file for repeatability and reusability.

I have started the implementation of my YAML file using the Cloudformation template designer. This will help with the reusability through use of a premade template, that will lower potential for user error and speed up the process of designing the architecture.<br>
![cloudformation]