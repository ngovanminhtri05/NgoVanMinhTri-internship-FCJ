---
title: "Week 11 Worklog: Smart Office - Serverless API & Visualization"
date: 2025
weight: 1
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report.
{{% /notice %}}



### Week 11 Objectives:

* **Build the Logic Layer:** Develop an **AWS Lambda** function to retrieve and format historical sensor data from DynamoDB.
* **Expose via REST API:** Configure **Amazon API Gateway** to create a public-facing, secure endpoint for the application.
* **Expand Cloud Knowledge:** Participate in the **AWS Cloud Mastery Series #2** to deepen understanding of advanced architectural patterns.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Participate in event AWS Cloud Mastery Series #2** <br>&emsp; + Topic: Advanced Serverless Patterns / Resilience. <br>&emsp; + *Key Takeaway:* Learned about idempotency and cold starts. | 11/07/2025 | 11/07/2025 | *Event Recording / Notes* |
| 3 | - **Develop Backend Logic (Lambda):** <br>&emsp; + Create a Lambda function (Node.js/Python). <br>&emsp; + Attach **IAM Role** with `dynamodb:Scan` or `dynamodb:Query` permissions. <br>&emsp; + Write code to fetch the last 10 readings from the `SmartOffice_Telemetry` table. | 11/18/2025 | 11/18/2025 | [Building Lambda functions with DynamoDB](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html) |
| 4 | - **Configure API Gateway:** <br>&emsp; + Create a **REST API**. <br>&emsp; + Create a Resource `/data` and Method `GET`. <br>&emsp; + Integrate the Method with the Lambda function (Proxy Integration). | 11/19/2025 | 11/19/2025 | [Build a REST API with Lambda proxy integration](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-create-api-as-simple-proxy-for-lambda.html) |
| 5 | - **Security & CORS:** <br>&emsp; + Enable **CORS** (Cross-Origin Resource Sharing) on API Gateway to allow browser access. <br>&emsp; + Deploy the API to a **Stage** (e.g., `dev`). <br>&emsp; + Test the Invoke URL using Postman or `curl`. | 11/20/2025 | 11/20/2025 | [Enabling CORS for a REST API resource](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-cors.html) |
| 6 | - **Visualization (Frontend POC):** <br>&emsp; + Update the S3 Static Website (from Week 4/7). <br>&emsp; + Add a simple JavaScript `fetch()` call to the new API Endpoint. <br>&emsp; + Display the live temperature data on the webpage. | 11/21/2025 | 11/21/2025 | *Self-Verification* |

### Week 11 Achievements:

* **Developed Serverless Business Logic:**
    * Successfully wrote and deployed an **AWS Lambda** function to bridge the gap between the storage layer (DynamoDB) and the user.
    * Implemented **IAM Least Privilege** by granting the function only Read access to the specific telemetry table.

* **Created a Secure REST Interface:**
    * Deployed **Amazon API Gateway** to expose the IoT data over standard HTTP(S).
    * Enabled **CORS**, resolving common browser-security issues when connecting a frontend (S3) to a backend API.

* **Continuous Learning:**
    * Actively participated in **AWS Cloud Mastery Series #2**, gaining insights into industry-standard cloud patterns which I applied to the current Smart Office project.