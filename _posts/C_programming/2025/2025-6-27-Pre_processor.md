---
title: Pre-processor C
date: 2025-06-27
categories: [C]
tags: []
author: Trieu
---

## I. Những directive phổ biến trong C

| Directive                         | Use                                              | 
| :-------------------------------- | :----------------------------------------------- | 
| `#define`                         | Định nghĩa hằng số or macro                      | 
| `#undef`                          | Hủy định nghĩa đã dùng `#define`                 |      
| `#include`                        | Thêm file header                                 |   
| `#if`, `#elif`, `#else`, `#endif` | Điều kiện compiler                               |   
| `#ifdef`, `#ifndef`               | Kiểm tra xem macro đã được định nghĩa hay chưa ? | 
| `#error`                          | Hiện thị lỗi khi compiler                        | 
| `#pragma`                         | Gửi chỉ dẫn đặc biệt tới compiler                | 

## II. Cách sử dụng
### 1. `#if`, `#elif`, `#else`, `#endif`
**Cách dùng:** Kiểm tra giá trị của macro.
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

### 2. `#ifdef` , `#ifndef`
**Cách dùng:** Kiểm tra macro có được định nghĩa hay chưa
~~~c
#include <stdio.h>

#define DEBUG   // or #define DEBUG X (X là giá trị bất kì mà ta mong muốn)

int main()
{
    
#ifdef DEBUG    // CÓ define DEBUG thì mới thực thi code
    printf("Debug mode\n");
#endif

#ifndef RELEASE // KHÔNG define RELEASE thì mới thực thi code 
    printf("Not release mode\n");
#endif

    printf("End!");
    return 0;
}
~~~
**Output**
~~~
Debug mode
Not release mode
End!
~~~


Nếu 
~~~c
Bỏ    #define DEBUG
Thêm  #define RELEASE // or #define RELEASE X (X là giá trị bất kì mà ta mong muốn)
~~~
Thì Output: 
~~~
End!
~~~

Hoặc ta có thể sử dụng một cách viết khác thông dụng hơn
~~~c
#include <stdio.h>

#define DEBUG
#define RELEASE

int main()
{
    
#if defined(DEBUG)    
    printf("Debug mode\n");
#endif

#if !defined(RELEASE) 
    printf("Not release mode\n");
#endif

    printf("End!");
    return 0;
}
~~~
**OUTPUT:**
~~~
Debug mode
End!
~~~

### 3. `#error`
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
