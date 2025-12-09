---
title: "Worklog Tuần 11: Smart Office - Serverless API & Trực quan hóa"
date: 2025
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Thông tin dưới đây chỉ mang tính chất tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn.
{{% /notice %}}



### Mục tiêu Tuần 11:

* **Xây dựng Logic xử lý (Business Layer):** Phát triển hàm **AWS Lambda** để truy xuất và định dạng dữ liệu lịch sử cảm biến từ DynamoDB.
* **Công khai dữ liệu qua REST API:** Cấu hình **Amazon API Gateway** để tạo một endpoint HTTP bảo mật cho ứng dụng.
* **Mở rộng kiến thức Cloud:** Tham gia sự kiện **AWS Cloud Mastery Series #2** để hiểu sâu hơn về các mô hình kiến trúc nâng cao.

### Các công việc thực hiện trong tuần:
| Ngày | Nhiệm vụ | Ngày Bắt đầu | Ngày Hoàn thành | Tài liệu Tham khảo |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Tham gia sự kiện AWS Cloud Mastery Series #2** <br>&emsp; + Chủ đề: Advanced Serverless Patterns / Resilience. <br>&emsp; + *Bài học:* Hiểu về khái niệm idempotency và cold starts trong Serverless. | 17/11/2025 | 17/11/2025 | *Ghi chú từ sự kiện* |
| 3 | - **Phát triển Logic Backend (Lambda):** <br>&emsp; + Tạo hàm Lambda (Node.js/Python). <br>&emsp; + Gắn **IAM Role** với quyền `dynamodb:Scan` hoặc `dynamodb:Query`. <br>&emsp; + Viết code để lấy 10 bản ghi mới nhất từ bảng `SmartOffice_Telemetry`. | 18/11/2025 | 18/11/2025 | [Xây dựng Lambda function với DynamoDB](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html) |
| 4 | - **Cấu hình API Gateway:** <br>&emsp; + Tạo **REST API**. <br>&emsp; + Tạo Resource `/data` và Method `GET`. <br>&emsp; + Tích hợp Method với hàm Lambda (Proxy Integration). | 19/11/2025 | 19/11/2025 | [Xây dựng REST API với Lambda proxy](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-create-api-as-simple-proxy-for-lambda.html) |
| 5 | - **Bảo mật & CORS:** <br>&emsp; + Bật **CORS** (Cross-Origin Resource Sharing) trên API Gateway để cho phép trình duyệt truy cập. <br>&emsp; + Deploy API ra môi trường (Stage: `dev`). <br>&emsp; + Test Invoke URL bằng Postman hoặc `curl`. | 20/11/2025 | 20/11/2025 | [Bật CORS cho REST API](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-cors.html) |
| 6 | - **Trực quan hóa (Frontend POC):** <br>&emsp; + Cập nhật S3 Static Website (từ Tuần 4/7). <br>&emsp; + Thêm mã JavaScript `fetch()` gọi tới API Endpoint mới tạo. <br>&emsp; + Hiển thị nhiệt độ thời gian thực lên trang web. | 21/11/2025 | 21/11/2025 | *Tự kiểm chứng* |

### Kết quả đạt được trong Tuần 11:

* **Phát triển thành công Serverless Business Logic:**
    * Đã viết và triển khai hàm **AWS Lambda** đóng vai trò cầu nối giữa lớp lưu trữ (DynamoDB) và người dùng cuối.
    * Áp dụng nguyên tắc **IAM Least Privilege** (Đặc quyền tối thiểu) bằng cách chỉ cấp quyền Read cho hàm Lambda trên đúng bảng dữ liệu cần thiết.

* **Xây dựng giao diện REST an toàn:**
    * Triển khai **Amazon API Gateway** để public dữ liệu IoT qua giao thức HTTP(S) tiêu chuẩn.
    * Cấu hình **CORS**, giải quyết vấn đề bảo mật trình duyệt phổ biến khi kết nối Frontend (S3) với Backend API.

* **Học tập liên tục:**
    * Tích cực tham gia **AWS Cloud Mastery Series #2**, thu nạp các kiến thức thực tiễn về mô hình cloud chuẩn công nghiệp để áp dụng ngay vào dự án Smart Office hiện tại.