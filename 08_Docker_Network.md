# Networking trong Docker
## **1) Container Networking Model (CNM)**
- Dưới đây là hình ảnh mô tả kiến trúc Network của Container hay còn gọi là Container Networking Model (CNM) :
    
    <p align=center><img src=https://i.imgur.com/QCxQZKD.png width=60%></p>

- Đây là cấu trúc high-level trong **CNM**. Theo đó, ta có:
    - **Sandbox** - Chứa các cấu hình của ngăn xếp mạng container. Bao gồm quản lý network interface, route table và các thiết lập DNS. Một Sandbox có thể được coi là một namespace network và có thể chứa nhiều endpoit từ nhiều mạng.
    - **Endpoint** - Là điểm kết nối một Sandbox tới một mạng.
    - **Network** - CNM không chỉ định một mạng tuân theo mô hình OSI. Việc triển khai mạng có thể là VLAN, Bridge, ... Các endpoint không kết nối với mạng thì không có kết nối trên mạng.
    - **CNM** cung cấp 2 interface có thể được sử dụng cho việc liên lạc, điều khiển, ... container trong mạng:
        - ***Network Drivers*** - Cung cấp, thực hiện thực tế việc tạo ra một mạng hoạt động. Được sử dụng với các drivers khác và thay đổi một cách dễ dàng đối với các trường hợp cụ thể. Nhiều network driver có thể được sử dụng trong Docker nhưng mỗi một network chỉ là một khởi tạo từ một network driver duy nhất. Theo đó mà ta có 2 loại chính của CNM network drivers như sau:
            - ***Native Network Drivers*** - Là một phần của Docker Engine và được cung cấp bới Docker. Có nhiều drivers để dễ dàng lựa chọn cho khả năng của mạng như overlay networks hay local bridges.
            - ***Remote Network Drivers*** - Là các network drivers được tạo ra bởi cộng đồng và các nhà cung cấp. Được sử dụng để tích hợp vào các phần mềm hoặc phần cứng đang hoạt động.
        - **IPAM Drivers**: Drivers quản lý các địa chỉ IP cung cấp mặc định cho các mạng con hoặc địa chỉ IP cho các mạng và endpoint nếu chúng không được chỉ định. Địa chỉ IP cũng có thể gán thủ công qua mạng, container, ...
- Luồng giao tiếp giữa docker engine - libnetwork - driver :
    
    <p align=center><img src=https://i.imgur.com/iJu43IX.png width=50%></p>

- **Docker Native Network Drivers** - là một phần của Docker Engine và không yêu cầu cần phải có nhiều modules. Được gọi và sử dụng thông qua các câu lệnh `docker network`. Dưới đây là native network hiện có:

    | Driver | Mô tả |
    |--------|-------|
    | Host | Với host driver, một container sẽ sử dụng ngăn xếp mạng của host. Không có sự phân biệt giữa namespace, tất cả các interface trên host có thể được sử dụng trực tiếp bởi container |
    | Bridge | Bridge driver tạo ra Linux bridges trên host và được quản lý bởi Docker. Mặc định, containers trên một bridge có thể liên lạc với nhau. Việc truy cập từ bên ngoài tới các container có thể được cấu hình thông qua bridge driver. |
    | Overlay | Overlay driver tạo ra một overlay network hỗ trợ cho các mạng multi-host. Được sử dụng kết hợp với Linux bridges và VXLAN để che đi liên lạc giữa các container qua cơ sở mạng hạ tầng vật lý. |
    | MACVLAN | MACVLAN driver sử dụng chế độ MACVLAN bridge để thiết lập kết nối giữa container interface và host interface (hoặc sub-interface). Nó có thể được sử dụng để cung cấp địa chỉ IP cho các container và định tuyến trên mạng vật lý. Ngoài ra VLANs có thể được trunked đến MACVLAN driver |
    | None | None driver cho một ngăn xếp mạng riêng và namespace nhưng không cấu hình interfaces bên trong container. Nếu không có cấu hình bổ sung, container hoàn toàn bị cô lập khỏi mạng của host |