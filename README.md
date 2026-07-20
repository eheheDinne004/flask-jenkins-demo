## ⚙️ 4. Các bước tiến hành chi tiết
---
### 📌 Bước 1: Khởi tạo Máy chủ & Mở Cổng Cần Thiết
* Thiết lập máy chủ Ubuntu trên VMware hoặc AWS EC2.
* Mở các cổng dịch vụ cần thiết: `8080` / `8081` (Jenkins Web UI) và `5000` (Flask Web App).
![Khởi tạo Security Group](fig1-aws-instance.png)
*Hình 1: Khởi tạo và cấu hình Security Group / Firewall cho máy chủ.*
---
### 📌 Bước 2: Cài đặt Jenkins và Docker trên Ubuntu
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
