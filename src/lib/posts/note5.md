---
title: "Buổi 5: notes 7 , các kiến trúc trong HPT"
date: "2025-05-19"
updated: "2025-05-19"
categories: ["hệ thống phân tán"]
excerpt: ""
coverImage: "/images/GK2/bai5.png"
---
## 1. So sánh các giao thức HTTP, TCP/IP, UDP, REST, GraphQL, SOAP, AJAX, RPC, gRPC

### Mục đích và vai trò:
- **TCP/IP**: Bộ giao thức nền tảng cho Internet, truyền dữ liệu đáng tin cậy qua mạng.
- **UDP**: Giao thức truyền tải không đáng tin cậy, không đảm bảo thứ tự hoặc nhận được gói tin.
- **HTTP**: Giao thức truyền siêu văn bản dùng cho web. Giao tiếp client-server.
- **REST**: Kiến trúc dựa trên HTTP, dùng để xây dựng API, dễ sử dụng và phổ biến.
- **GraphQL**: Ngôn ngữ truy vấn dữ liệu cho API, cho phép client chọn chính xác dữ liệu cần.
- **SOAP**: Giao thức dựa trên XML để truyền thông điệp, dùng trong hệ thống lớn, cần bảo mật cao.
- **AJAX**: Kỹ thuật trên trình duyệt để gửi/nhận dữ liệu không cần tải lại trang.
- **RPC**: Remote Procedure Call, gọi hàm từ xa như gọi hàm nội bộ.
- **gRPC**: Phiên bản hiện đại của RPC, hiệu suất cao, dùng giao thức HTTP/2 và protobuf.

### Tương quan và liên kết:
- HTTP là nền tảng cho REST, GraphQL, SOAP, AJAX.
- REST, GraphQL, SOAP đều là dạng API dùng trên HTTP.
- RPC và gRPC là kỹ thuật gọi hàm từ xa, gRPC hiện đại và hiệu quả hơn.
- AJAX sử dụng HTTP (thường là REST API) để cập nhật dữ liệu động.

---

## 2. Nghiên cứu thư viện OpenMPI và ứng dụng vào bài toán tính số nguyên tố

### OpenMPI là gì?
- OpenMPI là thư viện mã nguồn mở hỗ trợ lập trình song song phân tán bằng cách sử dụng chuẩn MPI (Message Passing Interface).
- Cho phép các tiến trình chạy trên nhiều máy tính khác nhau giao tiếp và phối hợp.

### Các hàm cơ bản:
- `MPI_Init`: Khởi tạo môi trường MPI.
- `MPI_Comm_size`: Lấy tổng số tiến trình.
- `MPI_Comm_rank`: Lấy ID của tiến trình hiện tại.
- `MPI_Send`, `MPI_Recv`: Gửi và nhận dữ liệu giữa các tiến trình.
- `MPI_Finalize`: Kết thúc môi trường MPI.

### Bài toán: Tính 10,000,000 số nguyên tố đầu tiên với 16 máy (mỗi máy 2 core)

#### Giải pháp:
1. **Thuật toán chọn**: Sử dụng thuật toán Sieve of Eratosthenes để tìm số nguyên tố.
2. **Phân chia công việc**:
   - Tổng cộng 32 core → chia đều khoảng cần tính cho 32 tiến trình.
   - Mỗi tiến trình tính một đoạn nhỏ trong khoảng [2, N].
3. **Giao tiếp**:
   - Tiến trình gốc (`rank 0`) chia khoảng cho các tiến trình con.
   - Các tiến trình con tính toán và gửi kết quả về tiến trình gốc.
4. **Linh hoạt số core**:
   - Sử dụng `MPI_Comm_size` để tự động lấy số tiến trình.
   - Mỗi tiến trình tính theo ID và số lượng tổng → dễ dàng chạy với 8, 10, 12, 32 core tùy ý.

### Kết luận:
- OpenMPI giúp xử lý phân tán bài toán lớn hiệu quả.
- Việc lập trình sử dụng MPI cần quan tâm chia dữ liệu đều, đồng bộ kết quả và tránh trùng lặp.

