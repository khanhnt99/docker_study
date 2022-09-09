# Docker engine storage and networking
## 1. Docker engine
- Docker CLI -> REST API -> Docker Daemon
    + `Docker Daemon`: quản lý các object của docker `(image, container,volume, network, storage)`.
    + `REST API`: cung cấp các API để có thể tương tác với docker daemon.
    + `Docker CLI`: sử dụng REST API để tương tác với docker daemon, công cụ giúp user thực hiện tạo, sửa, xóa, thao tác với container và image.
- **Containerization**
    + Docker sử dụng namespace để cô lập các tiến trình workspace.
        + Process ID
        + Unix Timesharing
        + Network
        + Mount
        + Inter-process
    + Khi khởi chạy container -> khởi chạy child system -> một process sẽ được khởi chạy -> Process thật chất vẫn chạy trên host (giả sử có id là 5) -> Nhưng nhìn trong không gian của container thì process này có id là 1.
    + Container sẽ nghĩ rằng nó có root tree của chính nó. Có process id là 1.
    + `croups`: container và host cùng sử dụng chung tài nguyên CPU và RAM -> `Cgroups` sinh ra để quản lý tài nguyên của container

## 2. Docker Storage

```
/var/lib/docker
├── buildkit
│   ├── cache.db
│   ├── containerdmeta.db
│   ├── content
│   │   └── ingest
│   ├── executor
│   ├── metadata_v2.db
│   └── snapshots.db
├── containers
├── image
│   └── overlay2
│       ├── distribution
│       ├── imagedb
│       │   ├── content
│       │   │   └── sha256
│       │   └── metadata
│       │       └── sha256
│       ├── layerdb
│       └── repositories.json
├── network
│   └── files
│       └── local-kv.db
├── overlay2
│   └── l
├── plugins
│   ├── storage
│   │   └── ingest
│   └── tmp
├── runtimes
├── swarm
├── tmp
├── trust
└── volumes
    ├── backingFsBlockDev
    └── metadata.db
```

- `Image Layers`: Read-only
- `Container Layers`: Read-Write
- Khi thực hiện câu lệnh `docker volume create volume_name` -> nó sẽ tạo ra một thư mục trong `/var/lib/docker/volumes/volume_name`
- Để mount volume vào container chạy câu lệnh `docker run -v volume_name:/var/lib/mysql mysql` -> volume mounting.
- Khi không tạo volume mà mount thư mục bên trong container vào 1 folder có sẵn `docker run -v /data/mysql:/var/lib/mysql mysql` -> bind mounting.
- Test
    + `docker run --name mysql-db --env MYSQL_ROOT_PASSWORD=db_pass123 mysql`
    + `docker exec mysql-db mysql -pdb_pass123 -e 'use foo; select * from myTable'`
    + `docker run --name mysql-db --env MYSQL_ROOT_PASSWORD=db_pass123 -v /opt/data:/var/lib/mysql mysql`

## 3. Networking
- `docker network ls`
- `docker inspect network bridge`
- `docker run --name alpine-2 --network none alpine`
- `docker network create wp-mysql-network --subnet 182.18.0.1/24 --gateway 182.18.0.1`
- `docker network create wp-mysql-network --subnet 182.18.0.1/24 --gateway 182.18.0.1`
- `docker run -d --name webapp -p 38080:8080 -e DB_Host=mysql-db -e DB_Password=db_pass123 --network=wp-mysql-network kodekloud/simple-webapp-mysql`

