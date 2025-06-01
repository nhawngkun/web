---
title: "Bài tập giữa kì số 4"
date: "2025-05-1010"
updated: "2025-05-1010"
categories:
  - sveltekit
  - markdown
coverImage: "/images/GK1/ANH1.jpg"
coverWidth: 16
coverHeight: 9
excerpt: "Deliverable 4 - Ứng dụng Phân tán - Giữa kỳ"
---
### 1. Fault Tolerance
**- Đã đáp ứng ở mức cơ bản.**

- Backend triển khai trên Render, tự động restart khi gặp lỗi.
- Database dùng Neo4j Aura (cloud), có sẵn cơ chế tự động backup, phục hồi, replication.
- Frontend trên Vercel, tự động phục hồi khi gặp lỗi triển khai.
- Xử lý lỗi: Backend có try-catch, trả về lỗi hợp lý, không làm sập toàn bộ hệ thống khi một API lỗi.
### 2. Distributed Communication
**- Đã đáp ứng.**

- Frontend (Vercel), Backend (Render), Database (Neo4j Aura) chạy trên các máy chủ/cloud khác nhau.
- Giao tiếp qua HTTP RESTful API (axios, express).
- Hệ thống hoạt động phân tán thực sự, không chỉ trên một máy.
### 3. Sharding hoặc Replication
**- Đã đáp ứng ở mức sử dụng Replication mặc định của Neo4j Aura.**

- Neo4j Aura hỗ trợ replication dữ liệu giữa các node trong cụm (cluster) để đảm bảo tính sẵn sàng và an toàn dữ liệu.
- Chưa có sharding (phân mảnh dữ liệu theo khóa) hoặc replication tự xây dựng, nhưng đã tận dụng tính năng replication của dịch vụ cloud.
### 4. Simple Monitoring / Logging
**- Đã đáp ứng ở mức cơ bản.**

Backend có log lỗi và log hoạt động ra console (xem các file controller, có console.log, console.error).
Frontend có thông báo lỗi/toast cho người dùng.
Admin Dashboard hiển thị thống kê số lượng user, sách, thể loại, lượt đọc (bảng điều khiển đơn giản).
Chưa có logging nâng cao (ghi file log, dashboard real-time), nhưng đã có monitoring cơ bản.
### 5. Basic Stress Test
Chưa thấy có kiểm thử tải tự động trong mã nguồn.

Chưa có script hoặc tài liệu mô phỏng nhiều request đồng thời (ví dụ: dùng JMeter, k6, hoặc script tự động gửi nhiều request).
Nếu bạn đã từng dùng Postman để gửi nhiều request hoặc kiểm thử tải thủ công, hãy bổ sung tài liệu/hình ảnh minh họa.
