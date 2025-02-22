---
title: Mô hình OSI
date: 2025-2-21
categories: [Network]
tags: []
author: Trieu
image:
  path: assets/articles/2025/OSI_Model/header.png
  alt: 
---
# Lời mở đầu
Để giảm độ phức tạp và cải thiện tính linh hoạt, các hệ thống phần mềm lớn thường được thiết kế theo phương pháp phân lớp.

Mô hình OSI được sử dụng để định nghĩa và hiểu cách dữ liệu được truyền từ máy tính này sang máy tính khác. Ở dạng cơ bản nhất, hai máy tính được kết nối với nhau thông qua cáp và đầu nối, chia sẻ dữ liệu với sự trợ giúp của card mạng để tạo thành một mạng.

Tuy nhiên, nếu một máy tính chạy Windows và máy còn lại chạy macOS, làm thế nào hai máy này có thể giao tiếp được với nhau?

![Format Code](/assets/articles/2025/OSI_Model/OSI_Model_1.png){: .normal }

Để các máy tính, mạng hoặc kiến trúc khác nhau có thể giao tiếp thành công, **Tổ chức Tiêu chuẩn hóa Quốc tế (ISO - International Organization for Standardization)** đã đề xuất **mô hình OSI (Open Systems Interconnection)** gồm bảy lớp, từ trên xuống dưới: 

![Format Code](/assets/articles/2025/OSI_Model/OSI_Model_2.png){: .normal }

Khi nhắc đến **Aplication layer**, ta không chỉ nói về các phần mềm như **Google Chrome**, **Coc Coc** hay **Firefox**, mà còn bao gồm các giao thức **"protocol"** ở **Aplication layer** giúp những phần mềm đó giao tiếp qua **Internet**🌐.

Những giao thức này định nghĩa **"cách"** mà các chương trình ứng dụng trao đổi dữ liệu trên **Internet**🌐.

📜 Ví dụ về một giao thức phổ biến
- **HTTP/HTTPS** – dùng khi duyệt web.
- **FTP** – dùng để truyền file.
- **SMTP/IMAP/POP3** – dùng cho email.
- **DNS** – dùng để phân giải tên miền.

![Format Code](/assets/articles/2025/OSI_Model/OSI_Model_3.png){: .normal }

Hãy cùng xem xét chi tiết mô hình bảy lớp OSI.

# Khám phá chi tiết về mô hình OSI
## 1. Physical layer
**Lớp Vật Lý (Physical Layer)** chịu trách nhiệm **truyền các bit thô** giữa các thiết bị qua các phương tiện vật lý (như cáp đồng, cáp quang, sóng vô tuyến, ...). Lớp này **không quan tâm đến ý nghĩa của chuỗi bit** mà chỉ đảm bảo dữ liệu được truyền đi. Đồng thời, nó cũng **che giấu sự phức tạp của các thiết bị vật lý** so với các lớp trên.

![Format Code](/assets/articles/2025/OSI_Model/Physical_layer_2.png){: .normal }

Dữ liệu từ ứng dụng (Application Layer) sẽ trải qua các bước xử lý sau:
- **Transport Layer**: Dữ liệu được chia thành các phần nhỏ gọi là phân đoạn **"segments"**.
- **Network Layer**: Các phân đoạn này được đóng gói thành gói dữ liệu **"packet"**, trong đó có chứa địa chỉ IP của người gửi và người nhận.
- **Data Link Layer**: Các gói dữ liệu được chuyển đổi thành khung **"frame"** dưới dạng chuỗi bits.
- **Physical Layer**: Cuối cùng, chuỗi bits được chuyển đổi thành các tín hiệu (điện, quang hoặc vô tuyến) và được truyền qua các phương tiện cục bộ như cáp đồng, cáp quang hoặc tín hiệu không dây. Cuối cùng dữ liệu sẽ được hiển thị trên máy tính người nhận 🖥️.

![Format Code](/assets/articles/2025/OSI_Model/Physical_layer.png){: .normal }

## 2. Data Link Layer
Lớp liên kết dữ liệu (Data Link Layer) nhận các gói dữ liệu ("packet") từ **Network Layer**, trong đó chứa địa chỉ IP của người gửi và người nhận. Khi dữ liệu được truyền qua mạng, có hai loại địa chỉ được sử dụng để đảm bảo thông tin đến đúng nơi: **địa chỉ logic (logical address)** và **địa chỉ vật lý (physical address)**.
- **Logical address (địa chỉ logic) — hay còn gọi là địa chỉ ảo (virtual address)** — được gán ở **Network Layer**. Đây chính là địa chỉ IP của người gửi và người nhận. Khi dữ liệu **"Data"** đi qua **Transport Layer**, nó được chia thành các "segments" và sau đó địa chỉ IP được chèn vào để tạo thành một gói tin IP **"IP packet"**.
- **Physical address (địa chỉ vật lý)** được xử lý tại **Data Link Laye**r. Ở đây, hệ thống sẽ thêm địa chỉ **MAC (Media Access Control)** của máy tính gửi và nhận vào gói dữ liệu IP, tạo thành một khung dữ liệu **"frame"**.
  - Địa chỉ MAC là một mã định danh duy nhất được nhà sản xuất gán cho mỗi thiết bị mạng. Ngay cả điện thoại di động, máy tính bảng hay laptop cũng đều có một địa chỉ MAC riêng biệt.

![Format Code](/assets/articles/2025/OSI_Model/Data_Link_layer.png){: .normal }

Đơn vị dữ liệu trong **Data Link Layer** được gọi là khung dữ liệu **"frame"**. Lớp liên kết dữ liệu thường được triển khai thông qua thẻ giao diện mạng (Network Interface Card – NIC) dưới dạng phần mềm hoặc phần cứng trong máy tính.

NIC cung cấp giao diện để truyền dữ liệu từ máy tính này sang máy tính khác thông qua các phương tiện truyền dẫn cục bộ như dây đồng, cáp quang hoặc tín hiệu vô tuyến.

![Format Code](/assets/articles/2025/OSI_Model/Data_Link_layer_2.png){: .normal }

**Data Link Layer** sử dụng khung **"frame"** làm đơn vị truyền và chịu trách nhiệm **"đánh"** địa chỉ Internet cũng như phát hiện lỗi. Nhờ đó, lớp này giải quyết các vấn đề truyền thông giữa nhiều máy chủ trong cùng một mạng và đảm bảo việc giao tiếp trong **mạng LAN**.

Trong môi trường truyền thông chia sẻ — nơi nhiều thiết bị cùng sử dụng chung một kênh truyền dữ liệu để giao tiếp (đặc biệt là trong mạng cục bộ - LAN) — chỉ riêng **Physical layer** là không đủ, vì lớp này chỉ truyền các bit mà **không thể xác định ai** sẽ nhận dữ liệu.

Để làm được điều này, dữ liệu **"Data"** cần được đóng gói vào các khung **"frame"** và bổ sung thông tin địa chỉ vào tiêu đề khung **"header"** để xác định nơi nhận dữ liệu. 

![Format Code](/assets/articles/2025/OSI_Model/Data_Link_layer_4.png){: .normal }

Nếu so sánh một khung dữ liệu (frame) với một lá thư, thì phần "header" giống như phong bì, còn phần "data" là nội dung lá thư.
- Phong bì cần ghi rõ địa chỉ người nhận và người gửi, giúp bưu điện biết lá thư phải đến đâu.
- Tương tự, "header" của khung dữ liệu chứa thông tin quan trọng như địa chỉ MAC/IP của người gửi và người nhận, giúp dữ liệu được chuyển đến đúng đích.
  
Nội dung lá thư chính là phần "data" trong frame, chứa thông tin thực sự mà người gửi muốn truyền tải, chẳng hạn như một tin nhắn, hình ảnh hoặc tệp tin.

Khi một thiết bị nhận được "frame", nó sẽ kiểm tra địa chỉ trong "header" để xác định xem "frame" có được gửi cho mình hay không?. Các thiết bị mạng ở Data Link, như bộ chuyển mạch (switch), sẽ chuyển tiếp "frame" đến chính xác người nhận dựa trên địa chỉ trong "header". Đây là địa chỉ MAC.

> **Làm thế nào để có thể phát hiện lỗi trong quá trình truyền❓**
{: .prompt-warning }

Trong quá trình truyền dữ liệu, môi trường truyền thông có thể gây nhiễu, dẫn đến lỗi dữ liệu. Điều này có thể làm sai lệch nội dung "frame" — và đây là một vấn đề lớn! 😖
Để phát hiện lỗi, người ta thường sử dụng thuật toán **kiểm tra tổng "checksum"** hoặc **CRC (Cyclic Redundancy Check)**:
- Sender tính toán giá trị kiểm tra tổng cho "frame" và thêm nó vào cuối "frame".
- Receiver sau đó tính lại kiểm tra tổng từ dữ liệu nhận được và so sánh với giá trị ban đầu.
- Nếu hai giá trị khớp nhau, dữ liệu được xem là hợp lệ. Nếu không, dữ liệu đã bị lỗi trong quá trình truyền.

![Format Code](/assets/articles/2025/OSI_Model/Data_Link_layer_3.png){: .normal }

Đôi khi có nhiều thiết bị kết nối vào cùng một phương tiện truyền dẫn. Nếu hai hoặc nhiều thiết bị truyền dữ liệu đồng thời, xung đột dữ liệu có thể xảy ra, khiến  "frame" trở nên không thể hiểu được đối với người nhận.

Để tránh tình trạng này, Data Link Layer sử dụng một cơ chế gọi là CSMA (Carrier Sense Multiple Access). CSMA hoạt động bằng cách:
-	Lắng nghe phương tiện truyền dẫn (carrier sensing) để kiểm tra xem nó có đang nhàn rỗi hay không.
-	Chỉ truyền dữ liệu khi phương tiện sẵn sàng, giúp giảm nguy cơ xung đột.

## 3. Network Layer
**Tầng vận chuyển (Transport Layer)** truyền dữ liệu đến **tầng mạng (Network Layer)**. **Network Layer** chịu trách nhiệm truyền các **phân đoạn dữ liệu (segments)** đã nhận từ máy tính này sang máy tính khác trong các mạng khác nhau. Đơn vị dữ liệu của tầng mạng được gọi là **gói "packet"**. Các chức năng chính của **Network Layer** bao gồm:
- Định địa chỉ logic (logical addressing).
- Định tuyến (routing).
- Xác định đường dẫn (path determination).

![Format Code](/assets/articles/2025/OSI_Model/Network_layer.png){: .normal }

### Định địa chỉ logic (Logical Addressing)
Địa chỉ IP (IP Address) được sử dụng ở Network Layer được gọi là địa chỉ logic. **Mỗi máy tính trong mạng đều có một địa chỉ IP duy nhất**. Network Layer sẽ gán địa chỉ IP của người gửi và người nhận cho mỗi phân đoạn (segment) để tạo thành một gói dữ liệu IP (IP Packet). Việc gán địa chỉ IP giúp đảm bảo rằng mỗi gói dữ liệu được định tuyến đến máy tính yêu cầu.

![Format Code](/assets/articles/2025/OSI_Model/Network_layer_2.png){: .normal }

### Định tuyến (Routing)
Định tuyến là quá trình xác định đường đi tối ưu để chuyển các gói dữ liệu từ nguồn đến đích, dựa trên địa chỉ IP (IPv4 hoặc IPv6). Các bộ định tuyến (router) sẽ sử dụng các bảng định tuyến (routing table) để quyết định đường truyền tốt nhất giúp gói dữ liệu đến đúng máy tính yêu cầu dữ liệu.

![Format Code](/assets/articles/2025/OSI_Model/Network_layer_3.png){: .normal }

Giả sử máy tính A được kết nối với Network 1 và máy tính B được kết nối với Network 2. Địa chỉ IP của máy tính B là `192.168.2.1` và được lớp mạng thêm vào gói dữ liệu (packet) để đảm bảo rằng gói được định tuyến chính xác đến máy tính B.

![Format Code](/assets/articles/2025/OSI_Model/Network_layer_4.png){: .normal }

### Lựa chọn đường dẫn (Path determination)
Máy tính có thể kết nối với máy chủ Internet thông qua nhiều tuyến đường khác nhau. Đường dẫn tối ưu để truyền dữ liệu từ nguồn đến đích được xác định thông qua quá trình "lựa chọn đường dẫn" (path selection).

Các giao thức định tuyến như **OSPF (Open Shortest Path First)**, **BGP (Border Gateway Protocol)** và **IS-IS (Intermediate System to Intermediate System)** hoạt động ở lớp Network Layer, giúp xác định tuyến đường tối ưu dựa trên các tiêu chí như độ trễ, số lượng hop, hoặc chính sách mạng.

![Format Code](/assets/articles/2025/OSI_Model/Network_layer_5.png){: .normal }

### Tóm tắt lại quá trình truyền dữ liệu ở Network layer
Network layer sử dụng các gói tin "packet" làm đơn vị truyền tải và chịu trách nhiệm chọn đường đi cũng như chuyển tiếp gói tin đến nơi yêu cầu.

Thông thường, **mạng LAN** chỉ hoạt động trong phạm vi nhỏ, nhưng khi nhiều mạng nhỏ được kết nối lại với nhau, chúng có thể tạo thành một mạng lớn hơn và phức tạp hơn — chẳng hạn như **mạng WAN (Wide Area Network)**.

![Format Code](/assets/articles/2025/OSI_Model/Network_layer_6.png){: .normal }

Data Link layer cho phép các thiết bị giao tiếp trong cùng một mạng cục bộ (LAN) nhưng **không hỗ trợ giao tiếp giữa các mạng LAN khác nhau**. Hãy tưởng tượng toàn bộ mạng như một đồ thị, trong đó mỗi thiết bị là một nút và việc truyền dữ liệu giữa các nút liền kề được đảm nhiệm bởi Data Link layer.

**Để kết nối các mạng Internet khác nhau, Network layer sẽ gán một địa chỉ duy nhất (- là địa chỉ IP) cho mỗi thiết bị, giúp nhận dạng và liên lạc trên toàn mạng Internet**. Ngoài ra, Network layer sử dụng gói tin "packet" làm đơn vị truyền và triển khai cơ chế chọn đường đi (routing) để đảm bảo dữ liệu đến đúng đích.

Ví dụ, ta sẽ truyền data từ thiết bị "ant" đến thiết bị "banana" với sự hỗ trợ của nhiều mạng trung gian.

![Format Code](/assets/articles/2025/OSI_Model/Network_layer_7.png){: .normal }

Hãy tưởng tượng quá trình gửi dữ liệu như sau:
- "ant" đóng gói dữ liệu cần gửi vào một gói tin "packet". Trong "header" của gói tin "packet" có ghi địa chỉ của chuối (banana) – thiết bị nhận.
- Để đến được banana, "packet" phải đi qua Bộ định tuyến 1 (Router 1) và Bộ định tuyến 2 (Router 2).
- Vì ant và Router 1 nằm trong cùng một mạng LAN, chúng có thể giao tiếp trực tiếp qua Data Link layer. Lúc này, ant sẽ đóng gói gói tin "packet" vào bên trong một khung "frame" và gửi đến Router 1.
- Router 1 nhận khung, mở ra và trích xuất gói tin "packet". Thấy rằng đích đến là banana, nó quyết định chuyển tiếp gói tin đến Router 2.
- Router 1 lại đóng gói gói tin vào một khung "frame" mới để gửi cho Router 2 (cũng thông qua lớp liên kết dữ liệu giữa chúng).
- Quá trình này tiếp tục đến khi Router 2 nhận được gói tin và chuyển tiếp nó đến banana.
     
Network layer được xây dựng dựa trên Data Link layer, mở rộng khả năng truyền thông ra toàn bộ mạng Internet và cho phép dữ liệu được gửi đến bất kỳ nút nào trong mạng lưới. Trong khi đó, Data Link layer chỉ đảm nhiệm truyền thông trong phạm vi mạng cục bộ, cung cấp dịch vụ cho Network layer bằng cách gửi các gói tin "packet" đến nút kế tiếp. Hai lớp này phối hợp chặt chẽ với nhau, mỗi lớp có vai trò riêng nhưng hỗ trợ lẫn nhau để đảm bảo quá trình truyền thông diễn ra hiệu quả.

## 4. Transport Layer
Bên dưới **Session Layer** là **Transport Layer**. Lớp này chịu trách nhiệm kiểm soát độ tin cậy (reliability) của quá trình truyền thông bằng cách thực hiện các chức năng quan trọng như:
•	**Phân đoạn (Segmentation)**: Chia nhỏ dữ liệu từ lớp trên thành các gói dữ liệu (segments) nhỏ hơn để dễ dàng truyền tải qua mạng.
•	**Kiểm soát luồng (Flow Control)**: Điều chỉnh tốc độ truyền dữ liệu giữa hai thiết bị để tránh tình trạng mất gói hoặc tắc nghẽn.
•	**Kiểm soát lỗi (Error Control)**: Đảm bảo dữ liệu đến nơi không bị lỗi thông qua các cơ chế như kiểm tra tổng (checksum) và yêu cầu truyền lại (retransmission) nếu phát hiện lỗi.

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer.png){: .normal }

### Quá trình phân đoạn (segmentation)
Transport Layer nhận dữ liệu "Data" từ Session Layer và chia nhỏ dữ liệu thành các đơn vị dữ liệu gọi là **phân đoạn (segments)**.

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_2.png){: .normal }

Mỗi "segment" chứa số cổng nguồn (port), dữ liệu (data) và số thứ tự (number).

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_3.png){: .normal }

Các số cổng (port) giúp định tuyến từng "segment" đến đúng **ứng dụng đang yêu cầu**.

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_4.png){: .normal }

Số thứ tự (number) giúp ghép lại các "segments" theo đúng thứ tự, tạo thành "Data" hoàn chỉnh và chính xác ở phía ứng dụng.

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_5.png){: .normal }

### Kiểm soát luồng (Flow Control)
Transport layer có thể điều chỉnh lượng dữ liệu được truyền giữa các thiết bị. Giả sử một thiết bị di động kết nối với máy chủ "server". Nếu máy chủ có khả năng truyền dữ liệu ở tốc độ `100 Mbps`, trong khi thiết bị di động chỉ xử lý tối đa `10 Mbps`, thì khi tải xuống một **file**, máy chủ có thể bắt đầu gửi dữ liệu ở tốc độ `50 Mbps` — vượt quá khả năng xử lý của thiết bị di động. Lúc này, Transport layer trên thiết bị di động sẽ gửi tín hiệu yêu cầu máy chủ giảm tốc độ truyền xuống `10 Mbps` để tránh mất mát dữ liệu.

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_6.png){: .normal }

### Kiểm soát lỗi (Error Control)
Transport Layer đảm bảo dữ liệu được truyền chính xác và đầy đủ giữa các thiết bị. Trong quá trình truyền, nếu một số phân đoạn (segment) bị mất hoặc hỏng, Transport Layer ở phía nhận sẽ kích hoạt cơ chế yêu cầu lặp lại tự động **(Automatic Repeat reQuest - ARQ)** để yêu cầu gửi lại dữ liệu bị lỗi.

Để phát hiện lỗi, mỗi phân đoạn "segment" được gắn thêm một mã kiểm tra (checksum) trước khi gửi đi. Khi nhận được dữ liệu, phía nhận sẽ kiểm tra checksum để xác định xem phân đoạn "segment" có bị lỗi trong quá trình truyền hay không. Nếu phát hiện lỗi, cơ chế ARQ sẽ yêu cầu gửi lại phân đoạn đó, đảm bảo dữ liệu đến nơi là hoàn chỉnh và chính xác.

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_7.png){: .normal }

### Protocol 
Giao thức điều khiển ở Transport layer bao gồm truyền dẫn hướng kết nối (connection-oriented) và truyền dẫn không kết nối (connectionless). Truyền dẫn hướng kết nối được thực hiện thông qua **TCP** và truyền dẫn không kết nối được thực hiện thông qua **UDP**.

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_8.png){: .normal }

**UDP (User Datagram Protocol)** nhanh hơn **TCP (Transmission Control Protocol)** vì nó không cung cấp bất kỳ phản hồi nào về việc dữ liệu có thực sự được chuyển thành công hay không. Trong khi đó, TCP sử dụng cơ chế phản hồi và kiểm soát lỗi, cho phép phát hiện và truyền lại các gói dữ liệu bị mất, đảm bảo độ tin cậy.

UDP thường được sử dụng trong các ứng dụng mà việc mất một số gói dữ liệu không ảnh hưởng nhiều đến trải nghiệm, chẳng hạn như phát trực tuyến video/audio (streaming), trò chơi trực tuyến (online gaming), thoại qua IP (VoIP), TFTP (Trivial File Transfer Protocol) và DNS (Domain Name System).

Ngược lại, TCP được áp dụng trong các tình huống yêu cầu truyền tải dữ liệu đầy đủ và chính xác, như truy cập web (HTTP/HTTPS), email (SMTP/IMAP/POP3), và truyền tệp (FTP - File Transfer Protocol).

### Tóm tắt quá trình "Data" di chuyển

Transport Layer dựa trên nền tảng của Network Layer để giúp các chương trình ứng dụng trên các máy tính khác nhau trao đổi dữ liệu một cách đáng tin cậy. Lớp này quản lý luồng dữ liệu liên tục, đảm bảo rằng các phân đoạn (segments) được gửi đi đúng thứ tự, không bị mất hay lộn xộn trong quá trình truyền. Điều này giúp dữ liệu đến tay ứng dụng một cách chính xác và toàn vẹn.

Network Layer chịu trách nhiệm chuyển dữ liệu từ máy tính nguồn đến máy tính đích trong mạng Internet. Tuy nhiên, vì một máy tính có thể chạy nhiều chương trình cùng lúc (như trình duyệt, email, game...), Transport Layer sử dụng các cổng (port) để xác định dữ liệu cần được chuyển đến chương trình nào. Điều này đảm bảo rằng gói tin không chỉ đến đúng máy mà còn đến đúng ứng dụng yêu cầu.

Transport Layer chia nhỏ dữ liệu thành các phần nhỏ hơn gọi là phân đoạn "segment". Mỗi segment đều chứa thông tin về cổng gửi và cổng nhận, giúp xác định chương trình nguồn và đích. Sau đó, các segment được Network Layer đóng gói thành gói tin (packet) và gửi đến máy tính đích.

Khi đến nơi, Transport Layer ở phía nhận "receiver" sẽ tách các segment ra khỏi gói tin "packet", sau đó ghép lại theo đúng thứ tự và chuyển dữ liệu đến đúng ứng dụng đang yêu cầu.

Hình ảnh

Như hình minh họa, banana là máy chủ "server" chạy nhiều tiến trình dịch vụ, còn ant là máy tính chạy nhiều tiến trình ứng dụng. Khi người dùng trên ant truy cập vào trang web do banana cung cấp, tiến trình trình duyệt trên ant sẽ giao tiếp với tiến trình dịch vụ web trên banana.
-	Khi tiến trình trình duyệt **"Google Chorme"** gửi dữ liệu đến tiến trình dịch vụ Web, hệ thống sẽ đóng gói dữ liệu vào một phân đoạn "segment", trong đó "header" của "segment" sẽ chứa số cổng "Port" của dịch vụ Web.
-	"Segment" này cần được gửi đến banana. Hệ thống tiếp tục đóng gói "segment" vào một gói tin "packet" và giao cho **Network layer** xử lý. "header" của gói tin "packet" sẽ chứa địa chỉ đích là banana.
-	**Network layer** thực hiện việc lựa chọn đường đi cho gói tin "packet". Trong trường hợp này, nó cần đi qua **Router 1** và **Router 2** trước khi đến banana.
-	Để gửi gói tin "packet" đến **Router 1**, ant sẽ đóng gói nó vào một khung "frame" và giao cho **Data Link layer**. Địa chỉ đích trong "frame" là **địa chỉ MAC** của **Router 1**.
-	Khi "frame" đến **Router 2**, router sẽ lấy gói tin "packet" ra khỏi khung "frame" và kiểm tra địa chỉ IP đích trong "header" của gói tin "packet". **Router 2** chỉ quan tâm đến địa chỉ này để quyết định chuyển tiếp, mà không xem xét dữ liệu bên trong gói tin.
-	Sau khi gói tin "packet" được chuyển tiếp qua các router trung gian, nó cuối cùng đến được banana. Tại đây, **Network layer** trên banana lấy phân đoạn "segment" ra khỏi gói tin "packet" và chuyển cho **Transport layer**. **Transport layer** sẽ dựa vào số cổng "port" trong "header" của phân đoạn "segment" để chuyển dữ liệu đến đúng tiến trình dịch vụ Web.

## 5. Session Layer
Trước khi tìm hiểu về lớp hội thoại (Session layer), hãy tưởng tượng rằng bạn đang lên kế hoạch tổ chức một bữa tiệc. Để đảm bảo mọi hoạt động diễn ra suôn sẻ, bạn cần thiết lập một quy trình cụ thể, chẳng hạn như: trang trí không gian, nấu ăn, dọn dẹp và chào khách mời.

![Format Code](/assets/articles/2025/OSI_Model/Session_layer_1.png){: .normal }

Cũng tương tự như vậy, Session layer  được sử dụng để thiết lập, quản lý và duy trì các kết nối giữa các thiết bị. Nó điều phối việc kích hoạt, truyền và đồng bộ dữ liệu giữa hai đầu giao tiếp.

Giống như việc bạn thuê người giúp việc để quản lý các công đoạn trong bữa tiệc, lớp phiên cũng có những “trợ lý” riêng, chẳng hạn như các API (Application Programming Interfaces) hoặc giao diện lập trình ứng dụng khác. Một ví dụ phổ biến là NetBIOS (Network Basic Input/Output System), một giao thức hỗ trợ các ứng dụng trên các máy tính khác nhau giao tiếp với nhau trong cùng một mạng.

![Format Code](/assets/articles/2025/OSI_Model/Session_layer_2.png){: .normal }

Trước khi máy khách (client) thiết lập phiên (session) với máy chủ (server), máy chủ sẽ thực hiện một bước gọi là xác thực (authentication). Ví dụ, khi bạn nhập tên người dùng và mật khẩu trên máy tính của máy khách (gửi yêu cầu thông qua API của ứng dụng máy chủ), nếu thông tin hợp lệ, một phiên hoặc liên kết sẽ được thiết lập giữa máy khách và máy chủ.

![Format Code](/assets/articles/2025/OSI_Model/Session_layer_3.png){: .normal }

Sau khi xác thực (authentication), máy chủ sẽ tiếp tục kiểm tra quyền ủy quyền (authorization) của người dùng. Quyền ủy quyền là quá trình máy chủ xác định xem bạn có đủ quyền truy cập vào tệp hoặc tài nguyên yêu cầu hay không. Nếu máy chủ phát hiện rằng tài khoản bạn đang sử dụng không có quyền cần thiết, máy khách (client) sẽ nhận được thông báo lỗi, chẳng hạn như: "You are not authorized to access this page."

![Format Code](/assets/articles/2025/OSI_Model/Session_layer_4.png){: .normal }

Cả chức năng xác thực (authentication) và ủy quyền (authorization) đều được thực hiện bởi lớp phiên (Session Layer), đồng thời lớp này cũng theo dõi các tệp đang được tải xuống. Ví dụ, khi duyệt web, chúng ta truy cập các trang chứa văn bản, hình ảnh và thông tin khác. Những dữ liệu này được lưu trữ dưới dạng các tệp riêng biệt trên máy chủ web (web server).

Khi bạn yêu cầu một trang web, trình duyệt web (web browser) sẽ thiết lập một phiên (session) riêng với máy chủ để tải về các tệp văn bản và hình ảnh dưới dạng các gói dữ liệu (data packets). Lớp phiên theo dõi từng gói dữ liệu nhận được để xác định chúng thuộc tệp nào — văn bản (text) hay hình ảnh (image) — và theo dõi vị trí của chúng trong tệp gốc. Cuối cùng, trình duyệt sẽ kết hợp các gói dữ liệu này và hiển thị trang web hoàn chỉnh.

Đây chính là chức năng quản lý phiên (session management) của lớp phiên. Ngoài ra, trình duyệt còn đảm nhiệm các chức năng của lớp ứng dụng (Application Layer) và lớp trình bày (Presentation Layer) để đảm bảo dữ liệu được truyền và hiển thị chính xác.

![Format Code](/assets/articles/2025/OSI_Model/Session_layer_5.png){: .normal }

Tóm lại, Session Layer có ba chức năng chính:
- Session Management: Thiết lập, duy trì và kết thúc các phiên giao tiếp giữa các thiết bị.
- Session Control: Điều phối luồng dữ liệu, đảm bảo rằng các gói dữ liệu được truyền đi đúng thứ tự và không bị mất.
- 3Authorization: Kiểm tra quyền truy cập, đảm bảo người dùng có đủ quyền để truy cập tài nguyên yêu cầu.

## 6. Presentation Layer
Lớp trình bày nhận dữ liệu từ lớp ứng dụng (Application layer). Dữ liệu này thường dưới dạng ký tự và số. Lớp trình bày chuyển đổi các ký tự và dữ liệu thành định dạng nhị phân (ví dụ: 1001 0110) mà máy tính có thể hiểu được. Chức năng này được gọi là "dịch" (translation), tức là chuyển đổi ngôn ngữ của con người sang ngôn ngữ máy.

Trước khi truyền dữ liệu, lớp trình bày thực hiện nén dữ liệu (data compression) để giảm số bit cần thiết cho việc biểu diễn dữ liệu gốc. Nén dữ liệu giúp tiết kiệm băng thông và tăng tốc độ truyền tải, đặc biệt hữu ích trong các ứng dụng truyền video và âm thanh thời gian thực, nơi yêu cầu dữ liệu đến đích nhanh chóng mà vẫn giữ được tính toàn vẹn (data integrity) và chất lượng mã hóa.

![Format Code](/assets/articles/2025/OSI_Model/Presentation_layer_1.png){: .normal }

Đồng thời, tại phía gửi, dữ liệu sẽ được mã hóa (encryption) ở Presentation layer. Khi đến phía nhận, dữ liệu sẽ được giải mã (decryption) tại Presentation layer  để đảm bảo tính bảo mật (security) trong quá trình truyền tải.

![Format Code](/assets/articles/2025/OSI_Model/Presentation_layer_2.png){: .normal }

## 7. Application Layer
Lớp ứng dụng (Application layer) được các ứng dụng mạng sử dụng và là lớp gần với người dùng nhất. Nó cung cấp các dịch vụ cho các ứng dụng mạng — tức là các ứng dụng máy tính sử dụng Internet — và thực hiện các thao tác của người dùng thông qua nhiều giao thức khác nhau, chẳng hạn như:
-	FTP (File Transfer Protocol) – giao thức truyền tệp,
-	HTTP/HTTPS (HyperText Transfer Protocol/Secure) – giao thức duyệt web,
-	SMTP (Simple Mail Transfer Protocol) – giao thức truyền thư điện tử,
- Telnet – giao thức truyền thông giữa các thiết bị đầu cuối ảo.

![Format Code](/assets/articles/2025/OSI_Model/Application_layer.png){: .normal }

# Lời kết
