# Docker images
## Layered architecture
- `docker build . -f Dockerfile -t khanhnt/test`

    ```
    # Dockerfile
    FROM ubuntu
    RUN apt-get update && apt-get -y install python
    RUN pip install flask flask-mysql
    COPY . /opt/source-code
    ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
    ```
    + Layer 1: Base ubuntu layer
    + Layer 2: Change in apt packages
    + Layer 3: Change in pip packages
    + Layer 4: Source code
    + Layer 5: Update Entrypoint with "flask" command
- `docker build -f Dockerfile-Lite . -t webapp-color:lite`
    ```
    FROM python:3.6
    RUN pip install flask
    COPY . /opt/
    EXPOSE 8080
    WORKDIR /opt
    ENTRYPOINT ["python", "app.py"]
    ```

## Environment Variables
- `docker inspect container_id | grep APP_COLOR`
- `docker run -p port_host:port_container --name container_name -e APP_COLOR=blue -d image/tag`
- `docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 -d mysql`

## CMD vs ENTRYPOINT
- `CMD` được thực hiện khi container khởi chạy -> Có thể ghi đè dễ dang lên command này.
- `ENTRYPOINT` chỉ chạy khi container chạy -> Không thể ghi đè lên command này. Nếu muốn ghi đè phải chạy container kèm theo option --entrypoint




