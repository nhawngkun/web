---
title: "Bài tập giữa kì số 4"
date: "2025-06-02"
updated: "2025-06-02"
categories:
  - sveltekit
  - markdown
coverImage: "/images/GK1/ANH1.jpg"
coverWidth: 16
coverHeight: 9
excerpt: "Deliverable 4 - Ứng dụng Phân tán - Giữa kỳ"
---
**Tải file FDP tại đây**
[File PDF](/images/GK2/Baocao.pdf)

**Link Gibhub**https://github.com/nhawngkun/BTlon-udpt-
## Tiêu chí bắt buộc :
### 1. Fault Tolerance

**- Đã đáp ứng ở mức cơ bản.**

**1. Backend (Render)**
- Triển khai trên Render:
Render là nền tảng cloud có khả năng tự động giám sát và khởi động lại ứng dụng Node.js/Express nếu gặp sự cố (crash, out-of-memory, lỗi mạng, v.v).
Tự động restart:
- Khi backend bị lỗi hoặc dừng đột ngột, Render sẽ tự động khởi động lại tiến trình, đảm bảo dịch vụ luôn sẵn sàng cho người dùng.
- Xử lý lỗi trong code:
   - Các controller backend đều sử dụng try-catch để bắt lỗi khi truy vấn database hoặc xử lý logic.
   - Nếu một API bị lỗi, backend trả về mã lỗi và thông báo hợp lý (ví dụ: 500 Internal Server Error), các API khác vẫn hoạt động bình thường.
Không có lỗi nào làm sập toàn bộ server. 

**2. Database (Neo4j Aura)**
- Dùng Neo4j Aura Cloud:
  - Neo4j Aura là dịch vụ database cloud có sẵn tính năng replication (sao lưu dữ liệu sang nhiều node) và backup tự động.
  - Nếu một node database gặp sự cố, hệ thống tự động chuyển sang node khác mà không ảnh hưởng đến ứng dụng.
Dữ liệu luôn được bảo vệ và phục hồi nhanh chóng khi có sự cố.

**3. Frontend (Vercel)**
- Triển khai trên Vercel:
   - Vercel tự động phục hồi khi gặp lỗi triển khai hoặc downtime.
   - Nếu frontend bị lỗi build/deploy, chỉ cần sửa lỗi và push lại code, Vercel sẽ tự động build lại và phục hồi dịch vụ.
   - Người dùng luôn có thể truy cập giao diện web khi backend/database vẫn hoạt động.
### 2. Distributed Communication

**- Đã đáp ứng.**


**1. Triển khai phân tán trên nhiều nền tảng cloud**
**Frontend:**
- Được triển khai trên Vercel (một nền tảng cloud chuyên cho frontend).
- Người dùng truy cập website qua domain công khai của Vercel từ bất kỳ đâu trên Internet.

**Backend:**
- Được triển khai trên Render (cloud platform cho backend).
- Backend có địa chỉ riêng, nhận các request từ frontend hoặc các client khác qua Internet.

**Database:**
- Sử dụng Neo4j Aura (cloud database), hoàn toàn tách biệt với backend và frontend.
- Backend kết nối tới Neo4j Aura qua Internet bằng giao thức bảo mật.

**2. Giao tiếp qua HTTP RESTful API**

**Frontend ↔ Backend:**
- Frontend sử dụng thư viện axios để gửi các yêu cầu HTTP (GET, POST, PUT, DELETE) tới backend.
- Ví dụ: Khi người dùng truy cập trang danh sách sách, frontend gửi request GET tới endpoint /books của backend trên Render.

**Backend ↔ Database:**
- Backend sử dụng thư viện neo4j-driver để giao tiếp với Neo4j Aura thông qua giao thức mạng (bolt+s hoặc https).
- Các truy vấn dữ liệu (Cypher query) được gửi qua mạng tới database cloud, nhận kết quả trả về.

**3. Đặc điểm phân tán thực sự**

**Các thành phần độc lập về vật lý:**
- Mỗi thành phần chạy trên một máy chủ/cloud riêng biệt, không phụ thuộc vào nhau về mặt vật lý.
- Có thể mở rộng hoặc thay thế từng thành phần mà không ảnh hưởng đến thành phần khác.
- Hệ thống vẫn hoạt động khi triển khai trên nhiều máy, không chỉ chạy trên localhost.


### 3. Sharding hoặc Replication

**- Đã đáp ứng ở mức sử dụng Replication mặc định của Neo4j Aura.**
- Neo4j Aura hỗ trợ replication dữ liệu giữa các node trong cụm (cluster) để đảm bảo tính sẵn sàng và an toàn dữ liệu.
- Chưa có sharding (phân mảnh dữ liệu theo khóa) hoặc replication tự xây dựng, nhưng đã tận dụng tính năng replication của dịch vụ cloud.

### 4. Simple Monitoring / Logging

**- Đã đáp ứng ở mức cơ bản.**

**1. Sử dụng Replication mặc định của Neo4j Aura**
- Neo4j Aura là dịch vụ cơ sở dữ liệu đồ thị trên cloud, cung cấp sẵn tính năng replication (sao chép dữ liệu) giữa nhiều node trong một cụm (cluster).
- Khi bạn ghi dữ liệu (thêm, sửa, xóa sách), dữ liệu sẽ được tự động sao chép sang các node khác trong cluster.
- Nếu một node gặp sự cố (lỗi phần cứng, mất kết nối...), các node còn lại vẫn giữ bản sao dữ liệu và tiếp tục phục vụ truy vấn, đảm bảo hệ thống không bị gián đoạn.

**2. Lợi ích của Replication**

**Tăng tính sẵn sàng:**

Hệ thống vẫn hoạt động bình thường ngay cả khi một hoặc nhiều node trong cluster bị lỗi.

**Bảo vệ dữ liệu:**
Dữ liệu không bị mất mát do luôn có nhiều bản sao trên các node khác nhau.
Tự động chuyển đổi (failover):
Khi node chính gặp sự cố, Neo4j Aura sẽ tự động chuyển vai trò sang node khác mà không cần can thiệp thủ công.

**3. Chưa có Sharding hoặc Replication tự xây dựng**

- Sharding (phân mảnh dữ liệu theo khóa, ví dụ: chia sách theo thể loại hoặc tác giả) chưa được áp dụng trong hệ thống hiện tại.
- Replication được sử dụng là tính năng mặc định của Neo4j Aura, bạn không cần tự viết code để đồng bộ dữ liệu giữa các node.

**4. Minh họa thực tế**
- Khi người dùng thêm một cuốn sách mới qua backend, dữ liệu sẽ được ghi vào Neo4j Aura và tự động sao chép sang các node khác trong cluster.
- Nếu một node database bị lỗi, các truy vấn vẫn được phục vụ từ các node còn lại mà người dùng không nhận thấy sự gián đoạn.

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
