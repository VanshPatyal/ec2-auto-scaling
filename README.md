# EC2 Auto Scaling & Load Balancing Setup

## 📋 Project Overview
This project demonstrates the configuration of an **Auto Scaling group** with dynamic scaling policies and an **Application Load Balancer (ALB)** on AWS. The setup ensures high availability, fault tolerance, and automatic scaling for applications running on EC2 instances.

## 🎯 Business Value
- **High Availability:** Automatically replaces failed instances.
- **Cost Efficiency:** Scales in/out based on actual demand (e.g., CPU utilization).
- **Fault Tolerance:** Distributes traffic across multiple Availability Zones.

## 🛠️ AWS Services Used
- EC2 (Instances, AMIs, Security Groups)
- Auto Scaling Groups
- Application Load Balancer (ALB)
- Target Groups
- CloudWatch (Alarms & Metrics)

## 🚀 Implementation Steps (Overview)
1.  **Launch Template:** Created a template with AMI, instance type, and security group.
2.  **Target Group:** Defined a target group for the load balancer to route traffic.
3.  **Load Balancer:** Set up an Application Load Balancer and a listener.
4.  **Auto Scaling Group:** Created an ASG using the launch template, attached it to the load balancer.
5.  **Scaling Policies:** Configured a dynamic scaling policy based on average CPU utilization.
6.  **Testing:** Simulated load to verify the ASG launches new instances and the ALB distributes traffic.

## 📊 Key Learnings & Results
- Successfully configured an auto-scaling environment that handles traffic spikes seamlessly.
- Demonstrated automatic instance recovery during simulated failure tests.
- Created a robust, production-ready architecture pattern.

## 🔗 Quick Links *(to be added as you create them)*
- [Auto Scaling Group Configuration Details](link-to-config-file)
- [CloudFormation/Terraform Template](link-to-template)
- [Screenshots](./screenshots)

## 📬 Contact
vanshpatyal321@gmail.com
