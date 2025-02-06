---
title: Tại sao sử dụng bộ nhớ ảo (Virtual Memory) ?
date: 2025-2-5
categories: [Linux, Operating System]
tags: [Virtual Memory]
author: Trieu

---
## Bộ nhớ ảo(Virtual Memory)
Nếu bạn là sinh viên chuyên ngành điện tử, chắc hẳn bạn sẽ phải làm quen với việc lập trình trên bộ vi điều khiển tại trường đại học.

Không giống như các hệ thống có hệ điều hành, bộ vi điều khiển hoạt động trực tiếp mà không có lớp trung gian hỗ trợ. Mỗi khi viết mã, bạn cần nạp chương trình vào bộ vi điều khiển thông qua
các công cụ chuyên dụng (mạch nạp) để chương trình có thể thực thi.

Bên cạnh đó, **CPU của bộ vi điều khiển thao tác trực tiếp trên các địa chỉ bộ nhớ vật lý**, giúp việc truy cập và xử lý dữ liệu diễn ra nhanh chóng nhưng cũng đòi hỏi lập trình viên hiểu rõ  về cấu trúc phần cứng.

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-5-Virtual_Memory_1.png){: .normal }

**Bạn không thể chạy hai chương trình cùng lúc trong trường hợp này.**
Giả sử cả hai chương trình đều cần sử dụng cùng một vị trí bộ nhớ, ví dụ địa chỉ `0x2000`. Khi chương trình đầu tiên ghi dữ liệu mới vào địa chỉ này, nó sẽ vô tình xóa dữ liệu mà chương trình 
thứ hai đã lưu trước đó. Điều này khiến dữ liệu bị chồng chéo, gây lỗi và làm cả hai chương trình bị treo hoặc hoạt động sai.

> Hệ điều hành đã giải quyết vấn đề này như thế nào ?
{: .prompt-info }

Vấn đề quan trọng ở đây là cả hai chương trình đều truy cập trực tiếp vào địa chỉ bộ nhớ vật lý tuyệt đối. Đây chính là điều cần tránh vì nó dễ gây ra xung đột dữ liệu khi nhiều chương 
trình cùng sử dụng một vùng nhớ.

Chúng ta có thể **“cô lập”** vùng nhớ mà mỗi chương trình sử dụng. Cụ thể, hệ điều hành sẽ cấp cho mỗi chương trình một vùng **“địa chỉ ảo”** riêng biệt. Nhờ đó, mỗi chương trình đều có không 
gian bộ nhớ riêng để **“thoải mái sử dụng”** mà không lo xung đột với chương trình khác.
Tuy nhiên, để làm được điều này, có một điều kiện quan trọng: **các chương trình không thể truy cập trực tiếp vào địa chỉ bộ nhớ vật lý**.
Cách mà địa chỉ ảo được ánh xạ sang bộ nhớ vật lý là hoàn toàn minh bạch đối với chương trình, vì hệ điều hành đã xử lý và sắp xếp mọi thứ ở phía sau.

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-5-Virtual_Memory_2.png){: width="500" height="200" }

**Hệ điều hành cung cấp cơ chế ánh xạ địa chỉ ảo sang địa chỉ vật lý.**

Khi các chương trình chạy, mỗi chương trình sẽ sử dụng các địa chỉ ảo riêng biệt. Nếu một chương trình muốn truy cập địa chỉ ảo này, hệ điều hành sẽ tự động chuyển đổi nó thành một địa chỉ
vật lý tương ứng trong bộ nhớ. Nhờ cơ chế này, các chương trình khác nhau sẽ ghi dữ liệu vào các địa chỉ vật lý khác nhau, tránh được xung đột dữ liệu khi chạy đồng thời.

Vì vậy, có hai loại địa chỉ bộ nhớ quan trọng:
- Địa chỉ mà chương trình của chúng ta sử dụng được gọi là ***địa chỉ bộ nhớ ảo (Virtual Address)***.
- Địa chỉ thực tế tồn tại trên phần cứng của máy tính được gọi là ***địa chỉ bộ nhớ vật lý (Physical Address)***.

**Hệ điều hành giới thiệu khái niệm bộ nhớ ảo và địa chỉ ảo.**

Khi một chương trình muốn truy cập bộ nhớ, địa chỉ ảo mà nó sử dụng sẽ được chuyển đổi thành địa chỉ vật lý thông qua cơ chế ánh xạ của Bộ quản lý bộ nhớ **(MMU - Memory Management Unit)**
tích hợp trong chip CPU. Sau khi chuyển đổi, hệ thống sẽ truy cập bộ nhớ thực tế thông qua địa chỉ vật lý này. Quy trình này được minh họa trong hình sau:

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-5-Virtual_Memory_3.png){: .normal }

> Hệ điều hành quản lý mối quan hệ giữa địa chỉ ảo và địa chỉ vật lý như thế nào?
{: .prompt-info }

Để thực hiện việc này, hệ điều hành sử dụng hai cơ chế chính: **Phân đoạn bộ nhớ (Memory Segmentation)** và **Phân trang bộ nhớ (Memory Paging)**.

Trước tiên, chúng ta sẽ tìm hiểu về cơ chế phân đoạn bộ nhớ.

## Phân đoạn bộ nhớ - Memory Segmentation
Chương trình thường được chia thành các phân đoạn logic khác nhau, chẳng hạn như phân đoạn mã (code segment), phân đoạn dữ liệu (data segment), phân đoạn ngăn xếp (stack segment) và phân đoạn đống (heap segment). Mỗi phân đoạn có các đặc điểm và chức năng riêng biệt, vì vậy việc sử dụng phân đoạn giúp tách biệt và quản lý các thành phần này một cách hiệu quả.

> Địa chỉ ảo và địa chỉ vật lý được ánh xạ như thế nào theo cơ chế phân đoạn ?
{: .prompt-info }

Địa chỉ ảo theo cơ chế phân đoạn gồm hai phần: hệ số lựa chọn phân đoạn (segment selector) và offset. Hệ số lựa chọn phân đoạn xác định phân đoạn cụ thể trong bộ nhớ, còn offset cho biết vị trí chính xác trong phân đoạn đó.

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-6-Memory_Seg_1.png){: .normal }

✅ Yếu tố lựa chọn phân đoạn và độ lệch trong phân đoạn

1️⃣ Bộ chọn phân đoạn (Segment Selector)
- Bộ chọn phân đoạn được lưu trong thanh ghi phân đoạn (segment register).
- Thành phần quan trọng nhất của bộ chọn là số phân đoạn (segment number), dùng để tra cứu bảng phân đoạn (segment table).
- Bảng phân đoạn (segment Table) chứa thông tin về phân đoạn, bao gồm:
  - Segment Base Address - Điểm bắt đầu của phân đoạn trong bộ nhớ.
  - Segment Boundaries - Giới hạn kích thước phân đoạn.
  - Privilege Level (DPL) - Quyền truy cập vào phân đoạn.
    
2️⃣ Độ lệch phân đoạn (Segment Offset)

- Độ lệch (offset) là vị trí tương đối của dữ liệu bên trong phân đoạn. Giá trị offset phải nằm trong khoảng từ **0 -> Segment Boundaries**.
- Nếu offset hợp lệ, địa chỉ vật lý được tính bằng: Physical Address = Segment Base Address + Offset
- Nếu offset vượt quá giới hạn, hệ thống sẽ báo lỗi **Segmentation Fault**.

Ở trên, chúng ta biết rằng địa chỉ ảo được ánh xạ tới địa chỉ vật lý thông qua **Segment Table**. Cơ chế phân đoạn chia địa chỉ ảo của chương trình thành **4 segment**. Mỗi segment có một mục tương ứng trong **Segment Table**. Mỗi mục trong Segment Table chứa Base Address của Segment. Khi một chương trình muốn truy cập vào một địa chỉ ảo, Base Address của segment được tìm trong bảng, sau đó cộng với **offset** để xác định địa chỉ thực tế trong bộ nhớ vật lý, như minh họa bên dưới:

📌 Physical Address = Segment Base Address + Offset

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-6-Memory_Seg_2.png){: .normal }

Nếu chúng ta muốn truy cập địa chỉ ảo tại vị trí offset = 500 trong segment 3, chúng ta có thể tính được địa chỉ vật lý = Base address + offset = 7000 + 500 = 7500

Phương pháp phân đoạn rất tốt, nó giải quyết được vấn đề là bản thân chương trình không cần quan tâm đến địa chỉ bộ nhớ vật lý cụ thể, nhưng nó cũng có một số nhược điểm ❌:
- 1️⃣ Phân mảnh bộ nhớ. (Memory Fragmentation)
- 2️⃣ Hiệu suất trao đổi bộ nhớ thấp.
  
Tiếp theo, ta hãy nói về lý do tại sao lại phát sinh hai vấn đề này.
