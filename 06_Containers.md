# Containers
## **1) Giới thiệu**
- **Containers** là các instance của **Docker images** mà có thể chạy qua lện `docker run`. Mục đích chính của **Docker** là để chạy các **containers**
## **2) Các lệnh thường dùng**
### **`docker run`**
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
### **`docker ps`**
- Lệnh này sẽ hiển thị chỉ những container đang chạy
- Cú pháp :
    ```
    # docker ps
    ```
    <img src=https://i.imgur.com/beMVt2W.png>

### **`docker ps -a`**
- Lệnh này được sử dụng để list tất cả các container đang có trong hệ thống, kể cả những container đang chạy hoặc không chạy
- Cú pháp :
    ```
    # docker ps -a
    ```
    <img src=https://i.imgur.com/W4OMC9f.png>
### **`docker history`**
- Với lệnh này, có thể kiểm tra lịch sử tất cả các command đã chạy với image qua container .
- Cú pháp :
    ```
    # docker history image_ID
    ```
- **VD :**
    ```
    # docker history nginx
    ```
    <img src=https://i.imgur.com/FiSLfVE.png>
### **`docker top`**
- Với lệnh này, ta có thể xem được các top process của container .
- Cú pháp :
    ```
    # docker top Container_ID
    ```
- **VD :**
    ```
    # docker top e1a4aa75fce4
    ```
    <img src=https://i.imgur.com/T8j55vJ.png>
### **`docker stop`**
- Lệnh này được sử dụng để stop một container đang chạy
- Cú pháp :
    ```
    # docker stop Container_ID
    ```
    - Output sẽ trả về ID của container bị tắt
- **VD :**
    ```
    # docker stop e1a4aa75fce4
    e1a4aa75fce4
    ```
### **`docker rm`** 
- Lệnh này được sử dụng để xóa 1 container
- Cú pháp :
    ```
    # docker rm e1a4aa75fce4
    ```
    > Sau khi stop container, vẫn có thể thấy container vẫn tồn tại trong server qua lệnh `docker ps -a`. Tuy nhiên khi sử dụng `docker rm` sẽ hoàn toàn xóa container khỏi server và sẽ không thể thấy trong output của lệnh `docker ps -a` nữa
- **VD :**
    ```
    # docker rm e1a4aa75fce4
    ```
### **`docker stats`**
- Lệnh này được sử dụng để show ra các số liệu thống kê cho container đang chạy. Outpu
- Cú pháp :
    ```
    # docker stats Container_ID
    ```
- **VD :**
    ```
    # docker stats 838b48cd3e1f
    ```
    <img src=https://i.imgur.com/SGZnnYk.png>
### **`docker attach ???`**
- Lệnh này được sử dụng để truy cập console của container
> Khi chạy container bằng lệnh `docker run -d` (detach), nếu truy cập console bằng `docker exec` thì sau khi exit, container sẽ không bị tắt. Tuy nhiên nếu truy cập console bằng lệnh `docker attach` rồi exit, container sẽ bị tắt. Khi sử dụng `docker attach` thì cần thoát bằng `Ctrl + P` hoặc `Ctrl +Q`
- Cú pháp :
    ```
    # docker attach Container_ID
    ```
- **VD :**
    ```
    # docker attach 838b48cd3e1f
    ```
### **`docker pause`**
- Lệnh này được sử dụng để tạm dừng tiến trình trên container đang chạy. Container vẫn có thể được nhìn thấy khi thực hiện lệnh `docker ps`
- Cú pháp :
    ```
    # docker pause Container_ID
    ```
- **VD :**
    ```
    # docker pause dcf0665b448a
    ```
    <img src=https://i.imgur.com/zdak8l9.png>
### **`docker unpause`**
- Lệnh được sử dụng để unpause tiến trình đã bị pause lại trước đó trên container đang chạy
- Cú pháp :
    ```
    # docker unpause Container_ID
    ```
- **VD :**
    ```
    # docker unpause dcf0665b448a
    ```
### **`docker kill`**
- Lệnh này được sử dụng để kill tiến trình trên container đang chạy.
- Cú pháp :
    ```
    # docker kill Container_ID
    ```
- **VD :** 
    ```
    # docker kill dcf0665b448a
    ```
    <img src=https://i.imgur.com/35pHypa.png>

## **3) Truy cập container's shell**

https://www.tutorialspoint.com/docker/docker_containers_and_shells.htm