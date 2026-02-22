# EC2 Auto Scaling & Load Balancing - Implementation Guide

## Architecture Overview
This setup creates a highly available application architecture with:
- Auto Scaling Group spanning multiple Availability Zones
- Application Load Balancer to distribute traffic
- Dynamic scaling policies based on CPU utilization

## Prerequisites
- AWS CLI installed and configured
- Basic understanding of EC2, VPC, and networking
- An AMI with your application pre-installed (or use Amazon Linux 2)

## Step-by-Step Implementation

### Step 1: Create Launch Template
1. Open EC2 Console → Launch Templates → Create launch template
2. **Name:** `web-app-template`
3. **AMI:** Amazon Linux 2 (or your custom AMI)
4. **Instance type:** t2.micro (for testing)
5. **Key pair:** Select or create
6. **Security group:** Allow HTTP (80) and SSH (22)
7. **User data (optional):**
```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from EC2 $(hostname -f)</h1>" > /var/www/html/index.html
