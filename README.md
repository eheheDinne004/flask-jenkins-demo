# Automated CI/CD Pipeline for Flask Application with Jenkins & Docker

##  Tổng quan dự án (Overview)
Dự án triển khai quy trình **Tích hợp và Triển khai Tự động (CI/CD)** cho một ứng dụng Web Python (Flask) chạy trên môi trường máy ảo **Ubuntu 24.04 (VMware)**. 

Hệ thống tự động kiểm thử, đóng gói ứng dụng thành **Docker Container** và triển khai dịch vụ mỗi khi có sự thay đổi mã nguồn trên GitHub repository.

---

## Công nghệ sử dụng (Technologies Used)
* **Hệ điều hành:** Ubuntu 24.04 LTS (VMware)
* **CI/CD Engine:** Jenkins (Java OpenJDK 21)
* **Containerization:** Docker
* **Source Control:** Git & GitHub
* **Framework:** Python 3.10 (Flask)

---

##  Kiến trúc Pipeline (Jenkinsfile)
1. **Stage 1 (Clone Code):** Tải mã nguồn mới nhất từ nhánh `main` của GitHub.
2. **Stage 2 (Build Docker Image):** Đóng gói ứng dụng Flask thành Docker Image (`docker build`).
3. **Stage 3 (Deploy Container):** Khởi chạy Container trên cổng `5000` (`docker run`).

---

## Cấu trúc thư mục (Directory Structure)
* `app.py`: Mã nguồn ứng dụng Web Flask.
* `Dockerfile`: Cấu hình đóng gói ứng dụng.
* `Jenkinsfile`: Kịch bản thực thi CI/CD cho Jenkins.
* `requirements.txt`: Thư viện phụ thuộc của Python.
* `README.md`: Tài liệu hướng dẫn dự án.

---

## Kết quả đạt được (Results)
* Pipeline chạy thành công tất cả các giai đoạn (Stages) trên Jenkins.
* Ứng dụng Flask truy cập thành công tại `http://localhost:5000`.
