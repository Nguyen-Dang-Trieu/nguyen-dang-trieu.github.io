---
title: Mô hình OSI
date: 2025-2-21
categories: [Network]
tags: []
author: Trieu
image:
  path: 
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
- 1.	Kiến (ant) đóng gói dữ liệu cần gửi vào một gói tin. Trong tiêu đề của gói tin có ghi địa chỉ của chuối (banana) – thiết bị nhận.
- 2.	Để đến được banana, gói tin phải đi qua Bộ định tuyến 1 (Router 1) và Bộ định tuyến 2 (Router 2).
- 3.	Vì ant và Router 1 nằm trong cùng một mạng LAN, chúng có thể giao tiếp trực tiếp qua lớp liên kết dữ liệu. Lúc này, ant sẽ đóng gói gói tin vào bên trong một khung (frame) và gửi đến Router 1.
- 4.	Router 1 nhận khung, mở ra và trích xuất gói tin. Thấy rằng đích đến là banana, nó quyết định chuyển tiếp gói tin đến Router 2.
- 5.	Router 1 lại đóng gói gói tin vào một khung mới để gửi cho Router 2 (cũng thông qua lớp liên kết dữ liệu giữa chúng).
- 6.	Quá trình này tiếp tục đến khi Router 2 nhận được gói tin và chuyển tiếp nó đến banana.
     
Lớp mạng được xây dựng dựa trên lớp liên kết dữ liệu, mở rộng khả năng truyền thông ra toàn bộ mạng và cho phép dữ liệu được gửi đến bất kỳ nút nào trong mạng. Trong khi đó, lớp liên kết dữ liệu chỉ đảm nhiệm truyền thông trong phạm vi mạng cục bộ, cung cấp dịch vụ cho lớp mạng bằng cách gửi các gói tin đến nút kế tiếp. Hai lớp này phối hợp chặt chẽ với nhau, mỗi lớp có vai trò riêng nhưng bổ trợ lẫn nhau để đảm bảo quá trình truyền thông diễn ra hiệu quả.

