# Docker compose
- Example file
  ```
  version: 2
  services: 
    redis:
        image: redis
        networks:
          - back-end
    db: 
        image: postgres:9.4
        networks: 
          - back-end

    vote:
        image: voting-app
        networks:
          - front-end
          - back-end

    result:
        image: result
        networks:
          - front-end
          - back-end

  networks:
      front-end:
      back-end:
  ```

- `docker run -d --name=clickcounter --link redis:redis -p 8085:5000 kodekloud/click-counter`

```
version: '2'
services:
  redis:
    image: redis:alpine

  clickcounter:
    image: kodekloud/click-counter
    ports:
      - 8085:5000
```

## Docker Registry
- `image`
  + `image: docker.io/nginx/nginx`
    + `docker.io`: Registry
    + `nginx`: user account
    + `nginx`: image repository

- `docker run --name my-registry --restart=always -p 5000:5000 registry:2`

__Docs__
- https://github.com/dockersamples/example-voting-app
- https://docs.docker.com/engine/reference/commandline/compose/

