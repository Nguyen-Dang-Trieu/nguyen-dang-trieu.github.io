---
title: Tại sao sử dụng bộ nhớ ảo (Virtual Memory) ?
date: 2025-2-5
categories: [Linux, Operating System]
tags: [Virtual Memory]
author: Trieu

---
## Bộ nhớ ảo(Virtual Memory)
Nếu bạn là sinh viên chuyên ngành điện tử, chắc hẳn bạn sẽ phải làm quen với việc lập trình trên bộ vi điều khiển tại trường đại học.

Không giống như các hệ thống có hệ điều hành, bộ vi điều khiển hoạt động trực tiếp mà không có lớp trung gian hỗ trợ. Mỗi khi viết mã, bạn cần nạp chương trình vào bộ vi điều khiển thông qua
các công cụ chuyên dụng (mạch nạp) để chương trình có thể thực thi.

Bên cạnh đó, **CPU của bộ vi điều khiển thao tác trực tiếp trên các địa chỉ bộ nhớ vật lý**, giúp việc truy cập và xử lý dữ liệu diễn ra nhanh chóng nhưng cũng đòi hỏi lập trình viên hiểu rõ  về cấu trúc phần cứng.

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-5-Virtual_Memory_1.png){: .normal }

**Bạn không thể chạy hai chương trình cùng lúc trong trường hợp này.**
Giả sử cả hai chương trình đều cần sử dụng cùng một vị trí bộ nhớ, ví dụ địa chỉ `0x2000`. Khi chương trình đầu tiên ghi dữ liệu mới vào địa chỉ này, nó sẽ vô tình xóa dữ liệu mà chương trình 
thứ hai đã lưu trước đó. Điều này khiến dữ liệu bị chồng chéo, gây lỗi và làm cả hai chương trình bị treo hoặc hoạt động sai.

> Hệ điều hành đã giải quyết vấn đề này như thế nào ?
{: .prompt-info }

Vấn đề quan trọng ở đây là cả hai chương trình đều truy cập trực tiếp vào địa chỉ bộ nhớ vật lý tuyệt đối. Đây chính là điều cần tránh vì nó dễ gây ra xung đột dữ liệu khi nhiều chương 
trình cùng sử dụng một vùng nhớ.

Chúng ta có thể **“cô lập”** vùng nhớ mà mỗi chương trình sử dụng. Cụ thể, hệ điều hành sẽ cấp cho mỗi chương trình một vùng **“địa chỉ ảo”** riêng biệt. Nhờ đó, mỗi chương trình đều có không 
gian bộ nhớ riêng để **“thoải mái sử dụng”** mà không lo xung đột với chương trình khác.
Tuy nhiên, để làm được điều này, có một điều kiện quan trọng: **các chương trình không thể truy cập trực tiếp vào địa chỉ bộ nhớ vật lý**.
Cách mà địa chỉ ảo được ánh xạ sang bộ nhớ vật lý là hoàn toàn minh bạch đối với chương trình, vì hệ điều hành đã xử lý và sắp xếp mọi thứ ở phía sau.

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-5-Virtual_Memory_2.png){: width="500" height="200" }

**Hệ điều hành cung cấp cơ chế ánh xạ địa chỉ ảo sang địa chỉ vật lý.**

Khi các chương trình chạy, mỗi chương trình sẽ sử dụng các địa chỉ ảo riêng biệt. Nếu một chương trình muốn truy cập địa chỉ ảo này, hệ điều hành sẽ tự động chuyển đổi nó thành một địa chỉ
vật lý tương ứng trong bộ nhớ. Nhờ cơ chế này, các chương trình khác nhau sẽ ghi dữ liệu vào các địa chỉ vật lý khác nhau, tránh được xung đột dữ liệu khi chạy đồng thời.

Vì vậy, có hai loại địa chỉ bộ nhớ quan trọng:
- Địa chỉ mà chương trình của chúng ta sử dụng được gọi là ***địa chỉ bộ nhớ ảo (Virtual Address)***.
- Địa chỉ thực tế tồn tại trên phần cứng của máy tính được gọi là ***địa chỉ bộ nhớ vật lý (Physical Address)***.

**Hệ điều hành giới thiệu khái niệm bộ nhớ ảo và địa chỉ ảo.**

Khi một chương trình muốn truy cập bộ nhớ, địa chỉ ảo mà nó sử dụng sẽ được chuyển đổi thành địa chỉ vật lý thông qua cơ chế ánh xạ của **bộ quản lý bộ nhớ (MMU - Memory Management Unit)** tích hợp trong chip CPU. Sau khi chuyển đổi, hệ thống sẽ truy cập bộ nhớ thực tế thông qua địa chỉ vật lý này. Quy trình này được minh họa trong hình sau:

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-5-Virtual_Memory_3.png){: .normal }

> Hệ điều hành quản lý mối quan hệ giữa địa chỉ ảo và địa chỉ vật lý như thế nào?
{: .prompt-info }

Để thực hiện việc này, hệ điều hành sử dụng hai cơ chế chính: **Phân đoạn bộ nhớ (Memory Segmentation)** và **Phân trang bộ nhớ (Memory Paging)**.

Trước tiên, chúng ta sẽ tìm hiểu về cơ chế phân đoạn bộ nhớ.

## Phân đoạn bộ nhớ - Memory Segmentation
Chương trình thường được chia thành các phân đoạn logic khác nhau, chẳng hạn như phân đoạn mã (code segment), phân đoạn dữ liệu (data segment), phân đoạn ngăn xếp (stack segment) và phân đoạn đống (heap segment). Mỗi phân đoạn có các đặc điểm và chức năng riêng biệt, vì vậy việc sử dụng phân đoạn giúp tách biệt và quản lý các thành phần này một cách hiệu quả.

> Địa chỉ ảo và địa chỉ vật lý được ánh xạ như thế nào theo cơ chế phân đoạn ?
{: .prompt-info }

Địa chỉ ảo theo cơ chế phân đoạn gồm hai phần: hệ số lựa chọn phân đoạn (segment selector) và offset. Hệ số lựa chọn phân đoạn xác định phân đoạn cụ thể trong bộ nhớ, còn offset cho biết vị trí chính xác trong phân đoạn đó.

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-6-Memory_Seg_1.png){: .normal }

✅ Yếu tố lựa chọn phân đoạn và độ lệch trong phân đoạn

1️⃣ Bộ chọn phân đoạn (Segment Selector)
- Bộ chọn phân đoạn được lưu trong thanh ghi phân đoạn (segment register).
- Thành phần quan trọng nhất của bộ chọn là số phân đoạn (segment number), dùng để tra cứu bảng phân đoạn (segment table).
- Bảng phân đoạn (segment Table) chứa thông tin về phân đoạn, bao gồm:
  - Segment Base Address - Điểm bắt đầu của phân đoạn trong bộ nhớ.
  - Segment Boundaries - Giới hạn kích thước phân đoạn.
  - Privilege Level (DPL) - Quyền truy cập vào phân đoạn.
    
2️⃣ Độ lệch phân đoạn (Segment Offset)

- Độ lệch (offset) là vị trí tương đối của dữ liệu bên trong phân đoạn. Giá trị offset phải nằm trong khoảng từ **0 -> Segment Boundaries**.
- Nếu offset hợp lệ, địa chỉ vật lý được tính bằng: Physical Address = Segment Base Address + Offset
- Nếu offset vượt quá giới hạn, hệ thống sẽ báo lỗi **Segmentation Fault**.

Ở trên, chúng ta biết rằng địa chỉ ảo được ánh xạ tới địa chỉ vật lý thông qua **Segment Table**. Cơ chế phân đoạn chia địa chỉ ảo của chương trình thành **4 segment**. Mỗi segment có một mục tương ứng trong **Segment Table**. Mỗi mục trong Segment Table chứa Base Address của Segment. Khi một chương trình muốn truy cập vào một địa chỉ ảo, Base Address của segment được tìm trong bảng, sau đó cộng với **offset** để xác định địa chỉ thực tế trong bộ nhớ vật lý, như minh họa bên dưới:

📌 Physical Address = Segment Base Address + Offset

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-6-Memory_Seg_2.png){: .normal }

Nếu chúng ta muốn truy cập địa chỉ ảo tại vị trí offset = 500 trong segment 3, chúng ta có thể tính được địa chỉ vật lý = Base address + offset = 7000 + 500 = 7500

Phương pháp phân đoạn rất tốt, nó giải quyết được vấn đề là bản thân chương trình không cần quan tâm đến địa chỉ bộ nhớ vật lý cụ thể, nhưng nó cũng có một số nhược điểm ❌:
- 1️⃣ Phân mảnh bộ nhớ. (Memory Fragmentation)
- 2️⃣ Hiệu suất trao đổi bộ nhớ thấp.
  
Tiếp theo, ta hãy nói về lý do tại sao lại phát sinh hai vấn đề này.

### Phân mảnh bộ nhớ. (Memory Fragmentation)
Đọc thêm: [Memory Fragmentation in operating system](https://er.yuvayana.org/memory-fragmentation-in-operating-system/#google_vignette)

Trước tiên, chúng ta hãy xem tại sao phân đoạn lại gây ra vấn đề phân mảnh bộ nhớ ❗

Hãy xem xét một ví dụ như này. Giả sử ta có `1 GB` bộ nhớ vật lý và người dùng - user thực hiện nhiều chương trình trên bộ nhớ đó:
- Game chiếm `512 MB`.
- Trình duyệt WEB chiếm `128 MB`.
- Music chiếm `256 MB`.

Lúc này nếu ta đóng trình duyệt WEB đi thì bộ nhớ vật lý sẽ trong như thế này: `1024 - 512 - 256 = 256 MB` vùng nhớ trống còn lại.

Nếu `256 MB` vùng nhớ này không nằm liên tục và được chia thành 2 phân đoạn, mỗi phân đoạn là `128 MB`. Vấn đề phát sinh đó là nếu ta muốn mở 1 chương trình `200 MB` thì sẽ không cấp phát đủ vùng nhớ, dù tổng bộ nhớ trống là `256 MB`.

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-6-Memory_Seg_3.png){: .normal }

> Liệu sự phân mảnh bộ nhớ có xảy ra do phân đoạn bộ nhớ không?
{: .prompt-info }

Phân mảnh bộ nhớ chủ yếu được chia thành phân mảnh bộ nhớ trong và phân mảnh bộ nhớ ngoài.
- Quản lý phân đoạn bộ nhớ cho phép phân bổ bộ nhớ cho các phân đoạn dựa trên nhu cầu thực tế, do đó kích thước của từng phân đoạn sẽ thay đổi theo yêu cầu. Điều này giúp tránh tình trạng phân mảnh bộ nhớ trong.
- Tuy nhiên, do độ dài của mỗi phân đoạn không cố định, có thể xảy ra tình trạng nhiều phân đoạn không sử dụng hết không gian bộ nhớ. Điều này khiến bộ nhớ trống bị phân tán thành các khối nhỏ không liên tiếp, tạo ra phân mảnh bộ nhớ ngoài, làm cho việc tải chương trình mới trở nên khó khăn hoặc không thể thực hiện được.

> Giải pháp cho vấn đề phân mảnh bộ nhớ ngoài là hoán đổi bộ nhớ **(Swapping)**.
{: .prompt-tip}

Khi gặp vấn đề phân mảnh bộ nhớ ngoài, một giải pháp là hoán đổi vùng nhớ mà chương trình đang sử dụng. Ví dụ: ta sẽ ghi **(swap out)** `256MB` vùng nhớ mà chương trình Music đang chiếm dụng vào ổ cứng **(disk)**. Lúc này, vùng nhớ trống sẽ là `512 MB` liền kề nhau.

Sau đó, khi cần, ta sẽ đọc ngược lại **(swap in)** dữ liệu từ disk vào lại bộ nhớ. Khi đó, bộ nhớ trống sẽ là `256 MB` liên tục, đủ để tải chương trình `200 MB` mà người dùng yêu cầu.

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-6-Memory_Seg_4.png){: .normal }

Không gian hoán đổi bộ nhớ này, trong các hệ thống như Linux, được gọi là swap space. Đây là không gian trên ổ cứng, tách biệt với bộ nhớ chính, dùng để hoán đổi giữa bộ nhớ vật lý và bộ nhớ ảo.

###  Hiệu suất
**Chúng ta hãy xem "Tại sao phân đoạn lại dẫn đến hiệu quả hoán đổi bộ nhớ thấp❓"**

Trong hệ thống đa tiến trình, bộ nhớ có thể bị phân mảnh ngoài khi sử dụng phân đoạn. Khi điều này xảy ra, hệ thống phải hoán đổi (swap) dữ liệu giữa RAM và ổ cứng, làm giảm hiệu suất do tốc độ truy cập ổ cứng chậm hơn nhiều so với RAM.

Nếu một chương trình chiếm nhiều bộ nhớ bị hoán đổi, toàn bộ hệ thống có thể bị chậm hoặc "đơ".

Để giải quyết vấn đề "phân mảnh bộ nhớ ngoài và hiệu suất hoán đổi bộ nhớ thấp" của phân đoạn bộ nhớ, phân trang bộ nhớ đã xuất hiện.

## Phân trang bộ nhớ (Memory Paging)
Ưu điểm của phân đoạn là nó cung cấp không gian bộ nhớ liên tục cho chương trình. Tuy nhiên, nó cũng gây ra vấn đề như phân mảnh bộ nhớ ngoài và không gian hoán đổi quá lớn.

Để khắc phục, chúng ta cần giảm phân mảnh bộ nhớ và tối ưu hóa quá trình hoán đổi. Nếu lượng dữ liệu cần hoán đổi hoặc tải từ ổ đĩa ít hơn, hiệu suất hệ thống sẽ được cải thiện. Giải pháp cho vấn đề này là **phân trang bộ nhớ - Memory Paging**.

Phân trang là quá trình chia bộ nhớ ảo và bộ nhớ vật lý thành các khối nhỏ có kích thước cố định. Mỗi khối có không gian bộ nhớ liên tục và kích thước cố định này được gọi là **trang (page)**. Trên Linux, kích thước mặc định của mỗi trang là `4 KB`.

Địa chỉ ảo và địa chỉ vật lý được ánh xạ thông qua **bảng trang - Page Table**, như được hiển thị bên dưới:

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-6-Memory_Page_1.png){: .normal }

Bảng trang được lưu trữ trong bộ nhớ và đơn vị quản lý bộ nhớ (MMU) có nhiệm vụ chuyển đổi địa chỉ bộ nhớ ảo thành địa chỉ vật lý.

Khi một tiến trình truy cập một địa chỉ ảo không có trong bảng trang, hệ thống sẽ kích hoạt một lỗi trang (Page Fault). Lúc này, quyền điều khiển sẽ được chuyển vào không gian hạt nhân để xử lý. Hệ thống sẽ cấp phát một khung trang trong bộ nhớ vật lý, cập nhật bảng trang của tiến trình, rồi quay lại không gian người dùng để tiếp tục thực thi tiến trình đó.

### Phân trang giải quyết vấn đề "phân mảnh bộ nhớ ngoài và hiệu suất hoán đổi bộ nhớ thấp" của phân đoạn như thế nào❓
Trong phân trang bộ nhớ, không gian bộ nhớ được chia thành các trang có kích thước cố định ngay từ đầu, giúp loại bỏ các khoảng trống nhỏ giữa các phân đoạn như trong phân đoạn bộ nhớ. Đây chính là nguyên nhân khiến phân đoạn gây ra phân mảnh bộ nhớ ngoài. Ngược lại, **với phân trang các trang được sắp xếp liên tục và chặt chẽ, do đó không xảy ra phân mảnh bên ngoài**.

Tuy nhiên, do đơn vị cấp phát bộ nhớ tối thiểu trong phân trang là một trang, nên ngay cả khi một chương trình có kích thước nhỏ hơn một trang, hệ thống vẫn phải cấp phát toàn bộ một trang. Điều này dẫn đến lãng phí bộ nhớ bên trong trang, **gây ra hiện tượng phân mảnh bộ nhớ trong trong cơ chế phân trang**.

Khi bộ nhớ không đủ, hệ điều hành sẽ giải phóng các trang không được sử dụng gần đây từ các tiến trình khác bằng cách ghi tạm thời chúng vào ổ cứng, quá trình này gọi là **Swap Out**. Khi cần, các trang này sẽ được nạp lại vào bộ nhớ, gọi là **Swap In**. Nhờ vậy, hệ thống chỉ hoán đổi một số trang thay vì toàn bộ tiến trình, giúp giảm thời gian ghi/đọc đĩa và tăng hiệu suất hoán đổi bộ nhớ.

![](/assets/articles/2025/Why_use_Virtual_Memory/2025-2-6-Memory_Page_2.png){: .normal }

Phương pháp phân trang cho phép chúng ta không phải tải toàn bộ chương trình vào bộ nhớ vật lý cùng một lúc. Thay vào đó, chúng ta có thể ánh xạ các trang giữa bộ nhớ ảo và bộ nhớ vật lý mà không cần phải tải chúng ngay lập tức. Chỉ khi chương trình cần truy cập các lệnh hoặc dữ liệu trong những trang bộ nhớ ảo tương ứng, thì hệ thống mới tải các trang đó vào bộ nhớ vật lý.

### 2. Địa chỉ ảo và địa chỉ vật lý được ánh xạ như thế nào theo cơ chế phân trang?
Trong cơ chế phân trang (paging), địa chỉ ảo (virtual address) được chia thành hai phần: số trang (page number) và độ lệch trong trang (page offset).
- Số trang (page number) được sử dụng làm chỉ mục trong bảng trang (page table), bảng này chứa địa chỉ cơ sở (base address) của từng trang trong bộ nhớ vật lý (physical memory).
- Sự kết hợp giữa địa chỉ cơ sở này và độ lệch trong trang (page offset) tạo thành địa chỉ bộ nhớ vật lý (physical address).

Hình ảnh
