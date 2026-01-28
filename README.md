# my-nas
A configuration overview for my DXP2800 running TrueNAS

I recently bought a UGREEN DXP2800, which ships with UGOS, UGREEN’s “proprietary” Debian-based distribution. Since it’s unclear where the modifications to that OS are (and if are) published (which may be a GPL violation), I decided to go down the rabbithole and install TrueNAS instead.

This repository documents my setup, configuration choices, and any tweaks or workarounds needed to get TrueNAS running smoothly on this hardware.

## Services

Right now, I have the following services running, installed as TrueNAS Apps:

- **[ddns-updater](https://github.com/qdm12/ddns-updater)** – Automatically updates dynamic DNS records, since I don't have a static public IP.
- **[immich](https://immich.app)** – A fantastic self-hosted photo and video backup solution, with a familiar Google Photos-like GUI.
- **[jellyfin](https://jellyfin.org)** – Media server for locally streaming movies and TV series.
- **[npm](https://nginxproxymanager.com)** – Nginx Proxy Manager for managing reverse proxies and SSL certificates, crucial in order to expose the services to the public.
- **[opencloud](https://opencloud.eu)** – Self-hosted file sharing and collaboration platform. The best ownCloud fork, EU-based and without useless features — what Nextcloud should be.
- **[qbittorrent](https://www.qbittorrent.org)** – Torrent client.

## Guides and configs

- **Hardware**
  - [Hardware overview](https://github.com/sh4tteredd/my-nas/blob/main/config/hardware/hw.md)
  - [Disks & storage layout](https://github.com/sh4tteredd/my-nas/blob/main/config/hardware/disks.md)

- **Operating System**
  - [TrueNAS & system configuration](https://github.com/sh4tteredd/my-nas/blob/main/config(os/ups.md)

- **Services configuration**
  - [Immich](https://github.com/sh4tteredd/my-nas/blob/main/config/services/immich.md)

