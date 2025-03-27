---
title: Static Library & Dynamic Library in C++
date: 2025-3-27
categories: [Linux, Operating System]
tags: [C++]
author: Trieu
---

## Thư viện là gì ?
Thư viện là những đoạn mã đã có sẵn, hoàn thiện và có thể tái sử dụng được. Trong thực tế, mọi chương trình đều phụ thuộc vào nhiều thư viện cơ bản bên dưới. Không phải ai cũng có thể bắt đầu viết code từ đầu, vì vậy sự tồn tại của thư viện có ý nghĩa vô cùng quan trọng.

Về cơ bản, thư viện là dạng nhị phân của mã thực thi có thể được tải vào bộ nhớ và được hệ điều hành thực thi. Có 2 loại thư viện: **thư viện tĩnh - Static Library**
(`.a`, `.lib`) và **thư viện động - Dynamic Library** (`.so`, `.dll`).

Cái gọi là tĩnh và động là đề cập đến sự liên kết. Chúng ta cùng xem lại các bước để biên dịch 1 chương trình thành 1 file thực thi

![](/assets/articles/2025/Static_Dynamic_Library/2025-3-27-image_1.png){: .normal }

## Static Library
Cách tạo thư viện tĩnh trên window: https://gist.github.com/RednibCoding/e89438bee48ce2a74ac18d5bb5cc2e0a


Lý do gọi là "thư viện tĩnh" là vì trong giai đoạn liên kết, các file `.o` (được tạo bởi compiler) sẽ được liên kết cùng với thư viện tĩnh và đóng gói thành một file thực thi. Do đó, phương pháp liên kết này được gọi là liên kết tĩnh.

Hãy tưởng tượng rằng thư viện tĩnh được liên kết cùng với các file `.o` để tạo ra tệp thực thi. Như vậy, thư viện tĩnh chắc chắn có định dạng tương tự như `.o`. Trên thực tế, có thể hiểu đơn giản rằng một thư viện tĩnh là một tập hợp các file `.o`/`.obj`, được nén và đóng gói lại thành một file duy nhất.

Tính năng của thư viện tĩnh
- Thư viện tĩnh được liên kết với chương trình trong giai đoạn biên dịch.
- Khi chương trình chạy, nó không còn liên quan đến thư viện nữa, giúp dễ dàng di chuyển và sử dụng trên các hệ thống khác.
- Tốn dung lượng và tài nguyên, vì tất cả các tệp mục tiêu liên quan và thư viện được liên kết thành một tệp thực thi duy nhất.

Bây giờ ta sẽ viết 1 thư viện đơn giản và biên dịch dịch file đó thành thư viện tĩnh trên linux

File `StaitcMath.h`
~~~cpp
#pragma once
class StaticMath
{
public:
    StaticMath(void);
    ~StaticMath(void);

    static double add(double a, double b);
    static double sub(double a, double b);
    static double mul(double a, double b);
    static double div(double a, double b);

    void print();
};
~~~
Trong linux, sử dụng `ar` để tạo thư viện tĩnh (`.a`). Còn trên window, sử dụng `lib.exe` để tạo thư viện tĩnh (`.lib`). Khi tạo thư viện tĩnh, các file `.o` được sắp xếp, lập chỉ mục để dễ dàng tra cứu trong quá trình liên kết.

![](/assets/articles/2025/Static_Dynamic_Library/2025-3-27-image_2.png){: .normal }
