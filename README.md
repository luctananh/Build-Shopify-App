# Quy trình Xây dựng Ứng dụng Shopify từ đầu

Tài liệu này mô tả chi tiết các bước để xây dựng một ứng dụng Shopify hoàn chỉnh từ một template trống, bao gồm frontend (React), backend (Express), và theme extension.

---

### **Bước 1: Khởi tạo ứng dụng Shopify trống**

Bắt đầu bằng cách tạo một cấu trúc ứng dụng Shopify cơ bản không có template sẵn.

```bash
shopify app init --template none
```

Sau khi khởi tạo xong, cài đặt các gói phụ thuộc ở thư mục gốc:

```bash
npm install
```

---

### **Bước 2: Tạo Theme Extension**

Thêm một module theme extension vào ứng dụng của bạn.

```bash
shopify generate extension theme
```

---

### **Bước 3: Thiết lập Frontend với React + Vite**

1.  Tạo thư mục `frontend`.
2.  Bên trong thư mục `frontend`, chạy lệnh sau để khởi tạo một dự án React bằng Vite:

    ```bash
    npm create vite@latest . -- --template react
    ```

---

### **Bước 4: Thiết lập Backend với Node.js + Express**

1.  Tạo thư mục `backend`.
2.  Bên trong thư mục `backend`, khởi tạo một dự án Node.js và cài đặt Express:

    ```bash
    npm init -y
    npm install express --save
    ```

3.  Tạo một tệp tin có tên `server.js` bên trong thư mục `backend`.

---

### **Bước 5: Cấu hình cho Frontend (shopify.web.toml)**

Trong **thư mục gốc** của dự án, tạo một tệp tin `shopify.web.toml` để Shopify CLI biết cách quản lý frontend của bạn.

```toml
# shopify.web.toml (trong thư mục gốc)

roles = ["frontend"]

[directories]
app = "frontend"

auth_callback_path = ["/custom/path1", "/custom/path2"]

webhooks_path = "/api/webhooks"

[commands]
dev = "npm run dev"
build = "npm run build"
```

---

### **Bước 6: Cấu hình cho Backend (shopify.web.toml)**

Trong **thư mục `backend`**, tạo một tệp tin `shopify.web.toml` khác để định nghĩa vai trò backend.

```toml
# backend/shopify.web.toml

roles = ["backend"]

[commands]
dev = "npm run dev"
```

---

### **Bước 7: Cập nhật cấu hình Vite**

Trong thư mục `frontend`, mở tệp `vite.config.js` và thêm vào khối `server`. Điều này giúp Vite tương thích với cách Shopify CLI quản lý các cổng (port).

```javascript
// frontend/vite.config.js

import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  server: {
    port: parseInt(process.env.PORT || "3000", 10),
  },
});
```

---

### **Bước 8: Chạy ứng dụng**

Cuối cùng, từ **thư mục gốc** của dự án, khởi động tất cả các dịch vụ ở chế độ phát triển.

```bash
shopify app dev --use-localhost
```

**Lưu ý:** Cờ `--use-localhost` là cần thiết để tránh các sự cố kết nối và chạy ứng dụng trực tiếp trên máy của bạn.
