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

Output: Khi chưa ~~`#define VERSION 1`~~
~~~
error: #error "You need to define macro VERSION before compiler!"
~~~

## III. MACRO C
Macro là một đoạn mã được định nghĩa bằng tên. Khi chương trình được biên dịch, **Pre-processor** sẽ thay thế tất cả các vị trí sử dụng tên macro bằng phần thân của nó trước khi biên dịch thực sự bắt đầu.

Trong C, macro thường dược khai báo bằng từ khóa `#define`, và có thể là:
- Object-like macro: giá trị hay là hằng số
- Function-like macro: đoạn mã có tham số.

### 1. Object-like macro
**Ví dụ:**
~~~c
#include <stdio.h>
#include <stdbool.h>

#define ONE   1
#define TWO   2
#define THREE 3

#define PI 3.14

int main()
{
  printf("Giá trị tăng dần: %d, %d, %d\n", ONE, TWO, THREE);  
  printf("Giá trị Pi: %0.2f", PI);
  

  return 0;
}
~~~
**Output**
~~~
Giá trị tăng dần: 1, 2, 3
Giá trị Pi: 3.14
~~~


### 2. Function-like macro
**Ví dụ:**
~~~c
#include <stdio.h>
#include <stdbool.h>


#define DEBUG_PRINT(x)  \
  do                    \
  {                     \
    printf("DEBUG: ");  \
    printf("%s",x);     \
    printf("\n");       \
  } while(false)

int main()
{
    DEBUG_PRINT("Error");
    
    return 0;
}
~~~
**Output**
~~~
DEBUG: Error
~~~

`Note`: Phía sau của `\` phải đảm bảo là không có `Tab` or `Space`.

### Reference
- https://www.math.utah.edu/docs/info/cpp_1.html
- https://en.wikipedia.org/wiki/C_preprocessor
