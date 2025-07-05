---
title: Memory layout trên ATmega328p
date: 2025-07-04
categories: [AVR]
tags: []
author: Trieu
---

## 👋 I. Lời mở đầu 
Trong bài viết này, tôi sẽ nói về bộ nhớ trên vi điều khiển ATmega328p (Arduino Uno). Qua đó có thể học được cách mà một vi điều khiển phân chia các vùng RAM để trữ 
các biến trong chương trình.

## 🌱 II. Bộ nhớ vi điều khiển
Memory của ATmega328p như sau:
- Flash 32k byte. (Bộ nhớ chương trình)
- SRAM 2k byte.
- EEPROM 1k byte.

Bộ nhớ Flash là nơi chúng ta lưu trữ chương trình khi viết. Trong số đó, có 500 byte dành cho bootloader. Bộ nhớ Flash và EEPROM là các vùng lưu trữ không mất dữ liệu
(non-volatile) , nghĩa là khi ta tắt chương trình thì dữ liệu vẫn còn đó.

Trong khi đó SRAM là vùng nhớ volatile và được truy cập khi chương trình chạy. SRAM chứa các biến static, global, local, heap.

Hình ảnh. https://embeddedwala.com/Blogs/embedded-c/memory-layout-of-c-program

Các phân vùng của SRAM
- `.data`: int a = 1 (global), static int b = 2 (global) (local).
- `.bss`: int a or int a = 0 (global), static int b or static in b = 0 (global) (local).
- `stack`: int a = 6 (local).
- `heap`: malloc(), free(). 

### 1. Vùng `.data`
Môi trường thử nghiệm: Arduino IDE và board Arduino Uno (Atmega328p).
~~~c

int initialization_variable = 1;
int *p;

static int static_initialization_variable = 2;
int *p2;

void setup() {
  p = &initialization_variable;
  p2 = &static_initialization_variable; 


  Serial.begin(9600);
}

int _delay = 0;
void loop() { 
  if(_delay < 1)
  {
    Serial.print("Address of initialization variable: 0x");
    Serial.println((uint16_t)p,HEX);

    Serial.print("Address of static initialization variable: 0x");
    Serial.println((uint16_t)p2,HEX);

  }
  _delay++;
  delay(1000);
}
~~~
**OUTPUT**:
~~~
Address of initialization variable: 0x102
Address of static initialization variable: 0x100
~~~

Dựa vào kết quả trên, tôi có phán đoán là vùng `.data` của atmega328p bắt đầu từ địa chỉ: `0x100`.

Câu hỏi: Tại vì sao `.data` lại bắt đầu từ `0x100` ?

### 2. Vùng `.bss`
Môi trường thử nghiệm: Arduino IDE và board Arduino Uno (Atmega328p).
~~~c
int Uninitialized_variable;
int *p;

static int static_Uninitialized_variable;
int *p2;

void setup() {
  p = &Uninitialized_variable;
  p2 = &static_Uninitialized_variable; 


  Serial.begin(9600);
}

int _delay = 0;
void loop() { 
  if(_delay < 1)
  {
    Serial.print("Address of Uninitialized variable: 0x");
    Serial.println((uint16_t)p,HEX);

    Serial.print("Address of static Uninitialized_variable: 0x");
    Serial.println((uint16_t)p2,HEX);

  }
  _delay++;
  delay(1000);
}
~~~
**OUTPUT:**
~~~
Address of Uninitialized variable: 0x16A
Address of static Uninitialized_variable: 0x168
~~~

Dựa vào kết quả trên, tôi có phán đoán là vùng `.bss` của atmega328p bắt đầu từ địa chỉ: `0x168`.

Câu hỏi: Tại vì sao `.bss` lại bắt đầu từ `0x168` ?

### Vùng `heap`

~~~c
uint8_t *p;

void setup() {

  p = malloc(20);
  Serial.begin(9600);

}

int _delay = 0;
void loop() { 
  if(_delay < 1)
  {
    Serial.print("Start address of p: 0x");
    Serial.println((uint16_t)p,HEX);

    Serial.print("End address of p: 0x");
    Serial.println((uint16_t)(p + 20),HEX);

  }
  _delay++;
  delay(1000);
}
~~~
**OUTPUT**:
~~~
Start address of p: 0x1F8
End address of p: 0x20C
~~~

Qua kết quả trên, tôi có nhận định:
- Vùng heap bắt đầu từ địa chỉ `0x1F8`.
- Khi lấy `0x20C - 0x1F8` = 20 byte -> Điều này chứng minh được cách hoạt động đúng của `malloc(20)` là cấp phát 20 byte.

Câu hỏi: Tại sao vùng `heap` lại bắt đầu tại địa chỉ `0x1F8`.

## Vùng `stack`
~~~c


void setup() {
  int i;      // biến local chưa khởi tạo
  int *p;

  p = &i;

  int i2 = 1; // biến local được khởi tạo
  int *p2;

  p2 = &i2;

  Serial.begin(9600);

  Serial.print("Address of i: 0x");
  Serial.println((uint16_t)p, HEX);

  Serial.print("Address of i2: 0x");
  Serial.println((uint16_t)p2, HEX);


}


void loop() { 

  delay(1000);
}

~~~
**OUTPUT:**
~~~
Address of i: 0x8FA
Address of i2: 0x8F8
~~~

Qua kết quả, tôi thấy được vùng `stack` bắt đầu tại: `0x8F8`.

Câu hỏi: Tại sao vùng `stack` lại bắt đầu tại địa chỉ `0x8F8` ?

![](/assets/articles/C_programming/2025/Enum/enum.png){: .normal }


### Refrence
- https://www.teachmemicro.com/arduino-programming-pointers/
