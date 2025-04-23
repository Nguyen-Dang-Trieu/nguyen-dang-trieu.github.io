---
title: Một hàm sẽ như thế nào trong bộ nhớ khi nó đang chạy?
date: 2025-4-23
categories: [System]
tags: [function]
author: Trieu
---

## 🌱 Lời mở đầu
Khi một chương trình bắt đầu chạy, hệ điều hành sẽ tạo ra một tiến trình (process). Mỗi tiến trình có thể chứa nhiều luồng (thread), và chính các luồng này chịu trách 
nhiệm thực thi các đoạn mã bên trong chương trình.

Ở cấp độ này – process và thread – ta có thể quan sát chương trình đang làm gì từ góc nhìn hệ thống. Nhưng khi cần hiểu sâu hơn, chẳng hạn: 
- Tại sao lỗi segmentation fault xảy ra?
- Stack overflow là gì?

Để trả lời những câu hỏi đó, ta cần tiếp tục đào sâu vào bên trong. Bên trong mỗi thread là hàng loạt lệnh gọi hàm – function call. Và chính cơ chế gọi hàm là nơi diễn 
ra các hoạt động quan trọng: cấp phát stack frame, lưu địa chỉ trả về, quản lý biến cục bộ...

Bài viết này sẽ giúp bạn hiểu rõ hơn cách một hàm được gọi, cách chương trình luân chuyển quyền điều khiển, và điều gì thực sự diễn ra bên trong bộ nhớ khi một chương 
trình chạy.

## Stack frame
Chắc hẳng ai trong số chúng ta cũng từng chơi 1 tựa game nào đó. Để có thể tiến về phía trước thì ta cần phải hoàn thành những nhiệm vụ chính, bên cạnh những nhiệm vụ chính đó còn có nhiều nhiệm vụ phụ khác mà bạn cần phải hoàn thành trước.
Điều này có nghĩa là gì? Tôi sẽ cho bạn một ví dụ để giúp bạn hiểu rõ.

Giả sử ta có nhiệm vụ chính là `task A` phụ thuộc vào `task B, C`. Nghĩa là cần phải hoàn thành `task B, C` trước thì `task A` mới có thể tiếp tục.
Và `task B` lại phụ thuộc vào `task D` khác.

Mối quan hệ của toàn bộ `task` được thể hiện như trong hình.

![](/assets/articles/2025/FunctionInMemory/1.png){: .normal }

Bây giờ, ta sẽ mô phỏng quá trình hoàn thành nhiệm vụ.
Trước hết ta sẽ thực thi `task A`.

![](/assets/articles/2025/FunctionInMemory/2.png){: .normal }

Trong quá trình thực thi `task A`, ta thấy `task A` phụ thuộc vào `task B`. Nên ta buộc phải tạm dừng `task A` và chuyển sang thực `task B`.

![](/assets/articles/2025/FunctionInMemory/3.png){: .normal }

Khi thực hiện `task B`, ta lại thấy `task B` phụ thuộc vào `task D`. Và ta dừng `task B` và chuyển sang thực thi `task D`.

![](/assets/articles/2025/FunctionInMemory/4.png){: .normal }

Khi thực thi `task D`, ta thấy `task D` không phục thuộc vào task nào hết. Cho nên sau khi thực thi xong `task D` ta sẽ quay lại nhiệm vụ trước đó là `task B`.

![](/assets/articles/2025/FunctionInMemory/3.png){: .normal }

Và sau khi `task B` được thi xong sẽ trở về thực thi `task A`.

![](/assets/articles/2025/FunctionInMemory/2.png){: .normal }

Bây giờ ta lại quay về `task A`, `task B` đã xong. Tiếp theo cần hoàn thành là `task C`.

![](/assets/articles/2025/FunctionInMemory/5.png){: .normal }

Giống như `task D`, `task C` không còn phụ thuộc vào task nào cả. Sau khi task C hoàn thành sẽ quay lại `task A`. Và sau đó `task A` sẽ được thực thi hoàn toàn.
Hãy cùng xem xét tiến trình hoạt động của toàn bộ nhiệm vụ:

![](/assets/articles/2025/FunctionInMemory/6.png){: .normal }

Nếu bạn để ý kỹ, bạn sẽ thấy rằng các task tuân theo nguyên tắc **Last In, First Out**, rất phù hợp để quản lý bằng một cấu trúc dữ liệu như **ngăn xếp (stack)**.

Nguyên tắc tương tự cũng áp dụng cho những lệnh gọi hàm. Nếu bạn thay thế **task A, B, C, D** thành **function A, B, C, D** thì bản chất vẫn không thay đổi.

Do đó bây giờ ta biết được cấu trúc ngăn xếp dùng để lưu trữ thông tin về gọi hàm.

Giống như mọi nhiệm vụ của trò chơi, mỗi hàm cũng có 1 hộp nhỏ riêng dùng để lưu trữ những thông tin cần thiết khi chạy. Những chiếc hộp nhỏ này được sắp xếp theo cấu trúc xếp chồng lên nhau. **Tạo thành khung ngăn xếp (stack frame)**. Hộp nhỏ này là bộ nhớ bị hàm chiếm dụng khi chạy.

> Vậy thông tin gì sẽ có khi một hàm được gọi?.
{: .prompt-info }

Khi `func A` gọi `func B`, CPU sẽ ngừng thực thi các lệnh của A và chuyển sang thực thi các lệnh của B. Hành động này gọi là chuyển quyền điều khiển từ A sang B.
Nói đơn giản, "quyền điều khiển" ở đây chính là quyền điều khiển luồng thực thi — tức là CPU sẽ chạy đoạn mã nào tiếp theo.
Để thực hiện quá trình chuyển quyền điều khiển này, CPU cần hai thông tin quan trọng:
- Từ đâu đến? (Địa chỉ lệnh trong hàm A — để biết sau khi hàm B chạy xong thì quay lại đâu)
-	Đi đến đâu? (Địa chỉ bắt đầu của hàm B — nơi CPU cần nhảy đến để thực thi lệnh đầu tiên của B)
  
Bạn có thể hình dung giống như đi du lịch:
- Bạn cần biết điểm đến (để biết phải đi đâu)
- Và bạn cũng cần nhớ địa chỉ nhà (để biết đường quay về sau khi chơi xong)
  
Lệnh gọi hàm cũng hoạt động như vậy.

Hai thông tin này đủ để CPU bắt đầu thực hiện các lệnh máy tương ứng với `func B` và nhảy trở lại `func A` khi `func B` hoàn tất.

> Vậy thông tin này được thu thập và lưu giữ như thế nào?
{: .prompt-info }

Bây giờ chúng ta có thể mở chiếc hộp nhỏ này ra và xem cách sử dụng nó.

Giả sử `func A` gọi `func B`, như thể hiện trong hình:

![](/assets/articles/2025/FunctionInMemory/7.png){: .normal }

Hiện tại, CPU đang thực thi một lệnh máy trong `func A`, tại địa chỉ `0x400564`. Ngay sau đó, CPU gặp lệnh:
~~~asm
call 0x400540
~~~

> Vậy lệnh máy này có ý nghĩa gì?
{: .prompt-info }

Lệnh `call` trong hợp ngữ tương ứng với một lệnh gọi hàm trong mã nguồn cấp cao. Nó ra lệnh cho CPU chuyển quyền điều khiển đến địa chỉ `0x400540` — chính là địa chỉ bắt đầu của `func B`. Nếu bạn quan sát kỹ hình minh họa, bạn sẽ thấy địa chỉ `0x400540` thực sự là lệnh đầu tiên trong `func B`.

Như vậy, chúng ta đã trả lời được câu hỏi: **"CPU sẽ đi đâu sau khi gặp lệnh `call`?"**

> Nhưng một câu hỏi quan trọng hơn là: **"Làm sao CPU biết quay lại `func B\A` sau khi thực thi xong `func B`?"**
{: .prompt-info }

Câu trả lời nằm ở cơ chế hoạt động của lệnh `call`: trước khi nhảy đến `func B`, CPU sẽ đẩy địa chỉ của lệnh tiếp theo trong `func A` (sau call) vào ngăn xếp (stack). Nhờ vậy, khi `func B` thực thi xong và gặp lệnh `ret`, *CPU sẽ lấy lại địa chỉ từ stack và tiếp tục chạy nốt phần còn lại của `func A`.*

![](/assets/articles/2025/FunctionInMemory/8.png){: .normal }

Lúc này, `stack frame` cho `func A` lớn hơn một chút vì địa chỉ trả về đã thêm vào
Bây giờ CPU bắt đầu thực hiện các lệnh máy tương ứng với `func B`. 

> Lưu ý rằng `func B` cũng có hộp nhỏ riêng (khung ngăn xếp) mà bạn có thể đưa một số thông tin cần thiết vào.
{: .prompt-warning }


![](/assets/articles/2025/FunctionInMemory/9.png){: .normal }

Vậy nếu trong quá trình thực thi, `func B` lại tiếp tục gọi các hàm khác thì sao?
Thực ra, nguyên tắc vẫn giống như khi `func A` gọi `func B`: mỗi lần gọi hàm, CPU sẽ đẩy địa chỉ trả về vào stack để có thể quay lại đúng chỗ sau khi hàm con kết thúc.

Bây giờ, hãy xem xét lệnh máy `ret` — thường xuất hiện ở cuối hàm B. Chức năng của lệnh ret là yêu cầu CPU nhảy đến địa chỉ trả về đã được lưu trên stack (địa chỉ trong hàm A ngay sau lệnh call). Nhờ đó, sau khi thực thi xong hàm B, CPU có thể quay trở lại hàm A và tiếp tục thực thi phần còn lại.

Tới đây, chúng ta đã thấy cách hệ thống giải quyết được câu hỏi **"Từ đâu đến?"** trong quá trình chuyển giao quyền điều khiển giữa các hàm.

Truyền tham số và giá trị trả về trong lời gọi hàm
Khi gọi một hàm và nhận giá trị trả về, không chỉ tên hàm được sử dụng, mà còn có hai yêu cầu quan trọng: truyền tham số cho hàm và nhận lại giá trị trả về sau khi hàm hoàn tất. Vậy cơ chế này được thực hiện như thế nào?
Trong kiến trúc x86-64, quá trình truyền tham số và nhận giá trị trả về chủ yếu được thực hiện qua các thanh ghi (registers).
Giả sử hàm A gọi hàm B. Trước khi thực hiện lời gọi, hàm A sẽ ghi các tham số vào các thanh ghi được quy định trước. Khi CPU bắt đầu thực thi hàm B, nó sẽ đọc các giá trị tham số trực tiếp từ những thanh ghi này.
Tương tự, sau khi hoàn tất công việc, hàm B sẽ ghi giá trị trả về vào một thanh ghi cụ thể (thường là RAX trong x86-64). Khi CPU quay lại hàm A, giá trị trả về đã sẵn sàng trong thanh ghi để sử dụng.


Khi số lượng tham số vượt quá số lượng thanh ghi
Tuy nhiên, như bạn biết, số lượng thanh ghi là có hạn. Vậy chuyện gì xảy ra nếu hàm cần nhận nhiều tham số hơn số thanh ghi có thể chứa?
Trong trường hợp đó, các tham số dư ra sẽ được lưu vào ngăn xếp (stack). Cụ thể, chúng sẽ được đặt trong khung ngăn xếp (stack frame) của hàm gọi trước khi lệnh gọi hàm được thực hiện. Nhờ đó, hàm được gọi vẫn có thể truy cập đầy đủ tất cả các tham số – những cái nằm trong thanh ghi và cả những cái được lưu trên ngăn xếp.
Điều này cũng khiến cấu trúc của khung ngăn xếp ngày càng phong phú, không chỉ chứa địa chỉ trả về, giá trị cũ của thanh ghi mà còn có thể chứa các tham số bổ sung.


![](/assets/articles/2025/FunctionInMemory/10.png){: .normal }


Từ hình vẽ ta có thể thấy khi gọi hàm B, một số tham số được đặt trong khung ngăn xếp của hàm A, và địa chỉ trả về vẫn được lưu ở đầu khung ngăn xếp của hàm A.


Biến cục bộ
Chúng ta biết rằng các biến được định nghĩa bên trong một hàm được gọi là biến cục bộ. Khi hàm đang chạy, các biến này sẽ được lưu trữ ở đâu?
Thực tế, các biến cục bộ có thể được lưu trong các thanh ghi. Tuy nhiên, khi số lượng biến vượt quá khả năng lưu trữ của thanh ghi, chúng sẽ được đặt trong khung ngăn xếp (stack frame).
Vì vậy, nội dung của khung ngăn xếp sẽ trở nên phong phú hơn khi có nhiều biến cục bộ được sử dụng.


![](/assets/articles/2025/FunctionInMemory/11.png){: .normal }


Một số sinh viên cẩn thận có thể đặt ra câu hỏi như sau:
Chúng ta biết rằng thanh ghi là tài nguyên chia sẻ và có thể được sử dụng bởi mọi hàm. Vì vậy, nếu các biến cục bộ của hàm A được lưu trong thanh ghi, thì khi hàm A gọi hàm B, các biến cục bộ của hàm B cũng có thể ghi đè lên cùng những thanh ghi đó.
Vậy điều gì sẽ xảy ra?
Khi hàm B thực thi và ghi vào các thanh ghi, các giá trị ban đầu của biến cục bộ trong hàm A sẽ bị ghi đè. Sau khi hàm B kết thúc và quay lại hàm A, các giá trị cũ không còn nữa — điều này sẽ gây ra lỗi.
Để tránh điều này, trước khi ghi biến cục bộ vào thanh ghi, chương trình cần lưu lại giá trị gốc của thanh ghi. Sau khi hàm được gọi (ví dụ hàm B) kết thúc, chương trình sẽ khôi phục lại giá trị ban đầu của thanh ghi để đảm bảo các biến cục bộ của hàm gọi (ví dụ hàm A) vẫn đúng.
Câu hỏi đặt ra: Chúng ta lưu các giá trị gốc của thanh ghi ở đâu?
Một số bạn có thể đã đoán được: Đúng vậy, các giá trị đó được lưu trong khung ngăn xếp (stack frame) của hàm.

![](/assets/articles/2025/FunctionInMemory/12.png){: .normal }


## Lời kết


