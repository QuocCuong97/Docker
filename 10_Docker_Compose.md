# Docker Compose
## **1) Giới thiệu**
- Docker compose là công cụ dùng để định nghĩa và run multi-container cho Docker application. Với compose bạn sử dụng file YAML để config các services cho application của bạn. Sau đó dùng command để create và run từ những config đó. Sử dụng cũng khá đơn giản chỉ với ba bước:
    - Khai báo app’s environment trong Dockerfile.
    - Khai báo các services cần thiết để chạy application trong file docker-compose.yml.
    - Run docker-compose up để start và run app.

    <p align=center><img src=https://i.imgur.com/lDhSW8t.png width=60%></p>

- Không giống như Dockerfile (build các image), Docker compose dùng để build và run các container. Các thao tác của `docker-compose` tương tự như lệnh: `docker run`.
- Docker compose cho phép tạo nhiều service(container) giống nhau bằng lệnh:
    ```
    docker-compose scale <tên service> = <số lượng>
    ```
## **2) Cài đặt docker-compose**
- **B1 :** Download bản stable mới nhất của `docker-compose` :
    ```
    $ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    ```
- **B2 :** Cấp quyền thực thi cho file binary :
    ```
    $ sudo chmod +x /usr/local/bin/docker-compose
    ```
- **B3 :** Kiểm tra lại phiên bản `docker-compose` vừa cài đặt :
    ```
    $ docker-compose --version
    docker-compose version 1.29.2, build 1110ad01
    ```