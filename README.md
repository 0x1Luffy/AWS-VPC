## About the Project

This project demonstrates how to create a VPC that you use for servers in a
production environment.
To improve resiliency, you deploy the servers in two Availability Zones, by using
an Auto Scaling group and an Application Load Balancer. For additional security,
you deploy the servers in private subnets. The server receive requests through
the load balancer. The servers can connect to the internet by using a NAT
gateway. To improve resiliency, you deploy the NAT gateway in bott Availability
Zones.

## Architecture Diagram 
