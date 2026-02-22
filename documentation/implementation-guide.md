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
Step 2: Create Target Group
EC2 Console → Target Groups → Create target group

Target type: Instances

Name: web-app-tg

Protocol: HTTP : 80

VPC: Select your default VPC

Health checks: Path: /

Step 3: Create Application Load Balancer
EC2 Console → Load Balancers → Create Load Balancer

Type: Application Load Balancer

Name: web-app-alb

Scheme: Internet-facing

Listeners: HTTP : 80

Availability Zones: Select at least 2 AZs

Security group: Allow HTTP (80)

Target group: Select web-app-tg

Step 4: Create Auto Scaling Group
EC2 Console → Auto Scaling Groups → Create Auto Scaling group

Name: web-app-asg

Launch template: Select web-app-template

VPC: Select same VPC as load balancer

Availability Zones: Select same AZs as load balancer

Load balancing: Attach to existing load balancer → Select web-app-alb

Health checks: Enable ELB health checks

Group size: Desired: 2, Minimum: 1, Maximum: 4

Step 5: Configure Dynamic Scaling Policy
In Auto Scaling group → Automatic scaling → Add policy

Policy type: Target tracking scaling

Name: cpu-target-policy

Metric type: Average CPU utilization

Target value: 50%

Step 6: Test the Setup
Get your Load Balancer DNS name:

bash
aws elbv2 describe-load-balancers --names web-app-alb --query 'LoadBalancers[0].DNSName'
Open in browser - you should see the web page

Test scaling:

Generate load using Apache Bench or similar

Watch Auto Scaling group launch new instances

Terminate an instance and see automatic replacement

Verification Commands
bash
# Describe Auto Scaling group
aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names web-app-asg

# List instances in target group
aws elbv2 describe-target-health --target-group-arn YOUR_TG_ARN

# Check CloudWatch metrics
aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --start-time 2023-01-01T00:00:00Z --end-time 2023-01-01T01:00:00Z --period 300 --statistics Average --dimensions Name=AutoScalingGroupName,Value=web-app-asg
Troubleshooting
Issue	Solution
Instances not registering with ALB	Check security groups allow traffic from ALB
Scaling not happening	Verify CloudWatch alarms are created
Health checks failing	Check application is responding on port 80
Instances stuck in termination	Check for scale-in protection settings
Clean Up
bash
# Delete Auto Scaling group
aws autoscaling delete-auto-scaling-group --auto-scaling-group-name web-app-asg --force-delete

# Delete load balancer
aws elbv2 delete-load-balancer --load-balancer-arn YOUR_ALB_ARN

# Delete target group
aws elbv2 delete-target-group --target-group-arn YOUR_TG_ARN

# Delete launch template
aws ec2 delete-launch-template --launch-template-name web-app-template
