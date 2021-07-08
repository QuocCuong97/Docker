# Tổng quan về Docker
## **1) Khái niệm** <img src=https://i.imgur.com/ThGXqSU.png align=right width=35%>
- **Docker** là một nền tảng dành cho các developer và sysadmin để phát triển, triển khai và chạy các ứng dụng bằng các container. Việc đóng gói thành container này giúp cho việc triển khai các ứng dụng trở nên dễ dàng hơn.
- **Docker** cũng là tên công ty gây dựng nên nó ( tên cũ là **dotCloud** ) . Trước đây công ty **dotCloud** chuyên cung cấp các dịch vụ điện toán đám mây . Hiện đã tập trung vào lĩnh vực container .
- GitHub : https://github.com/docker
- Trang chủ : https://www.docker.com/
- Viết bằng ngôn ngữ **Go** 
- Phiên bản ổn định mới nhất: `20.10.7`
- **Docker** giải quyết vấn đề khi mà các doanh nghiệp ngày nay đang chịu áp lực phải chuyển đổi kỹ thuật số nhưng bị hạn chế bởi các ứng dụng và cơ sở hạ tầng hiện tại đồng thời hợp lý hóa danh mục cloud, trung tâm dữ liệu và kiến trúc ứng dụng ngày càng đa dạng.
## **2) Các phiên bản của Docker**
- Có hai phiên bản chính của **Docker** là **Docker EE**, và **Docker CE**.
    - **Docker EE** (***Docker Enterprise Edition***) :
        - **Docker EE** có 3 versions chính là Basic, Standard, Advanced. Bản Basic bao gồm Docker platform, hỗ trợ support và certification. Bản Standard và Advanced thêm các tính năng như container management (Docker Datacenter) và Docker Security Scanning.
        - **Docker EE** được support bởi Alibaba, Canonical, HPE, IBM, Microsoft…
        - **Docker** cũng cung cấp một certification để giúp các doanh nghiệp đảm bảo các sản phẩm của họ được hoạt động với **Docker EE**.
    - **Docker CE** (***Docker Community Edition***)
        - **Docker CE**, đúng như tên gọi, nó là một phiên bản Docker do cộng đồng support và phát triển, hoàn toàn miễn phí.
        - Có hai phiên bản của **Docker CE** là ***Edge*** và ***Stable***. Bản ***Edge*** sẽ được release hàng tháng với các tính năng mới nhất, còn ***Stable*** sẽ release theo quý.
## **3) Chức năng, vai trò của Docker**
- Cho phép phát triển, di chuyển và chạy các ứng dụng dựa vào công nghệ ảo hóa container trong Linux.
- Tự động triển khai các ứng dụng bên trong các container bằng cách cung cấp thêm một lớp trừu tượng và tự động hóa việc ảo hóa "mức hệ điều hành".
- **Docker** có thể sử dụng được trên cả 3 hệ điều hành phổ biến: Windows, Linux và Mac OS.
- Lợi ích của **docker** bao gồm:
    - Nhanh trong việc triển khai, di chuyển, khởi động container
    - Bảo mật
    - Lightweight (tiết kiệm disk & CPU)
    - Mã nguồn mở
    - Hỗ trợ APIs để giao tiếp với container
    - Phù hợp trong môi trường làm việc đòi hòi phải liên tục tích hợp và triển khai các dịch vụ, phát triển cục bộ, các ứng dụng multi-tier.
## **4) Các khái niệm cần biết khi sử dụng docker**
### **4.1) Image**
- **Image** trong **Docker** hay còn gọi là ***Mirror***. Là một template có sẵn (hoặc có thể tự tạo) với các chỉ dẫn dùng để tạo ra các container.
- Được xây dựng từ một loạt các layers. Mỗi layer là một kết quả đại diện cho một lệnh trong **Dockerfile**.
- Lưu trữ dưới dạng read-only template.
### **4.2) Registry**
- **Docker Registry** là nơi lưu trữ các image với hai chế độ là private và public.
- Là nơi cho phép chia sẻ các image template để sử dụng trong quá trình làm việc với **Docker**.
### **4.3) Volume**
- **Volume** trong **Docker** là nơi dùng để chia sẻ dữ liệu cho các container.
- Có thể thực hiện sử dụng **Volume** đối với 2 trường hợp:
    - Chia sẻ giữa container với container.
    - Chia sẻ giữa container và host.
### **4.4) Container**
- **Docker Container** là một thể hiện của **Docker Image** với những thao tác cơ bản để sử dụng qua CLI như `start`, `stop`, `restart` hay `delete`, ...
- **Container Image** là một gói phần mềm thực thi lightweight, độc lập và có thể thực thi được bao gồm mọi thứ cần thiết để chạy được nó: code, runtime, system tools, system libraries, settings. Các ứng dụng có sẵn cho cả Linux và Windows, các container sẽ luôn chạy ổn định bất kể môi trường.

    <p align=center><img src=https://i.imgur.com/LLnvMc9.png width=60%></p>

- **Containers** và **virtual machines** có sự cách ly và phân bổ tài nguyên tương tự, nhưng có chức năng khác vì các container ảo hóa hệ điều hành thay vì phần cứng. Các **container** có tính portable và hiệu quả hơn.

    <p align=center><img src=https://i.imgur.com/fuOmIje.png width=60%></p>

- **Container** là một sự trừu tượng hóa ở lớp ứng dụng và code phụ thuộc vào nhau. Nhiều **container** có thể chạy trên cùng một máy và chia sẻ kernel của hệ điều hành với các **container** khác, mỗi máy đều chạy như các quá trình bị cô lập trong không gian người dùng. Các **container** chiếm ít không gian hơn các máy ảo (**container image** thường có vài trăm thậm chí là vài `MB`), và start gần như ngay lập tức.
- **Máy ảo (VM)** là một sự trừu tượng của phần cứng vật lý chuyển tiếp từ một máy chủ sang nhiều máy chủ. **Hypervisor** cho phép nhiều máy ảo chạy trên một máy duy nhất. Mỗi máy ảo bao gồm một bản sao đầy đủ của một hệ điều hành, một hoặc nhiều ứng dụng, các chương trình và thư viện cần thiết - chiếm hàng chục `GB`. Máy ảo cũng có thể khởi động chậm.
### **4.5) Dockerfile**
- **Docker Image** có thể được tạo ra một cách tự động bằng việc đọc các chỉ dẫn trong **Dockerfile**.
- **Dockerfile** là một dữ liệu văn bản bao gồm các câu lệnh mà người sử dụng có thể gọi qua các dòng lệnh để tạo ra một image.
- Bằng việc sử dụng `docker build` người dùng có thể tạo một tự động xây dựng thực hiện một số lệnh dòng lệnh liên tiếp.
## **5) Các thành phần, kiến trúc trong Docker**