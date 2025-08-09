## 🚀 Môi trường phát triển với DevPod (Development Environment with DevPod)

Dự án này sử dụng `https://devpod.sh/` để tạo một môi trường phát triển nhất quán, đóng gói và có thể tái tạo lại. Điều này giúp loại bỏ các lỗi "it works on my machine" và cho phép bạn bắt đầu code trong vài phút.

### Yêu cầu (Prerequisites)

Trước khi bắt đầu, hãy chắc chắn rằng bạn đã cài đặt:

1. ** `https://www.google.com/search?q=https://devpod.sh/docs/getting-started/installation` **
2. ** `https://www.docker.com/get-started` ** (DevPod sử dụng Docker làm "provider" mặc định để tạo container).

### Hướng dẫn cài đặt (Setup Guide)

Bạn có thể chọn một trong hai phương pháp sau tùy theo nhu cầu.

-----

#### **Lựa chọn 1: Cài đặt nhanh bằng một lệnh (Nhanh nhất)**

Phương pháp này không yêu cầu bạn phải `git clone` repository về máy. DevPod sẽ tự động clone mã nguồn vào bên trong dev container.

* **Để mở bằng VS Code trên trình duyệt:**

  ```bash
  devpod up github.com/tatsuyakari1203/probv2 --ide openvscode
  ```

* **Để mở bằng ứng dụng VS Code trên máy tính:**

  ```bash
  devpod up github.com/tatsuyakari1203/probv2
  ```

-----

#### **Lựa chọn 2: Cài đặt thủ công (Khi bạn muốn có code ở máy local)**

Sử dụng phương pháp này nếu bạn muốn có một bản sao của mã nguồn trên máy tính của mình.

1. **Clone repository về máy của bạn:**

   ```bash
   git clone `https://github.com/tatsuyakari1203/probv2.git`
   ```

2. **Di chuyển vào thư mục dự án:**

   ```bash
   cd probv2
   ```

3. **Khởi chạy môi trường DevPod từ thư mục hiện tại:**

   ```bash
   # Mở bằng VS Code trên trình duyệt
   devpod up . --ide openvscode

   # Hoặc mở bằng ứng dụng VS Code trên máy tính
   # devpod up .
   ```

-----

DevPod sẽ tự động đọc file `.devcontainer/devcontainer.json` có sẵn trong dự án, build image, khởi chạy container và mở IDE cho bạn.

### Các lệnh hữu ích (Useful Commands)

* Liệt kê tất cả các workspace đang có:
  ```bash
  devpod list
  ```
* Dừng một workspace:
  ```bash
  devpod down [tên-workspace]
  ```
* Xóa một workspace:
  ```bash
  devpod delete [tên-workspace]
  ```