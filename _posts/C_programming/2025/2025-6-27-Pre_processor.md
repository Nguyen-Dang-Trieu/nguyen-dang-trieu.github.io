---
title: Enum
date: 2025-06-27
categories: [C]
tags: []
author: Trieu
---

## ✅ Những directive phổ biến trong C
| Directive                         | Use                                              | 
| :-------------------------------- | :----------------------------------------------- | 
| `#define`                         | Định nghĩa hằng số or macro                      | 
| `#undef`                          | Hủy định nghĩa đã dùng `#define`                 |      
| `#include`                        | Thêm file header                                 |   
| `#if`, `#elif`, `#else`, `#endif` | Điều kiện compiler                               |   
| `#ifdef`, `#ifndef`               | Kiểm tra xem macro đã được định nghĩa hay chưa ? | 
| `#error`                          | Hiện thị lỗi khi compiler                        | 
| `#pragma`                         | Gửi chỉ dẫn đặc biệt tới compiler                | 

🧪 **Ví dụ 1:** `#if`, `#elif`, `#else`, `#endif`
~~~c
#define VERSION 2

#if VERSION == 1
    printf("Version 1\n");
#elif VERSION == 2
    printf("Version 2\n");
#else
    printf("Unknown version\n");
#endif
~~~

🧪 **Ví dụ 2:** `#ifdef` , `#ifndef`
~~~c
#define DEBUG

#ifdef DEBUG
    printf("Debug mode\n");
#endif

#ifndef RELEASE
    printf("Not release mode\n");
#endif
~~~

🧪 **Ví dụ 3:** `#error`
~~~c
#include <stdio.h>

#define VERSION 1

#ifndef VERSION
    #error "You need to define macro VERSION before compiler!"
#endif

int main() {
    printf("Version: %d\n", VERSION);
    return 0;
}
~~~

Output: Khi chưa `#define VERSION 1`
~~~
error: #error "You need to define macro VERSION before compiler!"
~~~


### Reference
- https://www.math.utah.edu/docs/info/cpp_1.html
- https://en.wikipedia.org/wiki/C_preprocessor
