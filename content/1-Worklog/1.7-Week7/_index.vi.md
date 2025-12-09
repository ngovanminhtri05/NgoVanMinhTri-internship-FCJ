---
title: "Worklog Tuần 7: Mạng phân phối nội dung (CDN) & Bảo mật tại biên"
date: 2025
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Thông tin dưới đây chỉ mang tính chất tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn.
{{% /notice %}}



### Mục tiêu Tuần 7:

* **Phân phối nội dung toàn cầu:** Giảm độ trễ (latency) cho người dùng cuối bằng cách cache các tài nguyên tĩnh (ảnh, CSS, JS) tại các **Edge Locations** sử dụng **Amazon CloudFront**.
* **Bảo vệ Origin (Gốc):** Triển khai **Origin Access Control (OAC)** để giới hạn quyền truy cập S3 bucket chỉ dành cho CloudFront, chặn hoàn toàn truy cập trực tiếp từ công cộng.
* **Bảo mật tại biên (Edge Security):** Triển khai **AWS WAF (Web Application Firewall)** để bảo vệ ứng dụng khỏi các cuộc tấn công web phổ biến (như SQLi, XSS) ngay từ lớp ngoài cùng.

### Các công việc thực hiện trong tuần:
| Ngày | Nhiệm vụ | Ngày Bắt đầu | Ngày Hoàn thành | Tài liệu Tham khảo |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Nghiên cứu kiến thức CDN:** <br>&emsp; + **Edge vs. Region:** Hiểu về mạng lưới toàn cầu của AWS. <br>&emsp; + **Hành vi Caching:** Khái niệm TTL (Time-to-Live) và Cache Invalidation (Làm mới cache). <br>&emsp; + **Security:** SSL/TLS Termination và bắt buộc sử dụng HTTPS. | 15/09/2025 | 15/09/2025 | [Amazon CloudFront là gì?](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) |
| 3 | - **Thực hành: CloudFront cho S3 (Static Site):** <br>&emsp; + Tạo CloudFront Distribution trỏ về S3 bucket (từ Tuần 4). <br>&emsp; + **Bước bảo mật quan trọng:** Cấu hình **Origin Access Control (OAC)**. <br>&emsp; + Cập nhật Bucket Policy để chặn truy cập trực tiếp từ internet, chỉ cho phép CloudFront. | 16/09/2025 | 16/09/2025 | [Giới hạn truy cập S3 Origin](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) |
| 4 | - **Thực hành: Tối ưu hóa Cache:** <br>&emsp; + Quan sát HTTP Headers trong DevTools (`X-Cache: Miss` vs. `Hit`). <br>&emsp; + Cấu hình tự động nén dữ liệu (Compression - Gzip/Brotli). <br>&emsp; + **Invalidation:** Thực hành xóa cache thủ công khi cập nhật file `index.html`. | 17/09/2025 | 17/09/2025 | [Quản lý hết hạn Cache](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Expiration.html) |
| 5 | - **Thực hành: Triển khai AWS WAF:** <br>&emsp; + Tạo Web ACL (Access Control List). <br>&emsp; + Thêm bộ luật được quản lý sẵn (**AWSManagedRulesCommonRuleSet** để chặn SQL Injection). <br>&emsp; + Gắn Web ACL vào CloudFront Distribution. | 18/09/2025 | 18/09/2025 | [AWS WAF Web ACLs](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html) |
| 6 | - **Kiểm chứng & Đánh giá (Benchmarking):** <br>&emsp; + So sánh tốc độ tải: URL trực tiếp S3 (Độ trễ cao) vs. URL CloudFront (Độ trễ thấp). <br>&emsp; + Test WAF: Giả lập tấn công (ví dụ: gắn thẻ script vào URL) và xác nhận nhận được lỗi `403 Forbidden`. | 19/09/2025 | 19/09/2025 | *Tự kiểm chứng* |

### Kết quả đạt được trong Tuần 7:

* **Tối ưu hóa Hiệu suất (Giảm độ trễ):**
    * Triển khai **Amazon CloudFront** để phục vụ nội dung tĩnh từ các Edge Locations (Vị trí biên) gần người dùng nhất.
    * Xác nhận tốc độ tải trang nhanh hơn đáng kể và đạt được **Cache Hit Ratio** cao, giúp giảm tải cho server gốc (Origin).

* **Bảo mật hóa Origin (S3 Hardening):**
    * Triển khai **Origin Access Control (OAC)** - chuẩn bảo mật hiện đại nhất cho kết nối S3-CloudFront.
    * Thành công trong việc thu hồi quyền truy cập công cộng (public read) trên S3 bucket, đảm bảo người dùng chỉ có thể truy cập qua đường dẫn CDN bảo mật (Tuân thủ DevSecOps).

* **Thiết lập Bảo mật vành đai (Perimeter Security):**
    * Tích hợp **AWS WAF** vào CloudFront distribution.
    * Áp dụng các bộ quy tắc (managed rule sets) để lọc bỏ các lưu lượng độc hại (theo chuẩn OWASP Top 10) trước khi chúng kịp chạm tới hạ tầng ứng dụng.