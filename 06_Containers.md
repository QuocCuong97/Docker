# Containers
## **1) Giới thiệu**
- **Containers** là các instance của **Docker images** mà có thể chạy qua lện `docker run`. Mục đích chính của **Docker** là để chạy các **containers**
## **2) Các lệnh thường dùng**
### **Chạy container**
- Việc chạy các **container** được quản lý qua lệnh `docker run` :
    ```
    # docker run <option> <image_name>
    ```
    - **Options** :
        - `-it ... <shell>` : truy cập interactive mode của container
    > Sử dụng lệnh `exit` hoặc `Ctrl+P` để thoát khỏi interactive mode của container
- **VD :**
    ```
    # docker pull centos
    # docker run -it centos bash
    ```
    <img src=https://i.imgur.com/pVyCeiu.png>
- Từ đây có thể chạy được nhiều instance với OS khác nhau trên OS chính
### **List các container đang chạy**
- Cú pháp :
    ```
    # docker ps
    ```
