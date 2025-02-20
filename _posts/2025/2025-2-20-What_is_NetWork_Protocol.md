---
title: Giao thức mạng là gì?
date: 2025-2-20
categories: [Network]
tags: []
author: Trieu

---
# 🍀 Lời mở đầu
Với sự phát triển mạnh mẽ của công nghệ Internet, lối sống của chúng ta đã có những thay đổi to lớn. Thông qua Internet, chúng ta có thể:
- 🌐 Duyệt web
- 📧 Gửi và nhận email
- 📱 Trò chuyện video
- 📂 Chuyển tập tin
- Và nhiều hơn thế nữa...
  
Internet hiện diện ở khắp mọi nơi và đã trở thành một phần không thể thiếu trong quá trình phát triển phần mềm hiện đại. Là một nhà phát triển trong thời đại Internet, việc hiểu rõ cách thức
 hoạt động của Internet là vô cùng quan trọng. Nếu bạn không nắm vững các nguyên tắc cơ bản, làm sao có thể xây dựng được những ứng dụng Internet hiệu quả?

> **🤔 Câu hỏi đặt ra:** "Internet hoạt động như thế nào ❓" 
{: .prompt-info }

Khi bạn mở trình duyệt và truy cập vào một trang web, điều gì xảy ra ở phía sau hậu trường? Hãy cùng khám phá những điều thú vị đó trong bài viết này. 😆

# 📖 Khám phá chi tiết 
Chúng ta đều biết rằng các trang web được cung cấp bởi máy chủ (Server). Khi muốn lấy dữ liệu từ một trang web, trình duyệt cần giao tiếp với máy chủ thông qua mạng. Quá trình này có thể 
khá phức tạp, bởi ngoài trình duyệt và máy chủ, còn có sự tham gia của các thiết bị mạng khác như bộ định tuyến (Router).

Hơn nữa, trình duyệt không chỉ truy cập vào một trang web hay một máy chủ duy nhất. Trên thực tế, mỗi trang web thường được lưu trữ trên các máy chủ khác nhau. Điều này đồng nghĩa rằng
khi bạn truy cập nhiều trang web, trình duyệt phải giao tiếp với nhiều máy chủ riêng biệt.

![Client - Server](/assets/articles/2025/What_is_NetWork_Protocol/client-server.png){: .normal }

Vì giao tiếp mạng diễn ra nhờ sự phối hợp của nhiều bên, tất cả các thành phần tham gia đều cần tuân thủ một bộ quy tắc và thỏa thuận chung để đảm bảo quá trình trao đổi diễn ra có trật 
tự và chính xác. Những quy tắc và thỏa thuận này được gọi là giao thức mạng (network protocol).

Nếu so sánh giao tiếp mạng với việc gửi thư, thì giao thức mạng chính là quy định về cách viết phong bì: bạn phải ghi đúng địa chỉ người nhận, địa chỉ người gửi, dán tem đúng vị trí, v.v. 
Nếu không tuân thủ các quy định này, bưu điện sẽ không thể chuyển thư đến đúng nơi. Tương tự, trong giao tiếp mạng, nếu không tuân theo giao thức, dữ liệu sẽ không thể truyền đi chính xác
giữa các thiết bị.

![Letter](/assets/articles/2025/What_is_NetWork_Protocol/buc-thu.png){: .normal }

Giống như một lá thư cần được phân loại và chuyển phát qua nhiều bưu cục trước khi đến tay người nhận, dữ liệu trong giao tiếp mạng cũng trải qua một hành trình tương tự. Chỉ những lá thư
được viết đúng định dạng mới được bưu điện phân loại và chuyển phát chính xác — và trong giao tiếp mạng, dữ liệu cũng phải tuân theo các quy tắc chặt chẽ để đến đúng nơi.

Khi dữ liệu được truyền qua mạng, nó được đóng gói thành các thông điệp. Để các thiết bị (bao gồm người gửi, người nhận và các bộ định tuyến trung gian) có thể phối hợp với nhau, tất cả 
các thông điệp phải tuân theo cùng một bộ giao thức mạng. Giao thức mạng quy định cấu trúc các trường trong thông điệp và chiến lược xử lý chúng.

Thông điệp mạng thường được chia thành hai phần chính:
- Tiêu đề (Header): Chứa thông tin điều khiển như địa chỉ nguồn, địa chỉ đích, loại giao thức, v.v.
- Dữ liệu (Payload): Phần nội dung chính được truyền tải.
  
**✏️ Ví dụ:** Trong giao thức IP, phần tiêu đề lưu trữ địa chỉ IP nguồn và đích. Khi thông điệp đi qua các bộ định tuyến trung gian, router sẽ đọc địa chỉ đích từ tiêu đề để xác định 
tuyến đường phù hợp và chuyển tiếp dữ liệu đến máy chủ cuối cùng.

> **🤔 Câu hỏi đặt ra:** "Giao thức mạng là gì ❓" 
{: .prompt-info }

Trên thực tế, có rất nhiều giao thức mạng khác nhau, nhưng phổ biến nhất là bộ **giao thức TCP/IP** — nền tảng cốt lõi của Internet. Bộ giao thức này bao gồm một tập hợp các giao thức hoạt
động phối hợp với nhau để đảm bảo dữ liệu được truyền tải chính xác và hiệu quả.

Một số giao thức quan trọng trong bộ TCP/IP:
- **IP (Internet Protocol)**: Chịu trách nhiệm định tuyến và truyền tải dữ liệu từ máy này sang máy khác qua mạng.
- **TCP (Transmission Control Protocol)**: Xây dựng trên giao thức IP, cung cấp khả năng truyền dữ liệu đáng tin cậy bằng cách kiểm soát lỗi và đảm bảo dữ liệu đến đúng thứ tự.
- **HTTP (HyperText Transfer Protocol)**: Xác định cách trình duyệt và máy chủ web giao tiếp, bao gồm định dạng yêu cầu và phản hồi.
- (Ngoài ra còn nhiều giao thức khác như UDP, FTP, SMTP, v.v.)
  
Trong thực tế, một quá trình giao tiếp mạng đơn giản thường đòi hỏi sự phối hợp của nhiều giao thức cùng lúc. 

**✏️ Ví dụ:** Khi bạn truy cập một trang web, trình duyệt sử dụng **HTTP** để yêu cầu nội dung từ máy chủ, **TCP** để đảm bảo dữ liệu được truyền an toàn, và **IP** để định tuyến các gói tin qua Internet.

**Thôi, bài viết đến đây cũng đủ dài rồi 🥱. Hẹn gặp bạn ở những chương tiếp theo nhá!**

Về cách các giao thức phối hợp với nhau ra sao, mình sẽ giải thích chi tiết hơn ở phần sau. Phần này chỉ mang tính sơ bộ để bạn có thể *"khởi động"* cho hành trình khám phá thế giới mạng
rộng lớn này. 🚀

# 🍂 Lời kết
Ở phần tiếp theo, chúng ta sẽ khám phá các giao thức mạng được tổ chức và hoạt động thông qua các mô hình phân lớp nổi tiếng.
- Mô hình 7 lớp OSI - khung lý thuyết kinh điển giúp hiểu rõ từng tầng trong quá trình giao tiếp mạng.
- Mô hình 4 lớp TCP/IP - nền tảng thực tế đang vận hành Internet ngày nay.
