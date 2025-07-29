<p align="center">
  <img src="https://raw.githubusercontent.com/OSA-Socks/.github/refs/heads/main/assets/Logo.png" alt="Logo" width="200"/>
</p>

# Docker Instructions

## What?

This README will guide you through the following:

- [Choosing a VPN](#choosing-a-vpn)
- [Installing Docker](#installing-docker)
- [Creating your Docker Compose](#creating-your-docker-compose)
- [Deploying](#deploying)
- [And what to do after that](#what-now)

## Foreword

This guide is NOT a crash course on ANY of the following:

- Docker & Docker Compose
- HTTP/HTTPS
- SOCKS
- The Shell
- Linux

Please acquaint yourself with these tools to an intermediate level before continuing.

The majority of these instructions will be for Linux, some adaptation may be required for windows machines (although, if you care about your privacy in 2025, you may want to switch to Linux too).

## Choosing a VPN

The easiest and most reliable way to host your own socks proxy is with gluetun and docker compose.
For this, a paid VPN provider is required: Personally, I believe the best are [Proton](https://protonvpn.com/) and [Mullvad](https://mullvad.net/en).
Either way, ensure your VPN of choice has a strict no log policy.

## Installing Docker

Docker installation methods vary from OS to OS and distro to distro, so I'll do my best to cover the big ones.

[Windows Instructions](https://docs.docker.com/desktop/setup/install/windows-install/)

[Mac Instructions](https://docs.docker.com/desktop/setup/install/mac-install/)

### [Arch Linux](https://archlinux.org) & [Manjaro Linux](https://manjaro.org)

Install the packages

```sh
sudo pacman -S docker docker-compose
```

### [Ubuntu](https://ubuntu.com)

Add the docker repo (classic Ubuntu horrors)

```sh
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Install the packages

```sh
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### [Fedora Linux](https://fedoraproject.org)

Add the Docker repo

```sh
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
```

Install the packages

```sh
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Distro Agnostic Finishes

Enable the service

```sh
sudo systemctl enable docker --now
```

This is required on ALL (most) distros!

## Creating your Docker Compose

First, make a directory in a meaningful location.

Preferrably an external drive, so you don't have to remake your docker compose on a system reset.

Create a file in said directory named exactly `docker-compose.yml`.

Now edit it in your preferred text editor (I recommend [VSCodium](https://vscodium.com/)!).

We want to deploy two services with docker: Gluetun, and SOCKS.

Here is an example compose config, you can substitute the 
[gluetun environment variables](https://github.com/qdm12/gluetun-wiki/tree/main/setup/providers)
 with the vars suited to your provider.

```yaml
services:
  gluetun:
    image: qmcgaw/gluetun:latest
    container_name: gluetun
    cap_add:
      - NET_ADMIN
    environment:
      - VPN_SERVICE_PROVIDER=protonvpn
      - OPENVPN_USER=userHere
      - OPENVPN_PASSWORD=passwordHere
      # Switzerland is a good country to use: fast, and typically uncensored
      - SERVER_COUNTRIES=Switzerland
      - SERVER_CITIES=Zurich
    ports:
    # This is the port for your SOCKS proxy!
      - 1080:1080
    restart: unless-stopped
    devices:
      - /dev/net/tun:/dev/net/tun
  socks5:
    image: serjs/go-socks5-proxy
    container_name: socks5_proxy
    restart: unless-stopped
    network_mode: "service:gluetun"
```

## Deploying
Now that you have created your docker compose, you need to deploy it. First, navigate to your directory in a shell.
This can be done by either:
- Clicking some variation of "Open in terminal" in the context menu of most Linux file managers
- Opening your shell normally, and running `cd (your docker folder path)`


Now that you're in the folder, simply run `docker compose up -d`.
On a first run, it may take a few minutes to download the images.

## What Now?
