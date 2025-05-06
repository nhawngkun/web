---
title: "Bài tập CC2 Ứng dụng phân tán"
date: "2025-05-06"
updated: "2025-05-06"
categories:
  - sveltekit
  - markdown
coverImage: "/images/CC-bai2/bg81.jpg"
coverWidth: 16
coverHeight: 9
excerpt: "Distributed System notes 2"
---
## Bài 1 
![CPU](/images/CC-bai2/CPU.png)
- **CPU**
+ CPU này có 6 nhân vật lý, hỗ trợ 12 luồng xử lý logic (do có SMT – Simultaneous Multithreading).

+ Hiện tại chỉ dùng 2% CPU – hệ thống đang rất nhàn rỗi.

+ RAM sử dụng 64% của 15.4 GB – tương đối nhiều, có thể do nhiều ứng dụng nền.

+ Có 2 GPU:

     NVIDIA GeForce GTX 1650

     AMD Radeon (tích hợp)

+ Giải thích lại ý bạn với cấu hình thực tế:
     CPU 6 nhân / 12 luồng cho phép xử lý 12 luồng đồng thời, nhưng nhờ vào cơ chế chuyển đổi ngữ cảnh (context switching), Windows có thể chạy hàng trăm tiến trình (246) và hàng ngàn luồng (3493).

     Virtual Processor và Virtual Memory là khái niệm quan trọng giúp hệ điều hành tối ưu tài nguyên vật lý.

     Virtual Processor giúp chia sẻ CPU giữa nhiều tiến trình.

     Virtual Memory (như paging và swap) giúp mở rộng RAM hiệu quả bằng ổ đĩa.
- **GPU**
![GPU](/images/CC-bai2/gpu.png)
![GPU1](/images/CC-bai2/gpu1.png)
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
![RAM](/images/CC-bai2/Ram.png)
+ Tổng dung lượng: 16.0 GB

+ Đang sử dụng: 9.4 GB (~62%)

+ RAM còn trống: Khoảng 6.0 GB

+ Loại RAM: DDR4 SODIMM

+ Tốc độ: 3200 MT/s

+ Khe cắm sử dụng: 2/2 (đã cắm đầy 2 khe)

+ Thông số mở rộng
    Memory in use (đã dùng): 9605 MB

    Compressed memory (bộ nhớ nén): 551 MB
→ Nén khoảng 1621 MB dữ liệu, giúp tiết kiệm được ~1070 MB RAM thực.

Committed memory (cam kết): 16.2 / 26.9 GB
→ Có nghĩa là hệ thống có thể sử dụng tới 26.9 GB RAM thông qua RAM ảo (page file).

Cached memory (bộ nhớ đệm): 3.5 GB

Paged pool / Non-paged pool: 783 MB / 461 MB

## Bài 2:
1. Web Server / HTTP Server
- Đa luồng: Mỗi request từ client được xử lý bởi một thread riêng hoặc thông qua thread-pool, giúp phục vụ nhiều kết nối đồng thời.

- Đa tiến trình: Một số server (như Apache prefork) dùng nhiều tiến trình con độc lập, mỗi tiến trình xử lý một nhóm kết nối để tăng độ ổn định và cách ly.
2. Database Server (MySQL, PostgreSQL)
- Đa luồng: Mỗi kết nối client được gán cho một thread để thực hiện truy vấn song song.

- Đa tiến trình: PostgreSQL khởi tạo một process riêng cho mỗi kết nối và có thêm các process nền như checkpoint, vacuum, WAL writer.
3. Hệ thống tính toán phân tán (MPI, HPC)
- Đa luồng: Bên trong mỗi tiến trình MPI, sử dụng OpenMP để chia nhỏ công việc cho nhiều thread chạy song song trên nhiều lõi CPU.

- Đa tiến trình: MPI phân phối tiến trình ra nhiều node trong cluster, mỗi tiến trình thực thi một phần nhiệm vụ.
4. Xử lý mã hóa/giải mã video (FFmpeg, HandBrake)
- Đa luồng: Video được chia thành các khung hình (frame), mỗi frame được encode hoặc decode song song bằng thread.

- Đa tiến trình: Khi xử lý nhiều video cùng lúc, mỗi video được giao cho một tiến trình riêng biệt.

5. Game Engine (Unity, Unreal)
- Đa luồng: Các phần xử lý như rendering, vật lý, AI, âm thanh được tách ra thành các thread riêng biệt để tăng hiệu suất.

- Đa tiến trình: Game client và server thường chạy ở hai tiến trình riêng biệt để độc lập xử lý và tăng bảo mật.
6. Ứng dụng giao diện người dùng (Electron, WPF, Qt)
- Đa luồng: Một thread chính xử lý UI, các tác vụ nặng (như I/O, xử lý ảnh) được đưa sang các thread phụ để tránh đơ giao diện.

- Đa tiến trình: Electron chia thành main process (điều khiển) và renderer process (hiển thị từng cửa sổ) để tăng độ ổn định.
7. CI/CD & Tự động build (Jenkins, GitLab CI, Make)
- Đa luồng: Thread dùng để quản lý pipeline, trigger các bước trong job.

- Đa tiến trình: Mỗi bước build (biên dịch, test) được thực thi trong process riêng, nhất là khi dùng make -j.
8. Web Crawler & Search Indexing
- Đa luồng: Nhiều thread đồng thời tải và phân tích HTML từ các URL khác nhau.

- Đa tiến trình: Crawler được phân chia theo domain hoặc theo phân vùng dữ liệu, mỗi tiến trình phụ trách một phần dữ liệu.
9. Message Broker (Kafka, RabbitMQ)
- Đa luồng: Mỗi consumer hoặc publisher có thể chạy trong thread riêng.

- Đa tiến trình: Mỗi node của cluster chạy như một tiến trình riêng biệt, giúp tăng tính chịu lỗi và khả năng mở rộng.
10. Big Data – Hadoop, Spark
- Đa luồng: Trong Spark, mỗi executor sử dụng nhiều thread để xử lý các phần dữ liệu (partition) cùng lúc.

- Đa tiến trình: Hadoop chạy các job MapReduce trong container hoặc JVM process riêng biệt.
11. Training Machine Learning / Deep Learning
- Đa luồng: DataLoader sử dụng nhiều thread để đọc và xử lý dữ liệu huấn luyện song song.

- Đa tiến trình: Mỗi GPU hoặc phần của mô hình thường chạy trong process riêng biệt để tránh tranh chấp bộ nhớ.
12. Xử lý dữ liệu cảm biến IoT / Thời gian thực
- Đa luồng: Mỗi thread đảm nhiệm một loại cảm biến, xử lý tín hiệu theo thời gian thực.

- Đa tiến trình: Mỗi thành phần như thu thập dữ liệu, phân tích, và hiển thị có thể là một process riêng biệt, dễ triển khai theo mô-đun.

## Bài 3:

![anh1](/images/CC-bai2/anh1.jpg)
![anh2](/images/CC-bai2/anh2.jpg)
![anh3](/images/CC-bai2/anh3.jpg)

## bài 4:
🧠 Phần 1: Hạ tầng phần cứng – CPU, GPU, RAM
ChatGPT được huấn luyện trên một hạ tầng siêu máy tính mạnh mẽ, bao gồm:

GPU: Sử dụng hàng nghìn GPU NVIDIA A100 80GB, mỗi node có 8 GPU, kết nối qua mạng tốc độ cao như InfiniBand hoặc Ethernet để đảm bảo băng thông lớn và độ trễ thấp trong quá trình huấn luyện. 
Reddit

CPU: Hệ thống sử dụng hàng trăm nghìn lõi CPU để hỗ trợ các tác vụ như xử lý dữ liệu, phân phối công việc và quản lý tài nguyên.

RAM: Mỗi node có bộ nhớ RAM lớn để lưu trữ mô hình và dữ liệu tạm thời, đảm bảo hiệu suất trong quá trình huấn luyện.

Hạ tầng mạng: Sử dụng các kết nối mạng tốc độ cao để đồng bộ hóa dữ liệu giữa các GPU và node, đảm bảo quá trình huấn luyện diễn ra mượt mà và hiệu quả. 
LinkedIn

⚙️ Phần 2: Quá trình huấn luyện phân tán
1. Phân chia mô hình và dữ liệu
Data Parallelism: Dữ liệu huấn luyện được chia nhỏ và phân phối cho các GPU khác nhau, mỗi GPU xử lý một phần dữ liệu và cập nhật trọng số mô hình sau mỗi vòng lặp.

Model Parallelism: Khi mô hình quá lớn để vừa vào bộ nhớ của một GPU, nó được chia thành các phần nhỏ và phân phối cho nhiều GPU để xử lý song song.

Pipeline Parallelism: Các lớp của mô hình được chia thành các giai đoạn và phân phối cho các GPU khác nhau, mỗi GPU xử lý một giai đoạn trong chuỗi huấn luyện.

2. Đồng bộ hóa và tối ưu hóa
Gradient Synchronization: Sau mỗi bước huấn luyện, các gradient được đồng bộ hóa giữa các GPU để đảm bảo mô hình được cập nhật nhất quán.

Sử dụng các thư viện hỗ trợ: Các thư viện như DeepSpeed và Horovod được sử dụng để tối ưu hóa quá trình huấn luyện phân tán, giảm thiểu chi phí và thời gian huấn luyện. 
Medium

📚 Phần 3: Tài liệu tham khảo từ Google

- **Giải thích chi tiết về cách ChatGPT hoạt động và được huấn luyện.**
https://assemblyai.com/blog/how-chatgpt-actually-works


- **Mô tả về công nghệ và quy trình huấn luyện ChatGPT, bao gồm các công cụ phân tán như Horovod và DeepSpeed.**
https://medium.com/@fahey_james/the-technology-and-process-behind-chatgpt-feb917533956


- **Thông tin chính thức từ OpenAI về cách phát triển và huấn luyện các mô hình nền tảng như ChatGPT.**
https://help.openai.com/en/articles/7842364-how-chatgpt-and-our-foundation-models-are-developed


- **Khảo sát về các phương pháp huấn luyện hiệu quả cho các mô hình ngôn ngữ lớn trên hạ tầng phân tán.**
https://arxiv.org/abs/2407.20018


- **Giới thiệu về DeepSpeed-Chat, một hệ thống huấn luyện RLHF hiệu quả cho các mô hình giống ChatGPT.**
https://arxiv.org/abs/2308.01320


