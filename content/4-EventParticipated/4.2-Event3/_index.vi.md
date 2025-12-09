---
title: "Sự kiện 4"
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# Báo cáo tóm tắt: "AWS Cloud Mastery Series #3: Trụ cột Bảo mật trong AWS Well-Architected"

### Mục tiêu sự kiện

- **Nền tảng Bảo mật (Security Foundation)**
- **Quản lý Định danh & Truy cập (IAM)**
- **Phát hiện & Giám sát**
- **Bảo vệ Cơ sở hạ tầng**
- **Bảo vệ Dữ liệu**
- **Ứng phó Sự cố (Incident Response)**

### Diễn giả

- Lê Vũ Xuân An
- Trần Đức Anh
- Trần Đoàn Công Lý
- Danh Hoàng Hiếu Nghị
- Thịnh Lâm
- Việt Nguyễn
- Mendel Branski (Long)

### Điểm nhấn chính

#### Giới thiệu về Cloud Club
- Giới thiệu các CLB Điện toán đám mây tại các trường đại học như UTE, SGU.
- Các hoạt động cộng đồng của Cloud Club.

#### Nền tảng Bảo mật
- Chính sách kiểm soát dịch vụ (Service Control Policies - SCP).
- Ranh giới quyền hạn (Permission Boundaries).
- Xác thực đa yếu tố (MFA).

#### Phát hiện và Giám sát
- Khả năng hiển thị bảo mật đa lớp (Multi-Layer Security Visibility).
- Cảnh báo & Tự động hóa với Amazon EventBridge.
- Detection-as-Code (Phát hiện rủi ro bằng mã).

#### Bảo vệ Mạng và Dữ liệu
- Chia sẻ VPC Security Group.
- Bảo mật cho các dịch vụ dựa trên API.
- Quản lý bí mật (Secret Management).

#### Ứng phó sự cố
- **Phòng ngừa:** Tại sao không ai có đủ thời gian để xử lý sự cố thủ công.
- **Sức khỏe tinh thần:** Hướng dẫn "ngủ ngon hơn" bằng cách giảm thiểu mệt mỏi vì cảnh báo (alert fatigue).
- **Quy trình:** Xây dựng quy trình ứng phó sự cố chuẩn.

### Bài học chính (Key Takeaways)

#### Service Control Policies (SCPs)
- **Định nghĩa:** Một loại chính sách cấp Tổ chức (Organization).
- **Chức năng:** Kiểm soát các quyền hạn *tối đa* có sẵn cho tất cả các tài khoản trong tổ chức.
- **Nguyên tắc:** SCP không bao giờ *cấp* quyền; chúng chỉ có thể *lọc* hoặc giới hạn quyền.

#### Permission Boundaries (Ranh giới quyền hạn)
- **Mục đích:** Tính năng IAM nâng cao được thiết kế để giải quyết vấn đề ủy quyền.
- **Chức năng:** Đặt ra quyền hạn tối đa mà một chính sách dựa trên định danh (identity-based policy) có thể cấp cho một User hoặc Role cụ thể.

#### Xác thực đa yếu tố (MFA)
- **TOTP:** Dựa trên bí mật chia sẻ, yêu cầu nhập mã 6 chữ số thủ công (ví dụ: Google Authenticator). Miễn phí, sao lưu và khôi phục linh hoạt.
- **FIDO2:** Sử dụng mật mã khóa công khai, yêu cầu quét sinh trắc học hoặc chạm đơn giản (ví dụ: YubiKey). Bảo mật cao nhưng yêu cầu quy trình sao lưu nghiêm ngặt.

#### Cảnh báo & Tự động hóa với EventBridge
- **Sự kiện thời gian thực:** Các sự kiện CloudTrail được chuyển đến EventBridge để xử lý ngay lập tức.
- **Cảnh báo tự động:** Phát hiện các hoạt động đáng ngờ trên tất cả các tài khoản của tổ chức.
- **Định tuyến sự kiện liên tài khoản (Cross-account):** Xử lý sự kiện tập trung và phản hồi tự động.
- **Tích hợp & Quy trình:** Tích hợp với SNS, Slack và SQS để phản hồi bảo mật tự động và thông báo cho đội ngũ.

#### Detection-as-Code
- **Triển khai IaC:** Triển khai GuardDuty trên toàn tổ chức bằng CloudFormation/Terraform (bật gói bảo vệ, cấu hình nguồn dữ liệu).
- **Quy tắc phát hiện tùy chỉnh:** Xây dựng các quy tắc loại trừ (suppression rules) & danh sách trắng IP để giảm dương tính giả (false positives).
- **Logic được kiểm soát phiên bản:** Theo dõi các quy tắc phát hiện trong Git và tích hợp vào quy trình DevSecOps để kiểm thử và triển khai.

#### Ứng phó sự cố
- **Chuẩn bị:** Chuẩn bị sẵn các trình xử lý tự động (automation handlers) cho sự cố.
- **Dự đoán:** Dự đoán các sự cố tương lai và thiết kế kế hoạch ứng phó.
- **Hậu sự cố:** Rút ra "Bài học kinh nghiệm" (Lessons Learned) sau mỗi sự cố.

### Áp dụng vào công việc

- **Đặc quyền tối thiểu:** Quy định và thực thi chính sách đặc quyền tối thiểu (least privilege) cho dự án.
- **Bắt buộc MFA:** Áp dụng MFA cho mọi tài khoản (Root và IAM user).
- **Lập kế hoạch:** Dự đoán và chuẩn bị sẵn sàng cho các sự cố trong tương lai.

### Trải nghiệm sự kiện

Tham dự hội thảo **“AWS Well-Architected Security Pillar”** đã giúp tôi cải thiện đáng kể kiến thức về bảo mật và ứng phó sự cố thông qua việc trải nghiệm các Trụ cột Bảo mật AWS, bao gồm:

#### Học hỏi từ các diễn giả có chuyên môn cao
- Học cách các Senior Engineer xử lý khi sự cố xảy ra và quy trình rút kinh nghiệm sau đó.
- Học cách bảo vệ dữ liệu và mạng lưới với các tính năng bảo mật của AWS.

#### Khám phá hoạt động Cloud Club
- Làm quen với các hoạt động của Cloud Club giúp kết nối cộng đồng người học AWS từ khắp mọi nơi.

#### Cảnh báo & Tự động hóa
- Có khả năng chuẩn bị cơ sở hạ tầng như CloudTrail, EventBridge, CloudWatch để quản lý tài nguyên trong thời gian thực ngay khi sự cố phát sinh.



> **Tổng kết:** Sự kiện là cơ hội để tôi mở rộng kiến thức về Cảnh báo, Tự động hóa, Bảo mật và Ứng phó sự cố. Tôi đã tích lũy được nhiều kinh nghiệm thông qua việc lắng nghe các Senior Cloud Engineer chia sẻ về công việc thực tế của họ.