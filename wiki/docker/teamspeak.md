# teamspeak

This is a [Docker](/wiki/docker.md) container for a
[Teamspeak3](/wiki/teamspeak.md) server.
The official container and documentation was made by
[Teamspeak](https://hub.docker.com/_/teamspeak).

## Set-up

Create the file `rebuild.sh`.
Change the settings according to your needs and run `./rebuild.sh` afterwards.

### Environment-variables

Set the following variables with the -e tag.

| Name                | Usage                        | Default |
| ------------------- | ---------------------------- | ------- |
| `TS3SERVER_LICENSE` | accept the license agreement |         |

### Volumes

Set the following volumes with the -v tag.

| Outside mount/volume name | Container mount  | Description                 |
| ------------------------- | ---------------- | --------------------------- |
| `teamspeak3`              | `/var/ts3server` | storage for ts3-server data |

### Ports

Set the following ports with the -p tag.

| Container Port | Recommended outside port | Protocol | Description |
| -------------- | ------------------------ | -------- | ----------- |
| `9987`         | `9987`                   | UDP      | ts3-intern  |
| `10011`        | `10011`                  | TCP      | ts3-intern  |
| `30033`        | `30033`                  | TCP      | ts3-intern  |

### rebuild.sh

```sh
#!/bin/sh
docker stop teamspeak3
docker rm teamspeak3
docker pull teamspeak:latest
docker run --name teamspeak3 \
    --restart unless-stopped \
    -p 9987:9987/udp \
    -p 10011:10011 \
    -p 30033:30033 \
    -v teamspeak3:/var/ts3server \
    -e TS3SERVER_LICENSE=accept \
    -d teamspeak:latest
```
