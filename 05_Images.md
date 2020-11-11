# Docker Image
## **1) Giới thiệu**
- Trong **Docker**, tất cả đều dựa trên **image** .
- **Image** là sự kết hợp giữa file system và các tham số truyền vào. 
## **2) Một số lệnh cơ bản**
- Liệt kê tất cả **image** có trong máy local :
    ```
    # docker images <option>
    ```
    <img src=https://i.imgur.com/7Hfblgo.png>

    - Trong đó :
        - `TAG` : được sử dụng để tag các **images** local
        - `IMAGE ID` : được sử dụng để định danh các **image**
        - `CREATED` : thời gian **image** được tạo ra
        - `SIZE` : kính cỡ **image**
    - `<option>` :
        - `-q` : chỉ trả về list các **image ID**
- Download **docker image** :
    ```
    # docker run <options> <image_name>
    ```
    - Trong đó :
        - `<image_name>` : tên của image. Nếu không tồn tại trong local, **Docker** sẽ tìm kiếm và download nó từ **Docker Hub**
    - **Options :**
        - `-d` : chạy container ở chế độ nền
- Xóa **docker image** :
    ```
    # docker rmi <image_ID>
    ```
    - Trong đó :
        - `<image_ID>` : ID của image trong local
- Xem thông tin chi tiết của **docker image** hoặc **container** :
    ```
    # docker inspect <image_name>
    ```
    - Trong đó :
        - `<image_name>` : tên của image trong local
- Tìm kiếm **image** trên **Docker Hub** :
    ```
    # docker search <text>
    ```
- Download image từ **Docker Hub** :
    ```
    # docker pull <image_name>
    ```