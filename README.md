# Cloud 5 Project: Elastic Load Balancer Implementation

## Project Statement
The goal of this project is to implement load balancing with an Elastic Load Balancer in order to distribute incoming traffic across multiple EC2 instances, ensuring high availability and preventing any single server from becoming overloaded.

## Team Name
**Team Name:** Cloud 5

## Steps Followed

### Step 1: Creating Instances
1. Launch two instances in different subnets for high availability.
   - Instance 1:
     - Name: Instance 1
     - AMI: Amazon Linux
     - Instance Type: t2.micro
     - Key Pair: Create a new key pair
     - Network Setting: Subnet 1a
     - Security Group: Create a new security group (allow SSH and HTTP from anywhere IPv4)
     - Advanced Details: User data script (optional)

     ```bash
     #!/bin/bash
     yum update -y
     yum install -y httpd

     # Fetch the public IP address of the instance
     PUBLIC_IP=$(ec2-metadata -o | cut -d " " -f 2)

     echo "Hello, World! This instance's public IP address is: ${PUBLIC_IP}" > /var/www/html/index.html

     systemctl start httpd
     systemctl enable httpd
     ```

   - Instance 2:
     - Same configuration as Instance 1 but in a different subnet (e.g., Subnet 1b).

### Step 2: Creating Load Balancer
2. Go to the EC2 console, navigate to Load Balancers, and create an Application Load Balancer.
   - Name: Demo Load Balancer
   - Keep other settings as default
   - Select all mappings
   - Create a new security group (allow SSH and HTTP from anywhere IPv4) and attach it
   - Create a new target group, choose the instance type
   - Name the target group and register both instances (include as pending)
   - Create the target group and attach it to the load balancer

### Step 3: Testing and Verification
3. Copy the DNS name of the load balancer.
4. Open a browser, paste the DNS name, and refresh the page.
   - Verify that different instance addresses are displayed after each refresh, indicating that the load balancer is distributing the server load.

   > Load balancers prevent any single server from becoming overloaded, ensuring optimal performance.

### Step 4: Health Check
5. To check the health of instances, purposely terminate one instance.
6. Go to the target group and check the registered instances.
   - Verify that one instance is marked as unused, demonstrating the load balancer's ability to handle instance failures.

## Conclusion
These steps successfully implement load balancing with an Elastic Load Balancer, providing high availability, even distribution of traffic, and efficient handling of instance failures. The project ensures optimal performance and reliability in a multi-instance environment.

Feel free to explore and modify the configuration based on specific project requirements.
