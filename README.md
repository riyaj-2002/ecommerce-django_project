# 🚀 Django Project Deployment on AWS EC2 (Ubuntu)

Deploy your Django web application on an AWS EC2 instance running Ubuntu. This guide includes setup with Nginx as the reverse proxy.

---

## ⚙️ Requirements

- AWS EC2 (Ubuntu)
- Python 
- Django 
- Git
- Nginx

---


## 🛠️ AWS Setup: VPC, Subnets, Route Table, Internet Gateway and EC2 Instance

### 1. Create a VPC
# Set the following:
   - **VPC Name:** `vpc-project`
   - **IPv4 CIDR block:** `10.0.0.0/24`
   - Set **Tenancy** as `Default`.
   - Click **Create VPC**.

### 2. Create Subnets
#### Public Subnet:
# Set the following for the public subnet:
   - **VPC**: Select the VPC.
   - **Subnet name**: `pub-sub-project`
   - **CIDR block**: `10.0.0.1/24`
# Click **Create Subnet**.

#### Private Subnet:
# Set the following for the private subnet:
   - **VPC**: Select the VPC.
   - **Subnet name**: `pri-sub-project`
   - **CIDR block**: `10.0.0.128/24`
# Click **Create Subnet**.

### 3. Create an Internet Gateway (IGW)
# Set the following for the private subnet:
  - **IGW Name:** `igw-project`
# Attach the IGW:
   - Select the VPC and click **Attach**.

### 4. Create a Route Table (RT)
# Set the following:
  - **RT Name:** `rt-project`
  - Add a route: `0.0.0.0/0` → Target: `igw-project`.
  - Associate the Route Table with the Public Subnet and Private Subnet

---

## 🛠️ EC2 Instance Setup

### 1. Launch EC2 Instance
# Set the following:
   - **InstanceType:** t2.micro
   - **KeyName:** key.pair.pem
   - **VPC:** vpc-project
   - **SUBNET:** pub-sub-project
   - **SECURITY GROUP:** SSH (port 22) , HTTP (port 80) , HTTPS (port 443)

## 🛠️ Setup Instructions

```bash
# 1. SSH into EC2
ssh -i "key.pair.pem" ubuntu@13.232.66.215

# 2. Install Dependencies
sudo apt update && sudo apt upgrade -y
sudo apt install python3-pip python3-venv nginx git -y

# 3. Clone the Project
git clone https://github.com/riyaj-2002/ecommerce-django_project.git
cd ecommerce-django_project

# 4. Set Up Virtual Environment
python3 -m venv venv
source venv/bin/activate

# 5. Install Dependencies from requirement.txt File
pip install -r requirements.txt

# 6. Configure Django
# In your settings.py, update ALLOWED_HOSTS
# ALLOWED_HOSTS = ['13.232.66.215']
python manage.py migrate

# 7. Run the Server
python manage.py runserver 0.0.0.0:8000
