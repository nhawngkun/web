---
title: "Hệ thống phân tán"
date: "2025-04-29"
updated: "2025-04-29"
categories:
  - sveltekit
  - markdown
coverImage: "/images/anh1.png"
coverWidth: 16
coverHeight: 9
excerpt: "Tổng quan về hệ thống phân tán: định nghĩa, ứng dụng, khái niệm cốt lõi và kiến trúc."
---
## Bài 1 
- **CPU**: CPU này có 6 nhân vật lý, hỗ trợ 12 luồng xử lý logic (do có SMT – Simultaneous Multithreading).

Hiện tại chỉ dùng 2% CPU – hệ thống đang rất nhàn rỗi.

RAM sử dụng 64% của 15.4 GB – tương đối nhiều, có thể do nhiều ứng dụng nền.

Có 2 GPU:

NVIDIA GeForce GTX 1650

AMD Radeon (tích hợp)

Giải thích lại ý bạn với cấu hình thực tế:
CPU 6 nhân / 12 luồng cho phép xử lý 12 luồng đồng thời, nhưng nhờ vào cơ chế chuyển đổi ngữ cảnh (context switching), Windows có thể chạy hàng trăm tiến trình (246) và hàng ngàn luồng (3493).

Virtual Processor và Virtual Memory là khái niệm quan trọng giúp hệ điều hành tối ưu tài nguyên vật lý.

Virtual Processor giúp chia sẻ CPU giữa nhiều tiến trình.

Virtual Memory (như paging và swap) giúp mở rộng RAM hiệu quả bằng ổ đĩa.
- **GPU**
+ Tên CPU: AMD Ryzen 5 4600H with Radeon Graphics

+ Số nhân vật lý (Cores): 6

+ Số luồng xử lý (Logical Processors): 12

+ Số tiến trình đang chạy: 246

+ Số luồng đang hoạt động: 3493

+ Tốc độ hoạt động hiện tại: ~2.88–3.34 GHz

+ Virtualization: Enabled

+ Cache:

   L1: 384 KB

   L2: 3.0 MB

   L3: 8.0 MB

⇒ Có thể xử lý tối đa 12 luồng cùng lúc, nhưng thực tế có thể xử lý hàng nghìn threads nhờ context switching (chuyển ngữ cảnh).
⇒ Chuyển ngữ cảnh (context switching) cho phép CPU tạm dừng xử lý một tiến trình/luồng và chuyển sang cái khác, mất vài micro giây (μs). Điều này giúp hệ điều hành xử lý song song tốt hơn dù tài nguyên giới hạn.
- **RAM** :
Tổng dung lượng: 16.0 GB

Đang sử dụng: 9.4 GB (~62%)

RAM còn trống: Khoảng 6.0 GB

Loại RAM: DDR4 SODIMM

Tốc độ: 3200 MT/s

Khe cắm sử dụng: 2/2 (đã cắm đầy 2 khe)

Thông số mở rộng
Memory in use (đã dùng): 9605 MB

Compressed memory (bộ nhớ nén): 551 MB
→ Nén khoảng 1621 MB dữ liệu, giúp tiết kiệm được ~1070 MB RAM thực.

Committed memory (cam kết): 16.2 / 26.9 GB
→ Có nghĩa là hệ thống có thể sử dụng tới 26.9 GB RAM thông qua RAM ảo (page file).

Cached memory (bộ nhớ đệm): 3.5 GB

Paged pool / Non-paged pool: 783 MB / 461 MB

## Bài 2:
1.Web Server / HTTP Server

Multithreading: mỗi kết nối HTTP thường được xử lý bởi một thread riêng (hoặc từ thread‑pool) để trả lời đồng thời nhiều client.

Multiprocessing: với mô hình “worker process” (ví dụ Apache prefork), mỗi tiến trình độc lập đảm nhận một tập kết nối, tăng độ cách ly và độ ổn định.

2.Database Server (MySQL, PostgreSQL)

Multithreading: mỗi client connection gán cho một thread, hoặc dùng thread‑pool để xử lý truy vấn.

Multiprocessing: một số engine (ví dụ PostgreSQL) khởi nhiều tiến trình con (background worker) cho checkpoint, vacuum, replication.

3.Hệ thống tính toán phân tán (MPI / HPC)

Multiprocessing: chạy song song nhiều process trên các node khác nhau (thường mpirun -np N).

Multithreading: bên trong mỗi process MPI có thể dùng OpenMP để tận dụng nhiều core.

4.Xử lý mã hóa/giải mã video (FFmpeg, HandBrake)

Multithreading: chia video thành các khung (frame) hoặc nhóm khung (GOP) và encode/decode song song.

Multiprocessing: khởi nhiều tiến trình FFmpeg để encode nhiều file đồng thời.

5.Game Engine (Unreal, Unity)

Multithreading: tách riêng thread cho rendering, physics, AI, audio, network…

Multiprocessing: ở cấp cao có thể khởi process riêng cho server game (dedicated server) và client.

6.Ứng dụng GUI (Electron, WPF, Qt)

Multithreading: UI thread riêng để vẽ giao diện; background worker thread xử lý I/O, tính toán nặng để không block UI.

Multiprocessing: đôi khi tách renderer process (Chromium) và main process (Electron).

7.Hệ thống CI/CD và Build Automation (Jenkins, GitLab CI, make -j)

Multithreading: plugin, worker thread trong runner dùng để kiểm soát các bước.

Multiprocessing: dùng tham số -j N (GNU Make) tạo N tiến trình song song để biên dịch file.

8.Web Crawler & Search Indexing

Multithreading: mỗi crawler thread fetch và parse một URL;

Multiprocessing: chia domain hoặc shard dữ liệu, mỗi process quản lý bộ queue riêng, tăng throughput.

9.Message Broker (RabbitMQ, Kafka)

Multithreading: mỗi connection / consumer xử lý trong thread riêng.

Multiprocessing: broker có thể chạy nhiều node (process) để clustering, high‑availability.

10.Big Data – MapReduce (Hadoop, Spark)

Multiprocessing: mỗi task map/reduce chạy như một container hoặc process độc lập.

Multithreading: trong Spark executor, mở nhiều thread để xử lý các partition song song.

11.Training Machine Learning / Deep Learning

Multiprocessing: dùng torch.multiprocessing hoặc tf.distribute spawn nhiều process để training trên nhiều GPU.

Multithreading: data loader (PyTorch DataLoader) sử dụng nhiều worker thread để preload batch dữ liệu.

12.IoT & Real‑time Sensor Processing

Multithreading: thread riêng đọc dữ liệu từ từng sensor, một thread xử lý filter/aggregation.

Multiprocessing: container hoá từng module (như edge‑gateway, analytics) dưới dạng process độc lập để dễ scale.

## Bài 3:


