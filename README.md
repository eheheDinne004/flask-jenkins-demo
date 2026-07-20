
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
## ⚙️ 3. Các bước tiến hành chi tiết
---
## Bước 1: Khởi tạo Máy chủ & Mở Cổng Cần Thiết
* Thiết lập máy chủ Ubuntu trên VMware hoặc AWS EC2.
* Mở các cổng dịch vụ cần thiết: `8080` / `8081` (Jenkins Web UI) và `5000` (Flask Web App).
<img width="449" height="200" alt="Screenshot 2026-07-20 134408" src="https://github.com/user-attachments/assets/6721f291-89d7-44c7-922b-9a41fe654737" />
*Hình 1: Khởi tạo và cấu hình Security Group / Firewall cho máy chủ.*
---
## Bước 2: Cài đặt Jenkins và Docker trên Ubuntu
* Cập nhật hệ thống
sudo apt update && sudo apt upgrade -y
* Cài đặt OpenJDK 21
sudo apt install openjdk-21-jre openjdk-21-jdk -y
* Thêm Jenkins Repository và Key
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  [https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key](https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key)
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  [https://pkg.jenkins.io/debian-stable](https://pkg.jenkins.io/debian-stable) binary/" | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
* Cài đặt và khởi động Jenkins
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins.

## Bước 3: Cài đặt Docker & Phân quyền cho Jenkins Java 21 và Jenkins
* Cài đặt Docker
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker

* Thêm user vào group docker
sudo groupadd docker
sudo usermod -aG docker $USER
sudo usermod -aG docker jenkins
sudo chmod 666 /var/run/docker.sock

* Restart Jenkins để áp dụng quyền
sudo systemctl restart jenkins

* Kiểm tra phân quyền
sudo -u jenkins docker ps

---
## Bước 4: Khởi Tạo Git & Push Code Lên GitHub   Bashgit init
* git add .
* git commit -m "Initial commit with Jenkinsfile"
* git branch -M main
* git remote add origin [https://github.com/](https://github.com/)<YOUR-USERNAME>/flask-jenkins-demo.git
* git push -u origin main
