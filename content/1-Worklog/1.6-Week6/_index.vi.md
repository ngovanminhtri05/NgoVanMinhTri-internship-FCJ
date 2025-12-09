---
title: "Worklog Tuần 6: Giám sát & Khả năng quan sát (Monitoring & Observability)"
date: 2025
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Thông tin dưới đây chỉ mang tính chất tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn.
{{% /notice %}}



### Mục tiêu Tuần 6:

* **Triển khai Giám sát Chủ động:** Chuyển đổi tư duy từ "kiểm tra xem web có chạy không" sang "biết ngay khi nào hệ thống gặp lỗi" bằng **Amazon CloudWatch**.
* **Tập trung hóa Log (Centralized Logging):** Cấu hình EC2 để đẩy log hệ điều hành và log ứng dụng về **CloudWatch Logs** thay vì lưu cục bộ.
* **Tự động hóa Cảnh báo (Alerting):** Thiết lập quy trình phản ứng sự cố sử dụng **CloudWatch Alarms** và **Amazon SNS**.

### Các công việc thực hiện trong tuần:
| Ngày | Nhiệm vụ | Ngày Bắt đầu | Ngày Hoàn thành | Tài liệu Tham khảo |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Nghiên cứu nguyên lý Observability:** <br>&emsp; + **Metrics (Chỉ số):** Phân biệt Metric tiêu chuẩn (CPU) và Custom Metric (RAM, Disk). <br>&emsp; + **Logs:** Khái niệm Log Groups, Log Streams và chính sách lưu trữ (Retention). <br>&emsp; + **Alarms:** Các trạng thái (OK, ALARM, INSUFFICIENT_DATA). | 08/09/2025 | 08/09/2025 | [Amazon CloudWatch là gì?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) |
| 3 | - **Thực hành: Cài đặt CloudWatch Agent:** <br>&emsp; + *Thách thức:* EC2 mặc định không báo cáo chỉ số RAM. <br>&emsp; + Gắn IAM Role (`CloudWatchAgentServerPolicy`) cho EC2. <br>&emsp; + Cài đặt và cấu hình **Unified CloudWatch Agent** để đẩy thông số % RAM và System Logs lên Cloud. | 09/09/2025 | 09/09/2025 | [Thu thập metrics và logs với Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) |
| 4 | - **Thực hành: Tạo Dashboard Vận hành:** <br>&emsp; + Tạo **CloudWatch Dashboard** tùy chỉnh. <br>&emsp; + Thêm các widget hiển thị KPI quan trọng: CPU Utilization (Avg/Max), Network In/Out, và Chi phí ước tính (Billing). | 10/09/2025 | 10/09/2025 | [Sử dụng Amazon CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) |
| 5 | - **Thực hành: Đường ống Cảnh báo (SNS):** <br>&emsp; + Tạo **Amazon SNS** Topic (ví dụ: `DevOps-Alerts`). <br>&emsp; + Subscribe email cá nhân vào topic và xác nhận. <br>&emsp; + Tạo CloudWatch Alarm: Kích hoạt nếu `CPU Utilization > 70%` trong 2 chu kỳ 5 phút. <br>&emsp; + Liên kết hành động của Alarm với SNS Topic. | 11/09/2025 | 11/09/2025 | [Sử dụng Amazon CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) |
| 6 | - **Stress Test & Kiểm chứng:** <br>&emsp; + Chạy công cụ stress (`stress-ng`) trên EC2 để làm tăng vọt CPU. <br>&emsp; + Quan sát trạng thái Alarm chuyển sang `ALARM`. <br>&emsp; + Xác nhận đã nhận được email cảnh báo từ AWS. | 12/09/2025 | 12/09/2025 | *Tự kiểm chứng* |


### Kết quả đạt được trong Tuần 6:

* **Thiết lập khả năng quan sát toàn diện (Full-Stack Observability):**
    * Nhận ra rằng các chỉ số mặc định của EC2 (cấp Hypervisor) là chưa đủ cho vận hành ứng dụng thực tế.
    * Đã triển khai thành công **CloudWatch Unified Agent** để thu thập các chỉ số cấp hệ điều hành (Memory/Disk usage) và logs.

* **Triển khai Dashboard giám sát tập trung ("Single Pane of Glass"):**
    * Tạo được **Custom Dashboard** cung cấp cái nhìn thời gian thực về sức khỏe hạ tầng.
    * Gom nhóm các chỉ số từ Load Balancer (ELB) và Compute (EC2) vào một giao diện duy nhất để xử lý sự cố nhanh hơn.

* **Tự động hóa phản ứng sự cố:**
    * Xây dựng chuỗi cảnh báo tự động: **Metric tăng đột biến → Alarm → SNS → Email**.
    * Thiết lập này cho phép đội ngũ phản ứng chủ động với các sự kiện tải cao trước khi hệ thống bị sập, đáp ứng trụ cột "Operational Excellence" trong Well-Architected Framework.