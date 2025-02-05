# mailserver - docker-mailserver

This is a [Docker](/wiki/docker.md) container for a mailserver.
The official container and documentation was made by
[mailserver](https://hub.docker.com/r/mailserver/docker-mailserver).
It contains dovecot, antispam, antivirus, ssl, etc.

## Set-up

The configuration is done automatically using scripts by mailserver.
First you need to download the essential setup files.

```sh
wget -O .env https://raw.githubusercontent.com/docker-mailserver/docker-mailserver/master/compose.env
wget https://raw.githubusercontent.com/docker-mailserver/docker-mailserver/master/docker-compose.yml
wget https://raw.githubusercontent.com/docker-mailserver/docker-mailserver/master/mailserver.env
wget https://raw.githubusercontent.com/docker-mailserver/docker-mailserver/v9.0.1/setup.sh
chmod a+x ./setup.sh
```

After that you need to edit the `.env` and the `mailserver.env<>` files to
configure the mailserver.

Create the file `rebuild.sh`.
You then need to start the container with the `rebuild.sh` and add email
accounts using the following command.

`./setup.sh email add <user@domain> (<password>)`
`./setup.sh alias add postmaster@<domain> <user@domain>`

And finally generate the DCIM keys and rebuild once again.

`./setup.sh config dkim`

Finally if a reverse proxy is used check the
[traefik entry](/wiki/docker/traefik.md#setup-mailserver) or the
[nginx entry](./nginx.md).

Afterwards you're ready to go by once again running the `rebuild.sh` file.

### rebuild.sh

```sh
#!/bin/sh
docker-compose down
docker pull mailserver/docker-mailserver:latest
docker-compose up -d
```
