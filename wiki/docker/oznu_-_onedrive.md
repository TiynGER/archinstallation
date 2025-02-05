# oznu - onedrive

This is a docker container for a onedrive client.
The official container and documentation was made by [oznu](https://hub.docker.com/r/oznu/onedrive).

## Set-up

Create the file `rebuild.sh`.
Change the settings according to your needs and run `./rebuild.sh` afterwards.

### Environment-variables

Set the following variables with the -e tag.

| Name   | Usage   | Default |
| ------ | ------- | ------- |
| `PUID` | UserID  |         |
| `PGID` | GroupID |         |

### Volumes

Set the following volumes with the -v tag.

| Outside mount/volume name | Container mount | Description                                     |
| ------------------------- | --------------- | ----------------------------------------------- |
| `onedrive_config`         | `/config`       | configuration storage for the server connection |
| `onedrive_doc`            | `/documents`    | storage for downloaded documents                |

### rebuild.sh

```sh
#!/bin/sh
docker stop onedrive
docker rm onedrive
docker pull oznu/onedrive:latest
docker run --name onedrive \
    --restart unless-stopped \
    -v onedrive_config:/config \
    -v onedrive_doc:/documents \
    -e PUID=$(id -u) \
    -e PGID=$(id -g) \
    -d oznu/onedrive:latest
```
