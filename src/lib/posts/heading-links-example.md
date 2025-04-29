---
title: "Hệ thống phân tán"
date: "2025-04-29"
updated: "2025-04-29"
categories:
  - sveltekit
  - markdown
coverImage: "/static/images/git.jpg"
coverWidth: 16
coverHeight: 9
excerpt: "Tổng quan về hệ thống phân tán: định nghĩa, ứng dụng, khái niệm cốt lõi và kiến trúc."
---

## 💻 1. Hệ thống phân tán là gì?

Hệ thống phân tán là một tập hợp các máy tính độc lập được kết nối qua mạng, phối hợp với nhau để đạt được một mục tiêu chung. Dù các thành phần có thể nằm ở nhiều vị trí địa lý khác nhau, hệ thống vẫn hoạt động như một thực thể thống nhất, giúp tăng khả năng mở rộng, độ tin cậy và hiệu suất.

## ⚙️ 2. Ứng dụng của hệ thống phân tán

Hệ thống phân tán có nhiều ứng dụng trong thực tế, bao gồm:

- **Điện toán đám mây**: Google Cloud, AWS, Azure
- **Mạng xã hội**: Facebook, Twitter
- **Thương mại điện tử**: Amazon, Shopee
- **Ngân hàng trực tuyến**
- **Video streaming**: Netflix, YouTube
- **Blockchain & tiền mã hóa**: Bitcoin, Ethereum
- **IoT & cảm biến thông minh**



## 🔑 3. Các khái niệm chính của hệ thống phân tán.

- ↔**Scalability**: Khả năng mở rộng hệ thống mà không giảm hiệu suất.
- **Fault Tolerance**: Khả năng tiếp tục hoạt động dù một hoặc nhiều thành phần gặp sự cố.
- **Availability**: Mức độ hệ thống luôn sẵn sàng phục vụ (đo bằng % uptime).
- **Transparency**: Ẩn đi các chi tiết phân tán đối với người dùng (vị trí, lỗi, di chuyển tài nguyên).
- **Concurrency**: Cho phép nhiều tiến trình hoặc người dùng tương tác đồng thời.
- **Parallelism**: Xử lý các tác vụ song song để tăng thông lượng.
- **Openness**: Hệ thống sử dụng chuẩn mở (REST, gRPC, JSON…) để tích hợp linh hoạt.
- **Vertical Scaling**: Tăng tài nguyên (CPU, RAM) cho một node đơn.
- **Horizontal Scaling**: Thêm nhiều node để xử lý tải.
- **Load Balancer**: Phân phối tải giữa các node để tránh nghẽn.
- **Replication**: Sao chép dữ liệu/dịch vụ để đảm bảo độ tin cậy và hiệu suất.

### Ví dụ: Netflix

- ↔**Scalability**: Mở rộng hạ tầng đám mây để phục vụ hàng triệu người dùng.
- **Fault Tolerance**: Tự động chuyển luồng phát khi server gặp lỗi.
- **Availability**: Sẵn sàng 24/7.
- **Transparency**: Người dùng không biết nội dung được phát từ khu vực AWS nào.
- **Concurrency & Parallelism**: Hàng ngàn người xem cùng lúc, nội dung được xử lý song song.
- **Openness**: Tương thích với TV, điện thoại, trình duyệt.
- **Vertical vs. Horizontal Scaling**: Ưu tiên thêm server thay vì nâng cấp đơn lẻ.
- **Load Balancer**: Phân phối yêu cầu đến các server.
- **Replication**: Sao lưu nội dung tại nhiều vùng địa lý.

## 🏛️ 4. Kiến trúc của hệ thống phân tán

Một số mô hình kiến trúc phổ biến:

1. **Client-Server**: Phân tầng client – server.
2. **3-Tier**: Presentation – Application – Data.
3. **Microservices**: Dịch vụ nhỏ, độc lập giao tiếp qua API.
4. **SOA**: Dịch vụ hướng đối tượng, giao tiếp XML/HTTP.
5. **Event-Driven**: Giao tiếp qua sự kiện (Kafka, RabbitMQ).
6. **Peer-to-Peer (P2P)**: Node ngang hàng chia sẻ tài nguyên.
7. **Serverless (FaaS)**: Chạy mã không cần quản lý server (AWS Lambda).

### Ví dụ: Kiến trúc Microservices của Shopee

- **API Gateway**: Quản lý toàn bộ yêu cầu từ client.
- **Cart Service**, **Search Service**, **Payment Service**, **Logistics Service**.
- **Database per Service**: Tách cơ sở dữ liệu riêng cho từng service.
- **Message Queue** (Kafka/RabbitMQ): Giao tiếp không đồng bộ.
- **Serverless Functions**: Xử lý tác vụ phụ (gửi email, thông báo).
- **Load Balancer** (NGINX/HAProxy): Phân bổ tải đến các node.

