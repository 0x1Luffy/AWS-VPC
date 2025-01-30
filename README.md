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
   ![Step 1](screenshots/step-1.png)
2. Click **Create VPC**.
   ![Step 2](screenshots/step-2.png)
3. Choose **VPC and more** (this option automates the creation of subnets, an internet gateway, and route tables).
   ![Step 3](screenshots/step-3.png)
4. Configure the VPC:
   - **Name tag auto-generation**: Enter a name prefix (e.g., `Production`).
     ![Step 4](screenshots/step-4.png)
   - **IPv4 CIDR block**: Use the default `10.0.0.0/16` or specify your own.
     ![Step 5](screenshots/step-5.png)
   - **Number of Availability Zones**: Choose **2**.
     ![Step 6](screenshots/step-6.png)
   - **Number of public subnets**: Choose **2** (these will be used for the NAT gateways and ALB).
     ![Step 7](screenshots/step-7.png)
   - **Number of private subnets**: Choose **2** (these will host your servers).
     ![Step 8](screenshots/step-8.png)
   - **NAT gateways**: Select **1 per AZ** for high availability.
     ![Step 9](screenshots/step-9.png)
   - **VPC endpoints**: Leave as default (or configure if needed).
     ![Step 10](screenshots/step-10.png)
   - **DNS options**: Enable DNS hostnames and DNS resolution.
     ![Step 11](screenshots/step-11.png)
5. Click **Create VPC**.
   ![Step 12](screenshots/step-12.png)
   - AWS will automatically create:
     - A VPC.
     - Public and private subnets in two Availability Zones.
     - An internet gateway.
     - Route tables for public and private subnets.
     - NAT gateways in the public subnets.

---

### 2. **Create an Application Load Balancer (ALB)**
1. Go to the **EC2 Dashboard** and select **Load Balancers**.
   ![Step 13](screenshots/step-13.png)
2. Click **Create Load Balancer** and choose **Application Load Balancer**.
   ![Step 14](screenshots/step-14.png)
3. Configure the ALB:
   - Select your VPC.
     ![Step 15](screenshots/step-15.png)
   - Add both public subnets (created automatically by the VPC wizard).
     ![Step 16](screenshots/step-16.png)
   - Configure security groups to allow HTTP/HTTPS traffic.
     ![Step 17](screenshots/step-17.png)
4. Create a target group for your servers.
   ![Step 18](screenshots/step-18.png)
5. Complete the setup and note the ALB’s DNS name.
   ![Step 19](screenshots/step-19.png)

---

### 3. **Create an Auto Scaling Group**
1. Go to the **EC2 Dashboard** and select **Auto Scaling Groups**.
   ![Step 20](screenshots/step-20.png)
2. Click **Create Auto Scaling Group**.
   ![Step 21](screenshots/step-21.png)
3. Configure the group:
   - Select a launch template or create a new one with your server configuration (e.g., AMI, instance type).
     ![Step 22](screenshots/step-22.png)
   - Choose the private subnets (created automatically by the VPC wizard).
     ![Step 23](screenshots/step-23.png)
   - Attach the ALB target group.
     ![Step 24](screenshots/step-24.png)
   - Set scaling policies (e.g., based on CPU utilization).
     ![Step 25](screenshots/step-25.png)
4. Complete the setup.
   ![Step 26](screenshots/step-26.png)

---

### 4. **Test the Setup**
1. Access your application using the ALB’s DNS name.
   ![Step 27](screenshots/step-27.png)
2. Verify that traffic is distributed across servers in both Availability Zones.
   ![Step 28](screenshots/step-28.png)
3. Ensure servers in private subnets can access the internet via the NAT gateways.
   ![Step 29](screenshots/step-29.png)

---

## Step-by-Step Creation Using Screenshots

Here are the screenshots for each step:

1. ![Step 1](screenshots/step-1.png)
2. ![Step 2](screenshots/step-2.png)
3. ![Step 3](screenshots/step-3.png)
4. ![Step 4](screenshots/step-4.png)
5. ![Step 5](screenshots/step-5.png)
6. ![Step 6](screenshots/step-6.png)
7. ![Step 7](screenshots/step-7.png)
8. ![Step 8](screenshots/step-8.png)
9. ![Step 9](screenshots/step-9.png)
10. ![Step 10](screenshots/step-10.png)
11. ![Step 11](screenshots/step-11.png)
12. ![Step 12](screenshots/step-12.png)
13. ![Step 13](screenshots/step-13.png)
14. ![Step 14](screenshots/step-14.png)
15. ![Step 15](screenshots/step-15.png)
16. ![Step 16](screenshots/step-16.png)
17. ![Step 17](screenshots/step-17.png)
18. ![Step 18](screenshots/step-18.png)
19. ![Step 19](screenshots/step-19.png)
20. ![Step 20](screenshots/step-20.png)
21. ![Step 21](screenshots/step-21.png)
22. ![Step 22](screenshots/step-22.png)
23. ![Step 23](screenshots/step-23.png)
24. ![Step 24](screenshots/step-24.png)
25. ![Step 25](screenshots/step-25.png)
26. ![Step 26](screenshots/step-26.png)
27. ![Step 27](screenshots/step-27.png)
28. ![Step 28](screenshots/step-28.png)
29. ![Step 29](screenshots/step-29.png)
30. ![Step 30](screenshots/step-30.png)
31. ![Step 31](screenshots/step-31.png)
32. ![Step 32](screenshots/step-32.png)
33. ![Step 33](screenshots/step-33.png)
34. ![Step 34](screenshots/step-34.png)
35. ![Step 35](screenshots/step-35.png)

---

## Notes
- The **VPC and more** option simplifies the setup by automating the creation of subnets, internet gateways, route tables, and NAT gateways.
- Ensure proper security group rules are in place to restrict access.
