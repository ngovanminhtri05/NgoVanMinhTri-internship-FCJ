---
title: "Week 5 Worklog: High Availability & Auto Scaling"
date: 2025
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report.
{{% /notice %}}



### Week 5 Objectives:

* **Design for Failure:** Eliminate Single Points of Failure (SPOF) by distributing traffic across multiple Availability Zones (AZs).
* **Implement Elasticity:** Configure **Auto Scaling Groups (ASG)** to automatically scale compute capacity out and in based on demand.
* **Traffic Management:** Deploy an **Application Load Balancer (ALB)** to act as the single entry point for the application.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Study High Availability Concepts:** <br>&emsp; + **Vertical vs. Horizontal Scaling:** Why scale out (add servers) vs. scale up (bigger CPU). <br>&emsp; + **Load Balancing:** Layer 7 (ALB) features like path-based routing. <br>&emsp; + **Target Groups & Health Checks:** How ALB detects bad instances. | 09/01/2025 | 09/01/2025 | [Elastic Load Balancing (ELB) Overview](https://aws.amazon.com/elasticloadbalancing/) |
| 3 | - **Practice: Create Launch Template:** <br>&emsp; + Create a **Launch Template** (modern replacement for Launch Config). <br>&emsp; + Define the AMI (Web Server from Week 3), Instance Type, Security Groups, and IAM Role. <br>&emsp; + *DevOps Note:* Ensure User Data is included for automated startup. | 09/02/2025 | 09/02/2025 | [Creating a Launch Template](https://docs.aws.amazon.com/autoscaling/ec2/userguide/create-launch-template.html) |
| 4 | - **Practice: Deploy Application Load Balancer (ALB):** <br>&emsp; + Create an ALB in **Public Subnets** (spanning 2 AZs). <br>&emsp; + Create a **Target Group** for HTTP traffic (Port 80). <br>&emsp; + Configure Health Checks to ping `/index.html`. | 09/03/2025 | 09/03/2025 | [Create an Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-application-load-balancer.html) |
| 5 | - **Practice: Configure Auto Scaling Group (ASG):** <br>&emsp; + Create an ASG using the Launch Template. <br>&emsp; + Define capacity: Min: 2, Desired: 2, Max: 4. <br>&emsp; + Attach the ASG to the ALB Target Group. <br>&emsp; + **Stress Test:** Simulate CPU load to trigger a scale-out event. | 09/04/2025 | 09/05/2025 | [Amazon EC2 Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html) |
| 6 | - **Verify Resilience:** <br>&emsp; + Manually terminate one EC2 instance to test **Self-Healing**. <br>&emsp; + Observe ASG automatically launching a replacement. | 09/05/2025 | 09/05/2025 | *Self-Verification* |

### Week 5 Achievements:

* **Architected High Availability (HA):**
    * Deployed the application across **multiple Availability Zones (AZs)**.
    * Ensured that if one Data Center (AZ) goes offline, the application remains accessible via the Load Balancer.

* **Implemented Immutable Infrastructure Patterns:**
    * Transitioned from manually launching instances to using **Launch Templates**.
    * Standardized the server configuration (AMI, Security Groups, IAM), ensuring every new server is an exact replica of the "Gold Image."

* **Mastered Traffic Orchestration:**
    * Configured an **Application Load Balancer (ALB)** to intelligently route internet traffic to healthy instances only.
    * Implemented **Health Checks**, preventing the ALB from sending requests to failed or booting instances.

* **Achieved Operational Resilience (Self-Healing):**
    * Successfully configured an **Auto Scaling Group (ASG)**.
    * Validated the "Self-Healing" capability: When an instance was manually terminated, the ASG detected the health check failure and automatically provisioned a replacement without human intervention.