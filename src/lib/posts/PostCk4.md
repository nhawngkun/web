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
## Tiêu chí bắt buộc :
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

**- Đã đáp ứng ở mức cơ bản.**

**high-load-test.js**

```javascript
import http from 'k6/http';
import { check, sleep } from 'k6';
import { Rate } from 'k6/metrics';

const errorRate = new Rate('errors');

export const options = {
  stages: [
    { duration: '10s', target: 200 },  // Tăng dần lên 200 users trong 10s
    { duration: '20s', target: 500 },  // Tăng lên 500 users trong 20s tiếp
    { duration: '20s', target: 1000 }, // Tăng lên 1000 users trong 20s tiếp
    { duration: '10s', target: 0 },    // Giảm về 0 trong 10s cuối
  ],
  thresholds: {
    http_req_duration: ['p(95)<3000'], // 95% requests phải hoàn thành trong 3s
    errors: ['rate<0.1'],              // Error rate dưới 10%
  },
};

const BASE_URL = 'https://bookstore-api.onrender.com';

export default function () {
  const responses = {
    getBooks: http.get(`${BASE_URL}/api/books`),
    getCategories: http.get(`${BASE_URL}/api/categories`),
  };

  check(responses.getBooks, {
    'books status 200': (r) => r.status === 200,
    'books response < 3s': (r) => r.timings.duration < 3000,
  }) || errorRate.add(1);

  check(responses.getCategories, {
    'categories status 200': (r) => r.status === 200,
    'categories response < 3s': (r) => r.timings.duration < 3000,
  }) || errorRate.add(1);

  sleep(0.1); // 100ms delay giữa các requests
}
```

**Công cụ sử dụng:** k6 Công cụ kiểm thử tải mã nguồn mở, dễ sử dụng cho REST API.

**Kịch bản kiểm thử:** 
  - Tăng dần lên 200 users trong 10s.
  - Tăng lên 500 users trong 20s tiếp.
  - Tăng lên 1000 users trong 20s tiếp.

**Kết quả:** 
![Kết quả kiểm thử tải](/images/GK2/ketqua.jpg)


**Phân tích kết quả**
- Số lượng người dùng ảo (VUs): Mục tiêu 1000 VUs
- Số iterations hoàn thành: 427
- Số iterations bị gián đoạn: 667
- Tỷ lệ thành công: ~39%

**Kết luận**
- Hệ thống có thể xử lý ổn định khoảng 400-450 người dùng đồng thời.
- Khi số lượng người dùng đồng thời tăng lên 1000, hệ thống không thể xử lý toàn bộ requests.

## Tiêu chí tùy chọn (Chọn 1–2):
**1. Consistency Guarantees:**Đảm bảo tính nhất quán của dữ liệu giữa các nút


- Tôi sử dụng Neo4j Aura làm database.
- Neo4j Aura là dịch vụ cloud, bản chất đã có replication và consistency ở mức cluster (theo mô hình causal consistency của Neo4j).
- Khi tôi ghi dữ liệu vào Aura, các node trong cluster sẽ tự động đồng bộ để đảm bảo dữ liệu nhất quán, không bị mất mát hoặc xung đột.
- Tôi không cần tự code logic nhất quán, vì Neo4j Aura đã đảm bảo điều này ở tầng hạ tầng.
Kết luận:
✔️ Tôi đã đáp ứng tiêu chí này ở mức sử dụng dịch vụ cloud database có consistency built-in.

**2. Deployment Automation** Sử dụng Docker Compose, Kubernetes, hoặc script để tự động triển khai hệ thống.
- Tôi đã deploy Frontend trên Vercel, Backend trên Render, Database trên Neo4j Aura và web đã được up lên serve giờ mọi người đều có thể truy cập.
[Click here](https://bookstoreudpt.vercel.app/)
