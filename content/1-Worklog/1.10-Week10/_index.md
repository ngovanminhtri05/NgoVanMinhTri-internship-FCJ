---
title: "Week 10 Worklog: Smart Office - Rules Engine & Data Persistence"
date: 2025
weight: 1
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report.
{{% /notice %}}



### Week 10 Objectives:

* **Implement Event-Driven Logic:** Use **AWS IoT Rules Engine** to filter and route incoming MQTT messages without managing servers.
* **Data Persistence:** Configure an **Amazon DynamoDB** integration to store time-series sensor data (Temperature, Humidity) for historical analysis.
* **Automated Alerting:** Set up an **Amazon SNS** topic to trigger immediate notifications when sensor readings exceed defined thresholds (e.g., Fire/High Temp).

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Database Design (NoSQL):** <br>&emsp; + Create a DynamoDB table `SmartOffice_Telemetry`. <br>&emsp; + **Partition Key:** `device_id` (String). <br>&emsp; + **Sort Key:** `timestamp` (Number/Epoch). <br>&emsp; + *Goal:* Optimize for time-series queries. | 11/10/2025 | 11/10/2025 | [Best Practices for DynamoDB Time-Series](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-time-series.html) |
| 3 | - **Configure IoT Rule (Persistence):** <br>&emsp; + Write SQL query: `SELECT * FROM 'smart-office/+/data'`. <br>&emsp; + Add **Action:** Insert message into DynamoDB table. <br>&emsp; + Create IAM Role granting IoT Core permission to `PutItem`. | 11/11/2025 | 11/11/2025 | [Creating an AWS IoT Rule](https://docs.aws.amazon.com/iot/latest/developerguide/iot-create-rule.html) |
| 4 | - **Configure IoT Rule (Alerting):** <br>&emsp; + Create SNS Topic `Office_Alerts` and subscribe email. <br>&emsp; + Write SQL query with **Condition**: `SELECT * FROM 'smart-office/+/data' WHERE temperature > 30`. <br>&emsp; + Add **Action:** Send message to SNS. | 11/12/2025 | 11/12/2025 | [IoT Rule SQL Reference](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sql-reference.html) |
| 5 | - **End-to-End Testing:** <br>&emsp; + Run the Device Simulator (from Week 9). <br>&emsp; + Send "Normal" data -> Verify entry in DynamoDB. <br>&emsp; + Send "High Temp" data -> Verify DynamoDB entry AND receive Email Alert. | 11/13/2025 | 11/13/2025 | *Self-Verification* |



### Week 10 Achievements:

* **Built a Serverless Backend for IoT:**
    * Leveraged **AWS IoT Rules Engine** to decouple the device layer from the storage layer. No EC2 instances were used to process the data, reducing operational overhead to zero.

* **Established Data Persistence:**
    * Successfully routed telemetry data into **Amazon DynamoDB**.
    * Validated the schema design (`device_id` + `timestamp`), ensuring data is stored efficiently for future dashboard retrieval.

* **Implemented Real-time Anomaly Detection:**
    * Configured conditional logic (`WHERE temperature > 30`) directly at the ingestion layer.
    * Verified that critical alerts (SNS) are triggered instantly, while normal data is simply logged, optimizing cost and noise.