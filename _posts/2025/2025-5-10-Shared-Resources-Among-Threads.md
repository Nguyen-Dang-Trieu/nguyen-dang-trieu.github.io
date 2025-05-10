---
title: Những tài nguyên nào của process được chia sẽ giữa các threads?
date: 2025-5-10
categories: [Câu hỏi]
tags: 
author: Trieu
---

## 🌱 Lời mở đầu
Hai khai niệm rất quan trọng khi ta nói đến hệ điều hành: tiến trình (process) và luồng (thread). Đây là 2 khái niệm rất trừu tượng nhưng cực kì thiết yếu. 

Một câu hỏi thường đặt ra là:
> Sự khác biệt giữa process và thread là gì❓
{: .prompt-info }

Nhiều người có thể nghĩ rằng bản thân có thể dễ dàng trả lời câu hỏi này

Ví dụ, câu trả lời

*Process là đơn vị phân bổ tài nguyên của hệ điều hành. Thread là đơn gị cơ bản để lặp lịch. Và các threads trong cùng một process thì cùng chia sẽ tài nguyên của process đó.*

Nghe có vẻ là đã đầy đủ nhưng sự thật là ta có hiểu được "chia sẽ tài nguyên" ở đây bản chất của nó là gì hay không?

Cụ thể hơn:
- Những tài nguyên nào được chia sẽ giữa các thread?
- "Chia sẽ" ở đây có nghĩa là gì?
- Và cơ chế nào giúp việc chia sẽ này trở nên khả thi trong hệ thống?

Nếu bạn chưa trả lời được những câu hỏi này một cách rõ ràng thì bài viết này dành cho bạn.

## 📂 Mục lục
### 1. Thread-private resources
Về bản chất, thread chính là quá trình thực thi hàm.

Mỗi thread đều bắt đầu từ một điểm nhập (entry function) — đây là hàm mà CPU sẽ gọi đầu tiên để khởi động thread.

Khi một chương trình tạo ra một thread, hệ điều hành sẽ lên lịch để CPU bắt đầu thực thi từ hàm này. Quá trình thực thi đó tạo thành một "luồng thực thi" (execution thread), và chúng ta thường đặt cho nó một cái tên hoặc một định danh — đó chính là "thread" mà ta nói đến.

> Vì bản chất của việc chạy thread là thực thi hàm, vậy khi thực thi hàm thì chứa những thông tin gì❓
{: .prompt-info }

Trong bài viết "[Một hàm sẽ như thế nào trong bộ nhớ khi nó đang chạy?](https://nguyen-dang-trieu.github.io/posts/function-in-memory/)", tôi đã nói các thông tin của một hàm khi chạy sẽ được lưu trữ trong khung ngăn xếp (frame stack) chẳng hạn như biến cục bộ, tham số, dịa chỉ trả về của lệnh gọi hàm, ... 

Giả sử hàm A gọi hàm B.

Hình ảnh
![](/assets/articles/2025/FunctionInMemory/8.png){: .normal }

Ngoài ra, thông tin về việc CPU đang thực thi lệnh nào được lưu trong một thanh ghi đặc biệt gọi là bộ đếm chương trình **(Program Counter – PC)**. Thanh ghi này cho biết lệnh tiếp theo cần thực thi là gì.

Do hệ điều hành có thể tạm dừng và chuyển đổi giữa các luồng (context switching) bất kỳ lúc nào, nên hệ thống cần lưu lại giá trị của bộ đếm chương trình để biết luồng đã tạm dừng ở đâu, và có thể khôi phục lại đúng vị trí đó khi luồng được chạy tiếp.

Bên cạnh đó, vì luồng thực chất là việc thực thi một hàm, nên trong quá trình chạy, luồng sẽ sử dụng một ngăn xếp riêng (stack) để lưu các thông tin như biến cục bộ, địa chỉ trả về, tham số hàm,…

Chính vì vậy, mỗi luồng sẽ có một vùng ngăn xếp độc lập, nhằm đảm bảo rằng hoạt động của một luồng không làm ảnh hưởng tới các luồng khác.

Hình ảnh

Ngoài ra, khi một hàm đang chạy trong luồng, CPU sẽ cần sử dụng một số thanh ghi để lưu trữ tạm thời thông tin như biến cục bộ, tham số hàm hoặc địa chỉ trả về. Những thanh ghi này là riêng cho từng luồng, tức là một luồng không thể truy cập hay can thiệp vào thanh ghi của luồng khác. 

Câu "những thanh ghi là riêng cho từng luồng" có nghĩa là

- *Khi một luồng đang được CPU thực thi, nó sử dụng các thanh ghi như R1, R2... để lưu trữ thông tin tạm thời. Nhưng khi CPU chuyển sang thực thi luồng khác, hệ điều hành sẽ lưu lại nội dung của các thanh ghi đó (liên quan đến luồng trước), và nạp vào giá trị mới tương ứng với luồng kế tiếp.*

- *Điều này tạo ra ảo tưởng rằng mỗi luồng có các thanh ghi **"riêng biệt"** của nó, dù trên thực tế chỉ có một tập thanh ghi vật lý duy nhất trong CPU*

Từ những phân tích trước, ta có thể thấy rằng: ngăn xếp (stack), bộ đếm chương trình (Program Counter), con trỏ ngăn xếp (Stack Pointer), các thanh ghi của CPU được sử dụng trong quá trình thực thi … tất cả đều là tài nguyên riêng tư của mỗi luồng -> Toàn bộ các thông tin đó được gọi chung là ngữ cảnh luồng **(thread context)**.

Chúng ta cũng đã đề cập rằng hệ điều hành có thể tạm dừng một luồng bất cứ lúc nào trong quá trình lập lịch, và sau đó tiếp tục thực thi luồng đó sau khi đã xử lý xong các luồng khác. Việc lưu lại và khôi phục ngữ cảnh luồng chính là cơ chế cho phép điều này xảy ra một cách chính xác.

Đến đây, bạn đã có thể **phân biệt rõ ràng tài nguyên nào là riêng tư của luồng** và vì sao chúng đóng vai trò quan trọng trong việc hệ điều hành quản lý đa luồng.

## 🔍 Reference
- https://cloud.tencent.com/developer/article/1768025
