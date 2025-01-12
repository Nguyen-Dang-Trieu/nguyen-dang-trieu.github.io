---
title: File Hex (Part 2)
date: 2025-1-12
categories: [Computer]
tags: [ASM]
author: Trieu
image:
  path: assets/articles/2025/FileHexPart2/2025-1-12-Header.png
  alt: File Hex
---

## 🌱 Lời mở đầu
Chào mọi người, trong [bài viết trước](https://nguyen-dang-trieu.github.io/posts/FileHex/) mặc dù đã nghiên cứu hầu hết các khía cạnh của tệp HEX, tôi nhận thấy phần **Record Type** chưa được trình bày chi tiết. Vì vậy, bài viết này sẽ tập trung giải thích rõ ràng về **Record Type** trong định dạng tệp HEX.

Series File Hex gồm:
- [Part 1](https://nguyen-dang-trieu.github.io/posts/FileHex/)
- [Part 2](https://nguyen-dang-trieu.github.io/posts/FileHexPart2/)

## Định dạng và phân tích sâu về File Hex
Trước khi đi vào phần Record type, ta sẽ tổng ôn sơ qua phần format của File Hex
### 1. Định dạng File Hex
![Format File Hex](/assets/articles/2025/FileHexPart2/2025-1-12-FormatHex.png){: .normal }
_Format File Hex_

- `RECORD MASK`: Mỗi dòng trong tệp HEX bắt đầu bằng ký tự dấu hai chấm (:), đây là ký hiệu xác định điểm bắt đầu của một record.
- `RECLEN (Record Length)`: Trường này cho biết số byte dữ liệu **(INFO OR DATA)** có trong record.
- `LOAD OFFSET`: Địa chỉ offset, xác định nơi dữ liệu sẽ được ghi vào bộ nhớ.
- `RECTYPE (Record Type)`: Trường này biểu thị loại record, giúp xác định ý nghĩa của dòng dữ liệu.
    - `0x00`: Record chứa dữ liệu **(INFO OR DATA)**, thường là nội dung chính của file nhị phân.
    - `0x01`: Record kết thúc tệp HEX, thường xuất hiện ở dòng cuối cùng của tệp.
    - `0x02`: Record chỉ định địa chỉ phân đoạn mở rộng **(Extended Segment Address)**.
    - `0x03`: Record chỉ định địa chỉ phân đoạn bắt đầu **(Start Segment Address)**.
    - `0x04`: Record chỉ định địa chỉ tuyến tính mở rộng **(Extended Linear Address)**.
    - `0x05`: Record chỉ định địa chỉ tuyến tính bắt đầu **(Start Linear Address)**, thường được sử dụng làm địa chỉ mục nhập cho chức năng chính của chương trình. (Tôi sẽ đưa ra ví dụ sau).
- `INFO OR DATA`: Trường này chứa dữ liệu thực tế hoặc thông tin địa chỉ liên quan đến record.
- `CHEKSUM`: 
Đây là tổng kiểm tra của record, được tính bằng công thức:

Checksum=(0x100 − S) & 0xFF
Trong đó:
- S là tổng của tất cả các byte trong record (ngoại trừ byte cuối cùng - **checksum**).
- Kết quả giúp phát hiện lỗi trong quá trình truyền dữ liệu.

**Lưu ý:** Các loại `RECTYPE` phổ biến là `0x00`, `0x01`, `0x04`, `0x05`.

### Record type 0x00
![Retype 0x00](/assets/articles/2025/FileHexPart2/2025-1-12-Retype00.png){: .normal }
_Record type 0x00_

**Example**: `:04 2000 00 FECACEFA 4C`
- `04`: Số bytes dữ liệu, ở đây là 4 bytes data.
- `2000`: địa chỉ offset, tức là `0x2000`.
- `00`: Loại bản ghi **(RECTYPE)**, với `00` biểu thị bản ghi dữ liệu.
- `FECACEFA`: Dữ liệu sẽ được ghi vào bộ nhớ, bao gồm các byte: `0xFE`, `0xCA`, `0xCE`, `0xFA`.
- `0x4C`: Checksum, được tính để kiểm tra tính toàn vẹn của dòng.
  - Tổng tích lũy S = (`0x04` + `0x20` + `0x00` + `0x00` + `0xFE` + `0xCA` + `0xCE` + `0xFA`) = `0x3B4`. (chỉ lấy byte cuối cùng của kết quả)
  - Tổng kiểm tra CHEKSUM = (`0x100` - S) & `0xFF` = `0x4C`.

### 2. Record type 0x01
![Retype 0x01](/assets/articles/2025/FileHexPart2/2025-1-12-Retype01.png){: .normal }
_Record type 0x01_

**Example**: `:00 0000 01 FF`
- `00`: Số bytes dữ liệu, ở đây là 0 bytes data. (Bạn có thể thấy hình bên trên không có phần data)
- `0000`: địa chỉ offset, tức là `0x0000`.
- `00`: Loại bản ghi **(RECTYPE)**, với `0x00` biểu thị kết thúc của File Hex.
- `0xFF`: Checksum, được tính để kiểm tra tính toàn vẹn của dòng.
  - Tổng tích lũy S = (`0x00` + `0x00` + `0x00` + `0x01`) = `0x01`. (Chỉ lấy byte cuối cùng của kết quả)
  - Tổng kiểm tra CHEKSUM = (`0x100` - S) & `0xFF` = `0xFF`.

### 3. Record type 0x02
![Retype 0x02](/assets/articles/2025/FileHexPart2/2025-1-12-Retype02.png){: .normal }
_Record type 0x02_

`USBA (Upper Segment Base Address)` là một phần trong bản ghi mở rộng địa chỉ đoạn **(Extended Segment Address Record)**. Đây là một giá trị 16-bit được sử dụng để ghi lại địa chỉ cơ sở của phân đoạn mở rộng, áp dụng trong các định dạng địa chỉ 16-bit hoặc 32-bit.

##### Cách hoạt động của USBA:
USBA ghi lại các bit từ 4 - 19 của địa chỉ đoạn mở rộng (Segment Base Address - SBA), trong khi các bit từ 0 - 3 được đặt bằng 0.

Khi cần tính địa chỉ vật lí (Physical address - địa chỉ thực tế mà dữ liệu sẽ được ghi vào) của một dữ liệu trong đoạn này, ta cộng `USBA` với `LOAD OFFSET`.

**Công thức:**
Physical Address=(USBA << 4) + LOAD OFFSET

**Example**:
Nếu `USBA` = `0x1234` và `LOAD OFFSET` = `0x0050`,
thì địa chỉ vật lí sẽ là: (`0x1234` << 4) + `0x0050` = `0x12340` + `0x0050` = `0x12390`.

### 4. Record type 0x03
![Retype 0x03](/assets/articles/2025/FileHexPart2/2025-1-12-Retype03.png){: .normal }
_Record type 0x03_
(Đang tiến hành)


### 5. Record type 0x04
![Retype 0x04](/assets/articles/2025/FileHexPart2/2025-1-12-Retype04.png){: .normal }
_Record type 0x04_
(Đang tiến hành)

### 6.Record type 0x05
![Retype 0x05](/assets/articles/2025/FileHexPart2/2025-1-12-Retype05.png){: .normal }
_Record type 0x05_
(Đang tiến hành)

## 🍁 Lời kết 
