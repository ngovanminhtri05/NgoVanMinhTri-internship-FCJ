---
title: "Worklog Tuần 5: Tính sẵn sàng cao (HA) & Tự động mở rộng"
date: 2025
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Thông tin dưới đây chỉ mang tính chất tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn.
{{% /notice %}}



### Mục tiêu Tuần 5:

* **Thiết kế chịu lỗi (Design for Failure):** Loại bỏ các điểm lỗi đơn lẻ (Single Points of Failure) bằng cách phân tán traffic qua nhiều Availability Zones (AZs).
* **Triển khai tính đàn hồi (Elasticity):** Cấu hình **Auto Scaling Groups (ASG)** để tự động tăng/giảm số lượng server dựa trên nhu cầu thực tế.
* **Quản lý lưu lượng:** Triển khai **Application Load Balancer (ALB)** đóng vai trò là điểm truy cập duy nhất cho ứng dụng.

### Các công việc thực hiện trong tuần:
| Ngày | Nhiệm vụ | Ngày Bắt đầu | Ngày Hoàn thành | Tài liệu Tham khảo |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Nghiên cứu khái niệm High Availability (HA):** <br>&emsp; + **Vertical vs. Horizontal Scaling:** Phân biệt mở rộng theo chiều dọc (nâng cấp CPU) và chiều ngang (thêm server). <br>&emsp; + **Load Balancing:** Các tính năng của ALB (Layer 7) như path-based routing. <br>&emsp; + **Target Groups & Health Checks:** Cách ALB phát hiện instance bị lỗi. | 01/09/2025 | 01/09/2025 | [Tổng quan về Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing/) |
| 3 | - **Thực hành: Tạo Launch Template:** <br>&emsp; + Tạo **Launch Template** (chuẩn mới thay thế cho Launch Config). <br>&emsp; + Định nghĩa cấu hình chuẩn: AMI (Web Server từ Tuần 3), Instance Type, Security Groups, và IAM Role. <br>&emsp; + *DevOps Note:* Đảm bảo tích hợp User Data để ứng dụng tự khởi chạy. | 02/09/2025 | 02/09/2025 | [Tạo Launch Template](https://docs.aws.amazon.com/autoscaling/ec2/userguide/create-launch-template.html) |
| 4 | - **Thực hành: Triển khai Application Load Balancer (ALB):** <br>&emsp; + Khởi tạo ALB trong **Public Subnets** (trải dài trên 2 AZs). <br>&emsp; + Tạo **Target Group** cho traffic HTTP (Port 80). <br>&emsp; + Cấu hình Health Checks để ping đường dẫn `/index.html`. | 03/09/2025 | 03/09/2025 | [Tạo Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-application-load-balancer.html) |
| 5 | - **Thực hành: Cấu hình Auto Scaling Group (ASG):** <br>&emsp; + Tạo ASG sử dụng Launch Template đã tạo. <br>&emsp; + Định nghĩa công suất (Capacity): Min: 2, Desired: 2, Max: 4. <br>&emsp; + Gắn ASG vào ALB Target Group. <br>&emsp; + **Stress Test:** Giả lập tải CPU cao để kích hoạt sự kiện scale-out (tự động thêm server). | 04/09/2025 | 05/09/2025 | [Hướng dẫn sử dụng Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html) |
| 6 | - **Kiểm thử khả năng phục hồi:** <br>&emsp; + Chủ động Terminate (xóa) một EC2 instance để kiểm tra tính năng **Self-Healing**. <br>&emsp; + Quan sát ASG tự động phát hiện lỗi và khởi tạo instance mới thay thế. | 05/09/2025 | 05/09/2025 | *Tự kiểm chứng* |

### Kết quả đạt được trong Tuần 5:

* **Xây dựng kiến trúc High Availability (HA):**
    * Triển khai ứng dụng chạy đồng thời trên **nhiều Availability Zones (AZs)**.
    * Đảm bảo rằng nếu một Data Center (AZ) gặp sự cố, ứng dụng vẫn hoạt động bình thường nhờ Load Balancer điều hướng traffic sang AZ còn lại.

* **Áp dụng mô hình Immutable Infrastructure:**
    * Chuyển từ việc khởi tạo instance thủ công sang sử dụng **Launch Templates**.
    * Tiêu chuẩn hóa cấu hình server (AMI, Security Groups, IAM), đảm bảo mọi server mới sinh ra đều là bản sao chính xác của "Gold Image".

* **Làm chủ điều phối lưu lượng (Traffic Orchestration):**
    * Cấu hình **Application Load Balancer (ALB)** để điều hướng thông minh traffic từ internet đến các instance khỏe mạnh (healthy).
    * Triển khai **Health Checks**, ngăn chặn ALB gửi request đến các server đang khởi động hoặc bị lỗi.

* **Đạt được khả năng tự phục hồi (Operational Resilience):**
    * Cấu hình thành công **Auto Scaling Group (ASG)**.
    * Kiểm chứng khả năng "Self-Healing" (Tự chữa lành): Khi một instance bị xóa thủ công, ASG lập tức phát hiện sự cố health check và tự động cung cấp instance mới mà không cần can thiệp của con người.