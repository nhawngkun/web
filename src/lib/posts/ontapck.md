---
title: "Bài kiểm tra ôn tập cuối kỳ"
date: "2025-05-23"
updated: "2025-05-23"
categories: ["hệ thống phân tán"]
excerpt: "Các câu hỏi ôn tập cuối kì"
coverImage: "images/GK1/ck1.jpg"
---
## 1. Hệ thống phân tán, tập trung, phi tập trung khác nhau như thế nào, nêu ví dụ về mỗi loại, làm thế nào để xác định sự khác biệt chính?
- Hệ thống tập trung (Centralized system): Toàn bộ dữ liệu và xử lý nằm ở một máy chủ trung tâm. Ví dụ: hệ thống ngân hàng nội bộ cũ.
- Hệ thống phân tán (Distributed system): Dữ liệu và xử lý được phân tán trên nhiều máy tính kết nối mạng. Ví dụ: Google Search, Hadoop.
- Hệ thống phi tập trung (Decentralized system): Không có máy chủ trung tâm, các nút có thể đồng cấp, cùng tham gia xử lý và ra quyết định. Ví dụ: Blockchain, Bitcoin.
- Phân biệt: Dựa trên cách tổ chức xử lý và lưu trữ dữ liệu (1 điểm trung tâm hay nhiều điểm) và quyền kiểm soát.

## 2. Các đặc tính của hệ phân tán là gì? giải thích cho từng đặc điểm thật kỹ (tìm trong slides)
- Tính minh bạch: Người dùng không biết tài nguyên đang ở đâu (vị trí), ai sở hữu (truy cập), sao lưu như thế nào (sao lưu),...
- Khả năng mở rộng: Có thể mở rộng về kích thước, địa lý, quản lý mà không ảnh hưởng hiệu năng.
- Tính chịu lỗi: Hệ thống vẫn hoạt động bình thường dù một phần bị lỗi.
- Hiệu năng: Tận dụng tài nguyên phân tán để cải thiện hiệu suất.
- Tính đồng bộ thời gian: Cần đồng bộ hóa đồng hồ để thống nhất thứ tự các sự kiện.

## 3. Mục đích của nút chủ trong một hệ phân tán là để làm gì? Điều gì sẽ xảy ra nếu nút chủ gặp sự cố?
- Nút chủ thường điều phối hoạt động (phân công công việc, quản lý tài nguyên).
- Nếu nút chủ bị lỗi: hệ thống có thể ngừng hoạt động, hoặc cần cơ chế chọn nút chủ mới (ví dụ: Bully algorithm).

## 4. Trong một mạng không gian, tại sao các máy thường giao tiếp với nhau qua gossip protocol mà không gửi thông tin đến tất cả các máy khác hoặc gửi về một nút trung tâm?
Gossip giúp truyền thông tin theo cách phân tán, chịu lỗi tốt, giảm tải mạng và tránh tắc nghẽn do broadcast hoặc gửi về trung tâm.

## 5. Các yếu tố cốt lõi của một hệ phân tán là gì?
- Giao tiếp mạng
- Đồng bộ hóa
- Tính nhất quán dữ liệu
- Quản lý lỗi
- Quản lý tài nguyên

## 6. Nêu những lí do sử dụng hệ phân tán

- Hiệu năng cao hơn 

- Chịu lỗi tốt hơn

- hả năng mở rộng linh hoạt

- Chia sẻ tài nguyên dễ dàng

## 7. Nêu định nghĩa hệ phân tán

Là hệ thống gồm nhiều máy tính độc lập, phối hợp với nhau để đạt mục tiêu chung, được nhìn như một hệ thống thống nhất với người dùng.

## 8. Chụp và để lên blog các hình của thuật toán Cristian, Berkeley, RBS, Lamport, Bully, Ring
Minh họa thuật toán Cristian
![Minh họa thuật toán Cristian](/images/cristian.webp)

Minh họa thuật toán Berkeley
![Minh họa thuật toán Berkeley](/images/berkeley.webp)

Minh họa thuật toán Lamport
![Minh họa thuật toán Lamport](/images/lamport.webp)

Minh họa thuật toán Bully
![Minh họa thuật toán Bully](/images/bully.webp)

Minh họa thuật toán Ring
![Minh họa thuật toán Ring](/images/ring.webp)


## 9. Kỹ thuật phân tán nào hỗ trợ lập trình thủ tục, lập trình web, hướng đối tượng, liệt kê
- Lập trình thủ tục: RPC
- Lập trình hướng đối tượng: CORBA, Java RMI
- Lập trình web: REST API, SOAP

## 10. Tiến trình nhẹ, tiến trình, luồng có những ưu điểm và nhược điểm gì, liệt kê. Khi một lời gọi hệ thống dừng thì đối với 3 loại như thế nào. Tiến trình nhẹ, tiến trình và luồng có mối quan hệ như thế nào với nhau và với chính nó?
### Tiến trình (Process):
- **Ưu điểm:**
  - Cách ly tốt giữa các tiến trình
  - Bảo mật cao do không chia sẻ bộ nhớ
  - Lỗi trong một tiến trình không ảnh hưởng đến tiến trình khác
- **Nhược điểm:**
  - Tốn tài nguyên hệ thống (mỗi tiến trình có không gian địa chỉ riêng)
  - Giao tiếp giữa các tiến trình phức tạp và chậm
  - Chi phí chuyển đổi ngữ cảnh cao

### Luồng (Thread):
- **Ưu điểm:**
  - Nhẹ hơn tiến trình, tốn ít tài nguyên
  - Chia sẻ tài nguyên trong cùng tiến trình (bộ nhớ, tệp đã mở)
  - Chuyển đổi ngữ cảnh nhanh hơn
  - Giao tiếp giữa các luồng dễ dàng
- **Nhược điểm:**
  - Dễ gây lỗi nếu không đồng bộ hóa tốt
  - Một luồng gặp sự cố có thể ảnh hưởng đến cả tiến trình
  - Khó gỡ lỗi hơn

### Tiến trình nhẹ (Lightweight Process - LWP):
- **Đặc điểm:** Là khái niệm trung gian giữa thread và process
- **Ưu điểm:**
  - Được lên lịch bởi hệ điều hành như process riêng biệt
  - Chia sẻ tài nguyên của tiến trình cha như thread
- **Nhược điểm:**
  - Phụ thuộc vào cài đặt cụ thể của hệ điều hành
  - Phức tạp trong quản lý

### Khi một lời gọi hệ thống dừng:
- **Tiến trình:** Toàn bộ tiến trình dừng lại cùng với tất cả các luồng bên trong
- **Luồng:** Chỉ dừng luồng thực hiện lời gọi, các luồng khác trong cùng tiến trình vẫn tiếp tục
- **Tiến trình nhẹ:** Phụ thuộc vào cài đặt hệ thống, thường chỉ dừng LWP đó

### Mối quan hệ:
- Một tiến trình chứa ít nhất một luồng (luồng chính)
- Nhiều luồng có thể tồn tại trong cùng một tiến trình
- Các luồng trong một tiến trình chia sẻ không gian địa chỉ và tài nguyên
- Tiến trình nhẹ là cách triển khai của luồng ở mức hệ điều hành

## 11. Mô hình client server là gì, vai trò của máy chủ và máy khách là gì.
Mô hình client-server là mô hình trong đó:
- Client: gửi yêu cầu.
- Server: nhận, xử lý và trả kết quả.
- Ví dụ: trình duyệt là client, máy chủ web là server.

## 12. Quá trình gọi thủ tục từ xa là gì
Là phương pháp cho phép gọi hàm từ một máy khác thông qua mạng như gọi cục bộ. Quá trình này gồm:
- Đóng gói tham số (marshalling)
- Truyền dữ liệu qua mạng
- Giải mã (unmarshalling) và thực thi hàm trên server
- Trả kết quả về cho client

## 13. Nêu mục đích và lợi ích của mô hình phân tầng giao thức, hướng thông điệp bền vững
### Mục đích:
- Tách biệt các chức năng xử lý
- Chuẩn hóa giao tiếp giữa các thành phần
- Đơn giản hóa thiết kế hệ thống
### Lợi ích:
- Dễ phát triển và bảo trì
- Tái sử dụng các tầng
- Hỗ trợ mở rộng
- Dễ dàng thay thế các thành phần
### Hướng thông điệp bền vững:
- Đảm bảo truyền tin không bị mất mát 
- Xử lý đúng thứ tự các thông điệp
- Hỗ trợ hoạt động offline và khắc phục lỗi

## 14. Sharding là gì
Là kỹ thuật chia nhỏ cơ sở dữ liệu lớn thành nhiều phần nhỏ (shard), mỗi shard lưu ở một server khác nhau để tăng hiệu năng và khả năng mở rộng.

## 15. Các gói luồng có thế làm những nhiệm vụ gì?
- Tạo và quản lý vòng đời luồng
- Đồng bộ hóa luồng
- Quản lý tài nguyên chia sẻ
- Giao tiếp giữa các luồng
- Quản lý lịch trình thực thi
- Hỗ trợ tạo thread pool

## 16. Phân loại sự khác nhau giữa luồng kiểu người dùng và luồng kiểu nhân
### User-level Thread:
- Quản lý bởi thư viện người dùng
- Không cần gọi hệ thống để chuyển đổi ngữ cảnh
- Không tận dụng được đa nhân CPU
- Độc lập với hệ điều hành
- Hiệu suất cao trong chuyển đổi ngữ cảnh

### Kernel-level Thread:
- Quản lý trực tiếp bởi hệ điều hành
- Tận dụng được đa nhân CPU
- Tốn tài nguyên hơn user-level thread
- Chuyển đổi ngữ cảnh chậm hơn
- Cung cấp khả năng lập lịch song song thực sự

## 17. Nêu các hàm chính ở trong RPC và giải thích chức năng của nó
### Client Stub:
- Đóng gói tham số (marshalling)
- Gửi yêu cầu đến server
- Nhận và giải mã kết quả trả về
### Server Stub:
- Nhận và giải mã yêu cầu từ client
- Gọi hàm thực tế trên server
- Đóng gói kết quả và gửi lại client
### RPC Runtime:
- Quản lý kết nối mạng
- Xử lý lỗi truyền thông
- Đảm bảo độ tin cậy của lời gọi
- Hỗ trợ tìm kiếm dịch vụ (service discovery)

## 18. Định nghĩa tiến trình, thread, multithread client, multithread server

### Tiến trình (Process):
- Đơn vị chương trình đang chạy độc lập
- Có không gian địa chỉ riêng và tài nguyên riêng
- Được quản lý bởi hệ điều hành

### Thread (Luồng):
- Luồng thực thi nhỏ nhất trong một tiến trình
- Chia sẻ không gian địa chỉ và tài nguyên với các thread khác trong cùng tiến trình
- Cho phép thực thi đồng thời trong một tiến trình

### Multithread Client:
- Client có thể xử lý nhiều tác vụ đồng thời (gửi nhiều yêu cầu)
- Giao diện người dùng phản hồi nhanh trong khi xử lý nền
- Cải thiện hiệu suất khi tương tác với nhiều server

### Multithread Server:
- Server xử lý nhiều yêu cầu từ client đồng thời
- Mỗi thread có thể phục vụ một kết nối/yêu cầu
- Tận dụng tối đa tài nguyên phần cứng

## 19. Review lại kiến thức căn bản của Map, Reduce và mục đích trong một hệ phân tán

### Map:
- Chia nhỏ dữ liệu đầu vào thành các phần độc lập
- Xử lý song song các phần này trên nhiều máy tính
- Tạo ra các cặp `<key, value>` trung gian

### Reduce:
- Gom nhóm các giá trị theo key từ kết quả Map
- Tổng hợp hoặc kết hợp dữ liệu từ các Map
- Tạo ra kết quả cuối cùng

### Mục đích trong hệ phân tán:
- Xử lý dữ liệu lớn hiệu quả bằng cách phân tán công việc
- Tận dụng tính song song của nhiều máy
- Đơn giản hóa xử lý phân tán thông qua mô hình lập trình thống nhất
- Chịu lỗi nhờ khả năng phục hồi tác vụ

## 20. Ảo hóa (Virtulization) là gì, mục đích của ảo hóa trong một hệ phân tán dùng để làm gì?

### Ảo hóa:
- Tạo ra các phiên bản ảo của tài nguyên vật lý (máy tính, mạng, lưu trữ)
- Cho phép nhiều hệ điều hành/ứng dụng chạy đồng thời trên cùng phần cứng
- Tách biệt logic giữa phần mềm và phần cứng

### Mục đích trong hệ phân tán:
- Tối ưu hóa sử dụng tài nguyên phần cứng
- Cách ly ứng dụng và dịch vụ
- Tăng tính linh hoạt trong triển khai và di chuyển dịch vụ
- Hỗ trợ khả năng mở rộng và co giãn động
- Đơn giản hóa quản lý tài nguyên phân tán

## 21. Review lại các kiến trúc của server đa luồng
### Thread-per-request:
- Mỗi yêu cầu từ client được xử lý bởi một thread riêng
- Đơn giản để triển khai
- Phù hợp với số lượng kết nối nhỏ
### Thread pool:
- Duy trì một nhóm thread sẵn sàng
- Phân phối các yêu cầu đến các thread trong pool
- Kiểm soát tốt hơn việc sử dụng tài nguyên
### Event-driven:
- Sử dụng vòng lặp sự kiện đơn luồng để nhận yêu cầu
- Chuyển các tác vụ dài đến thread pool riêng biệt
- Hiệu quả với số lượng kết nối lớn
- Mô hình reactor và proactor
## 22. Review lại các hướng tiếp cận cài đặt luồng
### Thư viện luồng:
- Sử dụng các API như pthread (POSIX threads)
- Java Thread API
- C++ std::thread
### API hệ điều hành:
- Windows Thread API
- Linux pthread
### Framework hỗ trợ:
- OpenMP: Cung cấp chỉ thị song song hóa mức cao
- Intel TBB: Thư viện template cho song song hóa
- Các framework như Akka, Erlang OTP hỗ trợ mô hình actor

## 23. Tại sao chúng ta sử dụng bảng băm phân tán (review slides), mục đích của bảng băm phân tán là gì, Consitent Hashing là gì, Finger table để làm gì, tại sao sử dụng finger table
### Bảng băm phân tán (DHT):
- Phân phối khóa/dữ liệu đều trên nhiều nút
- Cung cấp khả năng tìm kiếm và truy xuất hiệu quả
- Hỗ trợ mở rộng quy mô linh hoạt
### Mục đích của DHT:
- Phân phối tải đồng đều
- Định vị dữ liệu nhanh chóng
- Hoạt động phi tập trung
- Tự tổ chức khi nút tham gia/rời đi
### Consistent Hashing:
- Kỹ thuật băm giảm thiểu việc phân phối lại dữ liệu khi thêm/xóa nút
- Chỉ một phần nhỏ dữ liệu cần di chuyển khi topology thay đổi
- Sử dụng vòng băm để phân phối đều
### Finger Table:
- Bảng định tuyến trong DHT (ví dụ: Chord)
- Lưu trữ thông tin về một tập con các nút trong mạng
- Tối ưu hóa quá trình tìm kiếm
### Lý do sử dụng Finger Table:
- Giảm độ phức tạp tìm kiếm từ O(n) xuống O(log n)
- Tối ưu hóa không gian lưu trữ (không cần lưu tất cả các nút)
- Cân bằng giữa chi phí duy trì và hiệu suất tìm kiếm

## 24. Không gian phẳng là gì, định danh là gì, liệt kê các đặc điểm của không gian phẳng
### Không gian phẳng:
- Tổ chức không có cấu trúc phân cấp
- Mỗi thực thể được xác định bởi định danh duy nhất
- Mô hình tổ chức ngang hàng (peer-to-peer)
### Định danh:
- Chuỗi ký tự/số duy nhất xác định một thực thể
- Thường được tạo bằng hàm băm
- Độc lập với vị trí vật lý hoặc địa chỉ mạng
### Đặc điểm của không gian phẳng:
- Không có cấu trúc phân cấp
- Truy cập trực tiếp theo định danh
- Dễ mở rộng và phân tán
- Tự tổ chức
- Khó định tuyến hiệu quả nếu không có cơ chế bổ trợ
- Phù hợp với hệ phân tán ngang hàng

## 25. Tại sao cần đồng bộ hóa đồng hồ logic, tại sao đồng hồ vật lý không đảm bảo. mục đích của đồng bộ hóa và các thuật toán đồng bộ hóa là gì
### Lý do cần đồng bộ đồng hồ logic:
- Đồng hồ vật lý có sai số (clock drift)
- Không thể đảm bảo đồng bộ tuyệt đối
- Cần xác định thứ tự sự kiện trong hệ phân tán
### Vấn đề của đồng hồ vật lý:
- Mỗi máy có đồng hồ riêng chạy ở tốc độ khác nhau
- Sai lệch tích lũy theo thời gian
- Không thể đồng bộ hoàn hảo do độ trễ mạng
### Mục đích đồng bộ hóa:
- Xác định thứ tự của các sự kiện
- Đảm bảo tính nhất quán của hệ thống
- Phát hiện quan hệ nhân quả giữa các sự kiện
- Hỗ trợ điều phối hoạt động
### Các thuật toán đồng bộ hóa:
- Lamport timestamps: Đồng bộ đồng hồ logic
- Vector clocks: Bổ sung thông tin nhân quả
- Cristian's algorithm: Đồng bộ đồng hồ vật lý
- Berkeley algorithm: Tính thời gian trung bình
- NTP (Network Time Protocol): Đồng bộ qua internet

## 26. Đồng hồ Lamport giải quyết vấn đề gì, nêu và giải thích các rules của đồng hồ Lamport
### Vấn đề giải quyết:
- Xác định thứ tự logic của các sự kiện trong hệ phân tán
- Bảo toàn mối quan hệ "xảy ra trước" (happens-before)
- Đảm bảo tính nhất quán trong hệ thống

### Quy tắc của đồng hồ Lamport:
1. **Quy tắc 1 - Cập nhật cục bộ:**
   - Mỗi tiến trình tăng đồng hồ logic của nó trước khi tạo ra sự kiện
   - LC = LC + 1 (LC: Local Clock)
2. **Quy tắc 2 - Gửi thông điệp:**
   - Khi gửi thông điệp, timestamp của thông điệp = giá trị đồng hồ logic hiện tại
   - Gửi(message, LC)
3. **Quy tắc 3 - Nhận thông điệp:**
   - Khi nhận thông điệp, cập nhật đồng hồ logic:
   - LC = max(LC, timestamp_received) + 1

## 27. Tham khảo lại các bài tập của đồng hồ logic Lamport trong slides chương 4-5
Các bài tập trong slides chương 4-5 minh họa:
- Cách tính toán timestamp Lamport qua các sự kiện
- Vẽ biểu đồ không gian-thời gian
- Xác định quan hệ nhân quả giữa các sự kiện
- Phân tích các tình huống tranh chấp

## 28. Giao thức đồng bộ NTP là gì, PTP là gì, được tính toán như thế nào
### NTP (Network Time Protocol):
- Giao thức đồng bộ thời gian qua mạng internet
- Sắp xếp các máy chủ thời gian theo tầng (stratum)
- Độ chính xác đạt mức mili giây (ms)
- Sử dụng thuật toán lọc để giảm ảnh hưởng của độ trễ mạng
### PTP (Precision Time Protocol):
- Giao thức đồng bộ thời gian chính xác cao
- Độ chính xác đạt mức micro giây (µs) hoặc nano giây (ns)
- Thường dùng trong mạng LAN chuyên biệt
- Yêu cầu phần cứng hỗ trợ để đạt độ chính xác cao nhất
### Cách tính toán:
1. **NTP:**
   - T1: Thời gian client gửi yêu cầu
   - T2: Thời gian server nhận yêu cầu
   - T3: Thời gian server trả lời
   - T4: Thời gian client nhận phản hồi
   - Độ trễ: δ = [(T4-T1) - (T3-T2)]/2
   - Độ lệch: θ = [(T2-T1) + (T3-T4)]/2
2. **PTP:**
   - Sử dụng cơ chế master-slave
   - Gửi thông điệp Sync từ master đến slave
   - Đánh dấu thời gian chính xác bằng phần cứng
   - Tính toán và bù trừ độ trễ mạng

## 29. Review lại python cơ bản
### Biến và kiểu dữ liệu:
- Kiểu số: `int`, `float`, `complex`
- Kiểu chuỗi: `str`
- Kiểu tập hợp: `list`, `tuple`, `set`, `dict`
- Kiểu boolean: `True`, `False`
### Cấu trúc điều khiển:
- Câu lệnh điều kiện: `if`, `elif`, `else`
- Vòng lặp: `for`, `while`
- Xử lý ngoại lệ: `try`, `except`, `finally`
### Hàm và module:
- Định nghĩa hàm: `def function_name(params):`
- Hàm lambda: `lambda x: x*2`
- Import module: `import module_name` hoặc `from module import function`
### OOP trong Python:
- Khai báo lớp: `class ClassName:`
- Khởi tạo: `__init__(self, params)`
- Kế thừa: `class Child(Parent):`
- Tính đa hình: ghi đè phương thức
### Thư viện phổ biến:
- OS và hệ thống: `os`, `sys`, `shutil`
- Thời gian: `datetime`, `time`
- Mạng và web: `requests`, `urllib`
- Xử lý dữ liệu: `pandas`, `numpy`
- Đa luồng: `threading`, `multiprocessing`