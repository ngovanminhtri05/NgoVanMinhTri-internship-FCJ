---
title: "Sự kiện 1"
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Báo cáo tóm tắt: "AWS Cloud Mastery Series #1: AI/ML/GenAI trên AWS"

### Mục tiêu sự kiện

- **Giới thiệu về Foundation Models:** Hiểu các khái niệm cốt lõi của Mô hình Nền tảng (FM).
- **Kỹ thuật Prompt Engineering:** Hướng dẫn các kỹ thuật đặt câu lệnh (prompt) hiệu quả.
- **Khám phá Amazon Bedrock:** Đi sâu vào Generative AI sử dụng Amazon Bedrock.
- **Kiến trúc RAG:** Tìm hiểu về Retrieval-Augmented Generation (Thế hệ tăng cường truy xuất) và tích hợp Knowledge Base.

### Diễn giả

- Danh Hoàng Hiếu Nghị
- Lâm Trường Kiệt
- Đinh Lê Hoàng Anh

### Điểm nhấn chính

#### Kỹ thuật Prompting
- Các chiến lược để tạo ra những câu lệnh (prompts) hiệu quả nhằm mang lại kết quả chính xác và phù hợp với ngữ cảnh hơn.

#### RAG (Retrieval-Augmented Generation)
- Nâng cao năng lực của AI bằng cách nhúng các nguồn dữ liệu bên ngoài.
- Cho phép mô hình phản hồi với thông tin chính xác, thời gian thực được trích xuất từ dữ liệu nội bộ/độc quyền.

#### Embedding (Mô hình nhúng)
- **Định nghĩa:** Sự biểu diễn văn bản dưới dạng số (vectơ) giúp nắm bắt ngữ nghĩa và mối quan hệ giữa các từ.
- **Chức năng:** Các mô hình embedding nắm bắt các đặc điểm, ngữ cảnh và sắc thái của văn bản đầu vào.
- **So sánh:** Các mô hình embedding phong phú cho phép so sánh sự tương đồng về ngữ nghĩa của văn bản.
- **Hỗ trợ đa ngôn ngữ:** Có khả năng xác định ý nghĩa và mối quan hệ giữa các ngôn ngữ khác nhau.

#### Các dịch vụ AWS AI nổi bật
- **Phân tích hình ảnh & tài liệu:** Amazon Rekognition, Amazon Textract, Amazon Lookout.
- **Ngôn ngữ & Giọng nói:** Amazon Translate, Amazon Transcribe, Amazon Polly, Amazon Comprehend.
- **Tìm kiếm & Dữ liệu:** Amazon Kendra, Amazon Personalize.

### Bài học chính (Key Takeaways)

#### Kỹ thuật Prompting - Chain of Thought (Chuỗi suy luận)
- **Ví dụ theo ngữ cảnh:** Cung cấp cho AI các ví dụ trường hợp cụ thể (few-shot prompting).
- **Logic từng bước:** Hướng dẫn AI chia nhỏ quy trình giải quyết vấn đề thành các bước tuần tự để cải thiện khả năng suy luận.

#### Các trường hợp sử dụng RAG
- **Độ chính xác:** Giảm thiểu "ảo giác" (hallucinations) bằng cách căn cứ phản hồi vào dữ liệu doanh nghiệp đã được xác minh và cập nhật.
- **Nâng cao Chatbot:** Cải thiện khả năng của chatbot bằng cách tích hợp quyền truy cập dữ liệu thời gian thực.
- **Cá nhân hóa:** Cho phép chức năng tìm kiếm dựa trên lịch sử người dùng và chân dung (persona) cụ thể.
- **Trích xuất dữ liệu:** Truy xuất và tóm tắt các chi tiết giao dịch cụ thể từ các tập dữ liệu lớn.

#### Amazon Titan Embeddings
- **Chức năng:** Dịch đầu vào văn bản (từ, cụm từ) thành các vectơ số.
- **Ưu điểm:** So sánh embedding tạo ra các phản hồi phù hợp và có ngữ cảnh hơn nhiều so với việc khớp từ khóa đơn thuần.
- **Thông số:**
    - **Max Tokens:** 8,000
    - **Output Vectors:** 256, 512, 1,024
    - **Hỗ trợ ngôn ngữ:** Đa ngôn ngữ (hơn 100 ngôn ngữ trong bản preview)

### Áp dụng vào công việc

- Tôi có kế hoạch nghiên cứu thêm các Dịch vụ AWS AI khác để xác định tính khả thi của chúng khi tích hợp vào các dự án trong tương lai.

### Trải nghiệm sự kiện

Tham dự hội thảo **“AI/ML/GenAI trên AWS”** là một trải nghiệm cực kỳ giá trị. Tôi đã có cơ hội tiếp thu kiến thức kỹ thuật mới và kết nối với các chuyên gia CNTT khác. Những trải nghiệm chính bao gồm:

#### Học hỏi từ các chuyên gia trong ngành
- Các chuyên gia từ FACJ đã chia sẻ những **bài học thực tiễn tốt nhất (best practices)** để triển khai AI trong môi trường thực tế (production).
- Thông qua các case study thực tế, tôi đã hiểu sâu hơn về cách áp dụng các kỹ thuật prompting nâng cao một cách hiệu quả.

#### Khám phá các dịch vụ AWS AI
- Qua phần demo của diễn giả, tôi đã nắm được cơ chế hoạt động của các dịch vụ AWS AI khác nhau và quan sát việc ứng dụng chúng trong các tình huống thực tế.

#### Hướng dẫn xây dựng Agent
- Tôi đã nhận được những thông tin thực tiễn và kiến thức nền tảng cần thiết để bắt đầu xây dựng các AI Agent.

#### Một số hình ảnh sự kiện



> **Tổng kết:** Sự kiện này đã cung cấp cho tôi lượng kiến thức đáng kể về AI, có khả năng áp dụng trực tiếp vào các dự án thực tế.