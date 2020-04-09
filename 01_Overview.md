# Tổng quan về Docker
## **1) Khái niệm**
- **Docker** là một nền tảng dành cho các developer và sysadmin để phát triển, triển khai và chạy các ứng dụng bằng các container. Việc đóng gói thành container này giúp cho việc triển khai các ứng dụng trở nên dễ dàng hơn.
- **Docker** cũng là tên công ty gây dựng nên nó ( tên cũ là **dotCloud** ) . Trước đây công ty **dotCloud** chuyên cung cấp các dịch vụ điện toán đám mây . Hiện đã tập trung vào lĩnh vực container .
- GitHub : https://github.com/docker
- Trang chủ : https://www.docker.com/
- Công nghệ container ngày càng phổ biến bởi:
    - Linh hoạt: Có thể đóng gói từ ứng dụng đơn giản đến phức tạp
    - Nhỏ gọn: Các container tận dụng, sử dụng chung tài nguyên; kernel của host. Có thể chạy ở mọi nơi, mọi nền tảng.
    - Khả năng thay đổi linh hoạt: Cập nhật và nâng cấp nhanh chóng.
    - Khả năng mở rộng: Dễ dàng tăng và phân tán tự động các container
    - Phân tầng dịch vụ: Mỗi dịch vụ khi deploy sẽ được phân tầng, nằm trên các dịch vụ đang có sẵn. Như vậy sẽ không làm ảnh hưởng tới dịch vụ đang chạy.
- Một số hiểu lầm :
    - **Docker** không phải công cụ quản lý thiết lập hay thiết lập tự động ( **Puppet**, **Chef**,...)
    - **Docker** không phải giải pháp ảo hóa phần cứng ( **VMware**, **KVM**,...)
    - **Docker** không phải một nền tảng điện toán đám mây ( **OpenStack**, **CloudStack**,...)
    - **Docker** không phải là một development framework ( **Capistrano**, **Fabric**,...)
    - **Docker** không phải một công cụ quản lý workload ( **Mesos**, **Fleet**,...)
    - **Docker** không phải là một môi trường phát triển ( **Vagrant**,...)
## **2) Một số các thuật ngữ sử dụng trong Docker**
### **2.1) Image**
- Là một khuôn mẫu, lớp chứa các file cần thiết để tạo thành một **container** . ( tương tự như ***class*** trong OOP )
- Chứa những tài nguyên có sẵn .
- Không được tiếp cận vào CPU, memory, storage ,...
### **2.2) Container**
- ( tương tự như ***object*** trong OOP )
- Tồn tại trên host với 1 IP
- Được deploy, chạy và xóa bỏ thông qua remote client .
### **2.3) Docker Engine**
- Có nhiệm vụ tạo và chạy các **container** .
- Chạy lệnh trong chế độ daemon .
- Hệ thống sẽ trở thành một máy chủ **Docker**, trên đó **container** được deploy, chạy và xóa bỏ thông qua remote client .
    <p align=center><img src=https://i.imgur.com/HxSNXBV.png width=60%></p>
## **3) Các thành phần của Docker**
<img src=https://i.imgur.com/akXmmBU.png>

### **Docker Daemon** 
- Là tiến trình chạy ngầm quản lý **container**
### **Docker Client**
- Kiểm soát hầu hết các workflow của **Docker**
- Giao tiếp với các máy chủ **Docker** thông qua daemon .

    
### **Docker Hub (Registry)**
- Chứa các component Docker
- Cho phép lưu, sử dụng, tìm kiếm images
    <p align=center><img src=https://i.imgur.com/LXnaGgt.png></p>

## **4) Ưu/nhược điểm của Docker**
### **4.1) Ưu điểm**
- Sử dụng ít tài nguyên: Thay vì phải ảo hóa toàn bộ hệ điều hành thì chỉ cần build và chạy các container độc lập sử dụng chung kernel duy nhất.
- Tính đóng gói và di động: Tất cả các gói dependencies cần thiết đều được đóng gói vừa đủ trong container. Và sau đó có thể mang đi triển khai trên các server khác.
- Cô lập tài nguyên: server bố không biết ở trong container chạy gì và container cũng không cần biết bố nó là CentOs hay Ubuntu . Các container độc lập với nhau và có thể giao tiếp với nhau bằng một interface
- Hỗ trợ phát triển và quản lý ứng dụng nhanh: Đối với Dev, sử dụng docker giúp họ giảm thiểu thời gian setup môi trường, đóng gói được các môi trường giống nhau từ Dev - Staging - Production 
- Mã nguồn mở: Cộng đồng support lớn, các tính năng mới được release liên tục.
### **4.2) Nhược điểm**
- Docker base trên Linux 64bit và các tính năng cgroup, namespaces. Vì thế Linux 32bit hoặc môi trường Window không thể chạy được docker (đối với phiên bản CE).
- Sử dụng container tức là bạn sử dụng chung kernel của hệ điều hành. Trong trường hợp bạn download image có sẵn và trong đó có một số công cụ có thể kiểm soát được kernel thì server của bạn có thể bị mất kiểm soát hoàn toàn.
- Các tiến trình chạy container một khi bị stop thì sẽ mất hoàn toàn dữ liệu nếu không được mount hoặc backup. Điều này có thể sẽ gây ra một số bất tiện…
## **5) So sánh giữa VM và Container**
- Container chạy trực tiếp trên môi trường máy chủ như một tiến trình và chia sẻ phần kernel bên dưới dùng chung với máy chủ chứa nó .
- VM tạo ra một môi trường giả lập hoàn toàn tách biệt như 1 máy hoàn chỉnh thông qua việc phân bổ tài nguyên của máy chủ, do đó sẽ tốn tài nguyên nhiều hơn cho hệ điều hành của máy ảo

    <img src=https://i.imgur.com/A1WNQRz.png>

## **6) Các phiên bản của Docker**
- Có hai phiên bản chính của Docker là Docker EE, và Docker CE.
    - Docker EE (Docker Enterprise Edition) :
        - Docker EE có 3 versions chính là Basic, Standard, Advanced. Bản Basic bao gồm Docker platform, hỗ trợ support và certification. Bản Standard và Advanced thêm các tính năng như container management (Docker Datacenter) và Docker Security Scanning.
        - Docker EE được support bởi Alibaba, Canonical, HPE, IBM, Microsoft…
        - Docker cũng cung cấp một certification để giúp các doanh nghiệp đảm bảo các sản phẩm của họ được hoạt động với Docker EE.
    - Docker CE (Docker Community Edition)
        - Docker CE, đúng như tên gọi, nó là một phiên bản Docker do cộng đồng support và phát triển, hoàn toàn miễn phí.
        - Có hai phiên bản của Docker CE là Edge và Stable. Bản Edge sẽ được release hàng tháng với các tính năng mới nhất, còn Stable sẽ release theo quý.