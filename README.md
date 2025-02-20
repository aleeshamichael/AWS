![Alt txt] (architecture for l.b.png)
# AWS EC2 Apache Web Server and Load Balancer Setup

This guide outlines the steps to set up an Apache web server on an AWS EC2 instance, configure a load balancer, and deploy an HTML application for high availability and scalability.

## Prerequisites

An AWS account

A running EC2 instance (Amazon Linux)

Security Group configured to allow HTTP (port 80) and SSH (port 22)

Default VPC and subnets

## Step 1: Install Apache Web Server

Run the following command to install Apache (httpd) on the EC2 instance:
sh'''
sudo yum install httpd -y
'''
This installs the Apache web server package.

## Step 2: Start and Enable Apache

To start the Apache web server and enable it on boot, use:

**sudo systemctl start httpd
**sudo systemctl enable httpd

Check the status of the service with:

**sudo systemctl status httpd

## Step 3: Navigate to the Default Web Directory

Apache serves files from /var/www/html by default. Navigate to this directory:

**cd /var/www/html

## Step 4: Create and Deploy an HTML Application

To create an HTML file, run:

**sudo vi index.html

Add your HTML content and save the file with :wq!.

To remove the default Apache index file:

**sudo rm -rf index.html

To create a new directory for organizing files:

**sudo mkdir test
**cd test/

## 5: Configure an Application Load Balancer

Prerequisites

At least two subnets within the same Availability Zone (AZ)

The AZ used: ap-southeast-1a

Create a Load Balancer

Navigate to EC2 > Load Balancers

Create an Application Load Balancer (ALB)

Choose HTTP (port 80) as the listener and configure routing to the target group (TG)

Configure Target Group

Assign a health check path (e.g., /index.html).

If the status check shows the instance is unhealthy, verify the correct path.

Use ls to list files and directories:

**ls
**cd /var/www/html
**vi index.html

Add content like Hi and save using :wq!.

Restart Apache:

**sudo systemctl start httpd

Register Additional Servers

Create another EC2 instance and repeat Apache installation:

**sudo yum install httpd -y
**cd /var/www/html
**vi index.html

Add content like Hi from test server, then start the service:

**sudo systemctl start httpd

Register this instance to the Target Group (TG) under the Load Balancer (LB).

## Step 6: Verify Load Balancer Functionality

Access the application using the ALB DNS name

ALB uses a round-robin method to distribute traffic

The same application must be deployed on all instances for consistency

## Step 7: Subnet Configuration

If the ALB is connected to three subnets, it can route traffic to instances in each.

If an instance is missing from subnet 1c, the ALB will not send traffic there but will continue routing to 1a and 1b.

This setup ensures your web application is scalable and accessible through a load balancer, with proper health checks and multi-server redundancy.



