# Docker Run
- `Run - PORT mapping`
    + `docker run -p 80:5000 kodecloud/simple-webapp`
        + 80 là port listen ngoài host.
        + 5000 là port listen bên trong container.
- `Run - Volume mapping`
    + `docker run -v /opt/datadir:/var/lib/mysql mysql`
        + `/opt/datadir`: dir bên ngoài host.
        + `/var/lib/mysql`: dir bên trong container.
- Docker inspect
    + `docker inspect container_name_or_id`
- Docker logs
    + `docker logs container_name_or_id`
- Test
    + `docker run -p 38282:8080 kodekloud/simple-webapp:blue`
