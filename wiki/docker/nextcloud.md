# NextCloud

This is a [Docker](/wiki/docker.md) container for [NextCloud](../nextcloud.md).
The official container and documentation was made by
[NextCloud](https://hub.docker.com/_/nextcloud).
This docker-rebuild is made up by a `docker-compose.yml` file.
The services in this files are explained seperately.
The main Docker container needs a database in form of a [MariaDB](./mariadb.md)
Docker container.

## Set-up

Create the files `rebuild.sh` and `docker-compose.yml` at the same place.
Change the settings according to your needs and run `./rebuild.sh` afterwards.
It is recommended to use the versionnumber instead of `:latest` for both the MariaDB and the
NextCloud image like the following example.

```
nextcloud:22
mariadb:9
```

Make sure to substitute all the occurences of the `:latest` tag accordingly.
Updating has to be done manually so nothing breaks.

### Volumes

Set the following volumes in the `volumes:` section of the docker-compose file.

| Outside mount/volume name | Container mount | Description                              |
| ------------------------- | --------------- | ---------------------------------------- |
| `nextcloud`               | `/var/www/html` | storage for nextcloud plugins and config |

### Ports

Set the following ports in the `ports:` section.

| Container Port | Recommended outside port | Protocol | Description |
| -------------- | ------------------------ | -------- | ----------- |
| `80`           | `80`                     | TCP      | WebUI       |

### rebuild.sh

```sh
#!/bin/sh
docker-compose down
docker pull nextcloud:latest
docker pull mariadb:latest
docker-compose up -d
```

### docker-compose.yml

```yml
version: '2'

volumes:
  nextcloud:
  db:

services:
  db:
    image: mariadb:latest
    restart: always
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    volumes:
      - db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=<mysql root password>
      - MYSQL_PASSWORD=<mysql password>
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud

  app:
    image: nextcloud:latest
    restart: always
    ports:
      - 8080:80
    links:
      - db
    volumes:
      - nextcloud:/var/www/html
    environment:
      - MYSQL_PASSWORD=<mysql password>
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_HOST=db

volumes:
    db:
        driver: local
    nextcloud:
        driver: local
```
