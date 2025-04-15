# Three-Tier Web Architecture on AWS
<table>
  <tr>
    <td align="center" style="background-color:#f0f8ff; padding:10px;">
      <img src="https://github.com/arumullayaswanth/Three-Tier-AWS-Architecture-project/blob/ba799da6d68cf3817b9d4108e9511f826f812f32/pictures/Building%20a%203-Tier%20Architecture%20on%20AWS.png" width="90%">
      <br><b style="color:#1f75fe;">🔵 Architecture</b>
    </td>
  </tr>
</table>

## 🧾 Execution Plan: Three-Tier Web Architecture on AWS

### ✅ Objective:
To build and deploy a scalable, secure, and highly available Three-Tier Web Application Architecture on AWS using best practices, automation, and monitoring tools.

---

### 📋 Prerequisites
- 📌 AWS Account
- 📌 Basic knowledge of Linux

---

### 🧱 Core AWS Services by Tier

#### 🌐 1. Web Tier (Frontend Layer)
**Purpose:** Serve static files, handle user requests, and route traffic to the Application Tier.

| Service                         | Purpose                                       |
|---------------------------------|-----------------------------------------------|
| Amazon EC2                      | Host Nginx web servers                        |
| Application Load Balancer (ALB)| Internet-facing ALB for traffic distribution  |
| Amazon Route 53                 | Domain management and DNS routing             |
| Amazon ACM                     | Provision and manage HTTPS certificates       |
| Auto Scaling Group              | Ensure availability and scalability           |
| Security Groups                 | Control inbound access (HTTP/HTTPS/SSH)       |
| Amazon AMI                      | Golden image for web server setup            |

#### 🧠 2. Application Tier (Backend Logic)
**Purpose:** Execute application logic (Node.js), process API requests, and communicate with DB Tier.

| Service                         | Purpose                                       |
|---------------------------------|-----------------------------------------------|
| Amazon EC2                      | Run Node.js app server                        |
| Auto Scaling Group              | Maintain desired number of app servers        |
| Internal ALB                    | Load balance traffic between app servers      |
| Amazon S3                       | Store zipped Node.js application package      |
| Amazon IAM Role                 | Allow EC2 to access S3 and RDS securely       |
| Amazon AMI                      | Golden image for app server setup             |
| Security Groups                 | Restrict access only from Web Tier            |
| PM2                             | Process manager to run app persistently       |

#### 🛢️ 3. Database Tier
**Purpose:** Store and manage application data securely.

| Service               | Purpose                                        |
|------------------------|------------------------------------------------|
| Amazon RDS (MySQL)     | Managed relational database                   |
| Multi-AZ Deployment    | High availability & failover support          |
| DB Subnet Group        | Isolate RDS within private subnets            |
| Security Groups        | Allow traffic only from App Tier (port 3306)  |

#### 🛠️ Cross-Tier & Infrastructure Services

| Service                | Purpose                                                  |
|------------------------|----------------------------------------------------------|
| Amazon VPC             | Isolated network for the entire architecture             |
| Subnets (Public/Private)| Separate workloads logically and securely              |
| Internet Gateway       | Enable internet access for web tier                      |
| NAT Gateway            | Allow private instances (app/db) to access the internet  |
| Elastic IP             | Static IP for NAT Gateway                                |
| Route Tables           | Control traffic routing between subnets                  |
| Amazon CloudWatch (optional) | Monitor instance performance, scaling policies |

---

## 🏗️ Architecture of the Project


<table>
  <tr>
    <td align="center" style="background-color:#f0f8ff; padding:10px;">
      <img src="https://github.com/arumullayaswanth/Three-Tier-AWS-Architecture-project/blob/ba799da6d68cf3817b9d4108e9511f826f812f32/pictures/Design%20Diagram%20AWS%20Three%20Tier%20Web%20Architecture.png" width="90%">
      <br><b style="color:#1f75fe;">🔵 Architecture</b>
    </td>
  </tr>
</table>

<table>
  <tr>
    <td align="center" style="background-color:#f0f8ff; padding:10px;">
      <img src="https://github.com/arumullayaswanth/Three-Tier-AWS-Architecture-project/blob/ba799da6d68cf3817b9d4108e9511f826f812f32/pictures/Design%20Diagram%2C%203-tier%20Architecture%20Using%20AWS%20Console.webp" width="90%">
      <br><b style="color:#1f75fe;">🔵 Architecture</b>
    </td>
  </tr>
</table>






### Overview
Three main layers:
1. **Web Tier**: Handles client requests and serves the front-end website.
2. **Application Tier**: Processes API requests and handles the business logic.
3. **Database Tier**: Manages data storage and retrieval.

---

### Components Explanation

#### 1. External Load Balancer
- **Role**: Entry point for all client traffic.
- **Functionality**:
  - Distributes requests to web tier EC2s.
  - Performs health checks.

#### 2. Web Tier
- **Role**: Serves front-end and routes API calls.
- **Components**: Nginx on EC2, React.js front-end.
- **Functionality**:
  - Serve static files.
  - Redirect API to internal ALB.

#### 3. Internal Load Balancer
- **Role**: Manages traffic between Web and App Tier.
- **Functionality**:
  - Routes API calls to App EC2s.
  - Ensures high availability.

#### 4. Application Tier
- **Role**: Handles business logic.
- **Components**: Node.js on EC2.
- **Functionality**:
  - Process requests.
  - Interact with DB.
  - Return data to Web Tier.

#### 5. Database Tier
- **Role**: Reliable data storage.
- **Functionality**:
  - Structured data storage.
  - Multi-AZ high availability.
  - SQL queries and transactions.

---

### Additional Components

#### Load Balancing
- Ensures even traffic distribution.
- Implemented at both Web and App tiers.

#### Health Checks
- External ALB checks Web EC2s.
- Internal ALB checks App EC2s.

#### Auto Scaling Groups
- Web Tier and App Tier auto-scale based on metrics.

#### AWS Certificate Manager (ACM)
- SSL/TLS for `learnaws.co.in`.
- Auto-renew and bind with external ALB.

#### Amazon Route 53
- DNS management for `learnaws.co.in`.
- Routing and health-check-based failover.

---

### ✅ Summary
This architecture ensures **high availability**, **scalability**, and **reliability** by:
- Load balancing traffic across tiers.
- Monitoring instance health.
- Auto-scaling based on usage metrics.
- Isolated networking via VPC and Subnets.





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

<table>
  <tr>
    <td align="center" style="background-color:#f0f8ff; padding:10px;">
      <img src="https://github.com/arumullayaswanth/Three-Tier-AWS-Architecture-project/blob/ba799da6d68cf3817b9d4108e9511f826f812f32/pictures/Screenshot%202025-04-14%20193604.png" width="90%">
      <br><b style="color:#1f75fe;">🔵 VPC</b>
    </td>
  </tr>
</table>
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




# Phase 7: Web Server Setup and Configuration

## 🔹 Step 1: Create Target Group for Web Server
**Path:** EC2 → Target Groups → Create target group

1. Choose a target type: **Instances**
2. Target group name: **web-Target-group**
3. Protocol & Port: **HTTP:80**
4. VPC: **3tire-project-vpc**
5. Click **Next**
6. Register Targets: Select instance: **web-tier-instance (in us-east-1a)**
7. Click **Create target group**

---

## 🔹 Step 2: Create Application Load Balancer (ALB)
**Path:** EC2 → Load Balancers → Create Load Balancer

1. Load Balancer Name: **external-web-alb**
2. Scheme: **Internet-facing**
3. IP Address Type: **IPv4**
4. Network Mapping:  
   - VPC: **3tire-project-vpc**  
   - Availability Zones:  
     - **us-east-1a** → subnet-0b5732bacdd9d400c (web1)  
     - **us-east-1b** → subnet-052550855814614813 (web2)
5. Security Groups: **Wen-ALB-Security-group**
6. Listeners and Routing:  
   - Protocol: **HTTP**  
   - Port: **80**  
   - Default Action: **Forward to web-Target-group**
7. Click **Create Load Balancer**

---

## 🔹 Step 3: Configure Route 53 Hosted Zone
**Path:** Route 53 → Hosted Zones → Create hosted zone

1. Domain name: **aluru.site**
2. Type: **Public hosted zone**
3. Click **Create hosted zone**

---

## 🔹 Step 4: Create A Record in Route 53
**Path:** Route 53 → Hosted zones → aluru.site → Create record

1. Record name: *(Leave blank or add www)*
2. Record type: **A - IPv4 address**
3. Routing policy: **Simple**
4. Alias: **Yes**
5. Alias target: **Choose Application and Classic Load Balancer**
6. Region: **US East (N. Virginia)**
7. Alias target value: `dualstack.external-web-alb-xxxx.us-east-1.elb.amazonaws.com`
8. Alias hosted zone ID: `Z35SXDOTRQ7X7K`
9. Click **Create record**

---

## 🔹 Step 5: Update Domain Nameservers in Hostinger
**Path:** https://hpanel.hostinger.com/domain → Domains → aluru.site → Manage → DNS / Nameservers

1. Click **Edit Nameservers**
2. Paste the 4 NS records from Route 53:  
   - `ns-865.awsdns-84.net`  
   - `ns-1995.awsdns-97.co.uk`  
   - `ns-1418.awsdns-59.org`  
   - `ns-265.awsdns-73.com`
3. Click **Save**

---

## 🔹 Step 6: Request HTTPS Certificate using ACM
**Path:** AWS Certificate Manager → Request Certificate

1. Select: **Request a public certificate**
2. Click **Next**
3. Fully qualified domain name: **aluru.site**
4. Validation method: **DNS validation (recommended)**
5. Click **Request**

---

## 🔹 Step 7: Validate Domain in Route 53
**Path:** AWS Certificate Manager → Certificates → Your Certificate ID

1. Under domain, click **Create DNS record in Amazon Route 53**
2. Select your hosted zone: **aluru.site**
3. Click **Create record**
4. Wait a few minutes for validation to complete

---

## 🔹 Step 8: Add HTTPS Listener to ALB
**Path:** EC2 → Load Balancers → external-web-alb → Listeners → Add listener

1. Protocol: **HTTPS**
2. Port: **443**
3. Default action: **Forward to web-Target-group**
4. Security policy: **ELBSecurityPolicy-2021-06** (or latest)
5. Certificate from ACM: Select the one for **aluru.site**
6. Click **Add**

---

# Phase 8: App Server Setup (Auto Scaling)

## ✅ Step 1: Create AMI from App Tier Instance

1. Go to EC2 → Instances
2. Select the App-tier EC2 instance
3. Click **Actions** → **Image and templates** → **Create image**
4. Image name: **App-tier-gold-AMI**
5. Click **Create image**

---

## ✅ Step 2: Create Launch Template for App Tier

1. Go to EC2 → Launch Templates → Create launch template
2. Launch template name: **App-Launch-template**
3. Template version description: **App-Launch-template(v1)**
4. ✅ Check "Provide guidance to help me set up a template for EC2 Auto Scaling"
5. AMI: **App-tier-gold-AMI**
6. Instance type: **t2.micro**
7. Key pair: **my-Key-pair**
8. Security group: **APP-Security-group (sg-00d21f6e0ae74e57f)**
9. IAM instance profile: **3-tire-project-role**
10. Click **Create launch template**

---

## ✅ Step 3: Create Auto Scaling Group for App Tier

1. Go to EC2 → Auto Scaling Groups → Create Auto Scaling group
2. Name: **app-tier-Auto-Scaling-group**
3. Launch template: **App-Launch-template** --> Click Next
4. VPC: **3-tire-project-vpc**
5. Subnets:  
   - **us-east-1a** → subnet-APP1-us-east-1a  
   - **us-east-1b** → subnet-APP2-us-east-1b
6.	Click Next
7. Load balancing:  
   - ✅ Attach to an existing load balancer  
   - ALB: **app-int-alb | HTTP**  
   - Target group: **app-Target-group**
8. Click Next
9. Group size and scaling:  
   - Desired capacity: **2**  
   - Min: **2**  
   - Max: **2**
10. Tags:  
   - Key: **Name**  
   - Value: **APP-tier-instance**
11. Click **Create Auto Scaling group**



# Phase 9: WEB SERVER SETUP (Auto Scaling)

---

✅ **Step 1: Create AMI from Web Tier Instance**  
1. Go to EC2 → Instances  
2. Select the Web-tier EC2 instance  
3. Click **Actions → Image and templates → Create image**  
4. Image name: `web-tier-gold-AMI`  
5. Description: `web-tier-resources` (optional)  
6. Click **Create image**  

---

✅ **Step 2: Create Launch Template for Web Tier**  
1. Go to EC2 → Launch Templates → Create launch template  
2. Launch template name: `web-Launch-template`  
3. Template version description: `WEB-Launch-template(v1)`  
4. Check ✅ "Provide guidance to help me set up a template for EC2 Auto Scaling"  
5. AMI: Select `web-tier-gold-AMI`  
6. Instance type: `t2.micro`  
7. Key pair: `my-Key-pair`  
8. Security group: Select `web-server-Security-group`  
9. Expand Advanced details  
   - IAM instance profile: `3-tire-project-role`  
10. Click **Create launch template**  

---

✅ **Step 3: Create Auto Scaling Group for Web Tier**  
1. Go to EC2 → Auto Scaling Groups → Create Auto Scaling group  
2. Auto Scaling group name: `web-tier-Auto-Scaling`  
3. Launch template: Select `web-Launch-template`  
4. Click **Next**  
5. Network: Select `3-tire-project-vpc`  
6. Subnets:  
   - `us-east-1a` → `subnet-web1-us-east-1a`  
   - `us-east-1b` → `subnet-web2-us-east-1b`  
7. Click **Next**  
8. Load balancing:  
   - ✅ Attach to an existing load balancer  
   - Choose: `external-web-alb | HTTP`  
   - Target group: `web-Target-group`  
9. Click **Next**  
10. Group size and scaling:  
   - Desired capacity: 2  
   - Min: 2  
   - Max: 2  
11. Click **Next**  
12. Tags:  
   - Key: `Name`  
   - Value: `web-tier-instance`  
13. Click **Next → Review → Create Auto Scaling group**  

# Phase 10: Delete Entire 3-Tier AWS Project – Step-by-Step

---

**Step 1: Delete Auto Scaling Groups**  
1. Go to EC2 → Auto Scaling Groups  
2. Select:  
   - `web-tier-Auto-Scaling`  
   - `app-tier-Auto-Scaling-group`  
3. Click **Actions → Delete**  

---

**Step 2: Delete Load Balancers**  
1. Go to EC2 → Load Balancers  
2. Select:  
   - `app-int-alb`  
   - `external-web-alb`  
3. Click **Actions → Delete**  

---

**Step 3: Delete Target Groups**  
1. Go to EC2 → Target Groups  
2. Select:  
   - `app-int-alb`  
   - `web-Target-group`  
3. Click **Actions → Delete**  

---

**Step 4: Deregister AMIs**  
1. Go to EC2 → AMIs  
2. Select:  
   - `App-tire-gold-AMI`  
   - `web-tire-gold-AMI`  
3. Click **Actions → Deregister**  

---

**Step 5: Delete Snapshots**  
1. Go to EC2 → Snapshots  
2. Select the 2 associated snapshots  
3. Click **Actions → Delete**  

---

**Step 6: Delete RDS/Aurora Database**  
1. Go to RDS → Databases  
2. Select: `my3tireproject`  
3. Click **Actions → Delete**  
4. Confirm:  
   - ✅ Check the acknowledgment  
   - Type `delete me`  
   - Click **Delete**  

---

**Step 7: Delete S3 Bucket**  
1. Go to S3  
2. Select the relevant bucket (e.g., `app-deployment-bucket`)  
3. Click **Empty → then Delete Bucket**  

---

**Step 8: Delete SSL Certificate**  
1. Go to AWS Certificate Manager  
2. Go to Certificates  
3. Select your SSL cert (e.g., for `aluru.site`)  
4. Click **Delete**  

---

**Step 9: Delete Route 53 Records & Hosted Zone**  
1. Go to Route 53 → Hosted Zones  
2. Select `aluru.site`  
3. Delete the record:  
   - `A` record pointing to `external-web-alb`  
4. Then, delete the hosted zone: `aluru.site`  

---

**Step 10: Terminate EC2 Instances & Delete Security Groups**  

**A. Terminate Instances**  
1. Go to EC2 → Instances  
2. Select:  
   - `App-tier-instance`  
   - `web-tier-instance(1a)`  
3. Click **Instance State → Terminate**  

**B. Delete Security Groups**  
1. Go to EC2 → Security Groups  
2. Select and delete:  
   - `APP-Security-group`  
   - `web-server-Security-group`  
   - `RDS-security-group`  
   - `int-alb-Security-group`  
   - `web-ALB-Security-group`  

---

**Step 11: Delete NAT Gateway, Elastic IP, and VPC**  

**A. Delete NAT Gateway**  
1. Go to VPC → NAT Gateways  
2. Select: `3tire-project-nat-public1-us-east-1a`  
3. Click **Actions → Delete**  

**B. Release Elastic IP**  
1. Go to EC2 → Elastic IPs  
2. Select: `3tire-project-eip-us-east-1a`  
3. Click **Actions → Release Elastic IP**  

**C. Delete VPC**  
1. Go to VPC → Your VPCs  
2. Select: `3tire-project-vpc`  
3. Click **Actions → Delete**


  



