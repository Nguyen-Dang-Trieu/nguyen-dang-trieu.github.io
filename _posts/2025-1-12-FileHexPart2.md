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

## Lời mở đầu
Chào mọi người, trong [bài viết trước]() ta đã tìm hiểu về hầu như là toàn bộ kiến thức về File Hex, nhưng tôi nhận thấy bài viết đó chưa nói rõ chi tiết về phần `Record Type` trong định dạng của File Hex. Nên tôi làm riêng một bài viết này để tập trung nói về phần đó.
Series File Hex gồm:
- [Part 1]()
- [Part 2](https://nguyen-dang-trieu.github.io/posts/FileHexPart2/)

## Định dạng và phân tích sâu về File Hex
Trước khi đi vào phần Record type, ta sẽ tổng ôn sơ qua phần format của File Hex
### Định dạng File Hex
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

### Record type 0x01
![Retype 0x01](/assets/articles/2025/FileHexPart2/2025-1-12-Retype01.png){: .normal }
_Record type 0x01_


### Record type 0x02
![Retype 0x02](/assets/articles/2025/FileHexPart2/2025-1-12-Retype02.png){: .normal }
_Record type 0x02_

### Record type 0x03
![Retype 0x03](/assets/articles/2025/FileHexPart2/2025-1-12-Retype03.png){: .normal }
_Record type 0x03_

### Record type 0x04
![Retype 0x04](/assets/articles/2025/FileHexPart2/2025-1-12-Retype04.png){: .normal }
_Record type 0x04_

### Record type 0x05
![Retype 0x05](/assets/articles/2025/FileHexPart2/2025-1-12-Retype05.png){: .normal }
_Record type 0x05_
