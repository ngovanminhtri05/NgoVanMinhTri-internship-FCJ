---
title: "Worklog Tuần 10: Smart Office - Rules Engine & Lưu trữ dữ liệu"
date: 2025
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice warning %}} 
⚠️ **Lưu ý:** Thông tin dưới đây chỉ mang tính chất tham khảo. Vui lòng **không sao chép nguyên văn** cho báo cáo của bạn.
{{% /notice %}}



### Mục tiêu Tuần 10:

* **Triển khai Logic Hướng sự kiện (Event-Driven):** Sử dụng **AWS IoT Rules Engine** để lọc và điều hướng các tin nhắn MQTT đến mà không cần quản lý server.
* **Lưu trữ dữ liệu bền vững (Persistence):** Cấu hình tích hợp **Amazon DynamoDB** để lưu trữ lịch sử dữ liệu cảm biến (Nhiệt độ, Độ ẩm) phục vụ phân tích sau này.
* **Cảnh báo tự động:** Thiết lập **Amazon SNS** topic để gửi thông báo tức thì khi chỉ số cảm biến vượt quá ngưỡng quy định (ví dụ: Cháy/Nhiệt độ cao).

### Các công việc thực hiện trong tuần:
| Ngày | Nhiệm vụ | Ngày Bắt đầu | Ngày Hoàn thành | Tài liệu Tham khảo |
| :--- | :--- | :--- | :--- | :--- |
| 2 | - **Thiết kế Database (NoSQL):** <br>&emsp; + Tạo bảng DynamoDB `SmartOffice_Telemetry`. <br>&emsp; + **Partition Key:** `device_id` (String). <br>&emsp; + **Sort Key:** `timestamp` (Number/Epoch). <br>&emsp; + *Mục tiêu:* Tối ưu hóa cho các truy vấn theo chuỗi thời gian. | 10/11/2025 | 10/11/2025 | [Best Practices cho DynamoDB Time-Series](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-time-series.html) |
| 3 | - **Cấu hình IoT Rule (Lưu trữ):** <br>&emsp; + Viết câu truy vấn SQL: `SELECT * FROM 'smart-office/+/data'`. <br>&emsp; + Thêm **Action:** Insert message vào bảng DynamoDB. <br>&emsp; + Tạo IAM Role cấp quyền cho IoT Core thực hiện `PutItem`. | 11/11/2025 | 11/11/2025 | [Tạo AWS IoT Rule](https://docs.aws.amazon.com/iot/latest/developerguide/iot-create-rule.html) |
| 4 | - **Cấu hình IoT Rule (Cảnh báo):** <br>&emsp; + Tạo SNS Topic `Office_Alerts` và subscribe email. <br>&emsp; + Viết truy vấn SQL với **Condition**: `SELECT * FROM 'smart-office/+/data' WHERE temperature > 30`. <br>&emsp; + Thêm **Action:** Gửi tin nhắn đến SNS. | 12/11/2025 | 12/11/2025 | [Tài liệu tham khảo SQL IoT](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sql-reference.html) |
| 5 | - **Kiểm thử toàn trình (End-to-End Testing):** <br>&emsp; + Chạy thiết bị giả lập (từ Tuần 9). <br>&emsp; + Gửi dữ liệu "Bình thường" -> Kiểm tra dữ liệu xuất hiện trong DynamoDB. <br>&emsp; + Gửi dữ liệu "Nhiệt độ cao" -> Kiểm tra DynamoDB VÀ nhận Email cảnh báo. | 13/11/2025 | 13/11/2025 | *Tự kiểm chứng* |
| 6 | - **Xử lý lỗi (Error Handling):** <br>&emsp; + Cấu hình **Error Action** cho các IoT Rules. <br>&emsp; + Điều hướng các tin nhắn bị lỗi (ví dụ: do DynamoDB throttling) sang S3 hoặc SQS để debug. | 14/11/2025 | 14/11/2025 | [Xử lý lỗi IoT Rules](https://docs.aws.amazon.com/iot/latest/developerguide/rule-error-handling.html) |


### Kết quả đạt được trong Tuần 10:

* **Xây dựng Backend Serverless cho IoT:**
    * Tận dụng **AWS IoT Rules Engine** để tách biệt lớp thiết bị (Device layer) khỏi lớp lưu trữ (Storage layer). Không sử dụng bất kỳ EC2 instance nào để xử lý dữ liệu, giảm chi phí vận hành xuống mức tối thiểu.

* **Thiết lập lưu trữ dữ liệu:**
    * Đã điều hướng thành công dữ liệu telemetry vào **Amazon DynamoDB**.
    * Kiểm chứng thiết kế schema (`device_id` + `timestamp`), đảm bảo dữ liệu được lưu trữ hiệu quả cho việc truy xuất dashboard sau này.

* **Triển khai phát hiện bất thường thời gian thực:**
    * Cấu hình logic điều kiện (`WHERE temperature > 30`) ngay tại lớp tiếp nhận dữ liệu.
    * Xác nhận các cảnh báo quan trọng (SNS) được kích hoạt ngay lập tức, trong khi dữ liệu bình thường chỉ được log lại, giúp tối ưu hóa chi phí và giảm nhiễu.