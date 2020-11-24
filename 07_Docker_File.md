# Docker File
- **Docker file** là tính năng mà **Docker** cung cấp để tạo một **Docker image**. **Docker file** là một file text đơn giản với cấu trúc để build một image
### **Cách tạo Docker file**
### **Các lệnh trong Docker file**
#### **MAINTAINER**
- Lệnh `MAINTAINER` chỉ đơn giản được sử dụng để diễn tả ai chịu trách nhiệm duy trì, maintain cho Dockerfile.
- Mặc dù vậy, lệnh `MAINTAINER` không thường được sử dụng vì loại thông tin đó cũng thường có sẵn trong Git repo hoặc ở các nơi khác
- **VD :**
    ```dockerfile
    MAINTAINER   Joe Blocks <joe@blocks.com>
    ```
#### **FROM**
- Lệnh `FROM` chỉ định base image cho Docker image .
- **VD :** Nếu muốn tạo image dựa trên một image Linux cơ bản, sử dụng như sau :
    ```dockerfile
    FROM ubuntu:latest
    ```
#### **CMD**
- Lệnh `CMD` chỉ định lệnh sẽ thực thi khi Docker container khởi động dựa trên Docker image built từ Dockerfile. 
- **VD1 :**
    ```dockerfile
    CMD echo Docker container started.
    ```
    > Lệnh này sẽ print ra trong terminal `Docker container started` mỗi khi container khởi động
- **VD2 :** Chạy 1 ứng dụng Java :
    ```dockerfile
    CMD java -cp /myapp/myapp.jar com.jenkov.myapp.MainClass arg1 arg2 arg3
    ```
#### **COPY**
- Lệnh `COPY` được sử dụng để copy 1 hoặc nhiều file từ Docker host (máy build Docker image từ Dockerfile) vào Docker image. Lệnh `COPY` có thể copy cả file và thư mục từ Docker host sang Docker image.
- **VD1 :**
    ```dockerfile
    COPY    /myapp/config/prod    /myapp/config
    ```
    > Lệnh trên giúp copy thư mục `/myapp/config/prod` trên Dockerhost sang thư mục `/myapp/config` trên Docker image
- **VD2 :** 
    ```dockerfile
    COPY    /myapp/config/prod/conf1.cfg   /myapp/config/prod/conf2.cfg   /myapp/config/
    ```
    > Lệnh trên giúp copy file `/myapp/config/prod/conf1.cfg` và `/myapp/config/prod/conf2.cfg` trên Docker host sang thư mục `/myapp/config` trên Docker image 
#### *`ADD`**
- Lệnh `ADD` hoạt động tương tự như lệnh `COPY`, chỉ có một vài khác biệt nhỏ sau :
    - Lệnh `ADD` có thể copy và giải nén file TAR từ Docker host vào Docker images
    - Lệnh `ADD` có thể download file thông qua HTTP và copy chúng vào Docker image
- **VD1 :**
    ```dockerfile
    ADD    myapp.tar    /myapp/
    ```
    > Lệnh này sẽ giải nén file `myapp.tar` từ Docker host và copy vào thư mục `/myapp/` trong Docker image
- **VD2 :**
    ```dockerfile
    ADD    http://jenkov.com/myapp.jar    /myapp/
    ```
    > Lệnh này sẽ download file theo đường dẫn và copy vào thư mục `/myapp/`
#### **`ENV`**
- Lệnh `ENV` có thể set biến môi trường bên trong Docker image . Biến môi trường này có thể sử dụng cho app chạy bên trong Docker image với lệnh `CMD` .
- **VD :** 
    ```dockerfile
    ENV    MY_VAR   123
    ```
    > Lệnh trên set biến môi trường `ENV` 