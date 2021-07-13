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
    | **Host** | Với host driver, một container sẽ sử dụng ngăn xếp mạng của host. Không có sự phân biệt giữa namespace, tất cả các interface trên host có thể được sử dụng trực tiếp bởi container |
    | **Bridge** | Bridge driver tạo ra Linux bridges trên host và được quản lý bởi Docker. Mặc định, containers trên một bridge có thể liên lạc với nhau. Việc truy cập từ bên ngoài tới các container có thể được cấu hình thông qua bridge driver. |
    | **Overlay** | Overlay driver tạo ra một overlay network hỗ trợ cho các mạng multi-host. Được sử dụng kết hợp với Linux bridges và VXLAN để che đi liên lạc giữa các container qua cơ sở mạng hạ tầng vật lý. |
    | **MACVLAN** | MACVLAN driver sử dụng chế độ MACVLAN bridge để thiết lập kết nối giữa container interface và host interface (hoặc sub-interface). Nó có thể được sử dụng để cung cấp địa chỉ IP cho các container và định tuyến trên mạng vật lý. Ngoài ra VLANs có thể được trunked đến MACVLAN driver |
    | **None** | None driver cho một ngăn xếp mạng riêng và namespace nhưng không cấu hình interfaces bên trong container. Nếu không có cấu hình bổ sung, container hoàn toàn bị cô lập khỏi mạng của host |
- Đối với **native driver network** trong container, Ta có:
    - Chiều outbound khi các container sử dụng trong container :

        <p align=center><img src=https://i.imgur.com/LEAQ0ZV.png width=60%></p>

    - Chiều inbound khi các container sử dụng trong container :

        <p align=center><img src=https://i.imgur.com/LplNixt.png width=60%></p>

    - Container kết nối với network thông qua `docker0` interface:

        <p align=center><img src=https://i.imgur.com/Yr00IN4.png width=60%></p>

        - Khi chúng ta cài đặt Docker, những thiết lập sau sẽ được thực hiện:
            - Virtual bridge `docker0` sẽ được tạo ra
            - Docker tìm một subnet chưa được dùng trên host và gán một địa chỉ cho `docker0`
            - Sau đó, khi chúng ta khởi động một container (với bridge network), một `veth` (Virtual Ethernet) sẽ được tạo ra nối 1 đầu với docker0 và một đầu sẽ được nối với interface `eth0` trên container.
## **2) Chi tiết về các loại card mạng**
### **2.1) Default network**
- Mặc định khi cài đặt xong, Docker sẽ tạo ra 3 card mạng mặc định là:
    - `bridge`
    - `host only`
    - `none`
- Tương ứng với các nền tảo ảo hóa khác, ta có các chế độ card mạng của docker so với các nền tảng đấy là:

    | General Virtualization Term | Docker Network Driver |
    |-----------------------------|-----------------------|
    | NAT Network | bridge |
    | Bridged | macvlan, ipvlan (experimental since Docker 1.11) |
    | Private / Host-only | bridge |
    | Overlay Network / VXLAN | overlay |
- Để xem chi tiết, ta có thể dùng lệnh :
    ```
    docker network ls
    ```
    - Output :
        ```
        root@adk:/# docker network ls
        NETWORK ID          NAME                DRIVER              SCOPE
        1d8aa8d520a2        bridge              bridge              local
        a8ddedeecca8        host                host                local
        ad1c5f949ef2        none                null                local
        ```
- Mặc định khi tạo container mà ta không chỉ định dùng network nào, thì docker sẽ dùng dải `bridge`.
#### **2.1.1) None network**
- Các container thiết lập network này sẽ không được cấu hình mạng.
#### **2.1.2) Bridge**
- Docker sẽ tạo ra một switch ảo. Khi container được tạo ra, interface của container sẽ gắn vào switch ảo này và kết nối với interface của host.
    ```
    root@adk:~# docker network inspect bridge
    [
        {
            "Name": "bridge",
            "Id": "1d8aa8d520a2775b5f02279e2ff057e2d781a67e116e603d367edab58211a5d9",
            "Created": "2017-03-14T09:35:39.561628223+07:00",
            "Scope": "local",
            "Driver": "bridge",
            "EnableIPv6": false,
            "IPAM": {
                "Driver": "default",
                "Options": null,
                "Config": [
                    {
                        "Subnet": "172.17.0.0/16",
                        "Gateway": "172.17.0.1"
                    }
                ]
            },
            "Internal": false,
            "Attachable": false,
            "Containers": {},
            "Options": {
                "com.docker.network.bridge.default_bridge": "true",
                "com.docker.network.bridge.enable_icc": "true",
                "com.docker.network.bridge.enable_ip_masquerade": "true",
                "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
                "com.docker.network.bridge.name": "docker0",
                "com.docker.network.driver.mtu": "1500"
            },
            "Labels": {}
        }
    ]
    ```
#### **2.1.3) Hosts**
- Containers sẽ dùng mạng trực tiếp của máy host. Network configuration bên trong container đồng nhất với host.
### **2.2) User-defined networks**
- Ngoài việc sử dụng các network mặc định do docker cung cấp. Ta có thể tự định nghĩa ra các dải network phù hợp với công việc của mình.
- Để tạo network, ta dùng lệnh :
    ```
    docker network create --driver bridge --subnet 192.168.1.0/24 bridgexxx
    ```
    - Trong đó :
        - `--driver bridge` : chỉ định dải mạng mới được tạo ra sẽ thuộc kiểu nào: `bridge`, `host`, hay `none`.
        - `--subnet` : chỉ định địa địa chỉ mạng
        - `bridgexxx` : tên của dải mạng mới
- Khi chạy container chỉ định sử dụng 1 dải mạng đặc biệt, ta dùng lệnh :
    ```
    docker run --network=bridgexxx -itd --name=container3 busybox
    ```
    - Trong đó:
        - `--network=bridgexxx` : chỉ định ra dải mạng `bridgexxx` sẽ kết nối với container.
- Container mà bạn chạy trên network này đều phải thuộc về cùng một Docker host. Mỗi container trong network có thể communicate với các containers khác trong cùng network.
### **2.3) An overlay network with Docker Engine swarm mode**
- Overlay network là mạng có thể kết nối nhiều container trên các Docker Engine lại với nhau, trong môi trường cluster.
- Swarm tạo ra overlay network chỉ available với các nodes bên trong swarm. Khi bạn tạo ra một service sử dụng overlay network, manager node sẽ tự động kế thừa overlay network tới các nodes chạy các service tasks.
- Ví dụ sau sẽ hướng dẫn cách tạo ra một network và sử dụng nó cho một service từ một manager node bên trong swarm:
    ```
    # Create an overlay network `my-multi-host-network`.
    $ docker network create \
        --driver overlay \
        --subnet 10.0.9.0/24 \
        my-multi-host-network

    400g6bwzd68jizzdx5pgyoe95

    # Create an nginx service and extend the my-multi-host-network to nodes where
    # the service's tasks run.
    $ $ docker service create --replicas 2 --network my-multi-host-network --name my-web nginx

    716thylsndqma81j6kkkb5aus
    ```
### **2.4) Giao tiếp giữa các container với nhau**
- Trên cùng một host, các container chỉ cần dùng bridge network để nói chuyện được với nhau. Tuy nhiên, các container được cấp ip động nên nó có thể thay đổi, dẫn đến nhiều khó khăn. Vì vậy, thay vì dùng địa chỉ ip, ta có thể dùng name của các container để "liên lạc" giữa các container với nhau.
- Trong trường hợp sử dụng default bridge network thì ta khai báo thêm lệnh `--link=name_container`.
- Trong trường hợp sử dụng `user-defined bridge network` thì ta không cần phải link nữa.
#### **2.4.1) Trường hợp sử dụng default bridge network để kết nối các container**
- Giả sử ta có mô hình: `web - db`. Container `web` phải link được với container `db`.
    ```
    docker run -itd --name=db -e MYSQL_ROOT_PASSWORD=pass mysql:latest
    docker run -itd --name=web --link=db nginx:latest
    ```
    - Kiểm tra:
        ```
        docker exec -it web sh
        # ping redis
        PING redis (172.17.0.3): 56 data bytes
        64 bytes from 172.17.0.3: icmp_seq=0 ttl=64 time=0.245 ms
        ...
        # ping db
        PING db (172.17.0.2): 56 data bytes
        64 bytes from 172.17.0.2: icmp_seq=0 ttl=64 time=0.126 ms
        ...
        ```
#### **2.4.2) Trường hợp sử dụng user-defined bridge network để kết nối các container**
- Bạn không cần thực hiện thao tác link qua link lại giữa các container nữa.
    ```
    docker network create my-net
    docker network ls
    ```
    ```
    NETWORK ID          NAME                DRIVER
    716f591e185a        bridge              bridge              
    4b0041303d6d        host                host                
    7239bb9e0255        my-net              bridge              
    016cf6ec1791        none                null
    ```
    ```
    docker run -itd --name=web1 --net my-net nginx:latest
    docker run -itd --name=db1 --net my-net -e MYSQL_ROOT_PASSWORD=pass mysql:latest
    ```
    ```
    docker exec -it web1 sh
    # ping db1
    PING db1 (172.18.0.4): 56 data bytes
    64 bytes from 172.18.0.4: icmp_seq=0 ttl=64 time=0.161 ms
    # ping redis1
    PING redis1 (172.18.0.3): 56 data bytes
    64 bytes from 172.18.0.3: icmp_seq=0 ttl=64 time=0.168 ms
    ```
## **3) DNS server**
### **3.1) Default bridge network**
- Khi container được chạy với network default bridge thì container sẽ sao chép nội dung file `/etc/resolv.conf` của host vào trong container. Do đó, dns-server được cấu hình trên máy host như thế nào, thì trên container tương tự như vậy.
- Kiểm tra trên container:
    ```
    root@292a65a743ac:/# mount | grep /etc
    /dev/disk/by-uuid/d9ed83b8-98cb-428a-9ad0-e7bf8ae36117 on /etc/resolv.conf type ext4 (rw,relatime,errors=remount-ro,data=ordered)
    /dev/disk/by-uuid/d9ed83b8-98cb-428a-9ad0-e7bf8ae36117 on /etc/hostname type ext4 (rw,relatime,errors=remount-ro,data=ordered)
    /dev/disk/by-uuid/d9ed83b8-98cb-428a-9ad0-e7bf8ae36117 on /etc/hosts type ext4 (rw,relatime,errors=remount-ro,data=ordered)
    ```
    => Ta thấy các file trên container được mount từ các file trên máy host.
- Kiểm tra nội dung file `resolv.conf`:
    ```
    root@292a65a743ac:/# cat /etc/resolv.conf 
    # Dynamic resolv.conf(5) file for glibc resolver(3) generated by resolvconf(8)
    #     DO NOT EDIT THIS FILE BY HAND -- YOUR CHANGES WILL BE OVERWRITTEN
    nameserver 8.8.8.8
    nameserver 8.8.4.4
    ```
    => Tương tự trên máy host.
### **3.2) User-defined networks**
- Docker sẽ sử dụng built-in dns riêng cho các container cùng 1 network.
- Nội dung file `/etc/resolv.conf` :
    ```
    root@07c8c10a7a81:/# cat /etc/resolv.conf 
    nameserver 127.0.0.11
    options ndots:0
    root@07c8c10a7a81:/# 
    ```
- Docker chưa cung cấp công cụ để xem thông tin các record của built-in dns này.
### **3.3) Chỉ định sử dụng dns-server riêng**
- Dùng cờ `--dns`: để khai báo dns-server sẽ sử dụng trong container:
    ```
    root@docker2:/opt# docker run -it --name hcm --dns=10.10.10.1 ubuntu /bin/bash
    ```
## **4) MacVlan**
- Macvlan cho phép cấu hình sub-interfaces (hay còn gọi là slave devices) trên một Ethernet interface vật lý (còn gọi là upper device), mỗi sub-interfaces này có địa chỉ MAC riêng và do đó có địa chỉ IP riêng. Các ứng dụng, VM và các containers có thể kết nối với một sub-interface nhất định để kết nối trực tiếp với mạng vật lý, sử dụng địa chỉ MAC và địa chỉ IP riêng của chúng.
## **5) IPVlan**
https://github.com/hocchudong/ghichep-docker/blob/master/docs/docker-coban/docker-network.md