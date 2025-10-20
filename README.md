# Hướng dẫn Cài đặt và Chạy Dự án Shopify App

Đây là hướng dẫn chi tiết để cài đặt và chạy ứng dụng Shopify này, bao gồm cả frontend React và các extension.

## Yêu cầu

1.  **Node.js**: Đảm bảo bạn đã cài đặt phiên bản LTS mới nhất.
2.  **Shopify CLI**: Cài đặt công cụ dòng lệnh của Shopify. Hướng dẫn có thể được tìm thấy [tại đây](https://shopify.dev/docs/apps/tools/cli).
3.  **Tài khoản Shopify Partner**: Cần thiết để tạo và quản lý ứng dụng.
4.  **Development Store**: Một cửa hàng để thử nghiệm ứng dụng của bạn.

## Cài đặt

1.  **Sao chép (Clone) Repository**:

    ```bash
    git clone <URL_REPOSITORY_CUA_BAN>
    cd new-app
    ```

2.  **Cài đặt các gói phụ thuộc (Dependencies)**:
    Dự án này có các gói phụ thuộc ở cả thư mục gốc và thư mục `frontend`. Bạn cần cài đặt ở cả hai nơi.

    - **Thư mục gốc**:
      ```bash
      npm install
      ```
    - **Thư mục Frontend**:
      ```bash
      cd frontend
      npm install
      cd ..
      ```

## Chạy ứng dụng ở chế độ phát triển (Development)

Để khởi động ứng dụng, bạn cần sử dụng Shopify CLI. Lệnh này sẽ tự động khởi động cả backend, frontend (Vite) và các extension.

Từ thư mục gốc của dự án (`new-app`), chạy lệnh sau:

```bash
shopify app dev --use-localhost
```

**Lưu ý quan trọng:**

- Cờ `--use-localhost` là **bắt buộc** để tránh các sự cố kết nối với Cloudflare tunnel. Lệnh này sẽ chạy ứng dụng trên máy cục bộ của bạn.
- Shopify CLI sẽ cung cấp một URL xem trước để bạn có thể cài đặt và thử nghiệm ứng dụng trên development store của mình.

## Khắc phục sự cố (Troubleshooting)

### Lỗi `EADDRINUSE: address already in use`

Lỗi này xảy ra khi một cổng (port) cần thiết đã bị một tiến trình khác chiếm dụng.

1.  **Tìm tiến trình đang chiếm cổng**:
    Mở PowerShell và chạy lệnh sau, thay `<PORT>` bằng số cổng bị báo lỗi (ví dụ: `9293`):

    ```powershell
    netstat -ano | findstr ":<PORT>"
    ```

    Lệnh này sẽ trả về một dòng chứa ID của tiến trình (PID) ở cột cuối cùng.

2.  **Dừng tiến trình**:
    Sử dụng PID bạn vừa tìm thấy để dừng tiến-trình:

    ```powershell
    taskkill /PID <PID_CUA_BAN> /F
    ```

3.  **Thử lại**:
    Chạy lại lệnh `shopify app dev --use-localhost`.

### Lỗi `'vite' is not recognized`

Lỗi này có nghĩa là Vite chưa được cài đặt đúng cách trong thư mục `frontend`.

1.  Di chuyển vào thư mục `frontend`:
    ```bash
    cd frontend
    ```
2.  Cài đặt Vite và các gói liên quan:
    ```bash
    npm install vite @vitejs/plugin-react --save-dev
    ```
3.  Quay lại thư mục gốc và thử lại:
    ```bash
    cd ..
    shopify app dev --use-localhost
    ```
