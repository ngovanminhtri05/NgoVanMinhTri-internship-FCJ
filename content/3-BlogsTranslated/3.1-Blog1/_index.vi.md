---
title: "Đại học California Irvine Sao lưu Petabyte Dữ liệu Nghiên cứu lên AWS"
date: 2025-05-29
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Đại học California Irvine Sao lưu Petabyte Dữ liệu Nghiên cứu lên AWS

**Bởi Philip Papadopoulos, Abhijeet Lokhande, Evan Wood, Francisco Ramon Lopez, và Nicholas Santucci | 29 THÁNG 5 NĂM 2025**

**Danh mục:** Amazon Athena, Amazon CloudWatch, Amazon DynamoDB, Amazon EventBridge, Amazon S3 Glacier Deep Archive, Amazon Simple Notification Service (SNS), Amazon Simple Storage Service (S3), AWS Lambda

---

## Ghi chú của Biên tập viên

AWS không chịu trách nhiệm về kho lưu trữ GitHub công cộng của UCI được liên kết trong bài đăng này, được cung cấp để các bên quan tâm có thể khám phá giải pháp được mô tả trong bài đăng này chi tiết hơn.

---

Đại học California, Irvine (UCI) là một trường đại học nghiên cứu công lập có rất nhiều dữ liệu nghiên cứu được lưu trữ trên máy chủ trong môi trường lab trên khoảng 1500 môi trường lab-nghiên cứu của giáo sư trên khuôn viên. UCI cần một giải pháp để giải quyết thách thức thực tế và kinh tế của việc cung cấp sao lưu tập trung cho các máy chủ được quản lý độc lập này.

Mục tiêu của Trung tâm Cơ sở hạ tầng Siber Nghiên cứu UCI (RCIC) là cung cấp các công cụ và dung lượng lưu trữ sao lưu mà chủ sở hữu máy chủ cần để thực hiện sao lưu đồng thời cung cấp bảo vệ dữ liệu tốt hơn trong trường hợp xóa, cố ý hay vô tình. Trung tâm RCIC của UCI đã khảo sát các giải pháp sao lưu hiện có được triển khai độc lập xung quanh khuôn viên và không tìm thấy giải pháp nào đáp ứng các yêu cầu chi phí, phạm vi quy mô động và thích ứng với thực tế của quản trị hệ thống tập trung chứ không phải phân tán. Các hệ thống ngoài lề hiện có có thể có tới Petabyte trên một máy chủ duy nhất và hơn một tỷ tệp. UCI có khoảng 100 máy chủ có dung lượng lưu trữ ước tính tổng cộng là 10 PB cần được sao lưu.

Trong bài đăng này, chúng tôi hướng dẫn bạn qua các lựa chọn thiết kế cho giải pháp sao lưu tập trung của UCI, bao gồm cách các nhóm S3, vai trò AWS Identity and Access Management (IAM) và tài khoản AWS được tạo theo chương trình cho mỗi máy chủ để cung cấp sự cách ly thích hợp. Chúng tôi cũng sẽ cung cấp tổng quan về cách các yêu cầu cụ thể được ánh xạ tới các dịch vụ AWS và lệnh gọi rclone. Với giải pháp này, UCI đã có thể sao lưu hơn 5 PB dữ liệu (hơn 250 triệu tệp) với một hệ thống hiệu quả, có thể mở rộng được với lượng mã nhỏ gọn và dễ bảo trì.

---

## Tổng quan Giải pháp

Trung tâm RCIC của UCI đã phát triển một giải pháp sao lưu có thể mở rộng để đáp ứng các nhu cầu đa dạng của 1500 phòng lab nghiên cứu của giáo sư trên khuôn viên. Khung thực hiện tập trung vào một hệ thống được phát triển tùy chỉnh sử dụng [rclone](https://rclone.org/) để di chuyển dữ liệu trong khi kết hợp nhiều dịch vụ AWS để bảo mật và quản lý dữ liệu. Giải pháp này được thiết kế đặc biệt để xử lý sự khác biệt lớn trong kích thước máy chủ trên khuôn viên, từ 1 TB đến 2 PB, với yêu cầu lưu trữ tổng cộng khoảng 10 PB trên 100 máy chủ.

Kiến trúc thực hiện thiết lập ranh giới rõ ràng giữa vai trò quản lý hệ thống và quản lý đám mây, tăng cường bảo mật thông qua tách biệt nhiệm vụ. Các máy chủ tại chỗ (Lab1 và LabN) sử dụng rclone để thực hiện sao lưu hàng ngày tới các nhóm S3. Các nhóm S3 được định cấu hình với các chính sách Vòng đời S3 di chuyển dữ liệu tới S3 Glacier Deep Archive để lưu trữ dài hạn. Hệ thống được giám sát bằng Amazon CloudWatch với bảng điều khiển tùy chỉnh và cảnh báo, trong khi thông báo được xử lý thông qua Amazon Simple Notification Service (Amazon SNS). Các dịch vụ AWS bổ sung và tính năng như Amazon DynamoDB, AWS Lambda, AWS Step Functions, S3 Batch Operations và Amazon EventBridge được sử dụng để kiểm soát quyền truy cập và mô tả chính sách.

### Sơ đồ Kiến trúc

![Hình 1: Khung nhìn cấp cao về sao lưu tới Amazon S3 bằng rclone. Một loạt các dịch vụ AWS được sử dụng để kiểm soát quyền truy cập và mô tả chính sách. Một trình bao quanh Python3 tùy chỉnh xung quanh công cụ rclone mã nguồn mở hợp lý hóa mô tả các công việc sao lưu.](https://d2908q01vomqb2.cloudfront.net/e1822db470e60d090affd0956d743cb0e7cdf113/2025/05/29/UCI-Done-Page-1.png)

**Hình 1:** Khung nhìn cấp cao về sao lưu tới Amazon S3 bằng rclone. Một loạt các dịch vụ AWS được sử dụng để kiểm soát quyền truy cập và mô tả chính sách. Một trình bao quanh Python3 tùy chỉnh xung quanh công cụ rclone mã nguồn mở hợp lý hóa mô tả các công việc sao lưu.

---

### Các Thành phần Chính

Để duy trì hiệu quả chi phí đồng thời đảm bảo bảo vệ dữ liệu, Trung tâm RCIC của UCI đã phát triển một trình bao quanh Python tùy chỉnh hợp lý hóa các định nghĩa công việc sao lưu và thực hiện. Trình bao quanh này giao diện với các dịch vụ AWS bao gồm S3, IAM, Amazon CloudWatch và Amazon SNS để cung cấp khả năng giám sát và quản lý toàn diện.

![Hình 2: Một trình bao quanh Python tùy chỉnh xung quanh công cụ rclone mã nguồn mở hợp lý hóa mô tả các công việc sao lưu](https://d2908q01vomqb2.cloudfront.net/e1822db470e60d090affd0956d743cb0e7cdf113/2025/05/29/Figure-2-A-custom-Python-wrapper-around-the-open-source-tool-rclone-streamlines-the-description-of-backup-jobs.png)

**Hình 2:** Một trình bao quanh Python tùy chỉnh xung quanh công cụ rclone mã nguồn mở hợp lý hóa mô tả các công việc sao lưu

Giải pháp kết hợp ba thành phần hoạt động chính:
- **Sao lưu tăng hàng ngày** để bảo vệ dữ liệu hiệu quả
- **Đồng bộ sâu hàng tuần** để xác minh toàn diện
- **Quản lý vòng đời tự động** để tối ưu hóa chi phí

Việc triển khai cách tiếp cận phân tầng này cho phép UCI cân bằng thành công các yêu cầu cạnh tranh về hiệu suất, chi phí và bảo mật dữ liệu trong khi duy trì khả năng sẵn có của hệ thống cho các hoạt động nghiên cứu.

### Ma trận Thực hiện Tính năng

| Tính năng/Khả năng | Công nghệ/Dịch vụ | Kiểm soát |
| --- | --- | --- |
| Di chuyển dữ liệu/sao lưu | rclone | sysadmin |
| Định nghĩa công việc sao lưu | Tệp yaml tùy chỉnh | sysadmin |
| Thực hiện công việc sao lưu | Mã được phát triển bởi UCI bao quanh rclone | sysadmin (trên GitHub) |
| Lập lịch công việc sao lưu | Unix Cron, Windows Scheduled tasks | sysadmin |
| Xóa tệp trên sao lưu | rclone chạy ở chế độ đồng bộ | sysadmin |
| Bảo vệ ghi đè/xóa tệp | S3 Versioning | cloudadmin |
| Xóa vĩnh viễn bản sao lưu | Chính sách Vòng đời S3 | cloudadmin |
| Xóa sớm dữ liệu | S3 Admin – Yêu cầu MFA | cloudadmin |
| Cung cấp đích sao lưu | Tập lệnh tùy chỉnh nhắm mục tiêu S3 và áp dụng chính sách | cloudadmin |
| Hạn chế mạng IP | Chính sách quyền truy cập IAM | cloudadmin |
| Tổng quan và chế độ xem từng máy chủ | Bảng điều khiển Cloudwatch | cloudadmin |
| Hạn ngạch và cảnh báo hoạt động | Cảnh báo Cloudwatch | cloudadmin |
| Khôi phục tệp | S3 Glacier restore + python tùy chỉnh | cloudadmin/sysadmin |

---

## Phương pháp Sao lưu Tập trung

### A. Di chuyển Dữ liệu Cơ bản

#### Tại sao chọn rclone?

Rclone cung cấp hỗ trợ cho cả lưu trữ Amazon S3 và hệ thống tệp cục bộ, với xử lý tích hợp của versioning đối tượng S3 và tích hợp trực tiếp với các API lưu trữ đám mây. Công cụ này sử dụng phương pháp khám phá hệ thống tệp tăng dần, cho phép xử lý hiệu quả bộ nhớ của các hệ thống tệp lớn, được xác minh với các hệ thống chứa hơn 50 triệu tệp. Khi rclone cần đồng bộ hóa nội dung của hệ thống tệp cục bộ với bản sao trong Amazon S3, nó khám phá hệ thống tệp cục bộ theo cách tăng dần. UCI có các công việc sao lưu duy nhất với hơn 50 triệu tệp và rclone hoàn toàn khám phá hệ thống tệp mà không làm cạn kiệt bộ nhớ hệ thống.

Rclone cũng có đa luồng natively. Ví dụ, khi thực hiện "đầy đủ" sao lưu đầu tiên của máy chủ tệp 1 PB, UCI đã "giảm lại" rclone để nó không làm bão hòa liên kết 10 GbE và máy chủ tệp vẫn có khả năng phục vụ tệp. Tóm lại, nó có hiệu suất nhưng có thể được quản lý để nó không làm hệ thống không khả dụng.

UCI đã phát triển một tập lệnh Python nhỏ (gen-backup.py trong kho lưu trữ GitHub công cộng của UCI) bao quanh Rclone. Mã này diễn giải một tệp được định dạng YAML xác định một hoặc nhiều công việc sao lưu.

#### Ví dụ Định nghĩa Công việc Sao lưu

```yaml
## Đường dẫn là thông thường cho các công việc liên quan đến đường dẫn
---
srcpaths:
- path: /datadir
  ## Quyết định cục bộ loại trừ thư mục con .git
  exclude_global:
    - .git/**

  ## Mẫu từ một tệp để loại trừ
  exclude_file: common_excludes.yaml

  jobs:
    - name: backup1
      subdirectories:
        - DataImages
```

Nói tóm lại, công việc có tên "backup1" sao chép tất cả các tệp trong đường dẫn `/datadir/DataImages` và loại trừ các mẫu tệp thông thường. Để chạy tất cả các công việc sao lưu, chỉ cần phát hành lệnh sau:

```bash
gen-backup.py run
```

Có rất nhiều tùy chọn cho gen-backup để quản lý tính song song, công việc nào được chạy và ở chế độ nào chúng được chạy.

#### Các Bước Thực hiện Chuyển dữ liệu

1. **Cấu hình:** UCI đã cấu hình các hồ sơ sao lưu riêng lẻ cho mỗi máy chủ lab nghiên cứu
2. **Định nghĩa công việc:** Tạo các mẫu YAML được chuẩn hóa để định nghĩa sao lưu nhất quán
3. **Thực hiện:** Triển khai các quy trình sao lưu tự động thông qua trình bao quanh tùy chỉnh
4. **Giám sát:** Thiết lập các giao thức giám sát dành riêng cho lab
5. **Tối ưu hóa:** Phát triển các điều khiển tốc độ truyền dựa trên dung lượng máy chủ
6. **Lập lịch:** Tạo các lịch biểu sao lưu dành riêng cho lab dựa trên tỷ lệ thay đổi dữ liệu

Phương pháp có cấu trúc này đảm bảo các hoạt động sao lưu nhất quán, có thể tái tạo được trong khi duy trì hiệu suất và độ tin cậy của hệ thống trong các tham số hoạt động được xác định.

### B. Hạn ngạch Lưu trữ cho Lab

Đội IT trung tâm của khuôn viên quản lý chi phí sao lưu, đòi hỏi thực hiện các điều khiển cụ thể để ngăn chặn vượt quá chi phí. Để đạt được điều này, các yếu tố sau cần được hiểu cho mỗi máy chủ:

- Có bao nhiêu dữ liệu về khối lượng (tính bằng terabyte) được lưu trữ?
- Có bao nhiêu tệp trong hệ thống (tính bằng gia tăng 1 triệu đối tượng)?
- Liệu quyền truy cập vào nhóm S3 sao lưu có vẻ "lạm dụng" (quá nhiều giao dịch)?
- Một hệ thống đã không thực hiện bất kỳ lệnh gọi API nào tới nhóm (sao lưu có thể bị tắt trên máy chủ)?

Mỗi một trong các gạch đầu dòng này được cấu hình dưới dạng cảnh báo CloudWatch. Cả cloudadmin và sysadmin (các) phù hợp được thông báo bất cứ khi nào bất kỳ cảnh báo nào trong số này được kích hoạt bằng Amazon SNS. Có một bảng điều khiển cho phép cloudadmins xem trạng thái của các cảnh báo này cho tất cả các máy chủ đang được sao lưu.

![Hình 4: Cảnh báo được thiết lập cho mỗi máy chủ lưu trữ: giám sát hạn ngạch lưu trữ và đối tượng cũng như hoạt động](https://d2908q01vomqb2.cloudfront.net/e1822db470e60d090affd0956d743cb0e7cdf113/2025/05/23/image-3-6.png)

**Hình 4:** Cảnh báo được thiết lập cho mỗi máy chủ lưu trữ: giám sát hạn ngạch lưu trữ và đối tượng cũng như hoạt động

Một cảnh báo như vậy theo dõi các lệnh gọi API, được biểu thị dưới dạng tỷ lệ của yêu cầu tới các đối tượng. Nếu tỷ lệ này vượt quá 3,5, cảnh báo sẽ được kích hoạt. 3,5 được chọn làm đệm kỹ thuật để không báo động trong hầu hết các tình huống hoạt động.

Trong thời gian gần một năm, các cảnh báo này đã chứng minh hữu ích để báo động UCI về các trạng thái khác nhau - đặc biệt là trên các giới hạn terabyte và số lượng đối tượng.

### C. Giám sát và Tính Chất Hiển thị

Hệ thống giám sát cần phải thích ứng với 50-100 máy chủ hoạt động theo lịch biểu sao lưu độc lập. Với các hệ thống cuối được quản lý bởi các quản trị viên khác nhau và không có liên kết, chỉ có dữ liệu trong AWS có thể được sử dụng làm điểm tập hợp trung tâm. Để kiểm soát chi phí, UCI quan tâm nhất đến các mức trung bình. Một số máy chủ có thể có nhiều tệp nhỏ hoặc thay đổi dữ liệu đáng kể (dẫn đến tỷ lệ phần trăm cao hơn các đối tượng không hiện tại) và do đó "đắt hơn mỗi terabyte". Những tệp này được cân bằng bởi các máy chủ khác có các tệp tương đối lớn và/hoặc thay đổi dữ liệu thấp và do đó "rẻ hơn mỗi terabyte".

UCI đã xây dựng một bảng điều khiển tùy chỉnh trong CloudWatch kết hợp các số liệu từ tất cả các nhóm sao lưu, tách biệt các lệnh gọi API khác nhau để ước tính chi phí liên tục. Tiện ích hàng đầu hiển thị tổng dung lượng lưu trữ 2780 TB trên 186 triệu tệp. Trong tổng dung lượng lưu trữ này là 2690 TB trong S3 Glacier Deep Archive và 454 TB được giữ dưới dạng tệp đã xóa hoặc ghi đè mà chưa được xóa vĩnh viễn thông qua chính sách Vòng đời S3. Hình cuối cùng cho biết chi phí chung là 16,3% để giữ lại các đối tượng đã xóa/thay đổi.

![Hình 5: Bảng điều khiển Ước tính Chi phí hiển thị chi tiết về phân tích lưu trữ, hoạt động API và chi phí lưu trữ với cấu hình Glacier Deep Archive](https://d2908q01vomqb2.cloudfront.net/e1822db470e60d090affd0956d743cb0e7cdf113/2025/05/23/image-4-6.png)

**Hình 5:** Tổng thể lưu trữ, API và sử dụng chi phí trên tất cả các máy chủ

Các bất thường hàng tuần trên biểu đồ API đến từ việc đồng bộ sâu hàng tuần. Một kiểm tra cẩn thận cũng cho thấy tăng lên của lưu trữ tiêu chuẩn 2 TB ở đầu cửa sổ giám sát và 88,1 TB ở cuối. Điều này được giải thích bằng sự onboarding của máy chủ 1 PB mới.

Phần chiếm ưu thế của hóa đơn sao lưu hàng tháng của UCI đến từ chi phí của chính lưu trữ, chiếm khoảng 80% chi phí hàng tháng. Phân tích hóa đơn tháng 7 năm 2024 cho thấy:
- 80% lưu trữ
- 1,5% CloudWatch
- 18,5% API

Chi phí bổ sung không được hiển thị có tầm quan trọng là chi phí chuyển đổi S3 Lifecycle khi dữ liệu được di chuyển từ Amazon S3 Standard (nơi nó được đặt ban đầu) tới S3 Glacier Deep Archive.

#### Giám sát Per-Server

Bảng điều khiển tổng quan là hữu ích, nhưng các chế độ xem cấp cao cho mỗi máy chủ cũng rất quan trọng để hiểu rõ. Bảng điều khiển ước tính chi phí thứ hai được cấu hình để có được thông tin chi tiết này. Các hệ thống tại UCI có khả năng từ 11 TB đến 1040 TB và số lượng đối tượng trong phạm vi 652.000 đến 59,4 triệu. Bảng điều khiển này sao chép thông tin dòng trên cùng của chế độ xem tổng hợp, nhưng nó được điều chỉnh cho từng hệ thống.

**Hình 6:** Sử dụng lưu trữ và chi phí per-server

Tất cả các bảng điều khiển hiển thị được xây dựng theo chương trình để chúng có thể được sao chép dễ dàng trong môi trường khác.

### D. Sao lưu Có Hiệu suất

Khi xử lý các máy chủ dữ liệu lớn, khối lượng (terabyte) và số lượng tệp (số lượng đối tượng) là các yếu tố quan trọng ảnh hưởng đến hiệu suất. Có lẽ dễ nhất để minh họa các độ lớn này với một số ví dụ:

- 1024 TB @ 1 gbps – 10 triệu giây để truyền (100 Ngày)
- 1024 TB @ 10 gbps – 1 triệu giây để truyền (10 ngày)
- Đồng bộ hóa 50 triệu tệp ở 100 checks/second = 500.000 giây (5,8 ngày)
- Đồng bộ hóa 50 triệu tệp ở 1000 checks/second = 50.000 giây (13,8 giờ)

Để sao lưu lên đám mây thực tế trên các máy chủ 1 PB, bản sao đầu tiên của dữ liệu cần phải di chuyển với tốc độ vượt quá 1 gbps. Khi đánh giá phần mềm hiện có, rõ ràng là bất kỳ chiến lược sao lưu nào yêu cầu "đầy đủ" sao lưu thường xuyên của các loại máy chủ này là không thực tế. Mặc dù tư nhân hóa một sao lưu lớn có thể mất thời gian đáng kể (hàng tuần trong các trường hợp cực đoan) và không thể tránh khỏi, sao lưu liên tục cần phải "mãi mãi tăng dần".

Số liệu thứ hai (checks/sec) không quá rõ ràng thoạt đầu. Để đồng bộ hóa nội dung của hai "hệ thống" tệp, phần mềm sao lưu phải xác minh rằng mọi thứ cục bộ nằm trên bản sao lưu. Hơn nữa, mọi thứ đã bị xóa từ máy cục bộ cũng phải bị xóa từ bản sao lưu. Rclone thực hiện điều này với "những người kiểm tra" kiểm tra xem các bản sao cục bộ và đám mây có đồng bộ hóa hay không. Vì mỗi lần kiểm tra là một lệnh gọi API trên mạng có hàng chục mili giây trên mỗi lệnh gọi, nhiều kiểm tra như vậy phải xảy ra song song. Rclone làm điều này native và UCI thường xuyên quan sát khoảng 2000 checks/second.

Ngoài ra, một tổ chức có thể giữ một cơ sở dữ liệu cục bộ của trạng thái của từng tệp và giảm "đồng bộ hóa" thường xuyên. Đó là sự đánh đổi giữa không gian (một cơ sở dữ liệu cục bộ) và thời gian (đồng bộ hóa sâu hàng tuần). Nhiều chương trình sao lưu thương mại làm điều này (và phải luôn có cách thiết lập lại sự thật nếu cơ sở dữ liệu cục bộ bị hỏng). Quản lý cơ sở dữ liệu như vậy làm tăng độ phức tạp.

### E. Chi phí Thấp

Tối ưu hóa chi phí là một yêu cầu chính, yêu cầu phân tích cẩn thận về sử dụng tính năng AWS để kiểm soát chi phí. UCI phải hiểu một số kiến thức cơ bản về cách rclone giao diện với Amazon S3, sau đó thực hiện phép nhân. Sau khi kiểm tra các vấn đề hiệu suất thực tế, UCI đã chọn "đồng bộ sâu" hàng tuần của các hệ thống tệp. Các đồng bộ sâu dẫn đến một số lệnh gọi API lớn và tác động đến hệ thống tệp cục bộ. Rclone có một chế độ được gọi là đồng bộ "top up". Nó chỉ xem xét các tệp mới hoặc mới được viết để so sánh với bản sao đám mây. Các hệ thống chạy top-ups sáu ngày/tuần và một đồng bộ sâu một ngày/tuần.

### F. Xoay Mật khẩu và Phát hiện Non-Running

Tập lệnh sao lưu (gen-backup.py) phải có thể chạy không được giám sát từ một bộ lập lịch nhiệm vụ (ví dụ cron trên các hệ thống giống Linux, Task Scheduler trên Windows). Để làm điều này, rclone phải có quyền truy cập vào các thông tin xác thực hợp lệ. Điều này cung cấp một phần của một đặc vụ kỹ thuật: các khóa tạm thời (dài hạn) có thể hết hạn trước khi sao lưu hoàn tất trong khi thông tin xác thực dài hạn tạo ra những rủi ro của riêng chúng. Rclone tới S3 để sao lưu lớn (RCS3) "chia giữa": thông tin xác thực (cụ thể cho quá trình sao lưu chính nó) là dài hạn nhưng hết hạn sau khi hoàn tất mỗi lệnh gọi thành công của chương trình sao lưu (gen-backup.py). Theo hoạt động bình thường, điều này xảy ra hàng ngày.

Khi một nhóm được thiết lập để sao lưu, một tài khoản dịch vụ (người dùng sao lưu) được tạo và tài khoản này phải có đặc quyền tạo một cặp khóa truy cập mới/khóa truy cập bí mật chỉ cho chính nó. Sự thỏa hiệp này hoạt động rất tốt. Khi rclone bắt đầu, khóa đang được sử dụng không hết hạn trong khi sao lưu đang diễn ra, do đó ngăn chặn những lần khởi động lại sao lưu không cần thiết. Khi hoàn tất, khóa vừa được sử dụng hết hạn và một khóa mới được tạo. Trong hoạt động bình thường, khóa có tuổi thọ là 24 giờ.

Một người có thể sử dụng thực tế rằng hoạt động bình thường sẽ thấy một cập nhật khóa mỗi ngày. Điều này kích hoạt cảnh báo nếu một khóa cụ thể có tuổi hơn 48 giờ. Điều này cho biết rằng: một sao lưu mất lâu hơn hoặc một sao lưu không đi tới bước xoay khóa. Trường hợp sau là luôn luôn một vấn đề. Nguyên nhân gốc của cảnh báo đòi hỏi sysadmin phải điều tra.

---

## Kết luận

Giải pháp sao lưu toàn diện được phát triển bởi Trung tâm RCIC của UCI chứng minh cách các dịch vụ AWS có thể được tận dụng hiệu quả để giải quyết thách thức phức tạp của việc sao lưu lượng dữ liệu khổng lồ nghiên cứu trên nhiều phòng lab của giáo sư. Thông qua việc thực hiện chiến lược của rclone, các trình bao quanh Python tùy chỉnh và các dịch vụ AWS khác nhau bao gồm S3, IAM, CloudWatch và SNS, UCI đã tạo thành công một hệ thống có thể mở rộng có khả năng sao lưu hơn 5 PB dữ liệu bao gồm hơn 250 triệu tệp.

Kiến trúc giải pháp cân bằng hiệu quả các yêu cầu quan trọng bao gồm hiệu quả chi phí, tối ưu hóa hiệu suất, các điều khiển bảo mật và khả năng giám sát trong khi duy trì một ranh giới rõ ràng giữa vai trò quản lý hệ thống và quản lý đám mây.

### Lợi ích Chính

- **Tiết kiệm Chi phí:** Phân tầng lưu trữ thông minh và các chính sách vòng đời
- **Bảo mật:** Tách biệt nhiệm vụ và kiểm soát quyền truy cập nghiêm ngặt
- **Giám sát:** Khả năng toàn diện với cả cái nhìn tổng quan cấp cao và chi tiết per-server
- **Khả năng mở rộng:** Xử lý các máy chủ từ 1 TB đến 2 PB trong khi duy trì hiệu suất hệ thống

Đối với các tổ chức muốn thực hiện một giải pháp tương tự, cách tiếp cận của UCI chứng minh rằng với kế hoạch cẩn thận và kết hợp đúng đắn các công cụ và dịch vụ, ngay cả những yêu cầu sao lưu đòi hỏi nhất cũng có thể được đáp ứng hiệu quả.

### Tài nguyên Được Khuyến nghị

- [Kho lưu trữ GitHub công cộng của UCI](https://github.com/RCIC-UCI-Public/rcs3)
- [Tài liệu RCS3](https://rcs3.readthedocs.io/en/latest/about/index.html)

Tài liệu cung cấp tổng quan về hai bên quản lý của sao lưu và những gì cần thiết về phần mềm (Python, Thư viện Boto3 và rclone), khởi tạo môi trường đám mây cho sao lưu, onboarding các máy chủ, tạo các công việc sao lưu, xác định hạn ngạch và cập nhật bảng điều khiển.

### Lộ trình Tương lai

RCS3 đã chứng minh hiệu quả của việc quản lý các sao lưu quy mô Petabyte trong khi duy trì hiệu quả chi phí. Danh sách việc cần làm gần hạn của chúng tôi bao gồm:

- Các hoạt động khôi phục không được hỗ trợ (không có can thiệp cloudadmin)
- Tài liệu về các tùy chọn công việc sao lưu khác nhau có thể giải quyết các vấn đề phổ biến
- Chi tiết về thiết lập tất cả các biến thể hệ thống đã gặp cho đến nay

---

## Về các Tác giả

### Philip Papadopoulos
Philip Papadopoulos là Giám đốc Trung tâm Cơ sở hạ tầng Siber Nghiên cứu (RCIC) tại Đại học California, Irvine (UCI). RCIC phục vụ cộng đồng nghiên cứu của khuôn viên thông qua một cụm máy tính quy mô trung bình (khoảng 11K lõi và 150 GPU) và khoảng 10 PB lưu trữ song song. Tiến sĩ P. bắt đầu làm việc trong cộng đồng máy tính song song/phân tán tại Phòng thí nghiệm Quốc gia Oak Ridge như là một phần của PVM (tiền thân của MPI) vào cuối những năm 1990. Trước khi đến UCI, ông đã dành gần hai mươi năm tại Trung tâm Máy tính Siêu cấp San Diego xây dựng các hệ thống máy tính siêu cấp và phát triển bộ công cụ cụm Rocks.

### Abhijeet Lokhande
Abhijeet Lokhande là một Kiến trúc sư Giải pháp Cao cấp tại AWS, nơi anh ấy đóng một vai trò quan trọng trong việc trao quyền cho các công ty khởi nghiệp đổi mới nhất của Ấn Độ. Làm việc trong nhóm AWS Startups, anh ấy phục vụ như một cố vấn chiến lược, giúp các công ty mới nổi chuyển đổi các tầm nhìn tham vọng của họ thành các kiến trúc đám mây có thể mở rộng. Với chuyên môn sâu về bảo mật và tuân thủ, Abhijeet hướng dẫn các nhà sáng lập thông qua sự phức tạp của việc xây dựng các ứng dụng cấp doanh nghiệp an toàn trên AWS.

### Evan Wood
Evan là một Kiến trúc sư Giải pháp làm việc với nhóm Amazon Web Services (AWS) Công lập Toàn cầu. Anh ấy làm việc với Bộ Năng lượng trong không gian Dân sự Liên bang, giúp họ tăng tốc Hành trình đám mây của họ. Evan chuyên về triển khai IoT và cơ sở dữ liệu.

### Francisco Ramon Lopez
Francisco là một chuyên gia CNTT làm việc trong Trung tâm Cơ sở hạ tầng Siber Nghiên cứu (RCIC) tại Đại học California, Irvine. Anh ấy có hơn 25 năm kinh nghiệm với các hệ thống UNIX và bắt đầu làm việc với AWS vào năm 2015.

### Nicholas Santucci
Nick Santucci là một Kỹ sư Hệ thống Máy tính Hiệu suất Cao tại Trung tâm Cơ sở hạ tầng Siber Nghiên cứu (RCIC) của UC Irvine, thuộc phòng Công nghệ Dữ liệu và Thông tin (ODIT). Anh ấy chuyên về tích hợp phần mềm, máy tính cụm và lưu trữ hiệu suất cao, hỗ trợ sứ mệnh của RCIC cung cấp các tài nguyên máy tính và lưu trữ có thể mở rộng cho cộng đồng nghiên cứu của khuôn viên. Trước khi tham gia UC Irvine để làm việc trong máy tính hiệu suất cao/máy tính nghiên cứu, anh ấy đã dành một thập kỷ tại DIRECTV, nơi anh ấy phát triển và di chuyển các hệ thống giám sát tự động cho các hoạt động phát sóng, dịch chuyển thực hành từ giám sát dựa trên công cụ sang phân tích IT dựa trên dữ liệu. Nick có bằng cấp về thạc sĩ về khoa học thông tin và máy tính từ UC Irvine, có chuyên môn về mạng và hệ thống phân tán.

---

**Tags:** Amazon Athena, Amazon CloudWatch, Amazon DynamoDB, Amazon EventBridge, Amazon S3 Batch Operations, Amazon S3 Glacier Storage Classes, Amazon Simple Storage Service (Amazon S3), Amazon SNS, AWS Cloud Storage, AWS Lambda
