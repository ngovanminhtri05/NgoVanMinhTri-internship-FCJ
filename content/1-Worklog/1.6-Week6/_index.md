---
title: "Week 6 Worklog: Monitoring & Observability"
date: 2025
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report.
{{% /notice %}}



### Week 6 Objectives:

* **Implement Proactive Monitoring:** Move from "checking if it works" to "knowing when it breaks" using **Amazon CloudWatch**.
* **Centralize Logging:** Configure EC2 instances to stream OS-level logs (syslog/application logs) to **CloudWatch Logs**.
* **Automate Alerting:** Set up an incident response pipeline using **CloudWatch Alarms** and **Amazon SNS** (Simple Notification Service).

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Study Observability Fundamentals:** <br>&emsp; + **Metrics:** Standard (CPU, Network) vs. Custom Metrics (RAM, Disk Usage). <br>&emsp; + **Logs:** Log groups, log streams, and retention policies. <br>&emsp; + **Alarms:** States (OK, ALARM, INSUFFICIENT_DATA). | 09/08/2025 | 09/08/2025 | [What is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) |
| 3 | - **Practice: Install CloudWatch Agent:** <br>&emsp; + *Challenge:* EC2 does not report RAM usage by default. <br>&emsp; + Attach IAM Role (`CloudWatchAgentServerPolicy`) to EC2. <br>&emsp; + Install and configure the **Unified CloudWatch Agent** to push Memory % and System Logs. | 09/09/2025 | 09/09/2025 | [Collect metrics and logs with the CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) |
| 4 | - **Practice: Create Operational Dashboards:** <br>&emsp; + Create a custom **CloudWatch Dashboard**. <br>&emsp; + Add widgets for Key Performance Indicators (KPIs): CPU Utilization (Avg/Max), Network In/Out, and Estimated Charges. | 09/10/2025 | 09/10/2025 | [Using Amazon CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) |
| 5 | - **Practice: Alerting Pipeline (SNS):** <br>&emsp; + Create an **Amazon SNS** Topic (e.g., `DevOps-Alerts`). <br>&emsp; + Subscribe your email address to the topic and confirm subscription. <br>&emsp; + Create a CloudWatch Alarm: Trigger if `CPU Utilization > 70%` for 2 periods of 5 minutes. <br>&emsp; + Link Alarm action to the SNS Topic. | 09/11/2025 | 09/11/2025 | [Using Amazon CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) |
| 6 | - **Stress Test & Verification:** <br>&emsp; + Run a stress tool (`stress-ng`) on the EC2 instance to spike CPU. <br>&emsp; + Verify the Alarm state changes to `ALARM`. <br>&emsp; + Confirm receipt of the alert email from AWS. | 09/12/2025 | 09/12/2025 | *Self-Verification* |


### Week 6 Achievements:

* **Established Full-Stack Observability:**
    * Recognized that default EC2 metrics (Hypervisor level) are insufficient for operations.
    * Successfully deployed the **CloudWatch Unified Agent** to capture OS-level metrics (Memory/Disk usage) and logs.

* **Implemented "Single Pane of Glass" Monitoring:**
    * Created a **Custom Dashboard** providing a real-time view of the infrastructure's health.
    * Grouped metrics from the Load Balancer (ELB) and Compute (EC2) into a single view for faster troubleshooting.

* **Automated Incident Response (ChatOps Prep):**
    * Built an automated alerting chain: **Metric Spike → Alarm → SNS → Email**.
    * This setup allows the team to react to high-load events proactively before the system crashes, fulfilling the "Operational Excellence" pillar of the Well-Architected Framework.