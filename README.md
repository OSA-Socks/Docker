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

<details>
  <summary>On Linux</summary>

  <details>
    <summary>Arch Linux & Manjaro Linux</summary>

    Install the packages

    ```sh
    sudo pacman -S docker docker-compose
    ```

  </details>

  <details>
    <summary>Ubuntu</summary>

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

  </details>

  <details>
    <summary>Fedora Linux</summary>

    Add the Docker repo

    ```sh
    sudo dnf -y install dnf-plugins-core
    sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
    ```

    Install the packages

    ```sh
    sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```

  </details>

  ### Distro Agnostic Finishes

  Enable the service

  ```sh
  sudo systemctl enable docker --now
````

This is required on ALL (most) distros!

</details>

## Creating your Docker Compose

## Deploying

## What Now?