# matrixdotorg - synapse

This is a [Docker](/wiki/docker.md) container for a synapse server using the
[matrix](../matrix.md) protocol.
The official container and documentation was made by
[matrixdotorg](https://hub.docker.com/matrixdotorg/synapse).
This docker-rebuild is made up by a `docker-compose.yml` file.

## Set-up

Create the files `rebuild.sh` and `docker-compose.yml` at the same place.
Change the settings according to your needs and run `./rebuild.sh` afterwards.

### Environment-variables

Set the following environment-variables in the `environment:` section of the
docker-compose file.

| Name                  | Usage                     | Default                 |
| --------------------- | ------------------------- | ----------------------- |
| `SYNAPSE_CONFIG_DIR`  | config directory          | `/data`                 |
| `SYNAPSE_CONFIG_PATH` | config path               | `/data/homeserver.yaml` |
| `UID`                 | user id for synapse user  | `1000`                  |
| `GID`                 | group id for synapse user | `1000`                  |
| `TZ`                  | specify the timezone      | `Europe/London`         |

### Volumes

Set the following volumes in the `volumes:` section of the docker-compose file.

| Outside mount/volume name | Container mount | Description                       |
| ------------------------- | --------------- | --------------------------------- |
| `synapse`                 | `/data`         | directory for storage and configs |

### Ports

Set the following ports in the `ports:` section.

| Container Port | Recommended outside port | Protocol | Description            |
| -------------- | ------------------------ | -------- | ---------------------- |
| `8008`         | `443`                    | TCP      | matrix homeserver port |

### rebuild.sh

```sh
#!/bin/sh
docker-compose down
docker pull matrixdotorg/synapse:latest
docker-compose up -d
```

### docker-compose.yml

```yml
services:
  synapse:
    image: "matrixdotorg/synapse:latest"
    restart: "unless-stopped"
    environment:
      SYNAPSE_CONFIG_DIR: "/data"
      SYNAPSE_CONFIG_PATH: "/data/homeserver.yaml"
      UID: "1000"
      GID: "1000"
      TZ: "Europe/London"
    volumes:
      - synapse:/data

volumes:
  synapse:
    driver: local
```
