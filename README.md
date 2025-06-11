# API

---
## 📌 1. GIỚI THIỆU TỔNG QUAN VỀ API

### 1.1 API là gì?

* API (Application Programming Interface) là tập hợp các định nghĩa và giao thức cho phép các phần mềm tương tác với nhau.
* API như một "cầu nối" giữa client (frontend) và server (backend).

### 1.2 API hoạt động như thế nào?

<img src="https://github.com/user-attachments/assets/5e1eb91e-2583-43f6-91be-b4f18e9b782f" width="550">

* Client gửi request đến một địa chỉ (endpoint).
* Server xử lý request và gửi response lại cho client.
* Dữ liệu thường được truyền qua định dạng JSON.

### 1.3 Thành phần của API:

<img src="https://github.com/user-attachments/assets/6224f4a8-f755-4cf8-98ba-26fc3b75c0a4" width="550">


* **Endpoint**: URL cụ thể để gọi API (vd: `/api/users`).
* **Method**: phương thức HTTP (GET, POST...).
* **Headers**: chứa thông tin metadata (vd: Authorization).
* **Body**: nội dung gửi lên server (POST, PUT).
* **Status code**: mã phản hồi từ server (200, 404, 500...).

### 1.4 Các loại API:

* **REST API**: phổ biến, sử dụng HTTP, tuân thủ quy tắc REST.
* **SOAP API**: dùng XML, phức tạp hơn.
* **GraphQL**: linh hoạt, client tự định nghĩa dữ liệu cần lấy.

---

## ✅ 2. CÁC PHƯƠNG THỨC TRONG API (HTTP Methods)

| Phương thức | Mục đích           | Gửi dữ liệu ở đâu     | Idempotent (*) | Thay đổi dữ liệu |
|-------------|--------------------|------------------------|----------------|------------------|
| **GET**     | Lấy dữ liệu        | URL (query params)     | ✅ Có          | ❌ Không         |
| **POST**    | Tạo mới            | Body                   | ❌ Không       | ✅ Có            |
| **PUT**     | Cập nhật toàn bộ   | Body                   | ✅ Có          | ✅ Có            |
| **DELETE**  | Xóa dữ liệu        | Thường trong URL       | ✅ Có          | ✅ Có            |
> (*) *Idempotent*: Gọi nhiều lần với cùng dữ liệu sẽ không làm thay đổi kết quả trên server (ngoại trừ POST).

## 🔍 2.1 Chi Tiết Từng Phương Thức

### 🔹 GET
- **Mục đích**: Truy vấn/lấy dữ liệu.
- **Dữ liệu**: Truyền qua URL.
- **Đặc điểm**:
  - Không thay đổi dữ liệu.
  - Có thể cache.
  - Hiển thị rõ trên URL.

---

### 🔹 POST
- **Mục đích**: Gửi dữ liệu mới lên server (tạo mới bản ghi).
- **Dữ liệu**: Gửi trong body.
- **Đặc điểm**:
  - Có thể thay đổi dữ liệu.
  - Không idempotent: Gọi 2 lần có thể tạo 2 bản ghi khác nhau.
  - Không cache.

---

### 🔹 PUT
- **Mục đích**: Cập nhật toàn bộ tài nguyên (nếu không tồn tại có thể tạo mới tùy theo server).
- **Dữ liệu**: Gửi trong body.
- **Đặc điểm**:
  - Idempotent: Gọi nhiều lần với cùng dữ liệu vẫn cùng 1 kết quả.
  - Ghi đè toàn bộ thông tin.

---

### 🔹 DELETE
- **Mục đích**: Xóa tài nguyên.
- **Dữ liệu**: ID hoặc định danh nằm trong URL.
- **Đặc điểm**:
  - Idempotent: Gọi xóa 1 bản ghi đã bị xóa thì cũng không thay đổi gì thêm.

---

### 2.2 Ví dụ thực tế:

* `GET /api/products` → lấy danh sách sản phẩm.
* `POST /api/users` → đăng ký người dùng mới.
* `PUT /api/users/1` → cập nhật thông tin user ID=1.
* `DELETE /api/posts/5` → xoá bài viết có ID=5.

### 2.3 HTTP Status Code

* **200 OK** – Request thành công.
* **201 Created** – Tạo mới thành công.
* **400 Bad Request** – Request không hợp lệ.
* **401 Unauthorized** – Không xác thực.
* **403 Forbidden** – Không có quyền.
* **404 Not Found** – Không tìm thấy tài nguyên.
* **500 Internal Server Error** – Lỗi hệ thống.

---

## 3. MÔ HÌNH MVC TRONG NODE.JS

### 3.1 Khái niệm MVC

* **Model (M)**: quản lý dữ liệu và logic nghiệp vụ (liên kết DB).
* **View (V)**: hiển thị dữ liệu cho người dùng (ít dùng trong API).
* **Controller (C)**: nhận request, xử lý logic và trả response.

### 3.2 Minh họa dòng chảy

```
Client -> Route -> Controller -> Service (logic) -> Model -> DB
                             ↓
                        Response trả về Client
```

### 3.3 Ưu điểm của MVC

* Dễ quản lý, phân chia trách nhiệm rõ ràng.
* Dễ mở rộng, tái sử dụng code.
* Phù hợp với team làm việc nhiều người.

### 3.4 Tổ chức thư mục

```
src/
|-- controllers/     → xử lý logic cho request
|-- routes/          → định nghĩa các endpoint
|-- models/          → định nghĩa schema & kết nối DB
|-- services/        → chứa nghiệp vụ riêng biệt
|-- middlewares/     → xử lý trước/giữa các request
|-- utils/           → hàm tiện ích
|-- app.ts           → điểm bắt đầu ứng dụng
```

---

## 4. GIỚI THIỆU NODE.JS, EXPRESS, TYPESCRIPT (CHI TIẾT)

### 4.1 Node.js

* Nền tảng chạy JavaScript phía server (dựa trên Chrome V8 Engine).
* **Non-blocking I/O**: sử dụng event-loop để xử lý bất đồng bộ hiệu quả.
* **npm** (Node Package Manager): quản lý thư viện/phụ thuộc.
* Có thể dùng để xây dựng API, CLI, WebSocket, App IoT, v.v.

### 4.2 Express.js

* Framework nhẹ giúp tạo API với Node.js dễ dàng.
* Hỗ trợ routing, middleware, xử lý lỗi...
* Cú pháp đơn giản:

```ts
import express from 'express';
const app = express();

app.use(express.json());

app.get('/api/users', (req, res) => {
  res.status(200).json({ message: 'Lấy danh sách user' });
});

app.listen(3000, () => console.log('Server đang chạy'));
```

* Có thể tích hợp:

  * `cors`, `morgan`, `helmet`, `express-validator`...

### 4.3 TypeScript

* Ngôn ngữ mở rộng của JavaScript với hệ thống **kiểu tĩnh**.
* Giúp:

  * Tăng khả năng tự động hoàn thành code.
  * Phát hiện lỗi compile-time.
  * Tài liệu code rõ ràng hơn.

```ts
interface User {
  id: number;
  name: string;
}

const greetUser = (user: User): string => `Xin chào, ${user.name}`;
```

* File cấu hình: `tsconfig.json` – nơi bật các tùy chọn như `strict`, `target`, `paths`...

### 4.4 Các công cụ và kỹ thuật hỗ trợ:

* **ts-node-dev**, **nodemon**: tự động reload khi thay đổi code.
* **dotenv**: quản lý biến môi trường (`process.env`).
* **ESLint**, **Prettier**: giúp code sạch, chuẩn hóa format.
* **Jest**, **Supertest**: viết unit test, test API.

### 4.5 Ví dụ cấu trúc route đơn giản với controller

```ts
// routes/user.route.ts
import express from 'express';
import { getAllUsers } from '../controllers/user.controller';
const router = express.Router();

router.get('/', getAllUsers);
export default router;

// controllers/user.controller.ts
export const getAllUsers = (req, res) => {
  res.status(200).json([{ id: 1, name: 'An' }]);
};
```

---
