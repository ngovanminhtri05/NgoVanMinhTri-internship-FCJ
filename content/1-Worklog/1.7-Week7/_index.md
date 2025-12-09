---
title: "Week 7 Worklog: Content Delivery & Edge Security"
date: 2025
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report.
{{% /notice %}}



### Week 7 Objectives:

* **Global Content Delivery:** Reduce latency for end-users by caching static assets (images, CSS, JS) at **Edge Locations** using **Amazon CloudFront**.
* **Secure the Origin:** Implement **Origin Access Control (OAC)** to restrict S3 bucket access solely to CloudFront, disabling direct public access.
* **Edge Security:** Deploy **AWS WAF (Web Application Firewall)** to protect the application from common web exploits (e.g., SQLi, XSS) at the edge.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Study CDN Fundamentals:** <br>&emsp; + **Edge vs. Region:** Understanding the AWS Global Network. <br>&emsp; + **Caching Behaviors:** TTL (Time-to-Live), Cache Invalidation. <br>&emsp; + **Security:** SSL/TLS Termination and HTTPS enforcement. | 09/15/2025 | 09/15/2025 | [What is Amazon CloudFront?](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) |
| 3 | - **Practice: CloudFront for S3 (Static Site):** <br>&emsp; + Create a CloudFront Distribution pointing to the S3 bucket (from Week 4). <br>&emsp; + **Critical Security Step:** Configure **Origin Access Control (OAC)**. <br>&emsp; + Update S3 Bucket Policy to deny direct internet access and allow only CloudFront. | 09/16/2025 | 09/16/2025 | [Restricting access to an S3 origin](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) |
| 4 | - **Practice: Cache Optimization:** <br>&emsp; + Observe headers in the browser DevTools (`X-Cache: Miss` vs. `Hit`). <br>&emsp; + Configure Cache Behaviors to compress objects automatically (Gzip/Brotli). <br>&emsp; + **Invalidation:** Practice manually invalidating the cache when updating `index.html`. | 09/17/2025 | 09/17/2025 | [Managing Cache Expiration](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Expiration.html) |
| 5 | - **Practice: Implement AWS WAF:** <br>&emsp; + Create a Web ACL (Access Control List). <br>&emsp; + Add a Managed Rule (e.g., **AWSManagedRulesCommonRuleSet** to block SQL Injection). <br>&emsp; + Associate the Web ACL with the CloudFront Distribution. | 09/18/2025 | 09/18/2025 | [AWS WAF Web ACLs](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html) |
| 6 | - **Verification & Benchmarking:** <br>&emsp; + Compare load times: Direct S3 URL (High Latency) vs. CloudFront URL (Low Latency). <br>&emsp; + Test WAF: Attempt a simulated attack (e.g., passing a script tag in URL) and verify a `403 Forbidden` response. | 09/19/2025 | 09/19/2025 | *Self-Verification* |

### Week 7 Achievements:

* **Optimized Performance (Latency Reduction):**
    * Deployed **Amazon CloudFront** to serve static assets from Edge Locations closest to the user.
    * Verified a significant reduction in load times and achieved a high **Cache Hit Ratio**, reducing the load on the Origin server.

* **Secured the Origin (S3 Hardening):**
    * Implemented **Origin Access Control (OAC)**, the modern standard for S3-CloudFront security.
    * Successfully revoked public read access on the S3 bucket, ensuring that users can only access content through the secure CDN endpoint (DevSecOps best practice).

* **Implemented Perimeter Security (WAF):**
    * Integrated **AWS WAF** with the CloudFront distribution.
    * Applied managed rule sets to filter malicious traffic (OWASP Top 10 threats) before it even reaches the application infrastructure.