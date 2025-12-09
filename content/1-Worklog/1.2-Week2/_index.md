---
title: "Week 2 Worklog: AWS Foundations and Command Line Mastery"
date: 2025
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 2 Objectives:

* Connect and get acquainted with members of First Cloud Journey.
* **Master the fundamentals of core AWS service categories** to understand platform capabilities.
* **Establish secure, automated resource interaction** via the AWS Console and Command Line Interface (CLI).

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - Get acquainted with FCJ members <br> - Read and take note of internship unit rules and regulations | 08/11/2025 | 08/11/2025 | *Internal Onboarding Docs* |
| 3 | - Learn about AWS and its types of services: <br>&emsp; + **Compute:** EC2, Lambda, ECS <br>&emsp; + **Storage:** S3, EBS, EFS <br>&emsp; + **Networking:** VPC, Route 53 <br>&emsp; + **Database:** RDS, DynamoDB | 08/12/2025 | 08/12/2025 | [AWS Cloud Concepts Overview](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/what-is-cloud-computing.html) |
| 4 | - Create AWS Free Tier account with **IAM Best Practices** (MFA, Admin User). <br> - Install & configure **AWS CLI** to enable automation. <br> - **Practice:** <br>&emsp; + Configure CLI profiles (`aws configure`). <br> &emsp; + Verify access (`aws sts get-caller-identity`). | 08/13/2025 | 08/13/2025 | [AWS Free Tier Guide](https://aws.amazon.com/free/) <br> [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) |
| 5 | - Learn basic EC2 components: <br>&emsp; + **Instance Types:** (General Purpose vs. Compute Optimized) <br>&emsp; + **AMI:** Customizing OS images <br>&emsp; + **EBS:** Volume types (gp3, io2) & snapshots <br> - **Security:** SSH Key Pairs & Security Groups. | 08/14/2025 | 08/15/2025 | [Amazon EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) |
| 6 | - **Practice:** <br>&emsp; + Launch an EC2 instance via Console. <br>&emsp; + Connect via SSH (`ssh -i key.pem ec2-user@IP`). <br>&emsp; + Attach an EBS volume and mount it. | 08/15/2025 | 08/15/2025 | [Tutorial: Launch EC2 Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/tutorial-launch-my-first-ec2-instance.html) |


### Week 2 Achievements:

* **Established Foundational AWS Security:** Successfully created an AWS Free Tier account and immediately implemented **IAM best practices** by creating a dedicated administrative user with Multi-Factor Authentication (MFA), rather than using the root account.

* **Mastered Core AWS Service Categories:** Gained a working understanding of the foundational services (Compute, Storage, Networking, Database) and their roles in creating resilient, scalable cloud architecture. 
    * *Insight:* Recognized that **S3** is Object storage suitable for static assets, while **EBS** is Block storage required for EC2 OS drives.

* **Achieved Operational Automation via AWS CLI:** Installed, configured, and leveraged the AWS CLI (Command Line Interface). 
    * Configured credentials using `aws configure`.
    * Verified successful authentication using `aws sts get-caller-identity`.
    * *DevOps Note:* CLI mastery is the first step toward scripting and full **Infrastructure as Code (IaC)**.

* **Implemented and Validated EC2 Deployment:** Successfully launched, configured, and terminated an EC2 instance.
    * Practiced creating **Security Groups** (firewall rules) to allow SSH traffic on port 22 only from my specific IP.
    * Attached and mounted an **EBS volume** to a running Linux instance to simulate expanding server storage dynamically.