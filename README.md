# Báo Cáo Bài Tập 3: Đóng Gói Trang Web HTML Tĩnh Bằng Dockerfile Siêu Ngắn Gọn

- **Môn học**: DevOps Fundamentals (IT-209)
- **Session**: 09 - Docker Fundamentals & Containerization
- **Bài tập**: Bài 3 - Viết Dockerfile 3 dòng đóng gói trang Web HTML tĩnh
- **Đường dẫn nộp bài trên GitHub**: `homework/session_09/ex3/`
- **Học viên**: HoQuocKhai
- **Email**: khaiho6040@gmail.com

---

## 1. Mục tiêu bài thực hành
- Viết tệp `Dockerfile` tối giản để đóng gói ứng dụng web tĩnh vào Docker Image nền `nginx:alpine`.
- Build image tùy biến với tên `my-html-app:v1`.
- Khởi chạy container từ image vừa tạo và map cổng máy chủ `8081` vào cổng `80` của container.
- Kiểm tra phản hồi HTTP bằng `curl` để xác nhận Nginx phục vụ đúng nội dung trang web.

---

## 2. Cấu trúc thư mục bài làm
```text
homework/session_09/ex3/
├── Dockerfile
├── index.html
└── README.md
```

---

## 3. Nội dung các tệp cấu hình

### 3.1. Tệp mã nguồn `index.html`
```html
<h1>Hello Docker Session 09!</h1>
```

### 3.2. Tệp `Dockerfile`
```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

> **Giải thích:**
> - `FROM nginx:alpine`: Sử dụng base image Nginx chạy trên hệ điều hành Alpine Linux siêu nhẹ (~40MB).
> - `COPY index.html /usr/share/nginx/html/index.html`: Sao chép file `index.html` từ host vào thư mục web root mặc định của Nginx trong container.

---

## 4. Các bước thực hiện & Kết quả đầu ra

### Bước 1: Build Docker Image
Thực thi lệnh build image tại thư mục chứa `Dockerfile`:
```bash
docker build -t my-html-app:v1 .
```

**Kết quả đầu ra mẫu:**
```text
[+] Building 1.2s (7/7) FINISHED
 => [internal] load build definition from Dockerfile
 => => transferring dockerfile: 95B
 => [internal] load metadata for docker.io/library/nginx:alpine
 => [internal] load .dockerignore
 => => transferring context: 2B
 => [1/2] FROM docker.io/library/nginx:alpine
 => [internal] load build context
 => => transferring context: 45B
 => [2/2] COPY index.html /usr/share/nginx/html/index.html
 => exporting to image
 => => exporting layers
 => => writing image sha256:d8b74a3f12a89047c1b504e90d81b498f3c7e6c46a78210375dc021e5dfc5b98
 => => naming to docker.io/library/my-html-app:v1
```

---

### Bước 2: Khởi chạy Container
Chạy container ở chế độ detached (`-d`), ánh xạ cổng `8081:80`, đặt tên container là `html-app`:
```bash
docker run -d -p 8081:80 --name html-app my-html-app:v1
```

**Kết quả đầu ra mẫu:**
```text
e52f1469e73b45a0b77c385bb6cb1e85501869e5d9571fa08d0e6ba39d7fc214
```

Kiểm tra trạng thái container đang chạy:
```bash
docker ps --filter "name=html-app"
```

**Kết quả:**
```text
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                  NAMES
e52f1469e73b   my-html-app:v1    "/docker-entrypoint.…"   5 seconds ago   Up 4 seconds   0.0.0.0:8081->80/tcp   html-app
```

---

### Bước 3: Kiểm tra kết quả phản hồi (curl)
Gửi yêu cầu HTTP GET tới cổng `8081`:
```bash
curl http://localhost:8081
```

**Kết quả đầu ra thực tế:**
```html
<h1>Hello Docker Session 09!</h1>
```

---

## 5. Dọn dẹp tài nguyên sau khi kiểm tra
```bash
docker stop html-app
docker rm html-app
```
