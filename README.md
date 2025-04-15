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

