# aws-terraform-infra
Terraform script to create a highly scalable and reliable AWS infrastructure

1. Networking (VPC, Subnets, Routing)
VPC: Creates a virtual private cloud (VPC) (aws_vpc.main) with CIDR block 10.0.0.0/16.
Public Subnets: Two subnets (aws_subnet.public_subnet_1 and aws_subnet.public_subnet_2) in different availability zones with public IP assignment.
Private Subnets: Two subnets (aws_subnet.private_subnet_1 and aws_subnet.private_subnet_2) for internal services like RDS.
Internet Gateway: Enables internet access for instances in the public subnets.
Route Table: Configures public subnets to route traffic through the internet gateway.
2. Security
Security Group (aws_security_group.web_sg):
Allows inbound HTTP (port 80) from anywhere (0.0.0.0/0).
Allows all outbound traffic.
3. Load Balancer
Application Load Balancer (aws_lb.app_lb):
Distributes traffic across multiple EC2 instances.
Uses a target group (aws_lb_target_group.app_tg) to manage backend instances.
A listener (aws_lb_listener.http) forwards HTTP traffic to instances.
4. Auto Scaling
Launch Template (aws_launch_template.app_lt):
Defines the AMI (ami-12345678), instance type (t3.micro), and user data script (installs Apache HTTP server).
Auto Scaling Group (aws_autoscaling_group.app_asg):
Deploys EC2 instances across public subnets.
Scales between 2 and 5 instances.
5. Database (Amazon RDS - Aurora MySQL)
Database Cluster (aws_rds_cluster.db_cluster):
Uses aurora-mysql as the database engine.
Stores credentials (admin/SuperSecurePass123 - this should be securely managed via AWS Secrets Manager).
DB Subnet Group (aws_db_subnet_group.app_db_subnet_group):
Ensures the database is hosted in private subnets.
6. Output
Displays the Load Balancer DNS name, which can be used to access the web application.
