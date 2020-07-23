# Docker Hub
- **Docker Hub** là một dịch vụ registry trên nền tảng cloud cho phép upload các **Docker image** được xây dựng từ cộng đồng. Người dùng cũng có thể tự upload các **Docker images** tự built lên trên **Docker Hub**
- Trang chủ : https://hub.docker.com/

    <img src=https://i.imgur.com/ufRUyud.png>

- Giao diện sau khi đăng nhập:

    <img src=https://i.imgur.com/pWgoJ4r.png>

### **Cách download một Docker image từ Docker Hub**
- **B1 :** Tìm kiếm **image** tại thanh seacrch, chọn **image** muốn download :
    
    <img src=https://i.imgur.com/QTQjq4g.png>

- **B2 :** Copy lệnh download **image** :

    <img src=https://i.imgur.com/hKycQfE.png>

- **B3 :** Thực hiện lệnh trong terminal :
    ```
    $ sudo docker pull mariadb
    ```
    => Output :
    ```
    latest: Pulling from library/mariadb
    692c352adcf2: Pull complete 
    97058a342707: Pull complete 
    2821b8e766f4: Pull complete 
    4e643cc37772: Pull complete 
    dfd31a8b718f: Pull complete 
    b03768a32e57: Pull complete 
    2e4ef2ecadfe: Pull complete 
    36b1c98359f8: Pull complete 
    b4296564e05a: Pull complete 
    fed3238308db: Pull complete 
    ffd1a5c85817: Pull complete 
    cc3212c3b29b: Pull complete 
    78034e7ff7d2: Pull complete 
    Digest: sha256:a317e3553e49f718a462f544cfc0ad9f83d705667f73dd9dc774515c338c547a
    Status: Downloaded newer image for mariadb:latest
    docker.io/library/mariadb:latest
    ```
