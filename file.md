
# Phase 1: Create VPC for your Project

## 🛠️ VPC Creation – Step-by-Step (for 3-Tier Architecture)

---

## 🚀 Step-by-Step Guide

### 1. Go to VPC Dashboard
- Navigate to: **VPC → Your VPCs**

### 2. Click on "Create VPC"
- Choose: **VPC and more**

### 3. Configure VPC Settings
- **Name tag auto-generation**: ✅ Select "Auto-generate"  
  - **Name**: `3tier-project`
- **IPv4 CIDR block**: `192.168.0.0/22`  
  _(Gives you 1024 IPs – good for a test environment)_
- **IPv6 CIDR block**: ❌ Select "No IPv6 CIDR block"

### 4. Availability Zones & Subnets
- **Number of Availability Zones (AZs)**: `2`
- **Number of Public Subnets**: `2`
- **Number of Private Subnets**: `4`  
  _(2 per AZ: 1 for App Tier, 1 for DB Tier)_

### 5. NAT Gateway
- Select: ✅ **"In 1 AZ"**  
  _AWS will create a NAT Gateway in one public subnet to allow private subnets to access the internet (for software updates, etc.)_

### 6. VPC Endpoints
- ❌ Select: **"None"**

### 7. Click **"Create VPC"**

---

## 🔸 Step 2: Rename Subnets in the VPC

Navigate to: **VPC Dashboard → Subnets**, and follow the renaming process:

---

### ✅ Rename Private Subnets (Application Tier)
1. Select: `3tire-project-subnet-private1-us-east-1a`  
   ➤ Rename to: `3tire-project-subnet-APP1-us-east-1a`  
   ➤ Click **Save**

2. Select: `3tire-project-subnet-private2-us-east-1b`  
   ➤ Rename to: `3tire-project-subnet-APP2-us-east-1b`  
   ➤ Click **Save**

---

### ✅ Rename Private Subnets (Database Tier)
3. Select: `3tire-project-subnet-DB1-us-east-1a`  
   ➤ Rename to: `3tire-project-subnet-DB1-us-east-1a` (✅ already correct)  
   ➤ Click **Save**

4. Select: `3tire-project-subnet-DB2-us-east-1b`  
   ➤ Rename to: `3tire-project-subnet-DB2-us-east-1b` (✅ already correct)  
   ➤ Click **Save**

---

### ✅ Rename Public Subnets (Web Tier)
5. Select: `3tire-project-subnet-web1-us-east-1a`  
   ➤ Rename to: `3tire-project-subnet-web1-us-east-1a` (✅ already correct)  
   ➤ Click **Save**

6. Select: `3tire-project-subnet-web2-us-east-1b`  
   ➤ Rename to: `3tire-project-subnet-web2-us-east-1b` (✅ already correct)  
   ➤ Click **Save**

---

## ✅ Result

Once created, AWS will automatically set up:
- VPC with `192.168.0.0/22` CIDR
- 2 Public subnets + 4 Private subnets across 2 AZs
- Route Tables
- NAT Gateway
- Internet Gateway (attached to public subnets)
- All required components for a functional **Three-Tier Architecture** base.






# ☁️ Phase 2: S3 Bucket & IAM Role

## 🔸 Step 1: Clone Git Repository to Your Local Laptop
1. Open Terminal (Mac/Linux) or Git Bash / Command Prompt (Windows)
2. Run the following commands:
```bash
git clone https://github.com/avizway1/aws_3tier_architecture.git
cd aws_3tier_architecture
```
3. If the repository contains a ZIP file, unzip it:
```bash
unzip aws_3tier_architecture.zip
```
✅ Now you’ll have the project folder on your local system. You can inspect/edit the code before uploading it to your S3 bucket.

---

## 🔸 Step 2: Create S3 Bucket
1. Navigate to S3 → Click Create bucket
2. Set up the bucket:
   - **Region**: US East (N. Virginia) `us-east-1`
   - **Bucket type**: General purpose
   - **Bucket name**: `3tireproject523182`
   - ✅ Enable versioning
3. Click **Create bucket**
4. Open the new bucket → Click **Upload** → Add project code files

---

## 🔸 Step 3: Create IAM Role for EC2
1. Navigate to IAM Console → Roles → Create role
2. Select **Trusted Entity Type**: AWS service
3. Use Case: **EC2**
4. Attach permissions:
   - `AmazonEC2RoleforSSM` ✅
   - `AmazonS3ReadOnlyAccess` ✅ (optional but recommended)
5. **Role Name**: `3-tire-project-role`
6. Click **Create Role**

---

# 🛡️ Phase 3: Database Tier & Security Group Setup

## 🔹 Step 1: Create Security Group for Database
1. Go to EC2 → Security Groups → Create Security Group
2. **Security Group Name**: `RBS-security-group`
3. **Description**: Database security group
4. **VPC**: Select `3tire-project-vpc`
5. Inbound Rules:
   - **Type**: MySQL/Aurora
   - **Port**: 3306
   - **Source**: Custom → `192.168.0.0/24`
   - **Description**: VPC CIDR
6. Click **Create security group**

---

## 🔹 Step 2: Create Security Group for Web Application Load Balancer SG
1. EC2 → Security Groups → Create Security Group
2. **Name**: `Web-ALB-Security-group`
3. **Description**: Web Application Load Balancer SG
4. **VPC**: `3tire-project-vpc`
5. Inbound Rules:
   - HTTP (80) → Source: Anywhere-IPv4
   - HTTPS (443) → Source: Anywhere-IPv4
6. Click **Create security group**

---

## 🔹 Step 3: Create Security Group for Web Servers
1. EC2 → Security Groups → Create Security Group
2. **Name**: `web-server-Security-group`
3. **Description**: Web server SG
4. **VPC**: `3tire-project-vpc`
5. Inbound Rules:
   - HTTP (80) → Source: `Web-ALB-Security-group`
   - HTTP (80) → Source: `192.168.0.0/22`
6. Click **Create security group**

---

## 🔹 Step 4: Create Security Group for App Servers
1. EC2 → Security Groups → Create Security Group
2. **Name**: `APP-Security-group`
3. **Description**: App server SG
4. **VPC**: `3tire-project-vpc`
5. Inbound Rules:
   - Custom TCP Rule
   - Port: 4000
   - Source: `192.168.0.0/22`
6. Click **Create security group**

---

## 🔹 Step 5: Create Security Group for Internal ALB
1. EC2 → Security Groups → Create Security Group
2. **Name**: `int-alb-Security-group`
3. **Description**: Internal ALB SG
4. **VPC**: `3tire-project-vpc`
5. Inbound Rule:
   - HTTP (80) → Source: `192.168.0.0/22`
6. Click **Create security group**

---

## 🔹 Step 6: Create a DB Subnet Group
1. Go to RDS → Subnet groups → Create DB Subnet Group
2. **Name**: `project-tire-DB-subnet-group`
3. **Description**: `3tireproject-DB-subnet-group`
4. **VPC**: Select `3tire-project-vpc`
5. **Availability Zones**:
   - `us-east-1a`
   - `us-east-1b`
6. **Add Subnets**:
   - `3tire-project-subnet-DB1-us-east-1a`
   - `3tire-project-subnet-DB2-us-east-1b`
7. Click **Create**

---

## 🔹 Step 7: Launch RDS MySQL Database
1. Go to RDS → Databases → Create database
2. **Engine**: Select **MySQL**
3. **Version**: `8.0.35`
4. **Template**: Dev/Test
5. **Availability**: Select **Multi-AZ DB cluster (3 instances)**
6. **DB Cluster Identifier**: `my3tireproject`
7. **Credentials**:
   - Username: `admin`
   - Password: `yaswanth123`
8. **Instance Class**: `db.c6gd.medium` (1 vCPU, 2 GiB RAM)
9. **Storage**: General Purpose SSD (gp2), 20 GiB
10. **Connectivity Settings**:
    - Compute Resource: Don’t connect to EC2
    - Network Type: IPv4
    - VPC: `3tire-project-vpc`
    - Subnet Group: `project-tire-db-subnet-group`
    - Public Access: No
    - Security Group: `RBS-security-group`
11. **Backup**:
    - Enable Automated Backups
12. Click **Create Database**

