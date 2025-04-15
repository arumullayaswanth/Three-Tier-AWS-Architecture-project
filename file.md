
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




# ✅ Phase 4: Update App-Tier in S3 with DB Config

---

## 🔹 Step 1: Get Your RDS Endpoint
1. Go to AWS Console → RDS → Databases  
2. Click on `my3tireproject`  
3. Scroll down to **Connectivity & Security**  
4. Copy the Endpoint  
👉 Example:  
`my3tireproject.c0n8k0a0swtz.us-east-1.rds.amazonaws.com`

---

## 🔹 Step 2: Update `DbConfig.js` Locally
1. On your local machine, go to:  
```
C:\Users\Yaswanth Reddy\Videos\AWS Three-Tier Web Architecture\application-code\app-tier
```
2. Open `DbConfig.js` and edit it like this:
```js
module.exports = Object.freeze({
    DB_HOST : 'my3tireproject.c0n8k0a0swtz.us-east-1.rds.amazonaws.com',
    DB_USER : 'admin',
    DB_PWD : 'yaswanth123',
    DB_DATABASE : 'webappdb'
});
```
3. Save the file.

---

## 🔹 Step 3: Upload the Updated File to S3
1. Go to AWS Console  
2. Navigate to **S3**  
3. Click your bucket: `3tireproject523182`  
4. Go to: `application-code → app-tier`  
5. Click **Upload**  
6. Select the updated `DbConfig.js` file from your local path:  
```
C:\Users\Yaswanth Reddy\Videos\AWS Three-Tier Web Architecture\application-code\app-tier
```
7. Click **Upload**  
👉 If prompted to overwrite the file, choose **Yes** or **Replace**



# Phase 5: Create Application Tier Resources Including Internal Load Balancer

## ✅ STEP 1: Launch EC2 Instance for Application Tier
1. Go to **EC2 → Instances → Launch an Instance**
2. Set Name: `App-tier-instance`
3. Application and OS Image (AMI):  
   - Select: `Amazon Linux 2 AMI (HVM) - Kernel 5.10, SSD Volume Type`
4. Instance Type: `t2.micro`
5. Key Pair:  
   - Choose: `my-Key pair`
6. Network Settings → Click Edit  
   - VPC: `3tire-project-vpc`  
   - Subnet: `3tire-project-subnet-APP1-us-east-1a`  
   - Firewall (Security Group): `APP-Security-groupsg-00d21f6e0ae74e57f`
7. Advanced Details:  
   - IAM Role: `3-tire-project-role`
8. Click **Launch Instance**

---

## ✅ STEP 2: Connect to App-tier EC2 via Session Manager
1. Go to **EC2 → Instances**
2. Select `App-tier-instance`
3. Click **Connect → Session Manager → Connect**

---

## ✅ STEP 3: Switch to Root User
```bash
sudo su
cd /home/ec2-user/
```

---

## ✅ STEP 4: Install MySQL Client
```bash
sudo yum install mysql -y
```

---

## ✅ STEP 5: Connect to RDS Database
```bash
mysql -h my3tireproject.c0n8k0a0swtz.us-east-1.rds.amazonaws.com -u admin -p
# Enter your password: yaswanth123
```

---

## ✅ STEP 6: Run SQL Commands in MySQL Shell
```sql
CREATE DATABASE webappdb;
USE webappdb;

CREATE TABLE IF NOT EXISTS transactions (
    id INT NOT NULL AUTO_INCREMENT,
    amount DECIMAL(10,2),
    description VARCHAR(100),
    PRIMARY KEY(id)
);

SHOW TABLES;

INSERT INTO transactions (amount, description) VALUES ('400', 'groceries');

SELECT * FROM transactions;
exit;
```

---

## ✅ STEP 7: Install Node.js and PM2
```bash
curl -o- https://raw.githubusercontent.com/arumullayaswanth/Three-Tier-AWS-Architecture-project/master/install.sh | bash
source ~/.bashrc
nvm install 16
nvm use 16
npm install -g pm2
```

---

## ✅ STEP 8: Download App Code from S3
```bash
cd ~/
aws s3 cp s3://3tireproject523182/application-code/app-tier/ app-tier --recursive
cd ~/app-tier
```

---

## ✅ STEP 9: Start Node.js App with PM2
```bash
npm install
pm2 start index.js
pm2 status
pm2 logs
pm2 startup
pm2 save
```

---

## ✅ STEP 10: Verify App is Running
```bash
curl http://localhost:4000/health
# ✅ Output: This is the health check.
```

---

## ✅ STEP 11: Create Target Group for Internal Load Balancer
1. Go to **EC2 → Target Groups → Create Target Group**
2. Target type: `Instances`
3. Target group name: `app-int-alb`
4. Protocol: `HTTP`
5. Port: `4000`
6. VPC: `3tire-project-vpc`
7. Health Checks:  
   - Path: `/health`
8. Click **Next → Register Targets**  
   - Select your `App-tier-instance`
9. Click **Create Target Group**

---

## ✅ STEP 12: Create Internal Application Load Balancer
1. Go to **EC2 → Load Balancers → Create Load Balancer**
2. Select: `Application Load Balancer`
3. Name: `app-int-lb`
4. Scheme: `Internal`
5. VPC: `3tire-project-vpc`
6. Availability Zones:  
   - `us-east-1a`: `3tire-project-subnet-APP1-us-east-1a`  
   - `us-east-1b`: `3tire-project-subnet-APP2-us-east-1b`
7. Security Group: `int-alb-Security-group`
8. Listener & Routing:  
   - Protocol: `HTTP`  
   - Port: `80`  
   - Forward to Target Group: `app-int-alb`
9. Click **Create Load Balancer**

---

## ✅ STEP 13: Get Internal Load Balancer DNS
1. Go to **EC2 → Load Balancers**
2. Select: `app-int-lb`
3. Under **Details**, copy the DNS name:  
   ```
   internal-app-int-lb-1744284757.us-east-1.elb.amazonaws.com
   ```

---

## ✅ STEP 14: Update nginx.conf File on Your Local Machine
1. Navigate to:  
   `C:\\Users\\Yaswanth Reddy\\Videos\\AWS Three-Tier Web Architecture\\application-code`
2. Open `nginx.conf` in a text editor (Notepad++ or VS Code)
3. Add the following under the API proxy section:
```nginx
#proxy for internal lb
location /api/ {
    proxy_pass http://internal-app-int-lb-1744284757.us-east-1.elb.amazonaws.com:80/;
}
```
4. Save the file.

---

## ✅ STEP 15: Upload Updated Files to Amazon S3
1. Go to **AWS Console → S3 → Buckets**
2. Select your bucket: `3tireproject523182`
3. Navigate to: `application-code/`
4. Click **Upload → Add Files**
   - Browse and select:  
     - `nginx.conf`  
     - `DbConfig.js`
5. Click **Upload**
   - Confirm overwrite if prompted.
  


 
# Phase 6: Web Tier Setup – Launch EC2 Instance in Web Subnets (Custom VPC)

## ✅ STEP 1: Launch EC2 Instance in Web Subnet
1. Go to **EC2 → Instances → Launch Instance**
2. **Name**: `web-tier-instance(1a)`
3. **Application and OS Images (AMI)**:  
   Select → `Amazon Linux 2 AMI (HVM) - Kernel 5.10, SSD Volume Type`
4. **Instance type**: `t2.micro`
5. **Key pair (login)**: Select → `my-Key pair`
6. **Network settings**:
   - **VPC**: `vpc-0ca0b401d854f068b (3tire-project-vpc)`
   - **Subnet**: `subnet-0b5732bacdd9d400c3 (web1-us-east-1a)`
   - **Auto-assign Public IP**: `Enable`
   - **Firewall (Security Groups)**: Select → `web-server-Security-groupsg-0291f2a05e8571591`
7. **Advanced details**:
   - **IAM Role**: Select → `3-tire-project-role`
8. Click **Launch Instance**

---

## ✅ STEP 2: Connect to Web Tier Instance
1. Go to **EC2 → Instances**
2. Select `web-tier-instance(1a)`
3. Click **Connect → Session Manager → Connect**

```bash
sudo su ec2-user
cd /home/ec2-user/
pwd
```

---

## ✅ STEP 3: Install Node.js & Nginx

```bash
curl -o- https://raw.githubusercontent.com/arumullayaswanth/Three-Tier-AWS-Architecture-project/master/install.sh | bash
source ~/.bashrc
nvm install 16
nvm use 16
```

---

## ✅ STEP 4: Download Web Tier Code from S3

```bash
cd ~/
aws s3 cp s3://3tireproject523182/application-code/web-tier/ web-tier --recursive
cd ~/web-tier
npm install
npm run build
```

---

## ✅ STEP 5: Install and Configure NGINX

```bash
sudo amazon-linux-extras install nginx1 -y
cd /etc/nginx
sudo rm nginx.conf
sudo aws s3 cp s3://3tireproject523182/application-code/nginx.conf nginx.conf
sudo service nginx restart
```

---

## ✅ STEP 6: Permissions and Auto Start

```bash
chmod -R 755 /home/ec2-user
sudo chkconfig nginx on
```

---

## ✅ Additional Configuration:
- Generate an **ACM certificate** for your domain name.
- Add an **A Record** and **CNAME Record** in **Route 53** to map your domain to the Application Load Balancer.
- Once completed, you can access the application securely with the `https` protocol.
  



