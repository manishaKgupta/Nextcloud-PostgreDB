## Nextcloud with Postgres database
This project depicts an idea of how to use docker-compose for managing Nextcloud tool.
Nextcloud is a free open-source client-server software for creating and hosting file 
hosting services.A database is required to manage and deploy the data for further 
processing for which Postgres database is used.More details on how to do the setup is
mentioned below.

Project structure:
```
.
├── docker-compose.yaml
└── README.md
```

[_docker-compose.yaml_](docker-compose.yaml)
```
version: '3.7'
services:
  nc:
    image: nextcloud:apache
    environment:
      - POSTGRES_HOST=db
      - POSTGRES_PASSWORD=nextcloud
      - POSTGRES_DB=nextcloud
      - POSTGRES_USER=nextcloud
    ports:
      - 80:80
    restart: always
    volumes:
      - nc_data:/var/www/html
  db:
    image: postgres:alpine
    environment:
      - POSTGRES_PASSWORD=nextcloud
      - POSTGRES_DB=nextcloud
      - POSTGRES_USER=nextcloud
    restart: always
    volumes:
      - db_data:/var/lib/postgresql/data
volumes:
  db_data:
  nc_data:

```

When deploying this setup, docker-compose maps the nextcloud container port 80 to
port 80 of the host as specified in the compose file.

## Deploy with docker-compose

```
$ docker$ compose up -d

```


## Expected result

Check containers are running and the port mapping:
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
9884a9cc0144        postgres:alpine     "docker-entrypoint.s…"   12 minutes ago      Up 12 minutes       5432/tcp             nextcloud-postgres_db_1
bae385bee48b        nextcloud:apache    "/entrypoint.sh apac…"   12 minutes ago      Up 12 minutes       0.0.0.0:80->80/tcp   nextcloud-postgres_nc_1
```

Navigate to `http://localhost:80` in your web browser to access the installed
Nextcloud service.

Stop and remove the containers

```
$ docker-compose down
```
