# Kiến trúc và thành phần của Docker
## **1) Docker Engine**
- **Docker Engine** là một ứng dụng client-server. 
- Có hai phiên bản **Docker Engine** phổ biến là :
    - **Docker Community Edition (CE)** : Là phiên bản miễn phí và chủ yếu dựa vào các sản phầm nguồn mở khác.
    - **Docker Enterprise** : Khi sử dụng phiên bản này bạn sẽ nhận được sự support của nhà phát hành, có thêm các tính năng quản lý và security.
- Các thành phần chính của Docker Engine gồm có:
    - **Server** hay còn được gọi là ***docker daemon*** (`dockerd`): chịu trách nhiệm tạo, quản lý các Docker objects như images, containers, networks, volume.
    - **REST API** : docker daemon cung cấp các api cho Client sử dụng để thao tác với **Docker**
    - **Client** là thành phần đầu cuối cung cấp một tập hợp các câu lệnh sử dụng api để người dùng thao tác với **Docker**. (**VD :** `docker images`, `docker ps`, `docker rmi image` ...)

        <img src=https://i.imgur.com/yAxVnPU.jpg>

## **2) Kiến trúc và thành phần của Docker**
- **Docker** sử dụng kiến trúc ***client-server***. 
- **Docker server** (hay còn gọi là `daemon`) sẽ chịu trách nhiệm `build`, `run`, `distrubute` **Docker container**. **Docker client** và **Docker server** có thể nằm trên cùng một server hoặc khác server. Chúng giao tiếp với nhau thông qua **REST API** dựa trên UNIX sockets hoặc network interface.
### **2.1) Docker daemon**
- **Docker daemon** (`dockerd`) là thành phần core, lắng nghe API request và quản lý các Docker object. Docker daemon host này cũng có thể giao tiếp được với Docker daemon ở host khác.
### **2.2) Docker client**
- **Docker client** (`docker`) là phương thức chính để người dùng thao tác với Docker. Khi người dùng gõ lệnh để thao tác tức là người dùng sử dụng CLI và gửi request đến dockerd thông qua api, và sau đó **Docker daemon** sẽ xử lý tiếp.
- **Docker client** có thể giao tiếp và gửi request đến nhiều **Docker daemon**.
### **2.3) Docker registry**
- **Docker Registry** là một khu vực lưu trữ từ xa, nơi mà **Docker Images** được lưu trữ. 
- Chúng ta thường `pull/push` **docker image** từ **Docker Registry**. - Chúng ta có thể tự xây dựng **Docker Registry** cho riêng mình hoặc sử dụng Registry của nhà cung cấp như **AWS** và **Google Cloud**.
- **Docker Hub** là một registry lớn nhất của **Docker Images**, nó cũng là registry default. Bạn có thể tìm kiếm và lưu trữ **Docker Images** của bạn trên **Docker Hub** miễn phí.
### **2.4) Docker object**
- Là các đối tượng mà bạn thường xuyên gặp phải khi sử dụng **Docker** gồm :
- **Images** :
    - Là một template read-only sử dụng để chạy container.
    - Một image có thể base trên một image khác .
    - Có thể tự build image cho riêng mình hoặc tải các image có sẵn của người khác trên **Docker registry**.
- **Container** :
    - **Container** được chạy dựa trên 1 **image** cụ thể. Có thể tạo, `start`, `stop`, `move`, `delete` **container**.
    - Có thể kết nối các **container** với nhau hoặc attach storage cho nó, thậm chí là tạo lại một **image** từ chính state hiện tại của **container** .
    - **Container** cô lập tài nguyên với host và các **container** khác.

        <img src=https://i.imgur.com/akXmmBU.png>
    ## **3) Một số khái niệm mở rộng**
    ### **3.1) Docker Network**
    - **Docker Network** cho phép kết nối nhiều **container** lại với nhau, các **container** có thể nằm trên cùng 1 host hoặc nhiều host khác nhau .
    ### **3.2) Docker Compose**
    - **Docker Compose** là một công cụ giúp bạn dễ dàng hơn trong việc chạy ứng dụng trên nhiều **container**.
    - **Docker Compose** cho phép bạn di chuyển các command vào bên trong file `docker-compose.yml` cho việc sử dụng lại. **Docker Compose** command line interface (CLI) giúp chúng ta dễ tương tác hơn với ứng nhiều container. 
    - **Docker Compose** là miễn phí.
    ### **3.3) Docker Swarm**
    - Là một **orchestrate container**.
    ### **3.4) Kubernetes** <img src=https://i.imgur.com/scvTGbw.png width=30% align=right>
    - **Kubernetes** tự động hóa việc triển khai, Scaling, và quản lý các ứng dụng chạy bằng **container**. 
    - **K8s** áp đảo hoàn toàn trong số các công cụ container orchestration. Thay vì sử dụng **Docker Swarm**, nên sử dụng **K8s** để scale up project của bạn với hàng chục, hàng trăm container.