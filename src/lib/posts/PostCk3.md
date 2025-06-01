---
title: "Bài tập giữa kì số 3"
date: "2025-05-1010"
updated: "2025-05-1010"
categories:
  - sveltekit
  - markdown
coverImage: "/images/GK1/ANH1.jpg"
coverWidth: 16
coverHeight: 9
excerpt: "Deliverable 3 - Website"
---
### 1 Tóm tắt tiến độ dự án:
**1. Các tính năng chính đã triển khai**
**- Đồng bộ dữ liệu với Neo4j**

Đã xây dựng script syncBooksNeo4jFull.js với chức năng:

Xóa toàn bộ dữ liệu cũ: Xóa sạch các node Book, Author, Category trong Neo4j trước khi thêm mới để tránh dữ liệu lỗi, trùng lặp.

Nhập lại dữ liệu mới từ file mẫu (sampleBooks.js), đảm bảo dữ liệu luôn sạch, đồng bộ, và đúng cấu trúc mỗi lần chạy.

**- Tạo node và quan hệ liên kết**

**Node Book:**

- Các thuộc tính đầy đủ: id, name, author, lang, category, image, title, link, content, description.

**Node Author và Category:**

- Sử dụng lệnh MERGE thay vì CREATE để tránh tạo trùng lặp (mỗi tác giả/thể loại chỉ có 1 node duy nhất).

**Quan hệ:**

WRITTEN_BY: Liên kết giữa Book và Author.

BELONGS_TO: Liên kết giữa Book và Category.

**- Quản lý cấu hình bảo mật**

Sử dụng thư viện dotenv để quản lý các thông tin nhạy cảm (như user, password, uri Neo4j) qua file .env 
để :
Bảo mật thông tin, tránh để lộ dữ liệu nhạy cảm khi chia sẻ code.
Dễ dàng thay đổi cấu hình kết nối mà không cần sửa trực tiếp mã nguồn.
Tăng tính linh hoạt cho việc triển khai trên các môi trường khác nhau (dev, test, production).


**2. Những phần đã hoàn thành**
**- Script đồng bộ dữ liệu mẫu vào Neo4j, bao gồm:**

-Hoàn thành script đồng bộ dữ liệu mẫu syncBooksNeo4jFull.js gồm cả xóa dữ liệu cũ, tạo node, tạo quan hệ.

- Đảm bảo dữ liệu sạch, không bị lỗi logic.

- Cấu trúc backend rõ ràng:

Tách riêng Controllers, Models, Middleware, Routers → giúp dự án dễ mở rộng, dễ bảo trì.

Đã chuẩn bị sẵn bộ dữ liệu mẫu sampleBooks.js để dùng cho việc import seed dữ liệu.

**3. Những phần đang phát triển**

**-API backend**

**1.Các chức năng đã là xong nhưng vẫn muốn phát triển để tối ưu hơn:**

- Lấy danh sách sách,Tìm kiếm sách,Thêm/xóa/sửa sách, tác giả, thể loại và hiển thị những sách mới nhất hay đọc nhất.

**2.Tính năng nâng cao**

- Phân mảnh dữ liệu, sao lưu, đồng bộ đa nút database Neo4j.

- iao tiếp giữa các node trong hệ thống phân tán (phục vụ scaling lớn).

**4. Các vấn đề từng gặp và cách giải quyết**

**Trùng lặp node hoặc quan hệ**

- Ban đầu dùng CREATE, dẫn đến có nhiều node Author hoặc Category giống nhau.

- Đã sửa lại dùng MERGE → đảm bảo chỉ tạo mới nếu chưa tồn tại.

**Quản lý biến môi trường**

- Lỗi khi không load được file .env.

- Đã kiểm tra lại đường dẫn và dùng dotenv để đảm bảo load chính xác các biến cấu hình.

**Xóa dữ liệu cũ**

- Nếu không xóa trước, import mới sẽ khiến dữ liệu chồng lặp, sai lệch.

- Đã bổ sung bước xóa toàn bộ node trước khi nhập mới.

**Kết nối database thất bại**

- Lỗi xảy ra do sai thông tin cấu hình hoặc Neo4j chưa khởi động.

- Đã thêm kiểm tra và log lỗi chi tiết để phát hiện và xử lý nhanh.

### 2. Demo hoạt động của hệ thống (hoặc video minh họa):

### 3.Mã nguồn (Code Snippets):
**- syncBooksNeo4jFull.js**
```javascript
// Khởi tạo kết nối tới Neo4j và đồng bộ dữ liệu sách, tác giả, thể loại
import neo4j from 'neo4j-driver';
import dotenv from 'dotenv';
import sampleBooks from './sampleBooks.js';

dotenv.config({ path: './Config/config.env' });

const driver = neo4j.driver(
  process.env.NEO4J_URI,
  neo4j.auth.basic(process.env.NEO4J_USERNAME, process.env.NEO4J_PASSWORD)
);

const session = driver.session();

async function syncBooksWithAuthors() {
  try {
    // Xóa toàn bộ dữ liệu cũ để đảm bảo dữ liệu sạch
    await session.run('MATCH (b:Book) DETACH DELETE b');
    await session.run('MATCH (a:Author) DETACH DELETE a');
    await session.run('MATCH (c:Category) DETACH DELETE c');
    console.log('Deleted all existing books and authors');

    for (const book of sampleBooks) {
      // Tạo node Book với đầy đủ thuộc tính
      await session.run(
        `CREATE (b:Book {
          id: $id,
          name: $name,
          author: $author,
          lang: $lang,
          category: $category,
          image: $image,
          title: $title,
          link: $link,
          content: $content,
          description: $description
        })`,
        book
      );

      // Tạo hoặc tìm node Author, tránh trùng lặp
      await session.run(
        `MERGE (a:Author {name: $author})`,
        { author: book.author }
      );

      // Tạo hoặc tìm node Category, tránh trùng lặp
      await session.run(
        `MERGE (c:Category {name: $category})`,
        { category: book.category }
      );

      // Tạo quan hệ WRITTEN_BY giữa Book và Author
      await session.run(
        `MATCH (b:Book {id: $id}), (a:Author {name: $author})
         MERGE (b)-[:WRITTEN_BY]->(a)`,
        { id: book.id, author: book.author }
      );

      // Tạo quan hệ BELONGS_TO giữa Book và Category
      await session.run(
        `MATCH (b:Book {id: $id}), (c:Category {name: $category})
         MERGE (b)-[:BELONGS_TO]->(c)`,
        { id: book.id, category: book.category }
      );

      console.log(`Created book '${book.name}' and linked to author '${book.author}'`);
    }

    console.log('Finished syncing books with authors');
  } catch (error) {
    console.error('Error syncing books with authors:', error);
  } finally {
    await session.close();
    await driver.close();
  }
}

// Gọi hàm đồng bộ dữ liệu
syncBooksWithAuthors();
```
**- dbconnection.js**
```javascript
// Kết nối tới Neo4j sử dụng biến môi trường từ file cấu hình
import neo4j from 'neo4j-driver';
import { configDotenv } from 'dotenv';

// Nạp biến môi trường từ file config.env
configDotenv({ path: './Config/config.env' });

console.log("Loaded URI:", process.env.NEO4J_URI); // Kiểm tra đã load đúng URI

// Khởi tạo driver kết nối tới Neo4j
const driver = neo4j.driver(
    process.env.NEO4J_URI,
    neo4j.auth.basic(process.env.NEO4J_USERNAME, process.env.NEO4J_PASSWORD)
);

export default driver;
```
**- config.env**

```env
# Thông tin cấu hình kết nối tới Neo4j (bảo mật, dễ thay đổi)
NEO4J_URI=neo4j+s://58270351.databases.neo4j.io
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=UlXPYheImRAqEPXhXehOLc89qRc9AKM6us2x2VJHgkY

```
**Books.jsx**

```javascript
/* eslint-disable react/no-unescaped-entities */

import Card from './Card'; // Component hiển thị thông tin từng cuốn sách
import { useEffect, useState } from 'react';
import axios from 'axios'; // Thư viện gọi API
import AddBook from './AddBook'; // Component thêm sách (chỉ hiện với admin)
import EditBook from './EditBook'; // Component sửa sách (chỉ hiện với admin)
import DeleteBook from './DeleteBook'; // Component xóa sách (chỉ hiện với admin)
import API_URL from '../config'; // Đường dẫn gốc của API backend

const Books = () => {
    // State lưu danh sách sách lấy từ backend
    const [books, setBooks] = useState([])
    // State xác định người dùng hiện tại có phải admin không
    const [admin, setAdmin] = useState(false)

    // useEffect này sẽ chạy 1 lần khi component mount để lấy danh sách sách từ backend
    useEffect(() => {
        const fetchData = async () => {
            try {
                // Gọi API lấy danh sách sách
                const res = await axios.get(`${API_URL}/books`)
                setBooks(res.data) // Lưu dữ liệu vào state
            } catch (error) {
                console.log(error)
            }
        }
        fetchData()
    }, [])

    // useEffect này kiểm tra quyền admin của người dùng hiện tại
    useEffect(() => {
        handleAdmin()
    }, [])

    // Hàm kiểm tra quyền admin dựa trên thông tin lưu trong localStorage
    const handleAdmin = async () => {
        try {
            const userInfo = localStorage.getItem("User");
            if (!userInfo) return; // Nếu chưa đăng nhập thì không làm gì
            
            const userId = JSON.parse(userInfo).id; // Lấy id người dùng từ localStorage
            // Gọi API lấy thông tin profile của user
            const res = await axios.get(
                `${API_URL}/user/profile/${userId}`
            );
            console.log("User role:", res.data.role);
            
            // Nếu role là admin thì setAdmin(true) để hiển thị các chức năng quản trị
            if (res.data.role && (res.data.role.toLowerCase() === "admin")) {
                setAdmin(true);
            }
        } catch (error) {
            console.error("Error checking admin role:", error);
        }
    }

    return (
        <>
            <div className="max-w-screen-2xl container mx-auto md:px-20 px-7">
                {/* Phần giới thiệu và chức năng quản trị */}
                <div className="pt-20 items-center - justify-center text-center">
                    <h1 className="text-2xl md:text-4xl font-bold">
                        We're delighted to have you{" "}
                        <span className="text-pink-500">here :) </span>
                    </h1>
                    <p className='mt-12'>
                        {/* Đoạn mô tả về BookStore */}
                        Your one-stop destination for all things books! Here, you'll find a vast collection of titles across a wide range of genres, from timeless classics to the latest bestsellers, all carefully curated to cater to every type of reader. Whether you're in search of a thrilling mystery, a heartwarming romance, a thought-provoking nonfiction piece, or a captivating fantasy adventure, we've got something just for you. Our mission at bookStore is to make reading accessible and enjoyable, offering high-quality books that inspire, entertain, and educate. Each book listing comes with detailed descriptions, reader reviews, and recommendations to help you make the perfect choice. We also provide various formats—hardcover, paperback, and digital—to suit your reading preference. Explore our collection, discover new favorites, and let bookStore be your guide on a literary journey. Happy reading!
                    </p>
                    {/* Nếu là admin thì hiển thị các chức năng quản trị */}
                    {admin &&
                        (<div>
                            <AddBook />    {/* Nút/thành phần thêm sách */}
                            <EditBook />   {/* Nút/thành phần sửa sách */}
                            <DeleteBook /> {/* Nút/thành phần xóa sách */}
                        </div>)
                    }
                </div>
                {/* Hiển thị danh sách sách dưới dạng lưới */}
                <div className='mt-12 grid grid-cols-1 md:grid-cols-2  lg:grid-cols-4 gap-4'>
                    {books.map((item) => (
                        // Hiển thị từng cuốn sách bằng component Card
                        <Card key={item.id} item={item} /> // Changed from item._id to item.id
                    ))}
                </div>
            </div>
        </>
    );
};

export default Books;
```

### 4. Danh sách tính năng đã hoàn thành:

**1. Đồng bộ dữ liệu sách, tác giả, thể loại với Neo4j**
- Mô tả: Đã xây dựng script backend để xóa toàn bộ dữ liệu cũ và nhập lại dữ liệu mẫu từ file vào Neo4j, đồng thời tạo các quan hệ giữa Book, Author, Category.
- Ví dụ hoạt động: Khi chạy script syncBooksNeo4jFull.js, toàn bộ sách, tác giả, thể loại được làm mới và liên kết đúng quan hệ trong database đồ thị.

**2. API lấy danh sách sách**
- Mô tả: Đã xây dựng API /books ở backend để trả về danh sách sách từ Neo4j cho frontend.
Ví dụ hoạt động: Khi frontend gọi GET /books, API trả về mảng các sách, mỗi sách có đầy đủ thông tin (id, tên, tác giả, thể loại, mô tả...).

**3. Kết nối frontend-backend**
- Mô tả: Frontend sử dụng axios để gọi API backend, lấy dữ liệu sách và hiển thị bằng component Card.
- Ví dụ hoạt động: Khi người dùng truy cập trang sách, component Books.jsx tự động gọi API, nhận dữ liệu và render danh sách sách ra giao diện.

**4. Kiểm tra quyền admin và hiển thị chức năng quản trị**
- Mô tả: Khi người dùng đăng nhập, frontend kiểm tra quyền admin qua API /user/profile/:id và hiển thị các chức năng thêm, sửa, xóa sách nếu là admin.
- Ví dụ hoạt động: Nếu tài khoản có quyền admin, các component AddBook, EditBook, DeleteBook sẽ xuất hiện trên giao diện để quản trị viên thao tác.

**5. Xử lý lỗi cơ bản**
- Mô tả: Các thao tác gọi API đều có try-catch để log lỗi ra console, giúp phát hiện và xử lý lỗi khi kết nối hoặc truy vấn dữ liệu.
- Ví dụ hoạt động: Nếu API trả về lỗi hoặc không kết nối được, thông báo lỗi sẽ được ghi ra console để lập trình viên kiểm tra.

### 5.Kế hoạch tiếp theo :
**1. Các bước tiếp theo sẽ thực hiện**
**Xây dựng và hoàn thiện các API backend:**
- Hoàn thiện API xác thực và phân quyền người dùng (login, register, phân quyền admin/user).
**Tối ưu hóa giao diện frontend:**
- Xây dựng các trang chi tiết sách, trang quản trị, trang đăng nhập/đăng ký.
- Cải thiện trải nghiệm người dùng, bổ sung thông báo khi thao tác thành công/thất bại.
**Kết nối và kiểm thử toàn bộ luồng dữ liệu:**
- Đảm bảo frontend và backend giao tiếp ổn định, dữ liệu đồng bộ và hiển thị đúng.
**2. Các tính năng dự định hoàn thành trong giai đoạn tiếp theo**
- Chức năng tìm kiếm và lọc sách theo nhiều tiêu chí.
- Chức năng xác thực, phân quyền người dùng (admin/user).
- Xử lý lỗi nâng cao và thông báo cho người dùng khi thao tác thất bại/thành công.
- Bổ sung unit test cho backend để đảm bảo chất lượng code.

**3. Những vấn đề cần giải quyết để hoàn thiện dự án**

**Đảm bảo bảo mật API:**
- Xác thực JWT, kiểm soát truy cập các API nhạy cảm.

**Xử lý đồng bộ dữ liệu giữa frontend và backend:**
- Đảm bảo dữ liệu cập nhật realtime khi thêm/sửa/xóa.

**Tối ưu hóa hiệu năng truy vấn Neo4j:**
- Sử dụng chỉ mục, tối ưu câu lệnh Cypher.

**Kiểm thử toàn bộ hệ thống:**
- Phát hiện và sửa các lỗi phát sinh khi tích hợp nhiều thành phần.
- Triển khai và kiểm thử trên môi trường thực tế:
- Đảm bảo hệ thống hoạt động ổn định khi triển khai thật.