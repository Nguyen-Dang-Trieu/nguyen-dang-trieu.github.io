---
title: Buffer IO, Direct IO, mmap()
date: 2025-1-21
categories: [Computer]
tags: [ASM]
author: Trieu
---


![Format File Hex](/assets/articles/2025/Buffer_IO_Direct_IO_mmap/2025-1-21-LinuxIO.PNG){: .normal }

Trong Linux, I/O của file được chia làm 2 loại
-	Buffer I/O
-	Direct I/O

Buffer I/O (I/O bộ đệm)
Buffer I/O, còn được gọi là standard I/O, là cơ chế I/O mặc định của hầu hết các hệ thống tệp. Trong cơ chế này, dữ liệu được lưu trữ tạm thời trong Page Cache (bộ đệm trang) của kernel để tối ưu hóa các thao tác đọc/ghi. Hai lệnh gọi hệ thống thường được sử dụng là read() và write():
1.	Hoạt động read():
o	Nếu dữ liệu đã tồn tại trong Page Cache, dữ liệu sẽ được đọc trực tiếp và trả về cho ứng dụng.
o	Nếu không, dữ liệu sẽ được đọc từ đĩa, lưu vào Page Cache và sau đó sao chép từ Page Cache sang ứng dụng (application).
2.	Hoạt động write():
o	Khi write() được gọi, dữ liệu trước tiên được sao chép từ không gian người dùng (user-space) sang Page Cache trong kernel-space.
o	Sau đó, hệ điều hành sẽ định kỳ ghi dữ liệu từ Page Cache xuống đĩa theo cơ chế delayed write (ghi trễ).
Ưu điểm của Buffer I/O:
•	Tăng hiệu suất đọc/ghi: Bộ đệm trang giảm số lần đọc/ghi trực tiếp từ đĩa, cải thiện tốc độ.
•	Bảo vệ thiết bị lưu trữ: Nhờ cơ chế ghi trễ, Buffer I/O giảm thiểu việc ghi đĩa không cần thiết.
Tuy nhiên, Buffer I/O cũng có nhược điểm. Do Page Cache nằm trong kernel-space, dữ liệu cần được sao chép giữa kernel-space và user-space. Điều này tạo ra hai bản sao dữ liệu, làm tăng chi phí xử lý.
________________________________________
Tệp ánh xạ bộ nhớ (Memory-mapped File với mmap())
Để giảm chi phí sao chép giữa kernel-space và user-space, hệ thống tệp cung cấp một cơ chế đặc biệt gọi là memory-mapped file thông qua hàm mmap(). Khi sử dụng mmap():
•	Một vùng bộ nhớ được ánh xạ trực tiếp từ file hệ thống vào không gian địa chỉ của ứng dụng.
•	Việc đọc/ghi vào vùng nhớ này sẽ được chuyển thành các thao tác đọc/ghi file tương ứng.
•	Khi tiến trình thoát, các thay đổi trong bộ nhớ (dirty pages) sẽ được tự động ghi xuống file trên đĩa.
Cơ chế này loại bỏ một lớp sao chép dữ liệu, giúp tăng hiệu suất cho các ứng dụng yêu cầu truy cập dữ liệu lớn hoặc liên tục.

Direct I/O (I/O trực tiếp)
Direct I/O cho phép dữ liệu được truyền trực tiếp giữa bộ nhớ của ứng dụng (user-space memory) và thiết bị lưu trữ mà không thông qua Page Cache. Đây là cơ chế lý tưởng cho các ứng dụng như hệ thống quản lý cơ sở dữ liệu (Database Management Systems - DBMS), nơi hiệu suất và tính chính xác của dữ liệu được ưu tiên cao.
Lợi ích của Direct I/O:
•	Loại bỏ chi phí xử lý của Page Cache.
•	Ứng dụng có thể quản lý bộ nhớ đệm riêng, tối ưu hóa hiệu suất dựa trên đặc thù của dữ liệu.
Ví dụ:
Trong các DBMS, cơ sở dữ liệu thường sử dụng cơ chế cache riêng vì chúng "hiểu" dữ liệu hơn hệ điều hành. Điều này giúp tận dụng bộ nhớ hiệu quả hơn và giảm tải cho kernel.
