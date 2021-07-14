# Build docker file 

```
FROM ubuntu

MAINTAINER khanhnt

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update

RUN apt-get install apache2 -y

RUN apt-get install apache2-utils -y

RUN apt-get clean

COPY index.html /var/www/html

CMD ["apache2ctl", "-D", "FOREGROUND"]

EXPOSE 80
```
- `FROM ubuntu`: base image để cài đặt apache2 trên đó.
- `ENV DEBIAN_FRONTEND=noninteractive`: thiết lập môi trường không tương tác. 

```
RUN apt-get update

RUN apt-get install apache2 -y

RUN apt-get install apache2-utils -y

RUN apt-get clean

# install apache2
```


- `COPY index.html /var/www/html`: copy file, thư mục `src` và thêm chúng vào `filesytem` container `dest`.
- `EXPOSE 80`: hiển thị port 80 của Apache cho container.
   + `container` sẽ lắng nghe các cổng được chỉ định khi chạy. 
   + Cách này không có chức năng nat port từ máy host vào container.
   + Muốn nat port phải sử dụng `-p (nat port)` hoặc `-P (nat tất cả các port)` trong quá trình khởi tạo container.

- Build image thông qua folder `web-server-docker` với tên image là `webserver` tag là `v1`
   + `docker build web-server-docker/ -t webserver:v1`

```
khanhnt@ubuntu:~$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
webserver     v1        502c6f14464c   23 minutes ago   215MB
ubuntu        latest    c29284518f49   11 hours ago     72.8MB
hello-world   latest    d1165f221234   4 months ago     13.3kB
```

- Tạo container bằng image `webserver:v1`
  + `-p:` map port 80 apache container với port của host.
  + `-d:` chạy container ở chế độ tách rời, hay nói cách khác chạy ở chế độ backgroud.

```
khanhnt@ubuntu:~$ docker run -d -p 80:80 webserver:v1
```

```
khanhnt@ubuntu:~$ sudo netstat -lnupt 
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:2375          0.0.0.0:*               LISTEN      17947/dockerd       
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      76069/docker-proxy  
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      3091/systemd-resolv 
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1338/sshd: /usr/sbi 
tcp6       0      0 :::80                   :::*                    LISTEN      76077/docker-proxy  
tcp6       0      0 :::22                   :::*                    LISTEN      1338/sshd: /usr/sbi 
udp        0      0 127.0.0.53:53           0.0.0.0:*                           3091/systemd-resolv 
udp        0      0 0.0.0.0:68              0.0.0.0:*                           1341/dhclient       

-> docker proxy
```

__Docs__
- https://www.geeksforgeeks.org/how-to-build-a-web-server-docker-file/
- https://viblo.asia/p/tim-hieu-ve-dockerfile-va-tao-docker-image-V3m5WWag5O7