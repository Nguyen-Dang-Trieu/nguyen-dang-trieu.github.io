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

Hình ảnh

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

Hình ảnh

**Hệ điều hành cung cấp cơ chế ánh xạ địa chỉ ảo sang địa chỉ vật lý.**

Khi các chương trình chạy, mỗi chương trình sẽ sử dụng các địa chỉ ảo riêng biệt. Nếu một chương trình muốn truy cập địa chỉ ảo này, hệ điều hành sẽ tự động chuyển đổi nó thành một địa chỉ
vật lý tương ứng trong bộ nhớ. Nhờ cơ chế này, các chương trình khác nhau sẽ ghi dữ liệu vào các địa chỉ vật lý khác nhau, tránh được xung đột dữ liệu khi chạy đồng thời.

Vì vậy, có hai loại địa chỉ bộ nhớ quan trọng:
- Địa chỉ mà chương trình của chúng ta sử dụng được gọi là ***địa chỉ bộ nhớ ảo (Virtual Address)***.
- Địa chỉ thực tế tồn tại trên phần cứng của máy tính được gọi là ***địa chỉ bộ nhớ vật lý (Physical Address)***.

**Hệ điều hành giới thiệu khái niệm bộ nhớ ảo và địa chỉ ảo.**

Khi một chương trình muốn truy cập bộ nhớ, địa chỉ ảo mà nó sử dụng sẽ được chuyển đổi thành địa chỉ vật lý thông qua cơ chế ánh xạ của Bộ quản lý bộ nhớ **(MMU - Memory Management Unit)**
tích hợp trong chip CPU. Sau khi chuyển đổi, hệ thống sẽ truy cập bộ nhớ thực tế thông qua địa chỉ vật lý này. Quy trình này được minh họa trong hình sau:

Hình ảnh

> Hệ điều hành quản lý mối quan hệ giữa địa chỉ ảo và địa chỉ vật lý như thế nào?
{: .prompt-info }

Để thực hiện việc này, hệ điều hành sử dụng hai cơ chế chính: **Phân đoạn bộ nhớ (Memory Segmentation)** và **Phân trang bộ nhớ (Memory Paging)**.

Trước tiên, chúng ta sẽ tìm hiểu về cơ chế **phân đoạn bộ nhớ - Memory Paging**.

