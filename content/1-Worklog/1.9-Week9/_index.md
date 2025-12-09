---
title: "Week 9 Worklog: Project Kickoff - Smart Office IoT Core"
date: 2025
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report.
{{% /notice %}}



### Week 9 Objectives:

* **Project Initialization:** Understand the "Smart Office" architecture and requirements (Sensors: Temperature, Light, Door Status).
* **IoT Infrastructure Setup:** Configure **AWS IoT Core** to act as the central message broker.
* **Device Connectivity:** Register "Things" and establish secure MQTT communication using X.509 certificates.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Project Kickoff & Architecture Design:** <br>&emsp; + Analyze the Smart Office requirements. <br>&emsp; + Design the flow: Device -> IoT Core -> DynamoDB/SNS. <br>&emsp; + Define the **MQTT Topics** structure (e.g., `smart-office/room1/temp`). | 11/03/2025 | 11/03/2025 | [AWS IoT Core Architecture](https://aws.amazon.com/iot-core/features/) |
| 3 | - **Configure AWS IoT Core:** <br>&emsp; + Create an **IoT Policy** allowing `iot:Connect`, `iot:Publish`, and `iot:Subscribe`. <br>&emsp; + Create a **Thing** in the registry (e.g., `Virtual_Temp_Sensor`). | 11/04/2025 | 11/04/2025 | [Create AWS IoT Things](https://docs.aws.amazon.com/iot/latest/developerguide/iot-thing-management.html) |
| 4 | - **Device Security (Certificates):** <br>&emsp; + Generate **X.509 Certificates** (Public Key, Private Key, Root CA). <br>&emsp; + Attach the IoT Policy and Certificate to the Thing. <br>&emsp; + *DevSecOps:* Ensure private keys are stored securely. | 11/05/2025 | 11/05/2025 | [X.509 Client Certificates](https://docs.aws.amazon.com/iot/latest/developerguide/x509-client-certs.html) |
| 5 | - **Simulate IoT Device:** <br>&emsp; + Write a script (Python/Node.js) using **AWS IoT Device SDK**. <br>&emsp; + Simulate sensor data (e.g., sending random temperature values). <br>&emsp; + Connect to the IoT Core Endpoint. | 11/06/2025 | 11/06/2025 | [AWS IoT Device SDKs](https://aws.amazon.com/iot/sdks/) |
| 6 | - **Verify MQTT Messaging:** <br>&emsp; + Use the **MQTT Test Client** in AWS Console. <br>&emsp; + Subscribe to `#` (wildcard) or specific topic. <br>&emsp; + Verify that data sent from the script appears in the Console in real-time. | 11/07/2025 | 11/07/2025 | [View MQTT messages](https://docs.aws.amazon.com/iot/latest/developerguide/view-mqtt-messages.html) |


### Week 9 Achievements:

* **Architected the Smart Office Solution:**
    * Defined the data flow and topic hierarchy for the Smart Office environment.
    * Selected **MQTT** as the lightweight communication protocol suitable for sensor networks.

* **Provisioned IoT Infrastructure:**
    * Successfully set up **AWS IoT Core** resources.
    * Created and registered "Things" representing the physical/virtual sensors of the office.

* **Established Secure Device Communication:**
    * Implemented **Mutual TLS (mTLS)** authentication using X.509 certificates.
    * Successfully simulated a sensor device sending telemetry data to the cloud, verified via the MQTT Test Client.