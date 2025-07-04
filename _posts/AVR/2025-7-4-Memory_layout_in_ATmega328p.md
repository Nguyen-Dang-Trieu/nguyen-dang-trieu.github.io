---
title: Memory layout trên ATmega328p
date: 2025-07-04
categories: [AVR]
tags: []
author: Trieu
---

## 👋 Lời mở đầu 
Trong bài viết này, tôi sẽ nói về bộ nhớ trên vi điều khiển ATmega328p (Arduino Uno). Qua đó có thể học được cách mà một vi điều khiển phân chia các vùng RAM để trữ 
các biến trong chương trình.

## Bộ nhớ vi điều khiển
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


~~~c
int *p;
int i;


void setup() {
  p = &i;

  int *p2;
  int i2;
  p2 = &i2;


 Serial.begin(9600);

   Serial.print("Address of i2: 0x");
  Serial.println((uint16_t)p2,HEX);
}

int _delay = 0;
void loop() { 
  if(_delay < 2)
  {
    Serial.print("Address of i: 0x");
    Serial.println((uint16_t)p,HEX);


  }
  _delay++;
  delay(1000);
}
~~~

##

~~~c
uint8_t *p;

void setup() {

  p = malloc(20);
  Serial.begin(9600);

}

int _delay = 0;
void loop() { 
  if(_delay < 2)
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

![](/assets/articles/C_programming/2025/Enum/enum.png){: .normal }


### Refrence
- https://www.teachmemicro.com/arduino-programming-pointers/
