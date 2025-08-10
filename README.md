# PROJECT ON AWS NETWORK AND IT SECURITY

📜 Overview
This project builds a secure AWS two-tier network with a public-facing web server and a private backend database, following AWS best practices for networking and security.

🛠 Step-by-Step Setup
(Step-by-Step AWS Networking & Security Project
1. Define the Project Scope
Goal: Build a two-tier network in AWS with:

Public subnet → Internet-facing web server

Private subnet → Database server

Controlled access using Security Groups (SGs) and Network ACLs (NACLs)

Security: Follow the least privilege principle.

2. Set Up a VPC (Virtual Private Cloud)
Go to VPC Console → Create VPC.

Name: MySecureVPC.

IPv4 CIDR block: 10.0.0.0/16 (big enough for future growth).

Enable DNS hostnames and DNS support (important for name resolution).

3. Create Subnets
Public Subnet (for frontend/web servers)

CIDR: 10.0.1.0/24

Map Public IP: Yes

Private Subnet (for backend/database)

CIDR: 10.0.2.0/24

Map Public IP: No

4. Configure Internet Gateway (IGW)
Create an Internet Gateway.

Attach it to MySecureVPC.

Update Public Subnet Route Table:

Route: 0.0.0.0/0 → IGW.

5. Create a NAT Gateway (for Private Subnet Internet Access)
Create an Elastic IP.

Create NAT Gateway in Public Subnet.

Update Private Subnet Route Table:

Route: 0.0.0.0/0 → NAT Gateway.

6. Set Up Security Groups (SG)
Web SG (Frontend)

Inbound:

HTTP (80) from 0.0.0.0/0

HTTPS (443) from 0.0.0.0/0

SSH (22) from your own IP only

Outbound: Allow all (default)

DB SG (Backend)

Inbound:

MySQL (3306) from Web SG only

Outbound: Allow all (default)

7. Configure Network ACLs (NACLs)
Public Subnet NACL

Allow inbound HTTP(80), HTTPS(443), and SSH(22) from trusted IPs

Allow outbound ephemeral ports (1024–65535)

Private Subnet NACL

Allow inbound DB port (3306) from public subnet CIDR

Block all other inbound traffic

8. Launch EC2 Instances
Frontend EC2 in Public Subnet:

Amazon Linux 2 AMI

Attach Web SG

Assign public IP

Backend EC2 (DB) in Private Subnet:

Amazon Linux 2 AMI

Attach DB SG

No public IP

9. Install and Test Services
On Web Server:

bash
Copy
Edit
sudo yum install -y httpd
sudo systemctl enable httpd
sudo systemctl start httpd
echo "Hello Secure AWS" | sudo tee /var/www/html/index.html
On DB Server:

Install MySQL/MariaDB

Configure to only accept connections from Web SG

10. Enable Logging & Monitoring
VPC Flow Logs:

Create and send to CloudWatch Logs to monitor all traffic.

CloudTrail:

Turn on to track AWS API calls.

CloudWatch Alarms:

Set alarms for CPU usage, network spikes, or unusual behavior.

11. Add Advanced Security
AWS WAF (Web Application Firewall) to protect HTTP/HTTPS traffic.

AWS Shield for DDoS protection.

Secrets Manager for storing DB credentials securely.

IAM Roles for EC2 instead of embedding credentials.

12. Test Security
Try connecting to DB directly from the internet (should fail).

Try SSH from unapproved IP (should fail).

Check VPC Flow Logs for unexpected traffic.

13. Clean Up (Optional)
To avoid charges, terminate EC2s, delete NAT Gateway, IGW, and VPC.)

📊 Network Architecture Diagram


Here’s your AWS networking and security project diagram — it shows the VPC, public/private subnets, Internet Gateway, NAT Gateway, EC2 web server, and DB server with their security rules and traffic flow

[open Google](file:///home/agatha/Downloads/aws_networking_security_diagram.png]

<img width="2385" height="1516" alt="aws_networking_security_diagram" src="https://github.com/user-attachments/assets/5134486a-9313-448b-861e-7fcae4d80ce3" />


🔐 Security Summary
Security Groups: Restrict access based on principle of least privilege.

Network ACLs: Add an extra layer of subnet-level security.

IAM Roles: No hardcoded credentials.

VPC Flow Logs & CloudTrail: Monitor and log network activity.
