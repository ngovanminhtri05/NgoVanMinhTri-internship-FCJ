---
title: "Event 3"
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Summary Report: "AWS Cloud Mastery Series #3: AWS Well-Architected Security Pillar"

### Event Objectives

- **Security Foundations**
- **Identity & Access Management (IAM)**
- **Detection & Monitoring**
- **Infrastructure Protection**
- **Data Protection**
- **Incident Response**

### Speakers

- Le Vu Xuan An
- Tran Duc Anh
- Tran Doan Cong Ly
- Danh Hoang Hieu Nghi
- Thinh Lam
- Viet Nguyen
- Mendel Branski (Long)

### Key Highlights

#### Introduction to Cloud Clubs
- Introduction to University Cloud Clubs (e.g., UTE, SGU).
- Overview of Cloud Club activities and community building.

#### Security Foundation
- Service Control Policies (SCPs).
- Permission Boundaries.
- Multi-Factor Authentication (MFA).

#### Detection and Monitoring
- Multi-Layer Security Visibility.
- Alerting & Automation with Amazon EventBridge.
- Detection-as-Code.

#### Network and Data Protection
- VPC Security Group Sharing.
- API-Based Services Security.
- Secrets Management.

#### Incident Response
- **Prevention:** Why nobody has time for incidents.
- **Mental Health:** A guide to "sleeping better" by reducing alert fatigue.
- **Process:** structuring the incident response workflow.

### Key Takeaways

#### Service Control Policies (SCPs)
- **Definition:** An organizational policy type.
- **Function:** Controls the *maximum* available permissions for all accounts within the organization.
- **Rule:** SCPs never *grant* permissions; they can only *filter* or restrict them.

#### Permission Boundaries
- **Purpose:** An advanced IAM feature designed to solve delegation issues.
- **Function:** Sets the maximum permissions that an identity-based policy can grant to a specific User or Role.

#### Multi-Factor Authentication (MFA)
- **TOTP:** Shared secret based, requires a manual 6-digit code (e.g., Google Authenticator). It is free and offers flexible backup/recovery.
- **FIDO2:** Uses public-key cryptography, requires a simple touch/biometric scan (e.g., YubiKey). It is highly secure but requires strict backup strategies.

#### Alerting & Automation with EventBridge
- **Real-time Events:** CloudTrail events flow to EventBridge for immediate processing.
- **Automated Alerting:** Detect suspicious activities across all organization accounts.
- **Cross-account Event Routing:** Centralized event processing and automated response.
- **Integration & Workflows:** Integration with SNS, Slack, and SQS for automated security responses and team notifications.

#### Detection-as-Code
- **IaC Deployment:** Deploy Amazon GuardDuty across the organization using CloudFormation/Terraform (enable protection plans, configure data sources).
- **Custom Detection Rules:** Build suppression rules & IP whitelists to reduce false positives and adapt to the specific environment.
- **Version-Controlled Logic:** Track detection rules in Git and integrate them into the DevSecOps pipeline for testing and deployment.

#### Incident Response
- **Preparation:** Prepare automation handlers for potential incidents.
- **Forecasting:** Predict future incident scenarios and design response plans.
- **Post-Incident:** Document "Lessons Learned" after each incident to prevent recurrence.

### Applying to Work

- **Least Privilege:** Strictly specify and enforce least privilege policies for all projects.
- **MFA Enforcement:** Apply MFA to every account (Root and IAM users).
- **Proactive Planning:** Predict potential failure points and prepare response plans.

### Event Experience

Attending the **“AWS Well-Architected Security Pillar”** workshop helped me improve my knowledge of security and incident response significantly. I gained insights into the AWS Security Pillars through:

#### Learning from Skilled Speakers
- Learned how Senior Engineers handle high-pressure incidents and the "post-mortem" process.
- Learned how to protect data and networks using native AWS security features.

#### Exploring Cloud Club Activities
- Connected with the AWS Learner community from various universities.

#### Alerting & Automation
- Gained the ability to prepare infrastructure using CloudTrail, EventBridge, and CloudWatch to manage resources in real-time as soon as an incident occurs.


> **Overall:** The event was a great chance for me to expand my knowledge in Alerting, Automation, Security, and Incident Response. I have gained a lot of experience by listening to Senior Cloud Engineers discuss their daily work.