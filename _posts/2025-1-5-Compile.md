---
title: Quá trình Compile
date: 2025-1-5
categories: [Computer, Compile]
tags: [Linking, Compiler]
author: Trieu
image:
  path: assets/articles/2025/Compile/2025-1-5-headerCompile.png
  alt: Compile
---

## Giới thiệu

Nếu bạn đã từng lập trình bằng C, C++ hoặc Java, chắc hẳn bạn đã từng xây dựng (Build) hoặc biên dịch (Compile) mã nguồn mà mình viết để chạy chương trình, hoặc cũng có thể từng gặp phải lỗi biên dịch do viết sai mã nguồn.

Nếu bạn chưa hiểu rõ quá trình biên dịch thực sự làm những gì và chỉ mơ hồ nghĩ rằng "Biên dịch là kiểm tra cú pháp mã nguồn và thực thi chương trình", thì đây là lúc để tìm hiểu chi tiết hơn.

※ Hình minh họa hoặc ví dụ sẽ tập trung vào quá trình biên dịch mã C trên môi trường Linux, vì vậy có thể có sự khác biệt tùy thuộc vào ngôn ngữ hoặc môi trường bạn sử dụng.

## Định nghĩa Compile

Biên dịch là quá trình chuyển đổi mã nguồn viết bằng ngôn ngữ mà con người có thể hiểu (ngôn ngữ cấp cao như C, C++, Java, v.v.) thành ngôn ngữ mà CPU có thể hiểu được (ngôn ngữ cấp thấp, tức là mã máy).

Mã nguồn mà chúng ta viết bằng C, C++, Java không thể được máy tính hiểu trực tiếp, vì máy tính chỉ hiểu mã máy, được biểu diễn bằng các số 0 và 1. Vì vậy, một quá trình biên dịch là cần thiết để chuyển đổi mã nguồn của chúng ta thành mã máy mà máy tính có thể hiểu và thực thi.

Mã nguồn sau khi được biên dịch sẽ trở thành một tệp thực thi chứa mã máy. Khi chạy tệp này, nội dung của nó sẽ được nạp vào bộ nhớ thông qua Loader của hệ điều hành và chương trình sẽ được thực thi.

## Quá trình Compile

Thêm hình ảnh ở đây

Quá trình biên dịch được chia thành 4 bước:
- Quá trình tiền xử lý (Pre-processing).
- Quá trình biên dịch (Compilation).
- Quá trình lắp ráp (Assembly).
- Quá trình liên kết (Linking).

Cả 4 bước này thường được gọi chung là quá trình biên dịch hoặc quá trình xây dựng (build). Đôi khi, quá trình biên dịch và quá trình liên kết được tách riêng ra và gọi tên khác nhau.

Thông thường, quá trình xây dựng được hiểu theo nghĩa rộng hơn, bao gồm cả biên dịch và liên kết (Build = Compile + Link). Tuy nhiên, bạn có thể hiểu theo ngữ cảnh cụ thể tùy thuộc vào tình huống.

Hãy cùng tìm hiểu chi tiết từng bước trong quá trình này.

### 1. Quá trình tiền xử lý (Pre-processing)

Pre-processing à quá trình chuyển đổi **file source *.c*** thành **file source đã được xử lý *.i*** thông qua **Preprocessor**.
Trong quá trình này, có ba công việc chính thường được thực hiện:
- Loại bỏ Comments: Tất cả các chú thích trong mã nguồn sẽ bị loại bỏ. Chú thích chỉ là phần thông tin dành cho con người, không cần thiết cho máy tính hiểu.
- Chèn *.h: Khi gặp chỉ thị **#include**, **Preprocessor** sẽ tìm *.h tương ứng và sao chép tất cả nội dung trong *.h vào mã nguồn (*.c). File.h không được sử dụng trực tiếp trong quá trình biên dịch mà sẽ được sao chép toàn bộ vào trong tệp mã nguồn. Các khai báo hàm trong *.h sẽ được kết hợp với *.o chứa định nghĩa hàm thực tế trong quá trình liên kết (Linking).
- Sử dụng macro: Các macro được định nghĩa bằng chỉ thị **#define** sẽ được lưu trữ và khi gặp *các chuỗi giống nhau*, chúng sẽ được thay thế bằng nội dung mà macro đã định nghĩa. Nói một cách đơn giản,**Preprocessor** sẽ tìm kiếm tên của macro và thay thế nó bằng giá trị đã được định nghĩa.

### 2. Quá trình biên dịch (Compilation)
- Compilation là quá trình thông qua trình biên dịch (Compiler) để chuyển đổi tệp mã nguồn đã qua tiền xử lý **(*.i)** thành tệp mã Assembly **(*.s)**.
- Trong quá trình này, những gì chúng ta thường nghĩ khi nói đến việc biên dịch, đó là kiểm tra cú pháp của ngôn ngữ. Bên cạnh đó, quá trình này còn thực hiện việc cấp phát bộ nhớ cho các khu vực tĩnh **(Data, BSS)**. Những khu vực này chứa dữ liệu và các biến không được khởi tạo, và việc cấp phát bộ nhớ là bước cần thiết để chuẩn bị cho các quá trình tiếp theo.

#### Cấu trúc Compiler
Compiler bao gồm ba giai đoạn (Front-end - Middle-end - Back-end) .
##### Front-End
\- Trong quá trình frontend, các phần liên quan đến từng ngôn ngữ riêng biệt sẽ được xử lý. </br>
\- Kiểm tra mã nguồn: Quá trình này kiểm tra xem mã nguồn có được viết đúng theo cú pháp và ngữ nghĩa của ngôn ngữ không, bao gồm phân tích từ vựng, cú pháp và ngữ nghĩa. </br>
\- Tạo cây GIMPLE: Sau khi kiểm tra mã nguồn, một cây GIMPLE sẽ được tạo ra. GIMPLE là một cấu trúc dữ liệu biểu diễn mã nguồn dưới dạng cây, giúp thể hiện chương trình theo cách dễ dàng xử lý hơn. </br?>
\-Trong quá trình này, các ngôn ngữ như C, C++, Java sẽ được xử lý theo cách riêng biệt của từng ngôn ngữ, sau đó chuyển thành GIMPLE, một dạng biểu diễn trung gian (IR: Intermediate Representation) chung, giúp xử lý các phần phụ thuộc vào ngôn ngữ. </br>

##### Middle-End

Trong quá trình middle-end, các tối ưu hóa không phụ thuộc vào kiến trúc được thực hiện.
- Tối ưu hóa không phụ thuộc vào kiến trúc có nghĩa là những tối ưu hóa này có thể được áp dụng bất kể kiến trúc CPU là gì (như ARM, x86, v.v.). Các tối ưu hóa này thường liên quan đến việc cải thiện hiệu suất chương trình mà không cần quan tâm đến phần cứng cụ thể.

Sau khi nhận được cây GIMPLE từ frontend, middle-end thực hiện các tối ưu hóa không phụ thuộc vào kiến trúc. Sau đó, quá trình này tạo ra RTL (Register Transfer Language), là một dạng biểu diễn trung gian giữa ngôn ngữ cấp cao và mã Assembly. RTL giúp trình biên dịch chuyển đổi mã nguồn thành dạng có thể dễ dàng chuyển thành mã máy cho kiến trúc cụ thể trong giai đoạn backend.

##### Back-End
Trong quá trình back-end, các tối ưu hóa phụ thuộc vào kiến trúc được thực hiện.
- Tối ưu hóa phụ thuộc vào kiến trúc có nghĩa là các tối ưu hóa này được thực hiện dựa trên đặc điểm và khả năng của từng kiến trúc CPU cụ thể. Ví dụ, cùng một chức năng nhưng sẽ được thay thế bằng những lệnh hiệu quả hơn tùy thuộc vào kiến trúc của CPU (ARM, x86, v.v.) để nâng cao hiệu suất.
  
Sau khi nhận được RTL từ middle-end, back-end thực hiện tối ưu hóa dựa trên kiến trúc và khi quá trình này hoàn tất, mã Assembly sẽ được tạo ra.

Việc thực hiện tối ưu hóa phụ thuộc vào kiến trúc sẽ tạo ra mã Assembly mà chỉ có thể được hiểu bởi kiến trúc đó. Điều này có nghĩa là mã Assembly sẽ không thể được sử dụng nếu không tương thích với kiến trúc CPU cụ thể.

### 3. Quá trình lắp ráp (Assembly)

Quá trình Assembly chuyển đổi tệp Assembly **(.s)** thành tệp Object file **(.o)**, tệp đối tượng này sẽ chứa các mã máy (machine code) tương ứng với các lệnh đã được viết trong mã Assembly. Tuy nhiên, tệp đối tượng vẫn cần phải trải qua quá trình liên kết (linking) để kết hợp với các tệp đối tượng khác và thư viện để tạo thành một tệp thực thi hoàn chỉnh.

#### Đinh nghĩa Object file
Tệp đối tượng (Object file) là tệp chứa mã máy đã được biên dịch từ mã nguồn, nhưng chưa phải là tệp thực thi hoàn chỉnh. Tệp này thường có phần mở rộng **(*.o)** trong môi trường Linux.
- ELF (Executable and Linking Format): Định dạng tệp đối tượng sử dụng trên hệ điều hành Linux.

Phần quan trọng ở đây là phần bảng ký hiệu (Symbol Table) và phần thông tin tái phân bố (Relocation Information).

Ký hiệu (Symbol) là tên được sử dụng để xác định các hàm hoặc biến, và trong Bảng ký hiệu (Symbol Table), nó chứa thông tin về các ký hiệu được tham chiếu trong tệp đối tượng (như tên và địa chỉ dữ liệu).

Lúc này, bảng ký hiệu của tệp đối tượng chỉ chứa thông tin về các ký hiệu trong chính tệp đối tượng đó, vì vậy không thể lưu trữ thông tin ký hiệu mà các tệp khác tham chiếu.

### 4. Quá trình liên kết (Linking)
Quá trình Liên kết (Linking) là quá trình sử dụng Liên kết viên (Linker) để kết hợp các tệp đối tượng (.o) thành một tệp thực thi.

Trong quá trình này, các tệp đối tượng và các tệp thư viện mà chương trình sử dụng sẽ được liên kết lại để tạo thành một tệp thực thi duy nhất.

Tùy thuộc vào cách liên kết thư viện, quá trình này có thể chia thành hai loại:
- Liên kết tĩnh (Static Linking): Các thư viện được kết nối trực tiếp vào tệp thực thi trong quá trình biên dịch.
- Liên kết động (Dynamic Linking): Các thư viện không được liên kết vào tệp thực thi ngay lập tức, mà sẽ được tải và liên kết khi chương trình chạy.
