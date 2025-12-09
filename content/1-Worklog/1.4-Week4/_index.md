---
title: "Week 4 Worklog: Managed Databases & Storage"
date: 2025
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report.
{{% /notice %}}



### Week 4 Objectives:

* **Master Managed Services:** Understand the operational benefits of using **Amazon RDS** versus installing a database manually on EC2.
* **Implement a 2-Tier Architecture:** Connect a web server (EC2) securely to a database instance (RDS) within the VPC.
* **Host Static Content:** Deploy a serverless frontend using **Amazon S3 Static Website Hosting**.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Study Database Fundamentals on Cloud:** <br>&emsp; + Managed vs. Unmanaged (Why use RDS?). <br>&emsp; + **DB Subnet Groups:** Placing databases in Private Subnets. <br>&emsp; + **Security:** Encryption at rest and Security Group referencing. | 08/25/2025 | 08/25/2025 | [FCJ: Create Database on RDS](https://cloudjourney.awsstudygroup.com/vi/1-explore/8-rds/) |
| 3 | - **Practice: Launch RDS MySQL:** <br>&emsp; + Create a DB Subnet Group covering two private subnets. <br>&emsp; + Provision an RDS MySQL instance (Free Tier). <br>&emsp; + *Critical:* Configure **Master User** credentials securely. | 08/26/2025 | 08/26/2025 | [Creating an Amazon RDS DB instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html) |
| 4 | - **Practice: Security Group Chaining:** <br>&emsp; + Create a `DB-SG` Security Group. <br>&emsp; + **DevOps Task:** Allow Inbound on Port 3306 *only* from the `WebServer-SG` ID (Source: `sg-xxxxx`), NOT from `0.0.0.0/0`. | 08/27/2025 | 08/27/2025 | [Controlling Access with Security Groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.RDSSecurityGroups.html) |
| 5 | - **Practice: Connect App to DB:** <br>&emsp; + SSH into the EC2 instance (from Week 3). <br>&emsp; + Install `mysql-client`. <br>&emsp; + Verify connectivity: `mysql -h <RDS-Endpoint> -u admin -p`. | 08/28/2025 | 08/28/2025 | [Connecting to an RDS instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ConnectToInstance.html) |
| 6 | - **Static Website Project:** <br>&emsp; + Create an S3 Bucket with public read access (Block Public Access settings). <br>&emsp; + Enable "Static Website Hosting". <br>&emsp; + Upload `index.html` and `error.html`. | 08/29/2025 | 08/29/2025 | [FCJ: Hosting Static Website with S3](https://cloudjourney.awsstudygroup.com/vi/1-explore/7-static-web/) |


### Week 4 Achievements:

* **Deployed a Managed Database (RDS):**
    * Successfully provisioned a **MySQL RDS instance** within the VPC.
    * Understood the "Shared Responsibility Model": AWS manages the OS patching and backups, while I manage the data and schema.

* **Implemented Secure Network Segmentation (2-Tier Arch):**
    * Placed the Database in **Private Subnets**, ensuring it is not directly accessible from the internet.
    * Used **Security Group Chaining (Referencing):** Configured the Database Security Group to strictly allow traffic only from the Web Server's Security Group ID. This is a critical DevSecOps pattern that avoids hardcoding IP addresses.

* **Verified Application-Tier Connectivity:**
    * Installed the MySQL client on the EC2 Jumpbox/Web Server.
    * Successfully established a connection to the RDS Endpoint, validating the route tables and firewall rules.

* **Deployed Serverless Frontend (S3):**
    * Configured an **Amazon S3 bucket** for static website hosting.
    * managed Bucket Policies to allow public read access specifically for web assets, distinguishing between "Private Storage" and "Public Hosting" use cases.