# Explain 

```
version: '3'

services: 
    web:
        image: nginx:latest
        ports:
            - "8000:80"
        volumes:
            - ./code:/code
            - ./configs/site.conf:/etc/nginx/conf.d/default.conf
            - ./scripts:/scripts
        links:
            - php
        entrypoint: ./scripts/start_services.sh
        restart: always

    php:
        image: php:7-fpm
        volumes:
            - ./code:/code
        restart: always
        expose: 
            - 9000
```