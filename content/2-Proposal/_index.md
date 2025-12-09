---
title: "Proposal"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Smart Office Management System for Lab Research
## A Unified AWS Serverless Solution for Real-Time Monitoring & Administration

### 1. Executive Summary
The **Smart Office Management System** is a strategic initiative proposed by Team **Skyscraper** from **FPTU HCM Campus**. Inspired by the operational excellence observed during a field trip to the AWS office in Ho Chi Minh City, this project aims to modernize lab management. Traditional office oversight lacks real-time visibility into environmental conditions (temperature, humidity, light) and relies heavily on manual intervention. To address this, we propose a centralized **Management Console** built on a fully **AWS Serverless architecture**. By leveraging services such as AWS IoT Core, Lambda, and DynamoDB, the system collects sensor data at 2-5 minute intervals to support real-time monitoring and enable remote device configuration. This project also serves as a pivotal "First Cloud AI Journey," enabling the team to bridge the gap between theoretical knowledge and the practical application of Cloud Computing.

### 2. Problem Statement
#### The Challenge
Currently, managing research lab environments requires manual effort to check device statuses (lights, air conditioners) and environmental conditions. Managers often lack the granular data needed to make informed decisions regarding energy usage or occupant comfort. Operating devices on static schedules (e.g., 8:00 AM to 5:00 PM) without considering actual room usage or environmental factors leads to significant energy waste. Furthermore, the absence of a centralized dashboard makes it difficult for administrators to detect anomalies or configure settings across multiple rooms efficiently.

#### The Solution
Our platform utilizes **AWS IoT Core** to ingest MQTT telemetry from room sensors, **AWS Lambda** and **Amazon API Gateway** for backend logic and processing, **Amazon DynamoDB** for storing sensor logs and room configurations, and **Amazon S3** combined with **Amazon CloudFront** to host the web management dashboard. Security is paramount, with access strictly managed via **Amazon Cognito**. **Amazon EventBridge** orchestrates scheduled automation tasks, while **Amazon SNS** ensures timely notifications for system alerts. This solution transforms manual tracking into a digital, real-time management console capable of monitoring multiple rooms simultaneously.

#### Benefits and Return on Investment (ROI)
The Smart Office Management System enhances operational efficiency by providing a "single pane of glass" for monitoring and configuration. It empowers lab managers to control devices remotely and make data-driven decisions. Beyond operational improvements, the project establishes a reusable serverless foundation for future IoT research at the university.

**Cost Efficiency:** Monthly operating costs are estimated at just **$1.81 USD**, leveraging the AWS Free Tier for services like Lambda, API Gateway, and DynamoDB. The primary costs are attributed to CloudFront ($1.27) and CloudWatch ($0.25), totaling approximately **$21.72 USD per year**. Since the system utilizes existing ESP32 hardware (or simulation scripts during dev), there are no additional capital expenditures. The system delivers immediate value through time savings and reduced management overhead.

### 3. Solution Architecture
The Smart Office system adopts a fully serverless AWS architecture optimized for cost, performance, and scalability. Data from multiple sensor hubs is transmitted to AWS IoT Core, processed by Lambda functions, and stored in DynamoDB for real-time monitoring. EventBridge automates scheduled actions, while SNS handles system notifications. The web dashboard is hosted on S3 and delivered securely via CloudFront, with user authentication managed through Amazon Cognito. This architecture minimizes operational overhead and ensures high reliability.

![Smart Office Architecture](/images/2-Proposal/Smart-Office-Architect-Diagram.drawio.png)

#### AWS Services Used
- **AWS IoT Core**: Ingests and manages MQTT data from smart room hubs, enabling secure communication between edge devices and the cloud.
- **AWS Lambda**: Serverless compute that executes backend logic for processing sensor telemetry, handling API requests, and executing management commands.
- **Amazon API Gateway**: Exposes secure RESTful endpoints for the web dashboard to interact with backend services.
- **Amazon DynamoDB**: A fast, NoSQL database for storing room configurations, device states, and historical sensor logs.
- **Amazon EventBridge**: Orchestrates event-driven workflows, such as scheduled configuration updates or heartbeat checks.
- **Amazon SNS**: Sends email notifications to administrators regarding system alerts or critical updates.
- **Amazon S3**: Hosts frontend static assets (HTML, CSS, JS) for the Management Dashboard.
- **Amazon CloudFront**: Delivers the web application globally with low latency and SSL security.
- **Amazon Cognito**: Manages user identity, authentication, and access control for the Management Console.
- **Amazon CloudWatch**: Collects logs and metrics to monitor system health and debug Lambda executions.

#### Component Design
- **Sensor Hubs**: IoT-enabled devices (ESP32) in each room collect telemetry (temperature, humidity, light) and transmit it to **AWS IoT Core** periodically.
- **Data Ingestion**: **AWS IoT Core** rules trigger the **HandleTelemetry Lambda**, which validates data and persists it to **Amazon DynamoDB**.
- **Configuration Management**: Administrators use the dashboard to update room settings. The **RoomConfigHandler Lambda** updates DynamoDB and pushes changes to devices via IoT Core Shadows or MQTT topics.
- **User Interaction**: The **Web Dashboard** (served via **S3/CloudFront**) visualizes real-time data and provides a control interface.
- **User Authentication**: **Amazon Cognito** ensures only authorized lab members can log in and access sensitive room data.
- **Monitoring & Reliability**: **Amazon CloudWatch** tracks system performance, ensuring high availability and rapid troubleshooting.

### 4. Technical Implementation
**Implementation Phases**
- **Phase 1: Research & Foundation (Weeks 1-7)**: Deep dive into core AWS services (IoT Core, Lambda, DynamoDB, S3, API Gateway, Cognito) and study Serverless design patterns.
- **Phase 2: Design & Estimation (Week 8)**: Finalize the solution diagram for an 8-room setup and use the AWS Pricing Calculator to forecast the budget.
- **Phase 3: Development (Weeks 9-12)**:
    - *IoT:* Implement firmware/scripts for IoT data simulation.
    - *Backend:* Develop Lambda functions, DynamoDB tables, and API Gateway resources using CloudFormation/CDK.
    - *Frontend:* Build the Management Dashboard and integrate it with APIs.
- **Phase 4: Testing & Deployment (Week 13)**: Perform end-to-end testing, validate data flow from sensors to the dashboard, and deploy the system to the production environment.

**Technical Requirements**
- **Hardware Layer**: ESP32-based Sensor Hubs (or simulation scripts) monitoring environmental metrics.
- **Cloud Layer**: A fully serverless stack on AWS (IoT Core, Lambda, DynamoDB, API Gateway, S3, CloudFront, Cognito, EventBridge, SNS).
- **DevOps**: Infrastructure as Code (IaC) using AWS CloudFormation for reproducible deployments.
- **Interface**: A responsive web dashboard allowing real-time monitoring and configuration updates.

### 5. Timeline & Milestones
- **Weeks 1–7**: AWS Service mastery and completion of "First Cloud AI Journey" fundamental training.
- **Week 8**: System architecture design and cost estimation finalization.
- **Weeks 9–12**: Core development phase (Backend logic, Database schema, Frontend UI integration).
- **Week 13**: System integration testing, debugging, and Go-Live presentation.

### 6. Budget Estimation
You can view the detailed budget estimation on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=0db12150c448b012356e475becefd549c37094d8) or download the [Budget Estimation File](/file/proposal/smart_office_pricing_caculator.pdf).

**Operational Costs (Monthly):**
- **Amazon DynamoDB**: Free (Always Free Tier: 25GB Storage).
- **AWS Lambda**: Free (Always Free Tier: 1M requests/month).
- **AWS IoT Core**: ~$0.18 (8 devices, sending data every 2 mins).
- **Amazon API Gateway**: Free (Free Tier: 1M calls/month for 12 months).
- **Amazon S3**: Free (Standard storage < 5GB).
- **Amazon CloudFront**: ~$1.27 (Based on est. data transfer and HTTP requests).
- **Amazon EventBridge**: Free (Free Tier events).
- **Amazon SNS**: ~$0.02 (Email notifications).
- **Amazon CloudWatch**: ~$0.25 (Log ingestion and storage).
- **Amazon Cognito**: Free (Free Tier: 50,000 MAUs).
- **Hardware Layer**: $0.00 (Using existing hardware or Mock Scripts).

**Total:** ≈ **$1.81/month** (approx. **$21.72/year**), highly optimized within the AWS Free Tier.

### 7. Risk Assessment
#### Risk Matrix
- **IoT Connectivity Issues**: Medium Impact, Medium Probability.
- **Sensor Data Inaccuracy**: Medium Impact, Low Probability.
- **Unexpected AWS Charges**: Low Impact, Low Probability (mitigated by budget alerts).
- **Security Misconfiguration**: High Impact, Low Probability.

#### Mitigation Strategies
- **Connectivity**: Implement retry logic on edge devices and local data buffering.
- **Cost Control**: Configure AWS Budgets to alert when spending exceeds $5.00/month.
- **Security**: Enforce strict IAM policies (Least Privilege) and require authentication for all API access via Cognito.
- **Reliability**: Use CloudWatch Logs to trace errors in Lambda execution immediately.

#### Contingency Plans
- Enable manual override controls if the cloud system becomes unavailable.
- Maintain a backup of CloudFormation templates for rapid disaster recovery/redeployment.

### 8. Expected Outcomes
#### Technical Improvements:
- Transition from manual checks to **real-time digital monitoring**.
- Establish a **centralized platform** for managing configurations across multiple rooms.
- Create a **scalable architecture** capable of supporting more devices/sensors in the future.

#### Long-term Value:
- Serves as a practical **learning hub** for students to master AWS Serverless technologies.
- Provides actionable data insights that can lead to better energy usage policies.
- Demonstrates a cost-effective, production-ready cloud solution suitable for university deployment.

## Proposal Link

[Smart_Office_Proposal](/file/proposal/Smart_Office_Proposal.docx)