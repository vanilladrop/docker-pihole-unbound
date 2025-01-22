# Pi-hole + Unbound on Docker

A Docker image for running [Pi-hole](https://pi-hole.net) with integrated [Unbound](https://nlnetlabs.nl/projects/unbound/about/) DNS resolver in a single container.

This is the official and maintained successor repository of the original `cbcrowe/pihole-unbound` image by [@chriscrowe](https://github.com/chriscrowe)

## BREAKING CHANGE!

**TL;DR**: The image changed. `chriscrowe/pihole-unbound` is deprecated and no longer receives updates. Change your setup to use the new image `mpgirro/pihole-unbound`.

## Overview

The base image for the container is the [official Pi-Hole container](https://hub.docker.com/r/pihole/pihole), with an extra build step added to install the Unbound resolver directly into to the container based on [instructions provided directly by the Pi-Hole team](https://docs.pi-hole.net/guides/unbound/).

## Features

- **Automated Builds**: The Docker image is automatically built and published using [GitHub Actions](https://docs.github.com/en/actions) and [Renovate](https://docs.renovatebot.com/), ensuring you always have access to the latest version.
- **Unbound integration**: The Unbound DNS resolver is directly integrated and configured within this image. No need for setting up and linking a separate Unbound container.
- **Multi-Registry Publishing**: Images are published to both [Docker Hub](https://hub.docker.com/repository/docker/mpgirro/pihole-unbound) and [GitHub Container Registry (GHCR)](https://github.com/mpgirro/docker-pihole-unbound/pkgs/container/docker-pihole-unbound), giving you flexibility in where you pull your images from.
- **Consistent Tagging**: Images are tagged with the same version tags as the [official Pi-hole images](https://github.com/pi-hole/docker-pi-hole).

## Usage

The [example Docker Compose file](example/compose.yaml) demonstrates how this image can be used.

**Important**: If you have an old setup using `cbcrowe/pihole-unbound`, you need to migrate to one of the image locations mentioned below.

### Image Locations

- [Docker Hub](https://hub.docker.com/repository/docker/mpgirro/pihole-unbound): `docker pull mpgirro/pihole-unbound`
- [GitHub Container Registry](https://github.com/mpgirro/docker-pihole-unbound/pkgs/container/docker-pihole-unbound): `docker pull ghcr.io/mpgirro/docker-pihole-unbound`

### Configuration Options

You can use all environment variables supported by the official Pi-hole container. Some examples are listed below. Vars and descriptions are replicated from the [official documentation](https://github.com/pi-hole/docker-pi-hole/#environment-variables):

| Variable | Default | Value | Description |
| -------- | ------- | ----- | ---------- |
| `TZ` | UTC | `<Timezone>` | Set your [timezone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) to make sure logs rotate at local midnight instead of at UTC midnight.
| `WEBPASSWORD` | random | `<Admin password>` | http://pi.hole/admin password. Run `docker logs pihole \| grep random` to find your random pass.
| `FTLCONF_LOCAL_IPV4` | unset | `<Host's IP>` | Set to your server's LAN IP, used by web block modes and lighttpd bind address.
| `REV_SERVER` | `false` | `<"true"\|"false">` | Enable DNS conditional forwarding for device name resolution |
| `REV_SERVER_DOMAIN` | unset | Network Domain | If conditional forwarding is enabled, set the domain of the local network router |
| `REV_SERVER_TARGET` | unset | Router's IP | If conditional forwarding is enabled, set the IP of the local network router |
| `REV_SERVER_CIDR` | unset | Reverse DNS | If conditional forwarding is enabled, set the reverse DNS zone (e.g. `192.168.0.0/24`) |
| `WEBTHEME` | `default-light` | `<"default-dark"\|"default-darker"\|"default-light"\|"default-auto"\|"lcars">`| User interface theme to use.

Alternatively you can use an `.env` file in the same directory as your `compose.yaml` file:

```
FTLCONF_LOCAL_IPV4=192.168.1.10
TZ=America/Los_Angeles
WEBPASSWORD=ChangeMe!
REV_SERVER=true
REV_SERVER_DOMAIN=local
REV_SERVER_TARGET=192.168.1.1
REV_SERVER_CIDR=192.168.0.0/16
HOSTNAME=pihole
DOMAIN_NAME=pihole.local
PIHOLE_WEBPORT=80
WEBTHEME=default-light
```

## Filing Issues

Please file issues as follows:

- Pi-hole: For issues related to Pi-hole itself, please report them directly in the [Pi-hole repository](https://github.com/pi-hole/pi-hole/issues).
- Unbound: For issues related to the Unbound DNS resolver, please use the [Unbound repository](https://github.com/NLnetLabs/unbound/issues).
- Image Publishing: For issues specifically related to the image publishing (e.g., missing tags, images not being updated or published correctly), report them here in this repository.