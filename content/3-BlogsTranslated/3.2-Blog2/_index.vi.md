---
title: "Hướng dẫn Làm ấm IP và Miền và Di chuyển đến Amazon SES"
date: 2025-07-03
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---


**Bởi Tyler Holmes | 03 THÁNG 7 NĂM 2025**

**Danh mục:** Amazon Simple Email Service (SES), Messaging

---

## Giới thiệu

Chuyển giao khối lượng công việc email từ nhà cung cấp dịch vụ email khác (ESP) sang [Amazon Simple Email Service (Amazon SES)](https://aws.amazon.com/ses/) có thể là một thách thức, vì mỗi khối lượng công việc có thể là duy nhất. Trong bài đăng này, chúng tôi chỉ cho bạn cách sao ấm thành công các địa chỉ IP và miền khi di chuyển đến Amazon SES. Hướng dẫn này nhằm cung cấp tổng quan toàn diện về các thực tiễn tốt nhất sao ấm IP và miền để bạn có thể thực hiện chuyển đổi của mình sang Amazon SES một cách mượt mà nhất có thể. Chúng tôi thảo luận về một số thách thức bạn có thể gặp phải và cách vượt qua những vấn đề phổ biến đó khi chuyển sang nhà cung cấp dịch vụ email mới (ESP).

---

## Hiểu về Email IP và Miền Làm ấm

Làm ấm IP và sao ấm miền là các quy trình chiến lược được thiết kế để dần dần giới thiệu một danh tính gửi mới cho các nhà cung cấp hộp thư. Một danh tính gửi mới có thể là một địa chỉ IP dành riêng, miền mới, một miền phụ của miền đó hoặc bất kỳ kết hợp nào trong số chúng. Mục tiêu cốt lõi của việc sao ấm là xây dựng danh tiếng tích cực với các nhà cung cấp hộp thư để email của bạn được gửi đến hộp thư đến thay vì được lọc vào thư mục thư rác hoặc có khả năng bị chặn không được gửi đến hộp thư.

Các nhà cung cấp hộp thư như Gmail, Yahoo và Outlook cảnh báo về việc bảo vệ người dùng của họ khỏi thư rác và nội dung độc hại. Khi bạn giới thiệu một danh tính gửi mới, các nhà cung cấp hộp thư đánh giá danh tính gửi mới một cách thận trọng. Họ đánh giá việc gửi sớm từ miền và IP để đảm bảo họ đang gửi cho người dùng của nhà cung cấp hộp thư các tin nhắn mà người dùng muốn và không tham gia vào các thực tiễn lạm dụng như thư rác hoặc lừa đảo. Sao ấm cung cấp cho các nhà cung cấp hộp thư cơ hội để quan sát các mẫu gửi, nội dung và số liệu tham gia của bạn, cho phép họ dần dần xây dựng niềm tin vào danh tính gửi mới của bạn.

Sao ấm có thể khác nhau cho mỗi tình huống. Ví dụ, bạn có thể có các IP hoàn toàn được sao ấm, nhưng nếu miền gửi của bạn là mới, bạn sẽ có thể phải sao ấm nó cũng nhưng bạn sẽ không cần phải lo lắng về sao ấm IP nhiều. Một tình huống phổ biến khác là thêm IP mới nhưng gửi với miền được thiết lập. Trong trường hợp này, IP sẽ cần sao ấm, nhưng miền chính nó đang giúp sao ấm vì nó đã có danh tiếng được thiết lập. Khi bạn có net mới IP và net mới miền, bạn sẽ phải sao ấm chúng cùng nhau. Các thực tiễn tốt nhất sao ấm mà chúng tôi nêu trong bài đăng này, chẳng hạn như bắt đầu từ từ và nhắm mục tiêu đến những người đăng ký được tham gia cao nhất, áp dụng cho tình huống của bạn.

---

## Tại sao Làm ấm là Quan trọng

Làm ấm là rất cần thiết vì nhiều lý do, mỗi lý do góp phần vào thành công tổng thể và độ tin cậy của các chiến dịch email marketing của bạn:

### 1. Xây dựng Niềm tin với Nhà cung cấp Hộp thư

Danh tiếng người gửi tích cực là quan trọng đối với khả năng gửi email. Các nhà cung cấp hộp thư sử dụng các thuật toán phức tạp để đánh giá danh tiếng của những người gửi, và sao ấm giúp họ xây dựng niềm tin vào danh tính gửi mới của bạn.

### 2. Tránh các Vấn đề về Khả năng gửi Ban đầu

Khi bạn chuyển sang một ESP mới hoặc giới thiệu một danh tính gửi mới, cảm thấy cơ bản để trải nghiệm một sự sụt giảm ban đầu trong các số liệu khả năng gửi, chẳng hạn như tỷ lệ mở thấp hơn, tỷ lệ nhấp chuột hoặc tỷ lệ nảy cao hơn. Sao ấm có thể giảm nhẹ những vấn đề này bằng cách cấp cho các nhà cung cấp hộp thư thời gian để thích ứng với các mẫu gửi mới của bạn, cho dù đó là miền mới, miền phụ hoặc cơ sở hạ tầng IP mới.

### 3. Duy trì Hành vi gửi Nhất quán

Sao ấm khuyến khích bạn duy trì một nhịp độ gửi ổn định và có thể dự đoán được. Những thay đổi đột ngột, đáng kể về khối lượng gửi, nội dung hoặc tần suất có thể kích hoạt bộ lọc thư rác hộp thư vì những thay đổi như vậy có thể chỉ ra rằng người gửi đã bị xâm phạm hoặc tham gia vào các thực tiễn lạm dụng. Ngay cả những bất thường như những thay đổi lớn về khối lượng hoặc thông lượng trong các sự kiện theo mùa như Thứ Sáu Đen cũng có thể được hiểu là tiêu cực, và các nhà cung cấp hộp thư có cách tiếp cận thận trọng khi họ phát hiện những bất thường như vọt tăng đột ngột trong khối lượng.

### 4. Thành công về Khả năng gửi Dài hạn

Đó là một quan niệm sai lầm rằng sao ấm được thực hiện chỉ một lần. Trên thực tế, bạn cần phải duy trì khối lượng giao thông và nhịp độ gửi để giữ những danh tính gửi đó ấm. Ngoài ra, nếu bạn có kế hoạch tăng khối lượng đáng kể, ví dụ từ 1M đến 5M hoặc 5M đến 25M, bạn cần phải sao ấm lên những khối lượng đó. Những bước nhảy lớn này trong khối lượng trông đáng ngờ với các nhà cung cấp hộp thư ngay cả khi bạn đã gửi liên tục.

### 5. Thích ứng với Các Thay đổi của Nhà cung cấp Hộp thư

Các nhà cung cấp hộp thư cũng liên tục cập nhật các thuật toán của họ để tốt hơn phát hiện thư rác và hành vi lạm dụng. Nếu bạn xem sao ấm là một quá trình liên tục và liên tục giám sát khả năng gửi của bạn và các tín hiệu tham gia, bạn có thể thực hiện các điều chỉnh đối với chiến lược gửi của bạn khi cần thiết, đảm bảo rằng email của bạn tiếp tục đến hộp thư đến, ngay cả khi hành vi hộp thư và khán giả thay đổi.

---

## Những Thách thức Phổ biến để Di chuyển Lưu lượng truy cập và Làm ấm lên một ESP Mới

Chuyển giao lưu lượng email của bạn sang một ESP mới có thể có những thách thức duy nhất yêu cầu cân nhắc và kế hoạch chiến lược cẩn thận để vượt qua. Những thách thức này bao gồm những điều sau đây:

1. **Lưu lượng truy cập được điều khiển bởi sự kiện** – Nếu tất cả email của bạn được điều khiển bởi sự kiện, thật khó để kiểm soát khối lượng và thông lượng.

2. **Nhiều miền gửi** – Có nhiều miền gửi với các khối lượng giao thông và thông lượng khác nhau có thể làm phức tạp quá trình chuyển đổi.

3. **Không có IP được chia sẻ** – Một số tổ chức không được phép sử dụng các nhóm IP được chia sẻ.

4. **Thiếu dữ liệu tham gia** – Thiếu dữ liệu liên quan đến tham gia có thể khiến việc tối ưu hóa quá trình sao ấm trở nên khó khăn.

5. **Thông tin nảy và hủy đăng ký lỗi thời** – Không có thông tin nảy và hủy đăng ký cập nhật trong ESP hiện tại của bạn có thể dẫn đến các vấn đề về khả năng gửi.

6. **Miền cấp hai duy nhất** – Bạn hiện đang gửi thư của mình từ miền cấp hai, chẳng hạn như `example.com`, mà không có các miền phụ riêng biệt cho các trường hợp sử dụng logic như giao dịch hoặc tiếp thị.

7. **Những khoảng thời gian chặt chẽ** – Hợp đồng kết thúc hoặc các lý do khác có thể áp đặt khoảng thời gian chặt chẽ cho quá trình chuyển đổi.

8. **Những Thách thức đối với Nhà cung cấp dịch vụ độc lập (ISV) và Nhà cung cấp Phần mềm-như-một-Dịch vụ (SaaS)** – Những tổ chức này thường không có quyền kiểm soát hoàn toàn đối với khối lượng, nội dung, danh sách hoặc tính nhất quán gửi của khách hàng của họ. Họ cũng có thể không có quyền truy cập trực tiếp vào DNS cần thiết để cập nhật và căn chỉnh miền gửi và xác thực.

---

## Chiến lược cho Sao ấm và Di chuyển thành công

Danh sách sau đây không phù hợp với mọi trường hợp, và nhiều khách hàng sẽ sử dụng hơn một chiến lược để giải quyết những thách thức của họ và làm mượt quá trình chuyển đổi của họ sang Amazon SES:

### 1. Gửi đến Khán giả Tốt nhất của Bạn Trước

Điều quan trọng nhất bạn cần làm khi chuyển sang một ESP mới là gửi đến những người nhận hoạt động cao nhất và tích cực nhất của bạn trên ESP mới và để những phân khúc ít hoạt động hoặc rủi ro hơn trên ESP trước đó cho đến khi bạn sẵn sàng chuyển đổi đầy đủ. Ví dụ, nếu bạn là người gửi hàng ngày gửi đến 1M địa chỉ một ngày và có tỷ lệ mở khoảng 20%, bạn cần bắt đầu onboarding với các phân khúc bao gồm những người đang mở. Một chiến lược tốt là bắt đầu với những người mở cửa từ 30 ngày cuối cùng, sau đó chuyển đến những người mở 31–90 ngày, v.v.

### 2. Dần dần Di chuyển Người đăng ký Ít tham gia của Bạn

Sau khi bạn đã chuyển giao các phân khúc tích cực nhất của mình, bạn có thể bắt đầu bao gồm những người ít tham gia một chút. Bạn có thể rắc chúng vào với các phân khúc tham gia hơn để nếu bạn nhận được nảy hoặc phàn nàn, nó được lọc bởi các phân khúc tham gia hơn. Hãy chắc chắn tiếp tục theo dõi các vấn đề và ngay lập tức dừng tăng khối lượng công việc của bạn nếu bạn gặp các vấn đề về khả năng gửi như tăng tỷ lệ nảy hoặc spam.

### 3. Bắt đầu với Khối lượng công việc Dự đoán được

Bắt đầu với khối lượng công việc không phụ thuộc vào thời gian, chẳng hạn như các bản tin, dễ dàng kiểm soát và theo dõi hơn.

### 4. Batch Event-driven Messages

Đối với các tin nhắn được điều khiển bởi sự kiện mà không phụ thuộc vào thời gian, hãy cố gắng để batch và phân tán chúng để quản lý khối lượng.

### 5. Sử dụng Quy trình Sao ấm Tự động

[Tiêu chuẩn](https://docs.aws.amazon.com/ses/latest/dg/manual-dedicated-ips.html) và [IP dành riêng được quản lý](https://docs.aws.amazon.com/ses/latest/dg/managed-dedicated-sending.html) có thể giúp quản lý khối lượng hàng ngày bằng cách cho phép các mức lưu lượng được xác định trước trên các IP dành riêng của bạn và tràn ra ngoài các nhóm IP được chia sẻ khi khối lượng email đã đạt đến một mức mà chúng tôi cho là đủ cho các IP dành riêng của bạn. Điều này phụ thuộc vào tiến trình sao ấm của bạn cho đến nay. Tiêu chuẩn dành riêng là tăng tĩnh 45 ngày, nhưng được quản lý dành riêng có một quá trình tinh vi hơn. Để tìm hiểu thêm, hãy tham khảo [Địa chỉ IP dành riêng cho Amazon SES](https://docs.aws.amazon.com/ses/latest/dg/dedicated-ip.html).

### 6. Sử dụng Chiến lược các Nhóm IP được Chia sẻ

Sử dụng các nhóm IP được chia sẻ cho khối lượng công việc không yêu cầu IP dành riêng. Vì có khối lượng liên tục đã đi qua những IP này, chúng tha thứ hơn một chút so với IP dành riêng được sao ấm.

### 7. Chuyển đổi Dần dần sang IP Dành riêng

Bắt đầu với IP được chia sẻ và dần dần chuyển sang IP dành riêng khi chúng sao ấm.

### 8. Chuyển đổi Dần dần sang Miền phụ Logic

Chia nhỏ lưu lượng truy cập của bạn thành các khối lượng công việc logic có thể có khối lượng liên tục và thông lượng. Ngay cả một cái gì đó đơn giản như `marketing.example.com` và `transactional.example.com` thì tốt hơn việc gửi thư từ `example.com`

### 9. Onboard Khách hàng Mới trên ESP Mới

Đối với ISV và Nhà cung cấp SaaS, hãy cân nhắc onboarding các khách hàng mới trực tiếp trên ESP mới để thu thập dữ liệu ban đầu và thử nước. Các khách hàng mới đã cần được sao ấm, vì vậy nếu bạn sao ấm chúng trên Amazon SES thay vì ESP di sản của bạn, bạn không cần phải trải qua một quy trình sao ấm hai lần.

---

## Chuẩn bị để Di chuyển Email Traffic đến Amazon SES

Trước khi bạn di chuyển chương trình email của mình sang Amazon SES, điều quan trọng là phải lập tài liệu và sắp xếp thiết lập hiện tại của bạn hoàn toàn. Công việc chuẩn bị này sẽ tạo nền tảng cho một quá trình sao ấm và di chuyển thành công.

### Danh sách kiểm tra Chuẩn bị

1. **Lập tài liệu các trường hợp sử dụng của bạn** – Phân loại các trường hợp sử dụng của bạn là tiếp thị hoặc giao dịch. Điều này sẽ giúp bạn hiểu bản chất của các email bạn gửi và cách chúng nên được xử lý.

2. **Lập tài liệu các miền gửi của bạn** – Bao gồm các tên "từ" được liên kết với mỗi miền. Điều này sẽ hỗ trợ trong việc ánh xạ miền thích hợp tới loại email tương ứng. Lý tưởng nhất là bạn nên tránh gửi từ miền gốc của bạn. Ví dụ, sử dụng miền phụ như `email.brand.com` thay vì `brand.com`. Xem xét và lập tài liệu xác thực của bạn (ví dụ, SPF, DKIM hoặc DMARC). Trong một số trường hợp, bạn có thể không cần căn chỉnh tất cả chúng, nhưng bạn chắc chắn sẽ cần phải [căn chỉnh DMARC như một phần của các yêu cầu người gửi hàng loạt](https://aws.amazon.com/blogs/messaging-and-targeting/an-overview-of-bulk-sender-changes-at-yahoo-gmail/).

3. **Ánh xạ các trường hợp sử dụng sang các miền gửi và từ tên** – Tạo một tương ứng rõ ràng để đảm bảo các email phù hợp được gửi từ các miền thích hợp. Tối thiểu, đó là một thực tiễn tốt nhất để có các miền phụ riêng biệt cho các trường hợp sử dụng email giao dịch và khuyến mãi, chẳng hạn như `transactional.brand.com` và `promo.brand.com`.

4. **Lập tài liệu khối lượng và thông lượng tối đa** – Chụp thông tin này cho mỗi trường hợp sử dụng được ánh xạ tới các miền gửi của bạn. Điều này sẽ giúp bạn hiểu quy mô của hoạt động email của bạn và lên kế hoạch kiến trúc và chiến lược sao ấm của bạn cho phù hợp.

5. **Dự kiến một sự sụt giảm tạm thời trong các số liệu khả năng gửi** – Trong khi chuyển sang một ESP mới, bạn có thể trải nghiệm một biến động ngắn hạn trong các số liệu như tỷ lệ mở và tỷ lệ nhấp chuột. Đây là một điều phổ biến xảy ra và không nên được coi là một thất bại của dịch vụ. Đó là một phần mong đợi của quá trình di chuyển khi các nhà cung cấp hộp thư thích ứng với danh tính gửi mới của bạn. Bằng cách theo dõi chặt chẽ tỷ lệ nảy và phàn nàn của bạn, bạn có thể thực hiện các điều chỉnh chủ động đối với kế hoạch tăng của bạn để đảm bảo một quá trình chuyển đổi mượt mà.

6. **Lập tài liệu kế hoạch sao ấm của bạn** – Có một kế hoạch để dần dần tăng giao thông cho mỗi danh tính và giám sát các số liệu tham gia. Lên kế hoạch cho cách xử lý tỷ lệ nảy hoặc phàn nàn cao.

### Kế hoạch Sao ấm Mẫu

Bảng sau đây cho thấy một kế hoạch sao ấm mẫu. Nhận thấy rằng các ngày được phân loại theo các nhà cung cấp hộp thư lớn. Điều này là vì những nhà cung cấp này đều chấp nhận thư mới với các tỷ lệ khác nhau. Phân loại theo cách này là một thực tiễn được khuyến nghị, nhưng nếu bạn không thể phân khúc chi tiết đó, bạn có thể sử dụng cột Tổng số Hàng ngày làm hướng dẫn. Dịch vụ IP dành riêng được quản lý AWS tự động làm phân đoạn hóa và điều chỉnh ở cấp độ miền cho bạn.

Kế hoạch sau đây là một ramp điển hình. Bạn có thể tấn công tích cực hơn tỷ lệ tham gia tổng thể của bạn càng cao, vì vậy nếu bạn ở mức 40–60% tham gia, bạn có thể sử dụng sao ấm này. Nếu tỷ lệ của bạn thấp hơn, bạn có thể muốn thận trọng hơn một chút. Hãy chắc chắn thích ứng khi bạn bước vào kế hoạch sao ấm của mình vì bạn có thể cần duy trì cùng một tỷ lệ trong vài ngày hoặc thậm chí quay lại một bước nếu bạn đang trải nghiệm các xu hướng tiêu cực như sự sụt giảm khả năng gửi hoặc tham gia. Liên tục giám sát các số liệu của bạn trong thời gian quan trọng này.

| Ngày | @gmail.com | @hotmail.com | @outlook.com | @yahoo.com | @icloud.com | @aol.com | Khác | Tổng hàng ngày |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 150 | 150 | 150 | 150 | 150 | 150 | 150 | 1.050 |
| 2 | 300 | 300 | 300 | 300 | 300 | 300 | 300 | 2.100 |
| 3 | 600 | 600 | 600 | 600 | 600 | 600 | 600 | 4.200 |
| 4 | 1.200 | 1.200 | 1.200 | 1.200 | 1.200 | 1.200 | 1.200 | 8.400 |
| 5 | 2.400 | 2.400 | 2.400 | 2.400 | 2.400 | 2.400 | 2.400 | 16.800 |
| 6 | 5.000 | 5.000 | 5.000 | 5.000 | 5.000 | 5.000 | 5.000 | 35.000 |
| 7 | 10.000 | 10.000 | 10.000 | 10.000 | 10.000 | 10.000 | 10.000 | 70.000 |
| 8 | 20.000 | 20.000 | 20.000 | 20.000 | 20.000 | 20.000 | 20.000 | 140.000 |
| 9 | 40.000 | 40.000 | 40.000 | 40.000 | 40.000 | 40.000 | 40.000 | 280.000 |
| 10 | 80.000 | 80.000 | 80.000 | 80.000 | 80.000 | 80.000 | 80.000 | 560.000 |
| 11 | 150.000 | 150.000 | 150.000 | 150.000 | 150.000 | 150.000 | 150.000 | 1.050.000 |
| 12 | 300.000 | 300.000 | 300.000 | 300.000 | 300.000 | 300.000 | 300.000 | 2.100.000 |
| 13 | 425.000 | 425.000 | 425.000 | 425.000 | 425.000 | 425.000 | 425.000 | 2.975.000 |
| 14 | 500.000 | 500.000 | 500.000 | 500.000 | 500.000 | 500.000 | 500.000 | 3.500.000 |
| 15 | 600.000 | 600.000 | 600.000 | 600.000 | 600.000 | 600.000 | 600.000 | 4.200.000 |
| 16 | 650.000 | 650.000 | 650.000 | 650.000 | 650.000 | 650.000 | 650.000 | 4.550.000 |
| 17 | 700.000 | 700.000 | 700.000 | 700.000 | 700.000 | 700.000 | 700.000 | 4.900.000 |
| 18 | 800.000 | 800.000 | 800.000 | 800.000 | 800.000 | 800.000 | 800.000 | 5.600.000 |
| 19 | 900.000 | 900.000 | 900.000 | 900.000 | 900.000 | 900.000 | 900.000 | 6.300.000 |
| 20 | 1.000.000 | 1.000.000 | 1.000.000 | 1.000.000 | 1.000.000 | 1.000.000 | 1.000.000 | 7.000.000 |
| 21 | 1.100.000 | 1.100.000 | 1.100.000 | 1.100.000 | 1.100.000 | 1.100.000 | 1.100.000 | 7.700.000 |
| 22 | 1.200.000 | 1.200.000 | 1.200.000 | 1.200.000 | 1.200.000 | 1.200.000 | 1.200.000 | 8.400.000 |
| 23 | 1.300.000 | 1.300.000 | 1.300.000 | 1.300.000 | 1.300.000 | 1.300.000 | 1.300.000 | 9.100.000 |
| 24 | 1.400.000 | 1.400.000 | 1.400.000 | 1.400.000 | 1.400.000 | 1.400.000 | 1.400.000 | 9.800.000 |
| 25 | 1.500.000 | 1.500.000 | 1.500.000 | 1.500.000 | 1.500.000 | 1.500.000 | 1.500.000 | 10.500.000 |
| 26 | 1.600.000 | 1.600.000 | 1.600.000 | 1.600.000 | 1.600.000 | 1.600.000 | 1.600.000 | 11.200.000 |
| 27 | 1.700.000 | 1.700.000 | 1.700.000 | 1.700.000 | 1.700.000 | 1.700.000 | 1.700.000 | 11.900.000 |
| 28 | 1.800.000 | 1.800.000 | 1.800.000 | 1.800.000 | 1.800.000 | 1.800.000 | 1.800.000 | 12.600.000 |
| 29 | 1.900.000 | 1.900.000 | 1.900.000 | 1.900.000 | 1.900.000 | 1.900.000 | 1.900.000 | 13.300.000 |
| 30 | 2.000.000 | 2.000.000 | 2.000.000 | 2.000.000 | 2.000.000 | 2.000.000 | 2.000.000 | 14.000.000 |
| 31 | 2.100.000 | 2.100.000 | 2.100.000 | 2.100.000 | 2.100.000 | 2.100.000 | 2.100.000 | 14.700.000 |
| 32 | 2.200.000 | 2.200.000 | 2.200.000 | 2.200.000 | 2.200.000 | 2.200.000 | 2.200.000 | 15.400.000 |
| 33 | 2.300.000 | 2.300.000 | 2.300.000 | 2.300.000 | 2.300.000 | 2.300.000 | 2.300.000 | 16.100.000 |
| 34 | 2.400.000 | 2.400.000 | 2.400.000 | 2.400.000 | 2.400.000 | 2.400.000 | 2.400.000 | 16.800.000 |
| 35 | 2.500.000 | 2.500.000 | 2.500.000 | 2.500.000 | 2.500.000 | 2.500.000 | 2.500.000 | 17.500.000 |
| 36 | 2.600.000 | 2.600.000 | 2.600.000 | 2.600.000 | 2.600.000 | 2.600.000 | 2.600.000 | 18.200.000 |
| 37 | 2.700.000 | 2.700.000 | 2.700.000 | 2.700.000 | 2.700.000 | 2.700.000 | 2.700.000 | 18.900.000 |
| 38 | 2.800.000 | 2.800.000 | 2.800.000 | 2.800.000 | 2.800.000 | 2.800.000 | 2.800.000 | 19.600.000 |
| 39 | 2.900.000 | 2.900.000 | 2.900.000 | 2.900.000 | 2.900.000 | 2.900.000 | 2.900.000 | 20.300.000 |
| 40 | 3.000.000 | 3.000.000 | 3.000.000 | 3.000.000 | 3.000.000 | 3.000.000 | 3.000.000 | 21.000.000 |

---

## Các Thực tiễn Tốt nhất cho một Sao ấm IP thành công

Một sao ấm IP thành công liên quan đến một cách tiếp cận chiến lược kết hợp chuẩn bị kỹ thuật, những người đăng ký tham gia, nội dung thuyết phục và giám sát liên tục.

### 1. Đảm bảo Sự Sẵn sàng Kỹ thuật

[Cấu hình các bản ghi DNS](https://docs.aws.amazon.com/ses/latest/dg/configure-identities.html) và thiết lập [SPF](https://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-spf.html), [DKIM](https://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-dkim.html), [DMARC](https://docs.aws.amazon.com/ses/latest/dg/send-email-authentication-dmarc.html), và [BIMI](https://aws.amazon.com/blogs/messaging-and-targeting/what-is-bimi-and-how-to-use-it-with-amazon-ses/) để nội dung email của bạn tuân thủ các thực tiễn tốt nhất. Hãy chắc chắn [DMARC của bạn được căn chỉnh](https://aws.amazon.com/blogs/messaging-and-targeting/navigate-bulk-sender-requirements-with-amazon-ses/) nếu bạn đang gửi trên nhiều ESP hoặc ứng dụng.

### 2. Sử dụng Danh sách Gửi được tham gia, Dựa trên Quyền

Sử dụng danh sách sạch, opt-in của những người đăng ký quan tâm đến nội dung của bạn. Để biết thêm thông tin, hãy tham khảo [Tối ưu hóa Khả năng gửi Email: Cách tiếp cận Lập kế hoạch Danh sách và Giám sát Trung tâm Người dùng](https://aws.amazon.com/blogs/messaging-and-targeting/optimizing-email-deliverability-a-user-centric-approach-to-list-management-and-monitoring/).

### 3. Cung cấp Nội dung Email Thuyết phục, Có giá trị

Gửi nội dung cộng hưởng với khán giả của bạn và khuyến khích tham gia.

### 4. Dần dần Tăng Khối lượng và Nhịp độ Gửi

Bắt đầu với một khối lượng email nhỏ và dần dần tăng theo thời gian để cho phép các nhà cung cấp hộp thư quan sát các mẫu gửi của bạn.

### 5. Duy trì Tính nhất quán trong Hành vi gửi

Tránh những thay đổi đột ngột, đáng kể về khối lượng gửi, nội dung hoặc tần suất.

### 6. Liên tục Giám sát và Tối ưu hóa Các số liệu Chính

Theo dõi tỷ lệ mở, tỷ lệ nhấp chuột, tỷ lệ nảy và tỷ lệ phàn nàn, và thực hiện các điều chỉnh khi cần thiết. Để biết thêm thông tin, hãy tham khảo [Amazon SES – Thiết lập thông báo cho nảy và phàn nàn](https://aws.amazon.com/blogs/messaging-and-targeting/amazon-ses-set-up-notifications-for-bounces-and-complaints/).

### 7. Bảo trì Danh tiếng Người gửi Liên tục

Kiểm toán dòng dữ liệu, tăng lên dần dần những thay đổi, và tuân theo các thực tiễn tiếp thị email phát triển.

---

## Điều hướng Những Thách thức về Khả năng gửi Ban đầu

Khi chuyển sang một ESP mới, bạn có thể gặp phải một số thách thức khả năng gửi ban đầu. Điều quan trọng là giám sát và thực hiện cảnh báo nếu bạn quan sát thấy tỷ lệ nảy hoặc phàn nàn tăng. Nếu bạn có những thách thức, bạn cần xử lý chúng nhanh chóng. Duy trì cùng một khối lượng hoặc thậm chí giảm khối lượng ngày hôm sau nếu bạn gặp những vấn đề này:

### 1. Bùng nổ trong Tỷ lệ Nảy Cứng

Mang các danh sách triệt tiêu của bạn từ ESP bạn đang tắt, nhưng nếu ESP trước đó của bạn không quản lý những điều này tốt hoặc bạn tải một số địa chỉ cũ mà bạn không biết, cảm thấy cơ bản để trải nghiệm các đơn vị nảy cứng ở đầu. Nếu điều này xảy ra, làm chậm tăng khối lượng của bạn hoặc thậm chí dừng tăng cho đến khi mọi thứ ổn định. Điều quan trọng hơn là sao ấm đúng cách so với nó để đến mức độ sản xuất gửi càng nhanh càng tốt. Đây là một lý do khác tại sao lúc nào cũng tốt nhất để bắt đầu với các phân khúc tham gia nhất của bạn.

### 2. Tăng Phàn nàn Spam

Email có thể đạt đến những người nhận đã lọc chúng trước đây, dẫn đến nhiều phàn nàn spam. Thay đổi danh tính cũng có thể khiến người nhận của bạn nhấn nút spam vì họ không nhận ra nó. Thông báo những thay đổi danh tính trước khi thay đổi ESP để giảm nguy cơ của một vấn đề.

### 3. Sự Kinh sợ Tăng của Nhà cung cấp Hộp thư

Các nhà cung cấp hộp thư sẽ giám sát chặt chẽ những người gửi mới để xác nhận họ không tham gia vào các hoạt động độc hại. Điều này có thể chuyển hướng email tới bộ lọc thư rác ban đầu hoặc thậm chí bị điều chỉnh nếu bạn đạt đến giới hạn khối lượng hoặc thông lượng. Gmail được biết là nghiêm ngặt. Các IP dành riêng được quản lý Amazon SES sử dụng dữ liệu của chúng tôi để biết bao nhiêu thư các nhà cung cấp hộp thư lớn sẽ chấp nhận trong khi bạn đang sao ấm và giữ bạn khỏi vượt quá các giới hạn của họ.

### 4. Điều chỉnh ESP và Giới hạn Gửi

ESP mới có thể có các quy tắc khắt khe hơn về khối lượng email có thể được gửi đến các nhà cung cấp hộp thư riêng lẻ. Amazon SES có giới hạn tài khoản cho khối lượng hàng ngày và thông lượng tối đa, vì vậy điều chỉnh của bạn là những gì bạn sẽ cần. Để tìm hiểu thêm, hãy tham khảo [Tăng các hạn ngạch gửi Amazon SES của bạn](https://docs.aws.amazon.com/ses/latest/dg/manage-sending-quotas-request-increase.html) trong Hướng dẫn Nhà phát triển Amazon SES.

---

## Duy trì Danh tiếng IP Sau khi Sao ấm

Sao ấm IP là một quá trình liên tục. Ngay cả sau khi giai đoạn sao ấm ban đầu, điều quan trọng là duy trì danh tiếng người gửi của bạn bằng cách liên tục quản lý chương trình email của bạn. Tham gia người đăng ký của bạn có thể dao động khi danh sách của bạn phát triển và thay đổi. Tương tự, tăng khối lượng giao thông email cho một chiến dịch theo mùa sẽ yêu cầu điều chỉnh quá trình sao ấm của bạn. Bạn cần phải chủ động và điều chỉnh chiến lược sao ấm IP của bạn.

Kiểm toán dòng dữ liệu và các chiến dịch và giám sát các nguồn danh sách email, thực tiễn thu thập dữ liệu và hiệu suất chiến dịch. Khi giới thiệu các yếu tố mới, hãy làm điều đó từng bước để tránh kích hoạt các vấn đề danh tiếng. Để cho phép thời gian cho danh tiếng của bạn ổn định, hãy cung cấp ít nhất một tháng để một đường cơ sở mới được thiết lập sau khi những thay đổi chương trình lớn. Tương tác với khách hàng, cung cấp giá trị và thực hiện các chiến dịch tái tham gia để nuôi dưỡng các mối quan hệ khách hàng của bạn. Tuân thủ các thực tiễn tiếp thị email phát triển, bao gồm các giao thức xác thực thích hợp và công nghệ mới nổi. Hãy chủ động và theo dõi danh tiếng miền và IP để bạn có thể nhanh chóng giải quyết các vấn đề khả năng gửi phát sinh. Để tìm hiểu thêm về việc giám sát các công cụ hộp thư như Google Postmaster, hãy tham khảo [Hiểu Google Postmaster Tools (phàn nàn spam) cho những người gửi Amazon SES](https://aws.amazon.com/blogs/messaging-and-targeting/understanding-google-postmaster-tools-spam-complaints-for-amazon-ses-email-senders/).

---

## Kết luận

Chuyển giao chương trình email của bạn sang một ESP mới như Amazon SES có thể có vẻ phức tạp, nhưng nó có thể nhanh chóng và liền mạch nếu bạn tuân theo các thực tiễn tốt nhất được giải thích trong bài đăng này. Sao ấm IP là một thành phần quan trọng của quá trình này vì nó giúp xây dựng danh tiếng người gửi tích cực với các nhà cung cấp hộp thư và thúc đẩy việc gửi email đáng tin cậy.

Trong suốt hướng dẫn này, chúng tôi đã đề cập đến các khía cạnh chính của sao ấm IP và di chuyển email, từ việc hiểu tầm quan trọng của thực tiễn này đến xác định những thách thức phổ biến và nêu các chiến lược hiệu quả cho một quá trình chuyển đổi thành công. Bằng cách tuân theo các thực tiễn tốt nhất như tạo điều kiện sẵn sàng kỹ thuật, sử dụng cơ sở người đăng ký tham gia, cung cấp nội dung thuyết phục và dần dần tăng khối lượng gửi, bạn có thể điều hướng các thách thức khả năng gửi ban đầu và thiết lập một nền tảng vững chắc cho thành công chương trình email dài hạn.

Tuy nhiên, công việc không dừng lại khi giai đoạn sào ấm ban đầu hoàn tất. Duy trì danh tiếng IP và điều chỉnh chiến lược của bạn khi chương trình email và sự tham gia của người đăng ký phát triển là một quá trình liên tục. Liên tục giám sát các số liệu chính, kiểm toán dòng dữ liệu và cập nhật với các thực tiễn tiếp thị email phát triển là rất quan trọng để duy trì khả năng gửi. Danh tiếng người gửi lâu dài và mối quan hệ lâu dài với danh sách và người nhận của bạn là một số lợi ích chính của các thực tiễn tốt nhất sau đây. Chuyển sang một ESP mới là một nỗ lực đáng kể, nhưng với sự chuẩn bị thích hợp, thực hiện và cam kết duy trì liên tục, quá trình di chuyển của bạn có thể mượt mà và thành công.

---

## Về Tác giả

### Tyler Holmes

![Tyler Holmes](https://d2908q01vomqb2.cloudfront.net/632667547e7cd3e0466547863e1207a8c0c0c549/2025/07/02/image-1-2.png)

Tyler là một Kiến trúc sư Giải pháp Chuyên gia Cao cấp. Anh ấy có rất nhiều kinh nghiệm trong lĩnh vực giao tiếp như một cố vấn, một SA, một từng làm việc và lãnh đạo ở tất cả các cấp độ từ Startup đến Fortune 500. Anh ấy đã dành hơn 14 năm trong bán hàng, tiếp thị và hoạt động dịch vụ, làm việc cho các cơ quan, công ty tư vấn và thương hiệu, xây dựng các nhóm và tăng doanh thu.

---

## Tài nguyên cho Khả năng gửi

- [Hướng dẫn Nhà phát triển Amazon SES](https://docs.aws.amazon.com/ses/latest/dg/Welcome.html)
- [Tài liệu API Amazon SES](https://docs.aws.amazon.com/ses/latest/APIReference-V2/Welcome.html)
- [Hạn ngạch dịch vụ trong Amazon SES](https://docs.aws.amazon.com/ses/latest/dg/quotas.html)
- [Điểm cuối Amazon SES và hạn ngạch](https://docs.aws.amazon.com/general/latest/gr/ses.html#ses_region)
- [Giá Amazon SES](https://aws.amazon.com/ses/pricing/)
- [Amazon Pinpoint](https://aws.amazon.com/pinpoint/product-details)
- [Bắt đầu với Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/userguide/gettingstarted.html)

---

**Tags:** Amazon SES, Email Deliverability, IP Warming, Domain Warming, Email Migration, Email Marketing
