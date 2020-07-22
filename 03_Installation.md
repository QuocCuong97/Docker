# Cài đặt Docker
## **1) Cài đặt trên Linux**
### **1.1) Cài đặt trên Ubuntu (`x64`)**
- **B1 :** Xóa các phiên bản Docker đã cài đặt trước đó :
    ```
    $ sudo apt-get remove docker docker-engine docker.io containerd runc
    ```
- **B2 :** Update các repo hiện có :
    ```
    $ sudo apt-get update -y
    ```
- **B3 :** Cài đặt các package cần thiết :
    ```
    $ sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
    ```
- **B4 :** Thêm GPG Key :
    ```
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    $ sudo apt-key fingerprint 0EBFCD88
    ```
- **B5 :** Thêm repository:
    ```
    $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    $ sudo apt-get update -y
    ```
- **B6 :** Cài đặt **Docker Engine** :
    ```
    $ sudo apt-get install -y docker-ce docker-ce-cli containerd.io
    ```
- **B7 :** Kiểm tra xem Docker đã được cài chưa bằng image `Hello World` :
    ```
    $ sudo docker run hello-world
    ```
    => Output:
    ```sh
    cuongnq@ubuntu1804:~$ sudo docker run hello-world
    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    0e03bdcc26d7: Pull complete
    Digest: sha256:49a1c8800c94df04e9658809b006fd8a686cab8028d33cfba2cc049724254202
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

    To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
        (amd64)
    3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.

    To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/

    For more examples and ideas, visit:
    https://docs.docker.com/get-started/
    ```
### **1.2) Cài đặt trên CentOS 7**
- **B1 :** Xóa các phiên bản Docker đã cài đặt trước đó :
    ```
    # yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
    ```
- **B2 :** Cài đặt repo :
    ```
    # yum install -y yum-utils
    # yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    ```
- **B3 :** Cài đặt **Docker Engine** :
    ```
    # yum install -y docker-ce docker-ce-cli containerd.io
    ```
- **B4 :** Khởi động **Docker** :
    ```
    # systemctl start docker
    ```
- **B5 :** Kiểm tra xem Docker đã được cài chưa bằng image `Hello World` :
    ```
    # docker run hello-world
    ```
    => Output :
    ```
    [root@centos7-01 ~]# docker run hello-world
    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    0e03bdcc26d7: Pull complete
    Digest: sha256:49a1c8800c94df04e9658809b006fd8a686cab8028d33cfba2cc049724254202
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

    To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
        (amd64)
    3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.

    To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/

    For more examples and ideas, visit:
    https://docs.docker.com/get-started/
    ```
## **2) Cài đặt trên Windows**
- **B1 :** Download **Docker CE** tại trang web : https://hub.docker.com/editions/community/docker-ce-desktop-windows/

    <img src=https://i.imgur.com/nIUTEAL.png>

- **B2 :** Click vào file `Docker Desktop Installer.exe` vừa tải về để cài đặt :
    
    <img src=https://i.imgur.com/MWKqB5W.png>

- **B3 :** Chọn `OK` :

    <img src=https://i.imgur.com/IDZOJco.png>

- **B4 :** Quá trình cài đặt diễn ra :

    <img src=https://i.imgur.com/ccO0iFX.png>

- **B5 :** Quá trình cài đặt hoàn tất. Chọn `Close and Restart` để kết thúc và khởi động lại máy :

    <img src=https://i.imgur.com/ROgLe4W.png>

> **Lưu ý** : Quá trình cài đặt có thể bị lỗi do không thể update **WSL2**. Tải về bản update tại đây : https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel

- **B6 :** Sau khi cài đặt thành công, logo **Docker** sẽ xuất hiện trên thanh **Taskbar** :

    <img src=https://i.imgur.com/bGvze8C.png>

    - Dashboard của **Docker Desktop** :

        <img src=https://i.imgur.com/eTIZalD.png>