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

Mỗi lớp trong mô hình OSI thực chất là một tập hợp các giao thức **"protocol"**. Ví dụ, khi nói về lớp ứng dụng (Application layer), chúng ta không chỉ đề cập đến các ứng dụng máy tính như Google Chrome, Firefox hay các phần mềm khác, mà còn bao gồm rất nhiều giao thức lớp ứng dụng (application layer protocols). Những giao thức này cho phép các chương trình ứng dụng hoạt động chính xác trên mạng. 

![Format Code](/assets/articles/2025/OSI_Model/OSI_Model_3.png){: .normal }

Hãy cùng xem xét chi tiết mô hình bảy lớp OSI.

# Khám phá chi tiết về mô hình OSI
## 1. Physical layer
**Lớp Vật Lý (Physical Layer)** chịu trách nhiệm **truyền các bit thô** giữa các thiết bị qua các phương tiện vật lý (như cáp đồng, cáp quang, sóng vô tuyến, ...). Lớp này **không quan tâm đến ý nghĩa của chuỗi bit** mà chỉ đảm bảo dữ liệu được truyền đi. Đồng thời, nó cũng **che giấu sự phức tạp của các thiết bị vật lý** so với các lớp trên.

![Format Code](/assets/articles/2025/OSI_Model/Physical_layer_2.png){: .normal }

Dữ liệu từ ứng dụng (Application Layer) sẽ trải qua các bước xử lý sau:
- Transport Layer: Dữ liệu được chia thành các phần nhỏ gọi là phân đoạn (segments).
- Network Layer: Các phân đoạn này được đóng gói thành gói dữ liệu **"packets"**, trong đó có chứa địa chỉ IP của người gửi và người nhận.
- Data Link Layer: Các gói dữ liệu được chuyển đổi thành khung **"frames"** dưới dạng chuỗi nhị phân (binary stream).
- Physical Layer: Cuối cùng, chuỗi nhị phân được chuyển đổi thành các tín hiệu (điện, quang hoặc vô tuyến) và được truyền qua các phương tiện cục bộ như cáp đồng, cáp quang hoặc tín hiệu không dây. Cuối cùng dữ liệu sẽ được hiển thị trên máy tính mục tiêu.

![Format Code](/assets/articles/2025/OSI_Model/Physical_layer.png){: .normal }

## 2. Data Link Layer
**Lớp liên kết dữ liệu (Data Link Layer)** nhận các gói dữ liệu **"packets"** từ **Network Layer**, trong đó chứa địa chỉ IP (IP address) của người gửi và người nhận. Có hai loại địa chỉ được sử dụng: **địa chỉ logic (logical addressing)** và **địa chỉ vật lý (physical addressing)**.
-	Địa chỉ logic đã được thêm vào ở **Network Layer**, nghĩa là địa chỉ IP của người gửi và người nhận được chèn vào phân đoạn dữ liệu để tạo thành một gói dữ liệu IP (IP packet).
-	Địa chỉ vật lý được xử lý tại **Data Link Layer** bằng cách thêm **địa chỉ MAC (Media Access Control)** của máy tính gửi và nhận vào gói dữ liệu IP, tạo thành một khung dữ liệu (frame). Địa chỉ MAC là duy nhất và được nhà sản xuất gán cho mỗi thiết bị mạng. Thậm chí, điện thoại di động của chúng ta cũng có một địa chỉ MAC riêng biệt.

![Format Code](/assets/articles/2025/OSI_Model/Data_Link_layer.png){: .normal }

Đơn vị dữ liệu trong lớp liên kết dữ liệu (Data Link Layer) được gọi là khung dữ liệu (frame). Lớp liên kết dữ liệu thường được triển khai thông qua thẻ giao diện mạng (Network Interface Card – NIC) dưới dạng phần mềm hoặc phần cứng trong máy tính.

NIC cung cấp giao diện để truyền dữ liệu từ máy tính này sang máy tính khác thông qua các phương tiện truyền dẫn cục bộ như:
-	Dây đồng (twisted pair cable),
-	Cáp quang (fiber optic),
-	Hoặc tín hiệu vô tuyến (wireless signals).

![Format Code](/assets/articles/2025/OSI_Model/Data_Link_layer_2.png){: .normal }

Lớp liên kết dữ liệu sử dụng khung (frame) làm đơn vị truyền và chịu trách nhiệm định địa chỉ mạng cũng như phát hiện lỗi. Nhờ đó, lớp này giải quyết các vấn đề truyền thông giữa nhiều máy chủ trong cùng một mạng và đảm bảo việc giao tiếp trong mạng LAN.

Trong môi trường truyền thông chia sẻ — nơi nhiều thiết bị cùng sử dụng chung một kênh truyền dữ liệu để giao tiếp (đặc biệt là trong mạng cục bộ - LAN) — chỉ riêng lớp vật lý là không đủ, vì lớp này chỉ truyền các bit mà không thể xác định ai sẽ nhận dữ liệu.

Để làm được điều này, dữ liệu cần được đóng gói vào các khung (frame) và bổ sung thông tin địa chỉ vào tiêu đề khung để xác định nơi nhận dữ liệu. 

![Format Code](/assets/articles/2025/OSI_Model/Data_Link_layer_4.png){: .normal }

Nếu so sánh một khung với một lá thư, thì phần tiêu đề giống như phong bì, còn dữ liệu là nội dung lá thư. Phong bì cần ghi địa chỉ người nhận, tương tự như tiêu đề khung chứa địa chỉ đích.

Khi một thiết bị nhận được khung, nó sẽ kiểm tra địa chỉ trong tiêu đề để xác định xem khung có được gửi cho mình hay không. Các thiết bị mạng lớp 2, như bộ chuyển mạch (switch), sẽ chuyển tiếp khung chính xác đến người nhận dựa trên địa chỉ trong tiêu đề. Địa chỉ này được gọi là địa chỉ lớp 2 hay địa chỉ MAC.

**Làm thế nào để có thể phát hiện lỗi trong quá trình truyền?**

Trong quá trình truyền dữ liệu, môi trường truyền thông có thể gây nhiễu, dẫn đến lỗi dữ liệu. Điều này có thể làm sai lệch nội dung khung — và đây là một vấn đề lớn!
Để phát hiện lỗi, người ta thường sử dụng thuật toán kiểm tra tổng (checksum) hoặc CRC (Cyclic Redundancy Check):
- Sender tính toán giá trị kiểm tra tổng cho khung và thêm nó vào cuối khung.
- Receiver sau đó tính lại kiểm tra tổng từ dữ liệu nhận được và so sánh với giá trị ban đầu.
- Nếu hai giá trị khớp nhau, dữ liệu được xem là hợp lệ. Nếu không, dữ liệu đã bị lỗi trong quá trình truyền.

![Format Code](/assets/articles/2025/OSI_Model/Data_Link_layer_3.png){: .normal }

Đôi khi có nhiều thiết bị kết nối vào cùng một phương tiện truyền dẫn. Nếu hai hoặc nhiều thiết bị truyền dữ liệu đồng thời, xung đột dữ liệu có thể xảy ra, khiến thông điệp trở nên không thể hiểu được đối với người nhận.

Để tránh tình trạng này, lớp liên kết dữ liệu (Data Link Layer) sử dụng một cơ chế gọi là CSMA (Carrier Sense Multiple Access). CSMA hoạt động bằng cách:
-	Lắng nghe phương tiện truyền dẫn (carrier sensing) để kiểm tra xem nó có đang nhàn rỗi hay không.
-	Chỉ truyền dữ liệu khi phương tiện sẵn sàng, giúp giảm nguy cơ xung đột.

## 3. Network Layer
**Tầng vận chuyển (Transport Layer)** truyền dữ liệu đến **tầng mạng (Network Layer)**. **Network Layer** chịu trách nhiệm truyền các **phân đoạn dữ liệu (segments)** đã nhận từ máy tính này sang máy tính khác trong các mạng khác nhau. Đơn vị dữ liệu của tầng mạng được gọi là gói (packet). Các chức năng chính của **Network Layer** bao gồm:
- Định địa chỉ logic (logical addressing).
- Định tuyến (routing).
- Xác định đường dẫn (path determination).

![Format Code](/assets/articles/2025/OSI_Model/Network_layer.png){: .normal }

### Định địa chỉ logic (Logical Addressing)
Địa chỉ IP (IP Address) được sử dụng ở tầng mạng (Network Layer) được gọi là địa chỉ logic. Mỗi máy tính trong mạng đều có một địa chỉ IP duy nhất. Tầng mạng sẽ gán địa chỉ IP của người gửi (source IP) và người nhận (destination IP) cho mỗi phân đoạn (segment) để tạo thành một gói dữ liệu IP (IP Packet). Việc gán địa chỉ IP giúp đảm bảo rằng mỗi gói dữ liệu được định tuyến đến máy tính yêu cầu

![Format Code](/assets/articles/2025/OSI_Model/Network_layer_2.png){: .normal }

### Định tuyến (Routing)
Định tuyến là quá trình xác định đường đi tối ưu để chuyển các gói dữ liệu từ nguồn đến đích, dựa trên địa chỉ IP (IPv4 hoặc IPv6). Các bộ định tuyến (router) sẽ sử dụng các bảng định tuyến (routing table) để quyết định đường truyền tốt nhất giúp gói dữ liệu đến đúng máy tính đích.

![Format Code](/assets/articles/2025/OSI_Model/Network_layer_3.png){: .normal }

Giả sử máy tính A được kết nối với Network 1 và máy tính B được kết nối với Network 2. Địa chỉ IP của máy tính B là 192.168.2.1 và được lớp mạng thêm vào gói dữ liệu (packet) để đảm bảo rằng gói được định tuyến chính xác đến máy tính B.

![Format Code](/assets/articles/2025/OSI_Model/Network_layer_4.png){: .normal }

### Lựa chọn đường dẫn
Máy tính có thể kết nối với máy chủ Internet qua nhiều tuyến đường khác nhau. Đường dẫn tối ưu để truyền dữ liệu từ nguồn đến đích được gọi là 'lựa chọn đường dẫn' (path selection). Các giao thức hỗ trợ lựa chọn đường dẫn bao gồm OSPF (Open Shortest Path First), BGP (Border Gateway Protocol) và IS-IS (Intermediate System to Intermediate System), giúp xác định tuyến đường tối ưu để truyền dữ liệu.

![Format Code](/assets/articles/2025/OSI_Model/Network_layer_5.png){: .normal }

### Tóm tắt 
Lớp mạng sử dụng các gói tin (packet) làm đơn vị truyền tải và chịu trách nhiệm chọn đường đi cũng như chuyển tiếp gói tin đến đích.

Mạng LAN thường chỉ hoạt động trong phạm vi nhỏ, nhưng khi nhiều mạng nhỏ được kết nối lại với nhau, chúng có thể tạo thành một mạng lớn hơn và phức tạp hơn — chẳng hạn như mạng WAN (Wide Area Network).

![Format Code](/assets/articles/2025/OSI_Model/Network_layer_6.png){: .normal }

Lớp liên kết dữ liệu cho phép các thiết bị giao tiếp trong cùng một mạng cục bộ (LAN) nhưng không hỗ trợ giao tiếp giữa các mạng LAN khác nhau. Hãy tưởng tượng toàn bộ mạng như một đồ thị, trong đó mỗi thiết bị là một nút và việc truyền dữ liệu giữa các nút liền kề được đảm nhiệm bởi lớp liên kết dữ liệu.

Để kết nối các mạng khác nhau, lớp mạng sẽ gán một địa chỉ duy nhất (chẳng hạn như địa chỉ IP) cho mỗi thiết bị, giúp nhận dạng và liên lạc trên toàn mạng. Ngoài ra, lớp mạng sử dụng gói tin (packet) làm đơn vị truyền và triển khai cơ chế chọn đường đi (routing) để đảm bảo dữ liệu đến đúng đích.

Ví dụ, hãy tưởng tượng một con kiến muốn gửi thức ăn đến một nải chuối ở xa. Nó cần xác định đường đi qua nhiều nút (như lá cây, cành cây) để đến đích. Lớp mạng cũng hoạt động tương tự, giúp dữ liệu "tìm đường" qua nhiều mạng trung gian để đến đúng thiết bị nhận.

![Format Code](/assets/articles/2025/OSI_Model/Network_layer_7.png){: .normal }

Hãy tưởng tượng quá trình gửi dữ liệu như sau:
- Kiến (ant) đóng gói dữ liệu cần gửi vào một gói tin. Trong tiêu đề của gói tin có ghi địa chỉ của chuối (banana) – thiết bị nhận.
- Để đến được banana, gói tin phải đi qua Bộ định tuyến 1 (Router 1) và Bộ định tuyến 2 (Router 2).
- Vì ant và Router 1 nằm trong cùng một mạng LAN, chúng có thể giao tiếp trực tiếp qua lớp liên kết dữ liệu. Lúc này, ant sẽ đóng gói gói tin vào bên trong một khung (frame) và gửi đến Router 1.
- Router 1 nhận khung, mở ra và trích xuất gói tin. Thấy rằng đích đến là banana, nó quyết định chuyển tiếp gói tin đến Router 2.
- Router 1 lại đóng gói gói tin vào một khung mới để gửi cho Router 2 (cũng thông qua lớp liên kết dữ liệu giữa chúng).
- Quá trình này tiếp tục đến khi Router 2 nhận được gói tin và chuyển tiếp nó đến banana.
     
Lớp mạng được xây dựng dựa trên lớp liên kết dữ liệu, mở rộng khả năng truyền thông ra toàn bộ mạng và cho phép dữ liệu được gửi đến bất kỳ nút nào trong mạng. Trong khi đó, lớp liên kết dữ liệu chỉ đảm nhiệm truyền thông trong phạm vi mạng cục bộ, cung cấp dịch vụ cho lớp mạng bằng cách gửi các gói tin đến nút kế tiếp. Hai lớp này phối hợp chặt chẽ với nhau, mỗi lớp có vai trò riêng nhưng bổ trợ lẫn nhau để đảm bảo quá trình truyền thông diễn ra hiệu quả.

## 4. Transport Layer
Bên dưới lớp phiên (Session Layer) là lớp vận chuyển (Transport Layer). Lớp này chịu trách nhiệm kiểm soát độ tin cậy (reliability) của quá trình truyền thông bằng cách thực hiện các chức năng quan trọng như:
•	Phân đoạn (Segmentation): Chia nhỏ dữ liệu từ lớp trên thành các gói dữ liệu (segments) nhỏ hơn để dễ dàng truyền tải qua mạng.
•	Kiểm soát luồng (Flow Control): Điều chỉnh tốc độ truyền dữ liệu giữa hai thiết bị để tránh tình trạng mất gói hoặc tắc nghẽn.
•	Kiểm soát lỗi (Error Control): Đảm bảo dữ liệu đến nơi không bị lỗi thông qua các cơ chế như kiểm tra tổng (checksum) và yêu cầu truyền lại (retransmission) nếu phát hiện lỗi.

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer.png){: .normal }

Đầu tiên, trong quá trình phân đoạn (segmentation), lớp vận chuyển (Transport Layer) nhận dữ liệu từ lớp phiên (Session Layer) và chia nhỏ dữ liệu thành các đơn vị dữ liệu gọi là phân đoạn (segments).

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_2.png){: .normal }

Mỗi đơn vị dữ liệu (segment) chứa số cổng nguồn (port), dữ liệu (data) và số thứ tự (number).

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_3.png){: .normal }

Các số cổng (port) giúp định tuyến từng phân đoạn (segment) đến đúng ứng dụng đang yêu cầu

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_4.png){: .normal }

Số thứ tự (number) giúp ghép lại các phân đoạn (segments) theo đúng thứ tự, tạo thành message hoàn chỉnh và chính xác ở phía người nhận.

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_5.png){: .normal }

(2) Thứ hai, trong kiểm soát luồng (flow control), lớp vận chuyển (transport layer) có thể điều chỉnh lượng dữ liệu được truyền giữa các thiết bị. Giả sử một thiết bị di động kết nối với máy chủ. Nếu máy chủ có khả năng truyền dữ liệu ở tốc độ 100 Mbps, trong khi thiết bị di động chỉ xử lý tối đa 10 Mbps, thì khi tải xuống một tệp, máy chủ có thể bắt đầu gửi dữ liệu ở tốc độ 50 Mbps — vượt quá khả năng xử lý của thiết bị di động. Lúc này, lớp vận chuyển trên thiết bị di động sẽ gửi tín hiệu yêu cầu máy chủ giảm tốc độ truyền xuống 10 Mbps để tránh mất mát dữ liệu.

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_6.png){: .normal }

(3) Cuối cùng là kiểm soát lỗi (error control). Lớp vận chuyển (transport layer) giúp đảm bảo dữ liệu được truyền chính xác và đầy đủ. Trong quá trình truyền, nếu một số phân đoạn (segments) bị mất hoặc hỏng, lớp vận chuyển ở phía nhận sẽ sử dụng cơ chế yêu cầu lặp lại tự động (Automatic Repeat reQuest - ARQ) để yêu cầu gửi lại dữ liệu. Để phát hiện lỗi, lớp vận chuyển thêm một mã kiểm tra (checksum) vào mỗi phân đoạn. Phía nhận sẽ kiểm tra checksum để xác định xem dữ liệu có bị lỗi trong quá trình truyền hay không.

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_7.png){: .normal }

Giao thức điều khiển lớp vận chuyển (transport layer protocols) bao gồm truyền dẫn hướng kết nối (connection-oriented) và truyền dẫn không kết nối (connectionless). Truyền dẫn hướng kết nối được thực hiện thông qua TCP và truyền dẫn không kết nối được thực hiện thông qua UDP.

![Format Code](/assets/articles/2025/OSI_Model/Transport_layer_8.png){: .normal }

UDP (User Datagram Protocol) nhanh hơn TCP (Transmission Control Protocol) vì nó không cung cấp bất kỳ phản hồi nào về việc dữ liệu có thực sự được chuyển thành công hay không. Trong khi đó, TCP sử dụng cơ chế phản hồi và kiểm soát lỗi, cho phép phát hiện và truyền lại các gói dữ liệu bị mất, đảm bảo độ tin cậy.

UDP thường được sử dụng trong các ứng dụng mà việc mất một số gói dữ liệu không ảnh hưởng nhiều đến trải nghiệm, chẳng hạn như phát trực tuyến video/audio (streaming), trò chơi trực tuyến (online gaming), thoại qua IP (VoIP), TFTP (Trivial File Transfer Protocol) và DNS (Domain Name System).

Ngược lại, TCP được áp dụng trong các tình huống yêu cầu truyền tải dữ liệu đầy đủ và chính xác, như truy cập web (HTTP/HTTPS), email (SMTP/IMAP/POP3), và truyền tệp (FTP - File Transfer Protocol).

### Tóm tắt

Dựa trên lớp mạng, lớp vận chuyển giúp các chương trình ứng dụng trên các máy tính khác nhau có thể trao đổi dữ liệu với nhau một cách đáng tin cậy. Lớp này kiểm soát việc gửi dữ liệu liên tục, đảm bảo dữ liệu không bị mất hoặc lộn xộn trong quá trình truyền.

Lớp mạng có nhiệm vụ chuyển dữ liệu từ máy tính này đến máy tính khác trong mạng. Nhưng vì một máy tính có thể chạy nhiều chương trình cùng lúc (như trình duyệt, email, game...), lớp vận chuyển sử dụng các cổng để biết dữ liệu cần chuyển đến chương trình nào.

Lớp vận chuyển chia nhỏ dữ liệu thành các phần nhỏ hơn gọi là phân đoạn. Mỗi phân đoạn sẽ có thông tin về cổng gửi và cổng nhận. Sau đó, các phân đoạn này được lớp mạng đóng gói và gửi đến máy tính đích. Khi đến nơi, lớp vận chuyển sẽ lấy dữ liệu ra và gửi đúng đến chương trình cần nhận.

Hình ảnh

Như hình minh họa, banana là máy chủ chạy nhiều tiến trình dịch vụ, còn ant là máy tính chạy nhiều tiến trình ứng dụng. Khi người dùng trên ant truy cập vào trang web do banana cung cấp, tiến trình trình duyệt trên ant sẽ giao tiếp với tiến trình dịch vụ web trên banana.
-	Khi tiến trình trình duyệt gửi dữ liệu đến tiến trình dịch vụ Web, hệ thống sẽ đóng gói dữ liệu vào một phân đoạn (segment), trong đó tiêu đề của phân đoạn sẽ chứa số cổng của dịch vụ Web.
-	Phân đoạn này cần được gửi đến banana. Hệ thống tiếp tục đóng gói phân đoạn vào một gói tin (packet) và giao cho lớp mạng xử lý. Tiêu đề gói tin sẽ chứa địa chỉ đích là banana.
-	Lớp mạng thực hiện việc lựa chọn đường đi cho gói tin. Trong trường hợp này, gói tin cần đi qua Router 1 và Router 2 trước khi đến banana.
-	Để gửi gói tin đến Router 1, ant sẽ đóng gói gói tin vào một khung (frame) và giao cho lớp liên kết dữ liệu. Địa chỉ đích trong khung là địa chỉ MAC của Router 1.
-	Khi gói tin đến một bộ định tuyến trung gian (router), router sẽ lấy gói tin ra khỏi khung và kiểm tra địa chỉ IP đích trong tiêu đề gói tin. Router chỉ quan tâm đến địa chỉ này để quyết định chuyển tiếp, mà không xem xét dữ liệu bên trong gói tin.
-	Sau khi gói tin được chuyển tiếp qua các router trung gian, nó cuối cùng đến được banana. Tại đây, lớp mạng trên banana lấy phân đoạn ra khỏi gói tin và chuyển cho lớp giao vận. Lớp giao vận sẽ dựa vào số cổng trong tiêu đề phân đoạn để chuyển dữ liệu đến đúng tiến trình dịch vụ Web.

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
