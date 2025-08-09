## 🚀 Môi trường phát triển với DevPod (Development Environment with DevPod)

Dự án này sử dụng `https://devpod.sh/` để tạo một môi trường phát triển nhất quán, đóng gói và có thể tái tạo lại. Điều này giúp loại bỏ các lỗi "it works on my machine" và cho phép bạn bắt đầu code trong vài phút.

### Yêu cầu (Prerequisites)

Trước khi bắt đầu, hãy chắc chắn rằng bạn đã cài đặt:

1. ** `https://www.google.com/search?q=https://devpod.sh/docs/getting-started/installation` **
2. ** `https://www.docker.com/get-started` ** (DevPod sử dụng Docker làm "provider" mặc định để tạo container).

### Hướng dẫn cài đặt (Setup Guide)

Bạn có thể chọn một trong hai phương pháp sau tùy theo nhu cầu.

### Chạy nền (Daemon Mode)

Để chạy DevPod ở chế độ nền (daemon), giải phóng terminal của bạn và lưu log vào một file, bạn có thể sử dụng các lệnh sau trong môi trường shell tương thích với POSIX (như Git Bash, WSL, hoặc Linux).

**1. Khởi động và lưu log**

```bash
nohup devpod up . --ide openvscode --ide-option BIND_ADDRESS=0.0.0.0:10800 > devpod.log 2>&1 &
```

*   `nohup`: Đảm bảo tiến trình vẫn chạy ngay cả khi bạn đóng terminal.
*   `> devpod.log 2>&1`: Chuyển hướng tất cả output (cả stdout và stderr) vào file `devpod.log`.
*   `&`: Chạy lệnh trong nền.

**2. Lấy và lưu Process ID (PID)**

Ngay sau khi chạy lệnh trên, bạn có thể lấy PID của tiến trình và lưu vào file để tiện cho việc dừng sau này.

```bash
echo $! > devpod.pid
```

**3. Xem log**

Để theo dõi output của DevPod trong thời gian thực:

```bash
tail -f devpod.log
```

**4. Dừng DevPod**

Để dừng tiến trình DevPod đang chạy nền, sử dụng lệnh `kill` với PID đã lưu:

```bash
kill $(cat devpod.pid)
```

---
--

#### **Lựa chọn 1: Cài đặt nhanh bằng một lệnh (Nhanh nhất)**

Phương pháp này không yêu cầu bạn phải `git clone` repository về máy. DevPod sẽ tự động clone mã nguồn vào bên trong dev container.

* **Để mở bằng VS Code trên trình duyệt:**

  ```bash
  devpod up github.com/tatsuyakari1203/vscode-latex-devcontainer --ide openvscode
  ```

* **Để mở bằng ứng dụng VS Code trên máy tính:**

  ```bash
  devpod up github.com/tatsuyakari1203/vscode-latex-devcontainer
  ```

-----

#### **Lựa chọn 2: Cài đặt thủ công (Khi bạn muốn có code ở máy local)**

Sử dụng phương pháp này nếu bạn muốn có một bản sao của mã nguồn trên máy tính của mình.

1. **Clone repository về máy của bạn:**

   ```bash
   git clone https://github.com/tatsuyakari1203/vscode-latex-devcontainer.git
   ```

2. **Di chuyển vào thư mục dự án:**

   ```bash
   cd vscode-latex-devcontainer
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

### Lưu ý quan trọng (Important Notes)

**Tự động tắt do không hoạt động (Auto-shutdown on idle)**

DevPod có một tính năng mặc định sẽ tự động tắt workspace nếu không có hoạt động nào trong một khoảng thời gian ngắn để tiết kiệm tài nguyên. Bạn có thể gặp lỗi sau:

`fatal Stopping devpod up, because it stayed idle for a while.`

Để vô hiệu hóa tính năng này và giữ cho workspace của bạn luôn chạy, hãy thực thi lệnh sau trên terminal của bạn **một lần duy nhất**:

```bash
devpod context set-options -o EXIT_AFTER_TIMEOUT=false
```

Lệnh này sẽ thay đổi cài đặt chung của DevPod và áp dụng cho tất cả các workspace trong tương lai.

### Truy cập qua mạng LAN (LAN Access)

Lỗi `missing port in address` cho thấy DevPod yêu cầu bạn phải chỉ định cả địa chỉ và cổng khi truy cập qua mạng LAN. Cổng mặc định cho VS Code là `10800`.

Sử dụng tùy chọn `--ide-option BIND_ADDRESS=0.0.0.0:10800` khi chạy lệnh `devpod up`:

```bash
# Mở bằng VS Code trên trình duyệt, có thể truy cập qua LAN tại port 10800
devpod up . --ide openvscode --ide-option BIND_ADDRESS=0.0.0.0:10800
```

Sau khi chạy lệnh, bạn có thể truy cập môi trường phát triển từ một thiết bị khác trong cùng mạng LAN bằng cách truy cập `http://<ĐỊA_CHỈ_IP_MÁY_CHỦ>:10800`.

> **⚠️ Cảnh báo bảo mật:** Việc này sẽ đưa môi trường phát triển của bạn ra mạng LAN. Hãy chắc chắn rằng bạn đang ở trong một mạng an toàn và đáng tin cậy trước khi sử dụng tùy chọn này.

---

## 🚀 Development Environment with DevPod (English)

This project uses `https://devpod.sh/` to create a consistent, containerized, and reproducible development environment. This helps eliminate "it works on my machine" errors and allows you to start coding in minutes.

### Prerequisites

Before you begin, ensure you have installed:

1.  **`https://www.google.com/search?q=https://devpod.sh/docs/getting-started/installation`**
2.  **`https://www.docker.com/get-started`** (DevPod uses Docker as the default provider to create containers).

### Setup Guide

You can choose one of the following two methods based on your needs.

-----

#### **Option 1: Quick Setup with a Single Command (Fastest)**

This method does not require you to `git clone` the repository to your machine. DevPod will automatically clone the source code inside the dev container.

*   **To open with VS Code in the browser:**

    ```bash
    devpod up github.com/tatsuyakari1203/probv2 --ide openvscode
    ```

*   **To open with the VS Code desktop application:**

    ```bash
    devpod up github.com/tatsuyakari1203/probv2
    ```

-----

#### **Option 2: Manual Setup (When you want the code locally)**

Use this method if you want a local copy of the source code on your machine.

1.  **Clone the repository to your machine:**

    ```bash
    git clone `https://github.com/tatsuyakari1203/probv2.git`
    ```

2.  **Navigate to the project directory:**

    ```bash
    cd probv2
    ```

3.  **Launch the DevPod environment from the current directory:**

    ```bash
    # Open with VS Code in the browser
    devpod up . --ide openvscode

    # Or open with the VS Code desktop application
    # devpod up .
    ```

-----

DevPod will automatically read the `.devcontainer/devcontainer.json` file in the project, build the image, launch the container, and open the IDE for you.

### Daemon Mode

To run DevPod in the background (daemon mode), free up your terminal, and save logs to a file, you can use the following commands in a POSIX-compliant shell (like Git Bash, WSL, or Linux).

**1. Start and Log**

```bash
nohup devpod up . --ide openvscode --ide-option BIND_ADDRESS=0.0.0.0:10800 > devpod.log 2>&1 &
```

*   `nohup`: Ensures the process continues running even if you close the terminal.
*   `> devpod.log 2>&1`: Redirects all output (both stdout and stderr) to the `devpod.log` file.
*   `&`: Runs the command in the background.

**2. Get and Save Process ID (PID)**

Immediately after running the command above, you can get the PID of the process and save it to a file for easy stopping later.

```bash
echo $! > devpod.pid
```

**3. View Logs**

To monitor DevPod's output in real-time:

```bash
tail -f devpod.log
```

**4. Stop DevPod**

To stop the background DevPod process, use the `kill` command with the saved PID:

```bash
kill $(cat devpod.pid)
```

*   List all current workspaces:
    ```bash
    devpod list
    ```
*   Stop a workspace:
    ```bash
    devpod down [workspace-name]
    ```
*   Delete a workspace:
    ```bash
    devpod delete [workspace-name]
    ```

### Accessing via LAN

By default, the DevPod development environment is only accessible from the host machine (localhost). To access the VS Code instance from other devices on the same local network (e.g., from an iPad or another computer), you need to instruct DevPod to bind the server to the `0.0.0.0` address.

Use the `--ide-option BIND_ADDRESS=0.0.0.0` flag when running the `devpod up` command:

```bash
# Open with VS Code in the browser, accessible over the LAN
devpod up . --ide openvscode --ide-option BIND_ADDRESS=0.0.0.0
```

After running the command, DevPod will provide a URL. Replace `127.0.0.1` or `localhost` in that URL with the IP address of the host machine running DevPod to access it from another device.

> **⚠️ Security Warning:** This exposes your development environment to the local network. Ensure you are on a secure and trusted network before using this option.