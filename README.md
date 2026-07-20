# LAB 8: TRIỂN KHAI CI/CD PIPELINE VỚI JENKINS VÀ DOCKER

* **Người thực hiện:** Bùi Thủy Ngọc Duyên
* **Môi trường thực thi:** Ubuntu 24.04 LTS (VMware Virtual Machine)
* **Trạng thái:** Completed 

---

## 1. Mục tiêu bài thực hành
* Cài đặt và cấu hình máy chủ **Jenkins Server** và **Docker** trên hệ điều hành Ubuntu.
* Cấu hình phân quyền cho phép `jenkins` user tương tác trực tiếp với Docker socket không cần quyền `sudo`.
* Xây dựng một ứng dụng Web cơ bản bằng **Python Flask Framework**.
* Đóng gói ứng dụng thành **Docker Container** bằng `Dockerfile`.
* Định nghĩa kịch bản **CI/CD Pipeline** tự động hóa trong `Jenkinsfile`.
* Đồng bộ mã nguồn lên **GitHub** và khởi chạy tự động hóa Build & Deploy ứng dụng trên cổng `5000`.

---

## 🛠️ 2. Công nghệ & Công cụ sử dụng
* **Hệ điều hành:** Ubuntu 24.04 LTS (VMware)
* **Runtime:** Java OpenJDK 21
* **CI/CD Tool:** Jenkins
* **Containerization:** Docker
* **Source Control:** Git & GitHub
* **Web Framework:** Python 3.10 (Flask 3.0.2)

---

## 3. Kiến trúc Pipeline (Jenkinsfile Stages)

```text
  [ Developer Push Code ] 
            │
            ▼
   [ GitHub Repository ]
            │
            ▼ (SCM Trigger)
     [ Jenkins Server ]
            │
            ├── Stage 1: Clone Code (Kéo mã nguồn từ GitHub)
            ├── Stage 2: Build Docker Image (docker build -t flask-app:latest .)
            └── Stage 3: Deploy Container (docker run -d -p 5000:5000 flask-container)
            │
            ▼
  [ Web Application: http://localhost:5000 ]

---

### Bước 1: Khởi tạo Máy chủ & Mở Cổng Cần Thiết
* Thiết lập máy chủ Ubuntu trên VMware hoặc AWS EC2.
* Mở các cổng dịch vụ cần thiết: `8080` / `8081` (Jenkins Web UI) và `5000` (Flask Web App).

![Khởi tạo và cấu hình Security Group / Firewall cho máy chủ](fig1-aws-instance.png)
*Hình 1: Khởi tạo và cấu hình Security Group / Firewall cho máy chủ.*

---

### Bước 2: Cài đặt Jenkins và Docker trên Ubuntu

#### 1. Cài đặt Java 21 và Jenkins
```bash
# Cập nhật hệ thống
sudo apt update && sudo apt upgrade -y

# Cài đặt OpenJDK 21
sudo apt install openjdk-21-jre openjdk-21-jdk -y

# Thêm Jenkins Repository và Key
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  [https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key](https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key)

echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  [https://pkg.jenkins.io/debian-stable](https://pkg.jenkins.io/debian-stable) binary/" | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

# Cài đặt và khởi động Jenkins
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
