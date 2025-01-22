---
title: Buffer IO, Direct IO, mmap()
date: 2025-1-21
categories: [Linux]
tags: [IO]
author: Trieu
image:
  path: assets/articles/2025/Buffer_IO_Direct_IO_mmap/2025-1-21-header.png
  alt: Buffer IO
---

## 🍃 Lời mở đầu
*"Trong Linux, dữ liệu không chỉ được đọc hoặc ghi trực tiếp từ ổ đĩa. Thay vào đó, hệ điều hành sử dụng các cơ chế đặc biệt như Buffer I/O và Direct I/O để cân bằng giữa tốc độ và hiệu suất. Vậy chúng khác nhau như thế nào, và khi nào nên dùng cơ chế nào? Hãy cùng khám phá!"* 🧐

![LinuxIO](/assets/articles/2025/Buffer_IO_Direct_IO_mmap/2025-1-21-LinuxIO.PNG){: .normal }

Hình ảnh này được đơn giản hóa như sau:
![Simple Linux io](/assets/articles/2025/Buffer_IO_Direct_IO_mmap/2025-1-22-SimpleLinuxIO.png){: .normal }
_Simple Linux IO_

> Để tăng hiệu suất đọc và ghi tệp, nhân Linux chia tệp thành nhiều khối dữ liệu nhỏ với kích thước bằng kích thước page (thường là 4KB). Khi người dùng thực hiện thao tác đọc hoặc ghi lên một khối dữ liệu trong tệp, nhân Linux đầu tiên dành một vùng nhớ trong RAM (được gọi là **Page Cache** - **bộ đệm trang**) để liên kết với khối dữ liệu đó. Cơ chế này giúp giảm tần suất truy cập trực tiếp vào ổ đĩa, từ đó cải thiện tốc độ xử lý dữ liệu.
{: .prompt-info }

Trong Linux, I/O của file được chia làm 2 loại:
-	**Buffer I/O**
-	**Direct I/O**

### 1. Buffer I/O 
Buffer I/O, còn được gọi là standard I/O, là cơ chế I/O mặc định của hầu hết các hệ thống tệp. Trong cơ chế này, dữ liệu được lưu trữ tạm thời trong Page Cache của kernel để tối ưu hóa các thao tác đọc/ghi. Hai lệnh gọi hệ thống thường được sử dụng là `read()` và `write()`:
- Quá trình **read()**: Nếu dữ liệu đã tồn tại trong Page Cache, dữ liệu sẽ được đọc trực tiếp và trả về cho ứng dụng (user-space). Nếu không, dữ liệu sẽ được đọc từ đĩa (disk), lưu vào Page Cache và sau đó sao chép từ Page Cache sang ứng dụng (user-space).
- Quá trình **write()**: Khi write() được gọi, dữ liệu trước tiên được sao chép từ ứng dụng (user-space) sang Page Cache trong kernel-space. Sau đó, hệ điều hành sẽ định kỳ ghi dữ liệu từ Page Cache xuống đĩa (disk) theo cơ chế delayed write (ghi trễ).
    
👍 **Ưu điểm của Buffer I/O:**
- Tăng hiệu suất đọc/ghi: Bộ đệm trang giảm số lần đọc/ghi trực tiếp từ đĩa, cải thiện tốc độ.
- Bảo vệ thiết bị lưu trữ: Nhờ cơ chế ghi trễ, Buffer I/O giảm thiểu việc ghi đĩa không cần thiết.
  
⛓️‍💥 Tuy nhiên, Buffer I/O cũng có nhược điểm. Do Page Cache nằm trong kernel-space, dữ liệu cần được sao chép giữa kernel-space và user-space. Điều này tạo ra hai bản sao dữ liệu, làm tăng chi phí xử lý.

### 2. Direct I/O 
Direct I/O cho phép dữ liệu được truyền trực tiếp giữa bộ nhớ của ứng dụng (user-space memory) và thiết bị lưu trữ (ổ đĩa, SSD, v.v.), bỏ qua Page Cache. Điều này khác với Buffer I/O, nơi dữ liệu phải đi qua Page Cache trước khi đến ứng dụng hoặc ổ đĩa.

👍 **Ưu điểm của Direct I/O:**
- Loại bỏ chi phí xử lý của Page Cache. (Những ứng dụng cho lượng dữ liệu lớn)
- Ứng dụng có thể quản lý bộ nhớ đệm riêng, tối ưu hóa hiệu suất dựa trên đặc thù của dữ liệu.

### 3. Memory-mapped File với mmap()
Để giảm chi phí sao chép giữa kernel-space và user-space, hệ thống tệp cung cấp một cơ chế đặc biệt gọi là memory-mapped file thông qua hàm `mmap()`. Khi sử dụng mmap():
- Một vùng bộ nhớ được ánh xạ trực tiếp từ file hệ thống vào không gian địa chỉ của người dùng (user-space).
- Việc đọc/ghi vào vùng nhớ này sẽ được chuyển thành các thao tác đọc/ghi file tương ứng.
- Khi tiến trình thoát, các thay đổi trong bộ nhớ (dirty pages) sẽ được tự động ghi xuống file trên đĩa.

Cơ chế này loại bỏ một lớp sao chép dữ liệu, giúp tăng hiệu suất cho các ứng dụng yêu cầu truy cập dữ liệu lớn hoặc liên tục.

## 🎍 Lời kết 
- Hi vọng qua bài này, ta sẽ có cái nhìn tổng quan hơn khi làm việc với dữ liệu.
