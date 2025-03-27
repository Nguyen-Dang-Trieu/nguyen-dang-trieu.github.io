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

Bây giờ ta sẽ viết 1 thư viện toán học đơn giản và biên dịch dịch thư viện đó đó thành thư viện tĩnh trên linux.

Ở đây trong thư mục `~/Documents/CPP`. Tạo 2 file của thư viện StaticMath: `StaticMath.h` và `StaticMath.cpp`.

File `StaticMath.h`
~~~cpp
#ifndef STATICMATH_H
#define STATICMATH_H

class StaticMath
{
public:
    StaticMath(void);
    ~StaticMath(void);

    static double add(double a, double b);// Phep cong
    static double sub(double a, double b);// Phep tru
    static double mul(double a, double b);// Phep nhan
    static double div(double a, double b);// Phep chia

    void print();

};
#endif
~~~

 File `StaticMath.cpp`
 ~~~cpp
#include "StaticMath.h"
#include <iostream>

// Constructor
StaticMath::StaticMath()
{
	std::cout << "StaticMath object created!" << std::endl;
}

// Destructor
StaticMath::~StaticMath()
{
	std::cout << "StaticMath object destroyed!" << std::endl;
}

double StaticMath::add(double a, double b)
{
	return a + b;
}

double StaticMath::sub(double a, double b)
{
	return a - b;
}

double StaticMath::mul(double a, double b)
{
	return a * b;
}

double StaticMath::div(double a, double b)
{
	if (b == 0)
	{
		std::cerr << "Error:: Division by zero!" << std::endl;
		return 0;
	}
	return a / b;
}

void StaticMath::print()
{
	std::cout << "StaticMath library is working!" << std::endl;
}
~~~
~~~shell
trieu@ubuntu:~/Documents/CPP$ ls
StaticMath.cpp StaticMath.h
~~~

Trong linux, sử dụng `ar` để tạo thư viện tĩnh (`.a`). Còn trên window, sử dụng `lib.exe` để tạo thư viện tĩnh (`.lib`). Khi tạo thư viện tĩnh, các file `.o` được sắp xếp, lập chỉ mục để dễ dàng tra cứu trong quá trình liên kết.

![](/assets/articles/2025/Static_Dynamic_Library/2025-3-27-image_2.png){: .normal }

**B1: Tạo thư viện tĩnh**

Trước tiên cần phải biên dịch nó thành một `.o`
~~~shell
trieu@ubuntu:~/Documents/CPP$ g++ -c StaticMath.cpp -o StaticMath.o
trieu@ubuntu:~/Documents/CPP$ ls
StaticMath.cpp  StaticMath.o  StaticMath.h
~~~

Sau đó ta sẽ đóng gói `StaticMath.o` thành thư viện tĩnh (`libstaticmath.a`)

Quy ước đặt tên thư viện tĩnh của linux: `lib[tên thư viện].a`
~~~shell
trieu@ubuntu:~/Documents/CPP$ ar rcs libstaticmath.a StaticMath.o
trieu@ubuntu:~/Documents/CPP$ ls
libstaticmath.a  StaticMath.cpp  StaticMath.o  StaticMath.h
~~~
Các dự án lớn có nhiều file thì ta sẽ dùng Makefile hoặc là CMake để tạo các thư viện tĩnh tránh nhập lệnh phiền phức.

**B2: Sử dụng thư viện**

Bây giờ ta sẽ tạo một file `main.cpp`

File `main.cpp`
~~~cpp
#include "StaticMath.h"
#include <iostream>

using namespace std;

int main(int argc, char* argv[])
{
        double a = 10;
        double b = 2;

        cout << "a + b = " << StaticMath::add(a,b) << endl;
        cout << "a - b = " << StaticMath::sub(a,b) << endl;
        cout << "a * b = " << StaticMath::mul(a,b) << endl;
        cout << "a / b = " << StaticMath::div(a,b) << endl;

        StaticMath sm;
        sm.print();

        return 0;
}
~~~
Biên dịch file `main.cpp` và liên kết với thư viện 
~~~shell
trieu@ubuntu:~/Documents/CPP$ g++ main.cpp -o main -I/home/trieu/Documents/CPP -L/home/trieu/Documents/CPP -lstaticmath
trieu@ubuntu:~/Documents/CPP$ ls
libstaticmath.a  main.cpp  StaticMath.cpp  StaticMath.o
main            StaticMath.h
~~~
Để sử dụng thư viện tĩnh trong linux ta cần phải chỉ rõ đường dẫn dến thư viện.
- `-I`: dùng để chỉ thư mục chứa các file header (`.h`).
- `-L`: dùng để chỉ thư mục chứa thư viện tĩnh (`.a`) hoặc thư viện động (`.so`).
- `-l`:
  - Nguyên tắc: -l[tên thư viện tĩnh] và bỏ phần mở rộng `.a`
  - Linker sẽ tự động tìm thư viện tĩnh trong thư mục mà đã được chỉ định với `-L`

Có thể dùng lệnh `pwd` để lấy đường dẫn thư mục đang chứa thư viện.

Khi đã biên dịch xong ta sẽ được `main.o`. Để chạy file này dùng `./main`.
~~~shell
trieu@ubuntu:~/Documents/CPP$ ./main
a + b = 12
a - b = 8
a * b = 20
a / b = 5
StaticMath object created!
StaticMath library is working!
StaticMath object destroyed!
~~~

## Dynamic Library
Qua phần trình bày trên, chúng ta có thể thấy được thư viện tĩnh dễ sử dụng và dễ hiểu, đồng thời cũng đạt được mục đích tái sử dụng mã. Vậy tại sao chúng ta lại cần thư viện động ?

Tại sao chúng ta lại cần thư viện động?

Trên thực tế, điều này là do đặc điểm của thư viện tĩnh.
- Vấn đề lãng phí không gian bộ nhớ là vấn đề của các thư viện tĩnh.

![](/assets/articles/2025/Static_Dynamic_Library/2025-3-27-image_3.png){: .normal }

- Một vấn đề của thư viện tĩnh là chúng gây khó khăn trong việc cập nhật, triển khai và phát hành chương trình. Nếu thư viện tĩnh (`.a`, `.lib`) được cập nhật, tất cả các ứng dụng sử dụng thư viện đó sẽ phải được biên dịch lại và phát hành lại cho người dùng. Điều này có thể gây bất tiện, vì ngay cả khi chỉ có một thay đổi nhỏ trong thư viện, toàn bộ chương trình cũng phải được tải xuống và cập nhật lại.

Thư viện động không được liên kết trực tiếp vào mã chương trình khi biên dịch mà chỉ được tải vào bộ nhớ khi chương trình chạy. Điều này giúp tiết kiệm bộ nhớ, vì nếu nhiều ứng dụng sử dụng cùng một thư viện, chỉ cần một bản duy nhất của thư viện được nạp vào và dùng chung.

Ngoài ra, việc sử dụng thư viện động cũng khắc phục nhược điểm của thư viện tĩnh trong việc cập nhật, triển khai và phát hành chương trình. Người dùng chỉ cần cập nhật thư viện động thay vì tải lại toàn bộ chương trình, giúp quá trình cập nhật trở nên nhẹ hơn.
