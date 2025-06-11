# REST API

---
## 📌 1. GIỚI THIỆU TỔNG QUAN VỀ API

### 1.1 API là gì?

* API (Application Programming Interface) là tập hợp các định nghĩa và giao thức cho phép các phần mềm tương tác với nhau.
* API như một "cầu nối" giữa client (frontend) và server (backend).

### 1.2 API hoạt động như thế nào?

<img src="https://github.com/user-attachments/assets/5e1eb91e-2583-43f6-91be-b4f18e9b782f" width="650">

* Client gửi request đến một địa chỉ (endpoint).
* Server xử lý request và gửi response lại cho client.
* Dữ liệu thường được truyền qua định dạng JSON.

### 🧩 1.3 Thành phần của API:

<img src="https://github.com/user-attachments/assets/6224f4a8-f755-4cf8-98ba-26fc3b75c0a4" width="650">

* **Endpoint**: URL đại diện cho một tài nguyên cụ thể. (vd: `/api/users`).
* **Method**: phương thức HTTP (GET, POST...).
* **Headers**: chứa thông tin metadata (vd: Authorization).
* **Body**: nội dung gửi lên server (POST, PUT).
* **Status code**: mã phản hồi từ server (200, 404, 500...).

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

### 🚀 2.2 Ví dụ thực tế:

* `GET /api/products` → lấy danh sách sản phẩm.
* `POST /api/users` → đăng ký người dùng mới.
* `PUT /api/users/1` → cập nhật thông tin user ID=1.
* `DELETE /api/posts/5` → xoá bài viết có ID=5.

### 🚦 2.3 HTTP Status Code

* **200 OK** – Request thành công.
* **201 Created** – Tạo mới thành công.
* **400 Bad Request** – Request không hợp lệ.
* **401 Unauthorized** – Không xác thực.
* **403 Forbidden** – Không có quyền.
* **404 Not Found** – Không tìm thấy tài nguyên.
* **500 Internal Server Error** – Lỗi hệ thống.

---

## 🎯 3. Mô hình MVC trong API

### 3.1 Tổng quan

| Thành phần     | Vai trò                                                                 | Ghi chú                                                                  |
| -------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| **Model**      | Quản lý dữ liệu và logic liên quan đến dữ liệu (DB).                    | Thường dùng với ORM như Mongoose (MongoDB), Sequelize (MySQL/PostgreSQL) |
| **Controller** | Xử lý logic nghiệp vụ, điều phối dữ liệu từ Model và trả về cho client. | Nhận request → xử lý → gọi Model → trả response                          |
| **View**       | Trình bày dữ liệu cho người dùng.                                       | Với API REST thì **View = JSON response**, không cần render HTML         |

### 3.2 Tổ chức thư mục

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

### 3.3 Ưu điểm của MVC

* Dễ quản lý, phân chia trách nhiệm rõ ràng.
* Dễ mở rộng, tái sử dụng code.
* Phù hợp với team làm việc nhiều người.

---

## 🔰 4. GIỚI THIỆU NODE.JS, EXPRESS, TYPESCRIPT

### ⚙️ 4.1 Node.js

##### 4.1.1 Định nghĩa
* Là một nền tảng (runtime environment) dùng để chạy mã JavaScript phía server (thay vì chỉ trên trình duyệt như trước đây)..
* Được xây dựng trên V8 Engine (cũng là engine chạy JavaScript của Google Chrome).
* **Non-blocking I/O**: sử dụng event-loop để xử lý bất đồng bộ hiệu quả.
* **npm** (Node Package Manager): quản lý thư viện/phụ thuộc.
* Có thể dùng để xây dựng API, CLI, WebSocket, App IoT, v.v.

##### 4.1.2 Ưu điểm chính:

* Hiệu năng cao, xử lý bất đồng bộ tốt.
* Dễ học nếu đã biết JavaScript.
* Có hệ sinh thái phong phú (npm).

### 🌐 4.2 Express.js

<img src="https://github.com/user-attachments/assets/0cf8f2d7-5227-49c1-a687-3ed42cc8e434" width="650">

##### 4.2.1 Định nghĩa
* Express.js là một framework đơn giản được xây dựng trên nền tảng Node.js, ra đời với mục đích làm cho việc phát triển các ứng dụng web và API trở nên đơn giản, hiệu quả và dễ bảo trì hơn. 
* Cung cấp các tính năng như:
  * Routing (định tuyến request)
  * Middleware (xử lý request qua các lớp trung gian)
  * Xử lý JSON, form, cookies, file, v.v.

##### 4.2.2 Các tính năng chính của Express.js

##### ✅ Routing (Định tuyến request)
	
* Routing là cơ chế định nghĩa cách server phản hồi lại các request từ client với các URL và HTTP method cụ thể.
* Express giúp định nghĩa các tuyến (route) một cách rõ ràng và đơn giản.

	```ts
	// GET request tới /users
	app.get('/users', (req, res) => {
	  res.send('Danh sách người dùng');
	});
	
	// POST request tới /users
	app.post('/users', (req, res) => {
	  res.send('Tạo người dùng mới');
	});
	```

##### ✅ Middleware (Xử lý request qua các lớp trung gian)

* Middleware là các hàm trung gian, chạy trước khi request đến route handler cuối cùng.
* Có thể dùng middleware để:
	* Kiểm tra xác thực (authentication)
	* Ghi log
	* Xử lý lỗi
	* Parse dữ liệu request

	```ts
	// Middleware kiểm tra thời gian request
	app.use((req, res, next) => {
	  console.log('Thời gian:', Date.now());
	  next(); // chuyển tiếp tới middleware/route tiếp theo
	});
	```

### 4.3 TypeScript

<img src="https://github.com/user-attachments/assets/9359f928-55fc-47b8-9b88-12d8b51a3b90" width="650">

##### 4.3.1 Định nghĩa
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

### 📌 4.4 Kết hợp Node.js + Express + TypeScript

* Sự kết hợp này là lựa chọn phổ biến để xây dựng các RESTful API hiện đại:
* **Node.js**: chạy backend.
* **Express**: framework xây API.
* **TypeScript**: viết code nhanh chóng, dễ quản lý.


---
