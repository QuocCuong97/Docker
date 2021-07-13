# Docker Compose
## **1) Giới thiệu**
- Docker compose là công cụ dùng để định nghĩa và run multi-container cho Docker application. Với compose bạn sử dụng file YAML để config các services cho application của bạn. Sau đó dùng command để create và run từ những config đó. Sử dụng cũng khá đơn giản chỉ với ba bước:
    - Khai báo app’s environment trong Dockerfile.
    - Khai báo các services cần thiết để chạy application trong file docker-compose.yml.
    - Run docker-compose up để start và run app.

    <p align=center><img src=https://i.imgur.com/lDhSW8t.png width=60%></p>