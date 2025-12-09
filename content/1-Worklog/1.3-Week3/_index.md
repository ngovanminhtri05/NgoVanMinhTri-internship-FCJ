---
title: "Week 3 Worklog: Architecting VPC & Secure Compute"
date: 2025
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report.
{{% /notice %}}



### Week 3 Objectives:

* **Implement Cloud Networking:** Move beyond default settings to design a custom **Amazon VPC** (Virtual Private Cloud).
* **Secure Compute Identity:** Replace long-term credentials (access keys) with temporary credentials using **IAM Roles for EC2**.
* **Deploy Scalable Compute:** Master the lifecycle of **Amazon EC2** instances within a custom network.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Study VPC Fundamentals:** <br>&emsp; + CIDR blocks & Subnetting (Public vs. Private). <br>&emsp; + Internet Gateways (IGW) & Route Tables. <br>&emsp; + **Lab:** "Deploy Network Infrastructure with Amazon VPC". | 08/18/2025 | 08/18/2025 | [FCJ: Deploy Network Infrastructure (VPC)](https://000048.awsstudygroup.com/) |
| 3 | - **Study EC2 & IAM Roles:** <br>&emsp; + Understanding the separation of Compute (EC2) and Identity (IAM). <br>&emsp; + **Lab:** "Grant permissions to applications via IAM Role". <br>&emsp; + *Goal:* Allow EC2 to access S3 without hardcoded AWS keys. | 08/19/2025 | 08/19/2025 | [FCJ: IAM Roles for EC2](https://000048.awsstudygroup.com/) |
| 4 | - **Practice: Custom VPC Build:** <br>&emsp; + Create a VPC `10.0.0.0/16`. <br>&emsp; + Create Public Subnet `10.0.1.0/24`. <br>&emsp; + Configure Route Table to point `0.0.0.0/0` to IGW. | 08/20/2025 | 08/20/2025 | [AWS VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) |
| 5 | - **Practice: Secure Web Server Deployment:** <br>&emsp; + Launch EC2 into the Custom VPC. <br>&emsp; + Attach an **IAM Role** (S3ReadOnlyAccess). <br>&emsp; + Use **User Data** script to install Apache (`httpd`) at launch. | 08/21/2025 | 08/22/2025 | [FCJ: Launch App on EC2](https://cloudjourney.awsstudygroup.com/vi/1-explore/4-ec2/) |
| 6 | - **Bonus/Review:** <br>&emsp; + **Static Website Hosting:** Review S3 basics by hosting a simple static page. <br>&emsp; + Verify EC2 connectivity and IAM role access via CLI inside the instance. | 08/22/2025 | 08/22/2025 | [FCJ: S3 Static Website](https://cloudjourney.awsstudygroup.com/vi/1-explore/7-static-web/) |


### Week 3 Achievements:

* **Architected a Custom Network Topology (VPC):**
    * Successfully completed the **First Cloud Journey VPC Module**.
    * Built a custom **Virtual Private Cloud (VPC)** with defined CIDR blocks, confirming understanding of network isolation.
    * Configured **Public Subnets** and **Internet Gateways**, ensuring that only specific resources have exposure to the public internet.

* **Implemented "Zero Trust" Principles with IAM Roles:**
    * Moved away from storing AWS Access Keys (`AK/SK`) on servers.
    * Attached an **IAM Role** to an EC2 instance, proving the ability to grant temporary, rotating credentials to applications securely.
    * *Verification:* Verified via the command `aws sts get-caller-identity` inside the EC2 instance that it assumed the correct role.

* **Deployed Compute with Automation:**
    * Launched an Amazon Linux 2 instance into the custom VPC.
    * Used **Bootstrapping (User Data)** to auto-install a web server, adhering to the *First Cloud Journey* best practices for reproducible infrastructure.

* **Explored S3 Capabilities:**
    * (Optional) Configured an S3 bucket for **Static Website Hosting**, understanding the difference between Block Storage (EBS) and Object Storage (S3).