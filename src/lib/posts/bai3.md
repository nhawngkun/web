---
title: "Bài tập CC2 Ứng dụng phân tán bai3"
date: "2025-05-07"
updated: "2025-05-07"
categories:
  - sveltekit
  - markdown
coverImage: "/images/CC-bai3/thongdiep.png"
coverWidth: 16
coverHeight: 9
excerpt: "Truyền thông"
---


## 📘 Bài Tập 1: Tìm Hiểu RabbitMQ – Cơ chế, Chức năng và Cài đặt
### 1. Giới thiệu chung về RabbitMQ
RabbitMQ là một message broker mã nguồn mở, dùng để truyền thông điệp giữa các thành phần trong hệ thống phân tán thông qua mô hình hàng đợi. Nó hỗ trợ nhiều giao thức khác nhau, nổi bật nhất là AMQP (Advanced Message Queuing Protocol).
### 2. Kiến trúc và cơ chế hoạt động
🧩 Thành phần chính:
Producer: Tạo và gửi thông điệp.

Exchange: Nhận thông điệp từ Producer và quyết định chuyển đi đâu.

Queue: Hàng đợi lưu trữ thông điệp.

Consumer: Nhận và xử lý thông điệp từ hàng đợi.

🔁 Luồng hoạt động:
Producer gửi thông điệp đến Exchange.

Exchange dùng binding key để phân phối thông điệp đến Queue tương ứng.

Consumer lấy thông điệp từ Queue và xử lý.

🔄 Các loại Exchange:
Direct: Gửi thông điệp đến Queue theo routing key cụ thể.

Fanout: Gửi bản sao thông điệp đến tất cả Queue đã bind.

Topic: Dựa trên pattern routing key (ví dụ: logs.*).

Headers: Dựa trên header của message, không dùng routing key.

### 3. Chức năng chính của RabbitMQ
Giao tiếp phi đồng bộ giữa các hệ thống.

Đảm bảo độ bền và độ tin cậy của thông điệp (durability, acknowledgment, persistence).

Tăng khả năng mở rộng và phân tán.

Giao diện quản lý Web (Management UI).

Hỗ trợ plugin (authentication, federation, metrics,...).

### 4. Hướng dẫn cài đặt
✅ Cách 1: Dùng Docker
```
docker run -d --hostname rabbitmq-host --name rabbitmq \
  -p 5672:5672 -p 15672:15672 \
  rabbitmq:3-management
```
Truy cập Management UI: http://localhost:15672
(Tài khoản mặc định: guest / guest)

✅ Cách 2: Cài trực tiếp (Ubuntu)
```
sudo apt update
sudo apt install rabbitmq-server
sudo systemctl enable rabbitmq-server
sudo systemctl start rabbitmq-server
```

## 🧪 Bài Tập 2: Code Một Hệ Thống Sử Dụng RabbitMQ
**✅ Mục tiêu**
Xây dựng hệ thống đơn giản với một Producer gửi thông điệp và một Consumer nhận thông điệp bằng RabbitMQ.

**📦 Môi trường:**
Python

Thư viện: pika (client RabbitMQ cho Python)
```
pip install pika
```
**🖥️ Producer – Gửi thông điệp (sender.py)**
``` 
python
import pika

# Kết nối đến RabbitMQ
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Tạo queue nếu chưa có
channel.queue_declare(queue='hello')

# Gửi thông điệp
channel.basic_publish(exchange='',
                      routing_key='hello',
                      body='Hello RabbitMQ!')
print(" [x] Sent 'Hello RabbitMQ!'")

connection.close()
```
**📥 Consumer – Nhận thông điệp (receiver.py)**
```
python
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.queue_declare(queue='hello')

# Xử lý khi nhận được message
def callback(ch, method, properties, body):
    print(f" [x] Received {body}")

channel.basic_consume(queue='hello',
                      on_message_callback=callback,
                      auto_ack=True)

print(' [*] Waiting for messages...')
channel.start_consuming()
```
## 🔁 Bài Tập 3: Tìm Hiểu RPC với JSON - Demo Thư Viện JSON-RPC
**1. RPC là gì?**
RPC (Remote Procedure Call) là cơ chế cho phép một chương trình gọi hàm của chương trình khác trên một máy khác như thể là gọi hàm cục bộ.

Trong bài trước bạn đã dùng xmlrpc, bây giờ sẽ tìm hiểu một cách khác: JSON-RPC.
**2. JSON-RPC là gì?**
Là một chuẩn RPC sử dụng định dạng JSON.

Giao tiếp đơn giản qua HTTP hoặc TCP.

Không phụ thuộc vào ngôn ngữ lập trình.

**3. Các thư viện JSON-RPC phổ biến**

| Ngôn ngữ   | Thư viện                         |
| ---------- | -------------------------------- |
| Python     | `jsonrpcserver`, `jsonrpcclient` |
| JavaScript | `jayson`, `json-rpc-2.0`         |
| Go         | `golang-jsonrpc`                 |
| Java       | `jsonrpc4j`                      |


**4. Demo Python – jsonrpcserver và jsonrpcclient**
✅ Cài đặt:
```
pip install jsonrpcserver jsonrpcclient flask
```
🌐 Server – server.py
```
python
from flask import Flask, request
from jsonrpcserver import method, dispatch

app = Flask(__name__)

@method
def add(x, y):
    return x + y

@app.route("/", methods=["POST"])
def handle():
    return dispatch(request.get_data().decode())

if __name__ == "__main__":
    app.run(port=5000)
```
📡 Client – client.py
```
python
from jsonrpcclient import request

response = request("http://localhost:5000", "add", x=7, y=5)
print(response.data.result)  # Output: 12
```
**5. Ưu điểm của JSON-RPC so với XML-RPC: **

Nhẹ hơn, dễ đọc hơn.

Tương thích với REST API hiện đại.

Hỗ trợ tốt với các frontend JavaScript.