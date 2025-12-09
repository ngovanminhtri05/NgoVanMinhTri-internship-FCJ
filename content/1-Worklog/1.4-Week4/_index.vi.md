---
title: "Worklog Tuần 4: Cơ sở dữ liệu (Database) & Lưu trữ (Storage)"
date: 2025
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Thông tin dưới đây chỉ mang tính chất tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn.
{{% /notice %}}

[Hình ảnh sơ đồ kiến trúc 2-tier với EC2 trong Public Subnet và RDS trong Private Subnet]

### Mục tiêu Tuần 4:

* **Làm chủ Managed Services (Dịch vụ được quản lý):** Hiểu rõ lợi ích vận hành khi sử dụng **Amazon RDS** so với việc tự cài đặt database trên EC2.
* **Triển khai Kiến trúc 2-Tier:** Kết nối an toàn giữa Web Server (EC2) và Database (RDS) bên trong môi trường VPC.
* **Lưu trữ Nội dung Tĩnh:** Triển khai frontend dạng serverless sử dụng tính năng **Static Website Hosting của Amazon S3**.

### Các công việc thực hiện trong tuần:
| Ngày | Nhiệm vụ | Ngày Bắt đầu | Ngày Hoàn thành | Tài liệu Tham khảo |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Nghiên cứu Kiến thức Database trên Cloud:** <br>&emsp; + Managed vs. Unmanaged (Tại sao nên dùng RDS?). <br>&emsp; + **DB Subnet Groups:** Đặt database trong Private Subnet để bảo mật. <br>&emsp; + **Security:** Mã hóa dữ liệu (Encryption) và tham chiếu Security Group. | 25/08/2025 | 25/08/2025 | [FCJ: Tạo Database trên RDS](https://cloudjourney.awsstudygroup.com/vi/1-explore/8-rds/) |
| 3 | - **Thực hành: Khởi tạo RDS MySQL:** <br>&emsp; + Tạo DB Subnet Group bao gồm 2 Private Subnets. <br>&emsp; + Provision (cấp phát) một instance RDS MySQL (Free Tier). <br>&emsp; + *Quan trọng:* Cấu hình tài khoản **Master User** một cách bảo mật. | 26/08/2025 | 26/08/2025 | [Tạo Amazon RDS DB instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html) |
| 4 | - **Thực hành: Chuỗi Security Group (Security Group Chaining):** <br>&emsp; + Tạo Security Group mới tên `DB-SG`. <br>&emsp; + **Tác vụ DevOps:** Cấu hình Inbound Port 3306 *chỉ cho phép* từ ID của `WebServer-SG` (Source: `sg-xxxxx`), KHÔNG dùng `0.0.0.0/0`. | 27/08/2025 | 27/08/2025 | [Kiểm soát truy cập với Security Groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.RDSSecurityGroups.html) |
| 5 | - **Thực hành: Kết nối Ứng dụng với Database:** <br>&emsp; + SSH vào EC2 instance (từ Tuần 3). <br>&emsp; + Cài đặt `mysql-client`. <br>&emsp; + Kiểm tra kết nối: `mysql -h <RDS-Endpoint> -u admin -p`. | 28/08/2025 | 28/08/2025 | [Kết nối tới RDS instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ConnectToInstance.html) |
| 6 | - **Dự án Static Website:** <br>&emsp; + Tạo S3 Bucket với quyền public read (Cấu hình Block Public Access). <br>&emsp; + Bật tính năng "Static Website Hosting". <br>&emsp; + Upload file `index.html` và `error.html`. | 29/08/2025 | 29/08/2025 | [FCJ: Host Static Website với S3](https://cloudjourney.awsstudygroup.com/vi/1-explore/7-static-web/) |


### Kết quả đạt được trong Tuần 4:

* **Triển khai thành công Managed Database (RDS):**
    * Đã khởi tạo thành công một **MySQL RDS instance** bên trong VPC.
    * Hiểu rõ mô hình "Shared Responsibility" (Trách nhiệm chia sẻ): AWS lo phần hạ tầng và vá lỗi OS, trong khi tôi quản lý dữ liệu và schema.

* **Thiết lập Phân đoạn Mạng An toàn (Kiến trúc 2-Tier):**
    * Đặt Database nằm trong **Private Subnets**, đảm bảo database không thể bị truy cập trực tiếp từ Internet công cộng.
    * Ứng dụng kỹ thuật **Security Group Chaining (Tham chiếu):** Cấu hình firewall cho Database chỉ chấp nhận traffic từ Web Server thông qua Security Group ID. Đây là một thực hành DevSecOps quan trọng giúp tránh việc hardcode địa chỉ IP.

* **Xác thực Kết nối Tầng Ứng dụng:**
    * Cài đặt thành công MySQL client trên EC2 Jumpbox/Web Server.
    * Thiết lập kết nối thành công tới RDS Endpoint, qua đó kiểm chứng cấu hình Route Table và Firewall đã hoạt động đúng.

* **Triển khai Frontend Serverless (S3):**
    * Cấu hình **Amazon S3 bucket** để host static website.
    * Quản lý Bucket Policies để cho phép quyền đọc (public read access) dành riêng cho tài nguyên web, phân biệt rõ giữa "Lưu trữ riêng tư" và "Hosting công khai".