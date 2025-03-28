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

### Tạo và tải thư viện tĩnh trên Linux
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

![](/assets/articles/2025/Static_Dynamic_Library/2025-3-27-image_4.png){: .normal }

Các tính năng của thư viện động:
- **Liên kết và tải động**: Trì hoãn việc liên kết và tải một số hàm thư viện cho đến khi chương trình chạy.
- **Chia sẻ tài nguyên**: Cho phép nhiều tiến trình dùng chung một thư viện, giúp tiết kiệm bộ nhớ.
- **Hỗ trợ nâng cấp dễ dàng**: Có thể cập nhật thư viện mà không cần biên dịch lại chương trình chính.
- **Kiểm soát việc tải thư viện**: Lập trình viên có thể chủ động gọi và quản lý việc tải thư viện trong mã chương trình.

Không giống như khi tạo thư viện tĩnh (static library), việc tạo thư viện động (dynamic library) không cần sử dụng công cụ đóng gói như `ar` (trên Linux) hay `lib.exe` (trên Windows), mà có thể dùng trực tiếp trình biên dịch để tạo.

### Tạo và sử dụng thư viện động trên Linux
#### Quy tắc đặt tên thư viện động trong Linux
Trong Linux, thư viện động có định dạng `libxxx.so`, trong đó:
- `lib`: Tiền tố mặc định của thư viện động.
- `xxx`: Tên thư viện.
- `.so` (Shared Object): Phần mở rộng của thư viện động.

#### Tạo thư viện động
Tạo 2 file cho thư viện động:
File `DynamicMath.h`
~~~cpp
#ifndef DYNAMICMATH_H
#define DYNAMICMATH_H

class DynamicMath
{
public:
	DynamicMath();
	~DynamicMath();
	
	static double add(double a, double b);
	static double sub(double a, double b);
	static double mul(double a, double b);
	static double div(double a, double b);
	
	void print();
};

#endif
~~~
File `DynamicMath.cpp`
~~~cpp
#include "DynamicMath.h"
#include <iostream>

// Constructor
DynamicMath::DynamicMath()
{
	std::cout << "DynamicMath object created!" << std::endl;
}

// Destructor
DynamicMath::~DynamicMath()
{
	std::cout << "DynamicMath object destroyed!" << std::endl;
}

double DynamicMath::add(double a, double b)
{
	return a + b;
}

double DynamicMath::sub(double a, double b)
{
	return a - b;
}

double DynamicMath::mul(double a, double b)
{
	return a * b;
}

double DynamicMath::div(double a, double b)
{
	if (b == 0)
	{
		std::cerr << "Error:: Division by zero!" << std::endl;
		return 0;
	}
	return a / b;
}

void DynamicMath::print()
{
	std::cout << "DynamicMath library is working!" << std::endl;
}
~~~
~~~shell
trieu@ubuntu:~/Documents/CPP/DYNAMIC$ ls
DynamicMath.cpp  DynamicMath.h
~~~

**B1: Biên dịch file đối tượng (.o)**
~~~shell
trieu@ubuntu:~/Documents/CPP/DYNAMIC$ g++ -fPIC -c DynamicMath.cpp
trieu@ubuntu:~/Documents/CPP/DYNAMIC$ ls
DynamicMath.cpp  DynamicMath.h  DynamicMath.o
~~~
- `-fPIC`: Tạo mã Position-Independent Code (PIC - Mã độc lập vị trí), giúp thư viện động (.so) có thể được tải vào bất kỳ vị trí nào trong bộ nhớ.
- `-c`: Chỉ biên dịch file .cpp thành file đối tượng .o, không liên kết.

**B2: Tạo thư viện động (.so)**
~~~shell
trieu@ubuntu:~/Documents/CPP/DYNAMIC$ g++ -shared -o libdynmath.so DynamicMath.o
trieu@ubuntu:~/Documents/CPP/DYNAMIC$ ls
DynamicMath.cpp  DynamicMath.h  DynamicMath.o  libdynmath.so
~~~
- `-shared`: Tùy chọn này cho biết cần tạo một thư viện động (shared library).

Trên thực tế 2 bước trên có thể kết hợp lại thành một lệnh
~~~shell
g++ -fPIC -shared -o libdynmath.so DynamicMath.cpp
~~~

Sử dụng thư viện
File main.cpp
~~~cpp
#include "DynamicMath.h"
#include <iostream>

using namespace std;

int main(int argc, char* argv[])
{
	double a = 10;
	double b = 2;
		
	cout << "a + b = " << DynamicMath::add(a,b) << endl;
	cout << "a - b = " << DynamicMath::sub(a,b) << endl;
	cout << "a * b = " << DynamicMath::mul(a,b) << endl;
	cout << "a / b = " << DynamicMath::div(a,b) << endl;

	DynamicMath sm;
	sm.print();

	return 0;
}
~~~

Có 2 cách sử dụng thư viện động trong chương trình:
- Dynamic Linking tại Compile Time
Phương pháp này chỉ là liên kết thư viện với chương trình thôi. Khi nào chạy (Run time) thì thư viện .so mới thực sự được hệ điều hành tải vào bộ nhớ tự động.
~~~shell
trieu@ubuntu:~/Documents/CPP/DYNAMIC$ g++ main.cpp -I/home/trieu/Documents/CPP/DYNAMIC -L/home/trieu/Documents/CPP/DYNAMIC -ldynmath -o main
trieu@ubuntu:~/Documents/CPP/DYNAMIC$ ls
DynamicMath.cpp  DynamicMath.h  DynamicMath.o  libdynmath.so  main  main.cpp
trieu@ubuntu:~/Documents/CPP/DYNAMIC$ ./main
./main: error while loading shared libraries: libdynmath.so: cannot open shared object file: No such file or directory
~~~

Hướng dẫn fix lỗi này:

**Cách 1:** Thiết lập LD_LIBRARY_PATH (Tạm thời)
Trước khi chạy chương trình, bạn cần xuất biến môi trường LD_LIBRARY_PATH:
~~~shell
trieu@ubuntu:~/Documents/CPP/DYNAMIC$ export LD_LIBRARY_PATH=/home/trieu/Documents/CPP/DYNAMIC:$LD_LIBRARY_PATH
trieu@ubuntu:~/Documents/CPP/DYNAMIC$ ./main
a + b = 12
a - b = 8
a * b = 20
a / b = 5
DynamicMath object created!
DynamicMath library is working!
DynamicMath object destroyed!
~~~
Nếu ta tắt terminal hiện tại và mở lại cần phải `export` lại từ đầu mới có thể chạy được.

**Cách 2:** Thêm file thư viện vào `/etc/ld.so.conf` (Vĩnh viễn). Chú ý để ghi được file vào thư mục này cần mở quyền `sudo`.
~~~shell
trieu@ubuntu:~$ sudo nano /etc/ld.so.conf.d/dynamic.conf
[sudo] password for trieu: 
trieu@ubuntu:~$ sudo ldconfig
trieu@ubuntu:~$ cd Documents/CPP/DYNAMIC
trieu@ubuntu:~/Documents/CPP/DYNAMIC$ ls
DynamicMath.cpp  DynamicMath.h  DynamicMath.o  libdynmath.so  main  main.cpp
trieu@ubuntu:~/Documents/CPP/DYNAMIC$ ./main
a + b = 12
a - b = 8
a * b = 20
a / b = 5
DynamicMath object created!
DynamicMath library is working!
DynamicMath object destroyed!
~~~
Bây giờ ta có thể tắt và mở lại terminal và mọi thứ diễn ra bình thường.

- Dynamic Loading tại Runtime
~~~cpp
#include <dlfcn.h>  // Thư viện để tải file .so
~~~
