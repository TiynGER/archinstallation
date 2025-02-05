# olbat - cupsd

This is a [Docker](/wiki/docker.md) container for a [Cups](../cups.md) server.
The official container and documentation was made by
[olbat](https://hub.docker.com/r/olbat/cupsd).

## Set-up

Create the file `rebuild.sh`.
Change the settings according to your needs and run `./rebuild.sh` afterwards.

### Volumes

Set the following volumes with the -v tag.

| Outside mount/volume name | Container mount | Description                     |
| ------------------------- | --------------- | ------------------------------- |
| `cups`                    | `/etc/cups`     | configuration for printers, etc |
| `/var/run/dbus`           | `/var/run/dbus` | connection to host dbus         |

### Ports

Set the following ports with the -p tag.

| Container Port | Recommended outside port | Protocol | Description       |
| -------------- | ------------------------ | -------- | ----------------- |
| `631`          | `631`                    | TCP      | cups server webui |

### Additional

The default username is `print`, the default password is `print`.

### rebuild.sh

```sh
#!/bin/sh
docker stop cups
docker rm cups
docker pull olbat/cupsd:latest
docker run --name cups \
        --restart unless-stopped \
        --net=host \
        -p 631:631 \
        -v /var/run/dbus:/var/run/dbus \
        -v cups:/etc/cups/ \
        -d olbat/cupsd:latest
```
