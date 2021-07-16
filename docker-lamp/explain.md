```
# version docker compose
version: "3"

# network type bridge, name is my-network
networks: 
    my-network:
        driver: bridge


services: 
    # create container my-php from image php:latest connect to network my-network
    my-php:
        container_name: php-product
        image: 'php:latest'
        hostname: php
        restart: always
        networks: 
            - my-network
    

    # create container my-httpd from image httpd:latest connect to network my-network
    my-httpd:
        container_name: c-httpd01
        image: 'httpd:latest'
        hostname: httpd
        restart: always
        networks: 
            - my-network
        ports:
            - "9999:80"

    # create container my-sql from image mysql:latest connect to network my-network, config environment variable
    my-mysql:
        container_name: mysql-product
        image: "mysql:latest"
        hostname: mysql
        restart: always
        networks: 
            - my-network
        environment: 
            - MYSQL_ROOT_PASSWORD=vccloud123
            - MYSQL_DATABASE=db_site
            - MYSQL_USER=sites
            - MYSQL_PASSWORD=vccloud123
```

- Phần đầu là khai báo phiên bản docker composer `(3)`.
- Phần tiếp theo là khai báo các mạng, ở đây tạo một mạng tên là `my-network`, kiểu mạng là `bridge`.
- Phần tiếp theo là tạo các services. ở đây là tạo 3 services là 3 containers `php, httpd, mysql`.
- Start `docker-compose start`.
- Stop `docker-compose stop`.
- Kết thúc các service đang chạy và xóa hoàn toàn `container`:
  + `docker compose down`.
- Theo dõi Logs các services
  + `docker-compose logs [services]`.

__Docs__
- https://viblo.asia/p/thuc-hanh-docker-docker-compose-63vKj92652R