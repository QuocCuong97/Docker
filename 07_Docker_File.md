# Docker File
- **Docker file** là tính năng mà **Docker** cung cấp để tạo một **Docker image**. **Docker file** là một file text đơn giản với cấu trúc để build một image
### **Cách tạo Docker file**
### **Các lệnh trong Docker file**
#### **`MAINTAINER`**
- Lệnh `MAINTAINER` chỉ đơn giản được sử dụng để diễn tả ai chịu trách nhiệm duy trì, maintain cho Dockerfile.
- Mặc dù vậy, lệnh `MAINTAINER` không thường được sử dụng vì loại thông tin đó cũng thường có sẵn trong Git repo hoặc ở các nơi khác
- **VD :**
    ```dockerfile
    MAINTAINER   Joe Blocks <joe@blocks.com>
    ```
#### **`FROM`**
- Lệnh `FROM` chỉ định base image cho Docker image .
- **VD :** Nếu muốn tạo image dựa trên một image Linux cơ bản, sử dụng như sau :
    ```dockerfile
    FROM ubuntu:latest
    ```
#### **`CMD`**
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
#### **`COPY`**
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
#### **`ADD`**
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
    > Lệnh trên set biến môi trường `MY_VAR` có giá trị `123`
#### **`RUN`** 
- Lệnh `RUN` có thể thực thi các lệnh thực thi bên trong Docker image. Lệnh `RUN` sẽ được thực thi trong quá trình build image, vì vậy nó chỉ được thực hiện 1 lần. Lệnh `RUN` có thể được sử dụng để cài đặt các app trong Docker image, hoặc giải nén file, hoặc bất cứ command line nào cần thiết khác.
- **VD :** 
    ```dockerfile
    RUN apt-get install some-needed-app
    ```
#### **`ARG`**
- Lệnh `ARG` cho phép định nghĩa tham số có thể truyền vào Docker khi build Docker file.
- **VD :**
    ```dockerfile
    ARG tcpPort
    ```
    - Khi chạy lệnh Docker để build Dockerfile chứa lệnh `ARG` trên sẽ như sau :
        ```
        docker build --build-arg tcpPort=8080 .
        ```
- Các tham số được định nghĩa bằng lệnh `ARG` có thể được sử dụng ở mọi nơi trong Dockerfile.
- **VD :**
    ```dockerfile
    ARG tcpPort=8080
    ARG useTls=true

    CMD start-my-server.sh -port ${tcpPort} -tls ${useTls}
    ```
#### **`WORKDIR`**
- Lệnh `WORKDIR` chỉ định thư mục làm việc bên trong Docker image. Nó sẽ ảnh hưởng tới tất cả các lệnh theo phía sau nó
- **VD :**
    ```dockerfile
    WORKDIR    /java/jdk/bin
    ```
#### **`EXPOSE`**
- Lệnh `EXPOSE` sẽ mở port mạng trên Docker container thông ra mạng bên ngoài. Ví dụ nếu Docker container chạy một webserver, chắc chắn cần mở port 80 để client có thể truy cập.
- **VD :**
    ```dockerfile
    EXPOSE   8080
    ```
- Có thể chọn giao thức cho phép để giao tiếp với port. **VD :**
    ```dockerfile
    EXPOSE   8080/tcp 9999/udp
    ```
    > Nếu không khai báo giao thức, mặc định sẽ là `tcp`
#### **`VOLUME`**
- Lệnh `VOLUME` dùng để tạo thư mục bên trong Docker image mà có thể truy cập được từ bên ngoài Docker host. Nói cách khác, có thể tạo ra thư mục bên trong Docker image (vd `/data`) và mount nó vào thư mục `/container-data/container1` trên Docker host. Quá trình mount sẽ được thực hiện khi container khởi động
- **VD :** 
    ```dockerfile
    VOLUME   /data
    ```
#### **`ENTRYPOINT`**
- Lệnh `ENTRYPOINT` cung cấp entrypoint cho Docker container. Entrypoint là một app hoặc 1 lệnh có thể được thực hiện khi Docker container khởi động. Theo ngữ cảnh này thì `ENTRYPOINT` cũng giống như lệnh `CMD`, điểm khác biệt là khi sử dụng `ENTRYPOINT`, Docker container sẽ bị tắt khi `ENTRYPOINT` kết thúc
- **VD :**
    ```dockerfile
    ENTRYPOINT java -cp /apps/myapp/myapp.jar com.jenkov.myapp.Main
    ```
#### **`HEALTHCHECK`**
- Lệnh `HEALTHCHECK` được sử dụng theo đúng nghĩa đen của nó, thực hiện lệnh kiểm tra container theo khoảng thời gian được chỉ định, để giám sát các app đang chạy trong container. Nếu lệnh trả về `0` khi thoát, container và các app đang `healthy`. Nếu trả về `1` chứng minh điều ngược lại.
- **VD :**
    ```dockerfile
    HEALTHCHECK java -cp /apps/myapp/healthcheck.jar com.jenkov.myapp.HealthCheck https://localhost/healthcheck
    ```
- Mặc định, Docker sẽ thực hiện lệnh `HEALTHCHECK` mỗi `30s`, tuy nhiên có thể tùy chỉnh bằng tham số `--interval` :
    ```dockerfile
    HEALTHCHECK --interval=60s java -cp /apps/myapp/healthcheck.jar com.jenkov.myapp.HealthCheck https://localhost/healthcheck
    ```
- Mặc định, Docker sẽ thực hiện lệnh ngay khi Docker container chạy. Tuy nhiên, đôi khi một app khởi động cần một khoảng thời gian, vì vậy sẽ là không hợp lý khi check. Có thể set khoảng thời gian bắt đầu (start period) thực hiện lệnh `HEALTHCHECK` bằng tham số `--start-period` :
    ```dockerfile
    HEALTHCHECK --start-period=300s java -cp /apps/myapp/healthcheck.jar com.jenkov.myapp.HealthCheck https://localhost/healthcheck
    ```
- Có thể set timeout cho `HEALTHCHECK`. Nếu lệnh mất quá nhiều thời gian để thực hiện, Docker sẽ coi như lệnh bị timeout. Có thể set timeout bằng tham số `--timeout` :
    ```dockerfile
    HEALTHCHECK --timeout=5s java -cp /apps/myapp/healthcheck.jar com.jenkov.myapp.HealthCheck https://localhost/healthcheck
    ```
    > Nếu xảy ra timeout, Docker container cũng bị tính là unhealthy
- Nếu `HEALTHCHECK` thất bại, trả về `1`, hoặc timeout, Docker sẽ thử chạy lại `HEALTHCHECK` thêm 3 lần. Nếu vẫn thất bại, container mới bị unhealthy. Có thể tùy chỉnh số lần thử lại này bằng tham số `--retries` :
    ```dockerfile
    HEALTHCHECK --retries=5 java -cp /apps/myapp/healthcheck.jar com.jenkov.myapp.HealthCheck https://localhost/healthcheck
    ```