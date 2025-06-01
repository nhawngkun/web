---
title: "Bài tập giữa kì số 2"
date: "2025-05-1010"
updated: "2025-05-1010"
categories:
  - sveltekit
  - markdown
coverImage: "/images/GK1/ANH1.jpg"
coverWidth: 16
coverHeight: 9
excerpt: "Deliverable 2 - Website"
---
## Thiết kế hệ thống (Deliverable 2)	
### 1. Bản vẽ kiến trúc hệ thống: 
![Bản vẽ kiến trúc hệ thống](/images/GK2/neo4j.png)

### 2.Thành phần chính
**1.Frontend (React/ViteJS):**
**Vai trò:**
- Hiển thị giao diện người dùng.
- Gửi yêu cầu API tới backend để lấy dữ liệu.
- Hiển thị danh sách sách, thông tin chi tiết, và các chức năng quản trị.
**Hoạt động:**
- Sử dụng Axios để gửi yêu cầu HTTP tới backend.
- Render dữ liệu nhận được từ backend bằng các component React.

**2.Backend (NodeJS/Express):**
**Vai trò:**
- Xử lý yêu cầu từ frontend.
- Thực hiện các thao tác CRUD (thêm, sửa, xóa, lấy dữ liệu) trên cơ sở dữ liệu Neo4j.
- Quản lý xác thực và phân quyền người dùng.

**Hoạt động:**
- Nhận yêu cầu HTTP từ frontend qua RESTful API.
- Chuyển đổi yêu cầu thành truy vấn Cypher để tương tác với Neo4j.
- Trả về dữ liệu JSON cho frontend.

**3.Database (Neo4j):**

**Vai trò:**
- Lưu trữ dữ liệu sách, tác giả, thể loại dưới dạng đồ thị.
- Quản lý các quan hệ giữa các thực thể (ví dụ: WRITTEN_BY, BELONGS_TO).

**Hoạt động:**
- Nhận truy vấn Cypher từ backend.
- Trả về kết quả truy vấn (dữ liệu sách, tác giả, thể loại).

**4.API (RESTful):**

**Vai trò:**
- Là cầu nối giữa frontend và backend.
- Cung cấp các endpoint để thực hiện các thao tác như lấy danh sách sách, thêm sách, xóa sách, tìm kiếm sách, v.v.

**Hoạt động:**
- Nhận yêu cầu từ frontend, xử lý và trả về kết quả.

### 3. Công nghệ và thư viện sử dụng
#### Công nghệ:
**1.Neo4j:**
**Lý do chọn:**
- Cơ sở dữ liệu đồ thị phù hợp để quản lý các quan hệ phức tạp giữa sách, tác giả, thể loại.
- Hiệu quả trong việc truy vấn các mối quan hệ.

**2.Node.js/Express.js:**
**Lý do chọn:**
- Xử lý phía máy chủ nhanh chóng, dễ dàng tích hợp với Neo4j.
- Hỗ trợ xây dựng RESTful API.

**3.React/ViteJS:**
**Lý do chọn:**
- Tạo giao diện người dùng hiện đại, đáp ứng.
- Vite giúp tăng tốc độ phát triển và xây dựng ứng dụng.

**4.Tailwind CSS:**
**Lý do chọn:**
- Tạo kiểu giao diện nhanh chóng, dễ tùy chỉnh.
#### Thư viện:
**Axios:** Gửi yêu cầu HTTP từ frontend tới backend.

**dotenv:** Quản lý biến môi trường để bảo mật thông tin cấu hình.

**neo4j-driver:** Tương tác với cơ sở dữ liệu Neo4j từ backend.

**react-router-dom:** Quản lý điều hướng trong ứng dụng React.

**react-hot-toast:** Hiển thị thông báo cho người dùng.
### 4.Mô hình dữ liệu (Database Model)
**1.Book:**

**Thuộc tính:** id, name, author, lang, category, image, title, link, content, description.
**Quan hệ:**
- WRITTEN_BY → Author.
- BELONGS_TO → Category.

**2.Author:**

**Thuộc tính:** name.

**Quan hệ:**
- WRITES → Book.

**3.Category:**

**Thuộc tính:** name.

**Quan hệ:**
- CONTAINS → Book.

**4.User:**
**Thuộc tính:**
- id: Mã định danh duy nhất của người dùng.
- username: Tên đăng nhập.
- email: Email của người dùng.
- password: Mật khẩu đã được mã hóa.
- role: Vai trò của người dùng (user hoặc admin).

**Quan hệ:**
- ADDED → Book (nếu người dùng thêm sách).

**5.Admin:**

**Thuộc tính:**   
- id: Mã định danh duy nhất của admin.
- username: Tên đăng nhập.
- email: Email của admin.
- password: Mật khẩu đã được mã hóa.

**Quan hệ:**
- MANAGES → Book (nếu admin quản lý sách).
### 5.Chiến lược triển khai và cấu hình hệ thống:
**Hệ thống BookStore sẽ được triển khai trên các nền tảng cloud như sau:**

**1. Backend:** Sử dụng Render để chạy server Node.js/Express.

**2. Database:** Sử dụng Neo4j Aura để lưu trữ cơ sở dữ liệu đồ thị.

**3. Frontend:** Sử dụng Vercel để triển khai ứng dụng React.

**Chi tiết triển khai**

**1.Backend trên Render:**

**Cách triển khai:**
- Tạo repository trên GitHub chứa mã nguồn backend.
- Kết nối Render với repository GitHub.
- Cấu hình môi trường:
   - NEO4J_URI: Đường dẫn kết nối Neo4j Aura.
   - NEO4J_USERNAME: Tên người dùng Neo4j.
   - NEO4J_PASSWORD: Mật khẩu Neo4j.
   - PORT: Cổng chạy server (Render tự động gán cổng).
- Render sẽ tự động build và chạy server mỗi khi có thay đổi mã nguồn.

**Lợi ích:** Render hỗ trợ triển khai dễ dàng, tự động cập nhật khi có thay đổi mã nguồn.

**2. Database trên Neo4j Aura:**

**Cách triển khai:**
- Tạo một instance Neo4j Aura miễn phí hoặc trả phí.
- Cấu hình thông tin kết nối (URI, username, password) trong file .env của backend.
- Sử dụng neo4j-driver để kết nối từ backend tới Neo4j Aura.
**Lợi ích:** Neo4j Aura cung cấp cơ sở dữ liệu đồ thị mạnh mẽ, bảo mật và dễ quản lý.

**3. Frontend trên Vercel:**

**Cách triển khai:**
- Tạo repository trên GitHub chứa mã nguồn frontend.
- Kết nối Vercel với repository GitHub.
- Cấu hình biến môi trường:
  - API_URL: Đường dẫn tới backend trên Render.
- Vercel sẽ tự động build và triển khai ứng dụng React mỗi khi có thay đổi mã nguồn.

**Lợi ích:** Vercel hỗ trợ triển khai nhanh chóng, tối ưu hóa hiệu năng cho ứng dụng React.
