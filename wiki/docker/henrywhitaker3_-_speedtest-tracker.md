# henrywhitaker3 - speedtest-tracker

This is a [Docker](/wiki/docker.md) container for a selfhosted internet speed
tracker.
The official container and documentation was made by
[henrywhitaker3](https://hub.docker.com/r/henrywhitaker3/speedtest-tracker).

## Set-up

Create the file `rebuild.sh`.
Change the settings according to your needs and run `./rebuild.sh` afterwards.

### Environment-variables

Set the following variables with the -e tag.

| Name              | Usage                         | Default |
| ----------------- | ----------------------------- | ------- |
| `OOKLA_EULA_GDPR` | set to `true` to accept EULA  |         |
| `AUTH`            | set to `true` to enable login |         |

### Volumes

Set the following volumes with the -v tag.

| Outside mount/volume name | Container mount | Description        |
| ------------------------- | --------------- | ------------------ |
| `speedtest`               | `/config`       | storage for config |

### Ports

Set the following ports with the -p tag.

| Container Port | Recommended outside port | Protocol | Description |
| -------------- | ------------------------ | -------- | ----------- |
| `80`           | `8765`                   | TCP      | WebUI       |

### rebuild.sh

```sh
#!/bin/sh
docker stop speedtest
docker rm speedtest
docker pull henrywhitaker3/speedtest-tracker:latest
docker run --name speedtest \
    --restart unless-stopped \
    -p 8765:80 \
    -v speedtest:/config \
    -e OOKLA_EULA_GDPR=true \
    -e AUTH=true \
    -d henrywhitaker3/speedtest-tracker:latest
```
