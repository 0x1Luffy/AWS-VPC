# AWS VPC with Auto Scaling, Application Load Balancer, and NAT Gateway

## About the Project

This project demonstrates how to create a VPC that you use for servers in a production environment. To improve resiliency, you deploy the servers in two Availability Zones, by using an Auto Scaling group and an Application Load Balancer. For additional security, you deploy the servers in private subnets. The servers receive requests through the load balancer. The servers can connect to the internet by using a NAT gateway. To improve resiliency, you deploy the NAT gateway in both Availability Zones.

---

## Architecture Diagram

![Architecture Diagram](screenshots/architecture-diagram.png)

---

## Steps to Create the Infrastructure

### 1. **Create a VPC with Subnets, Internet Gateway, and Route Tables**
1. Go to the **VPC Dashboard** in the AWS Management Console.
   ![Step 1](screenshots/Step1.1.png)
2. Click **Create VPC**.
3. Choose **VPC and more** (this option automates the creation of subnets, an internet gateway, and route tables).
   ![Step 3](screenshots/Step1.2.png)
4. Configure the VPC:
   - **Name tag auto-generation**: Enter a name prefix (e.g., `Production`).
   - **IPv4 CIDR block**: Use the default `10.0.0.0/16` or specify your own.
   - **Number of Availability Zones**: Choose **2**.
   - **Number of public subnets**: Choose **2** (these will be used for the NAT gateways and ALB).
   - **Number of private subnets**: Choose **2** (these will host your servers).
   - **NAT gateways**: Select **1 per AZ** for high availability.
   - **VPC endpoints**: Leave as default (or configure if needed).
   - **DNS options**: Enable DNS hostnames and DNS resolution.
5. Click **Create VPC**.
   ![Step 12](screenshots/Step2.png)
   - AWS will automatically create:
     - A VPC.
     - Public and private subnets in two Availability Zones.
     - An internet gateway.
     - Route tables for public and private subnets.
     - NAT gateways in the public subnets.
    ![Step 12](screenshots/Step2.1.png)

---

### 2. **Create an Application Load Balancer (ALB)**
1. Go to the **EC2 Dashboard** and select **Load Balancers**.
   ![Step 13](screenshots/LoadStep1.png)
2. Click **Create Load Balancer** and choose **Application Load Balancer**.
   ![Step 14](screenshots/LoadStep2.png)
3. Configure the ALB:
   - Select your VPC.
     ![Step 15](screenshots/LoadStep3.png)
   - Add both public subnets (created automatically by the VPC wizard).
     ![Step 16](screenshots/LoadStep4.png)
   - Configure security groups to allow HTTP/HTTPS traffic.
     ![Step 17](screenshots/LoadStep5.png)
4. Create a target group for your servers.
   ![Step 18](screenshots/LoadStep7.png)
5. Complete the setup and note the ALB’s DNS name.
   ![Step 19](screenshots/LoadStep9.png)
   ![Step 19](screenshots/LoadStep10.png)
   

---

### 3. **Create an Auto Scaling Group**
1. Go to the **EC2 Dashboard** and select **Auto Scaling Groups**.
   ![Step 20](screenshots/Step3.png)
2. Click **Create Auto Scaling Group**.
3. Configure the group:
   - Select a launch template or create a new one with your server configuration (e.g., AMI, instance type).
     ![Step 22](screenshots/Step3.2.png)
   - Choose the private subnets (created automatically by the VPC wizard).
     ![Step 23](screenshots/Step3.3.png)
   - Attach the ALB target group.
   - Set scaling policies (e.g., based on CPU utilization).
4. Complete the setup.

---

### 4. **Test the Setup**
1. Access your application using the ALB’s DNS name.
2. Verify that traffic is distributed across servers in both Availability Zones.
3. Ensure servers in private subnets can access the internet via the NAT gateways.

---

## Step-by-Step Creation Using Screenshots

Here are the screenshots for each step(I will attach all the screenshots i took in the order):

1. ![Step 1](screenshots/Step1.png)
2. ![Step 2](screenshots/Step1.1.png)
3. ![Step 3](screenshots/Step1.2.png)
4. ![Step 4](screenshots/Step2.png)
5. ![Step 5](screenshots/Step2.1.png)
6. ![Step 6](screenshots/Step3.png)
7. ![Step 7](screenshots/Step3.1.png)
8. ![Step 8](screenshots/Step3.2.png)
9. ![Step 9](screenshots/Step3.3.png)
10. ![Step 10](screenshots/Step3.4.png)
11. ![Step 11](screenshots/Step3.5.png)
12. ![Step 12](screenshots/Step3.6.png)
13. ![Step 13](screenshots/Step3.7.png)
14. ![Step 14](screenshots/Step3.8.png)
15. ![Step 15](screenshots/Step3.9.png)
16. ![Step 16](screenshots/Step3.10.png)
17. ![Step 17](screenshots/Step3.11.png)
18. ![Step 18](screenshots/Step3.12.png)
19. ![Step 19](screenshots/LoadStep1.png)
20. ![Step 20](screenshots/LoadStep2.png)
21. ![Step 21](screenshots/LoadStep3.png)
22. ![Step 22](screenshots/LoadStep4.png)
23. ![Step 23](screenshots/LoadStep5.png)
24. ![Step 24](screenshots/LoadStep6.png)
25. ![Step 25](screenshots/LoadStep7.png)
26. ![Step 26](screenshots/LoadStep8.png)
27. ![Step 27](screenshots/Load8.1.png)
28. ![Step 28](screenshots/LoadStep9.png)
29. ![Step 29](screenshots/LoadStep10.png)

    ## Creating Bastain Host
    1. Go to EC2 Instances
    2. Click on Launch Instance
    3. Just Make the Network Changes as Following

      
31. ![Step 30](screenshots/BastainHost.png)
32. ![Step 31](screenshots/BastainHost2.png)
    ## To Copy .pem file into the Bastian Host Use the command entered in the image
   `` scp —i "Path of the Key" "Again same Path of the key" ubuntu@ipadress_of_bastain_Host:/home/ubuntu``

---

## Notes
- The **VPC and more** option simplifies the setup by automating the creation of subnets, internet gateways, route tables, and NAT gateways.
- Ensure proper security group rules are in place to restrict access.
