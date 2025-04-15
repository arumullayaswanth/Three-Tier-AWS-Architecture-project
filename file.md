
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
