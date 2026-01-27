# my-nas
A configuration overview for my DXP2800 running TrueNAS

I recently bought a UGREEN DXP2800, which ships with UGOS, UGREEN’s “proprietary” Debian-based distribution. Since it’s unclear where the modifications to that OS are (and if are) published (which may be a GPL violation), I decided to go down the rabbithole and install TrueNAS instead.

This repository documents my setup, configuration choices, and any tweaks or workarounds needed to get TrueNAS running smoothly on this hardware.

## Services

Right now, I have the following services running:

- **ddns-updater** – Automatically updates dynamic DNS records, since I don't have a static public IP.
- **immich** – A fantastic self-hosted photo and video backup solution, with a familiar Google Photos-like GUI.
- **jellyfin** – Media server for locally streaming movies and TV series.
- **npm** – Nginx Proxy Manager for managing reverse proxies and SSL certificates, crucial in order to expose the services to the public.
- **opencloud** – Self-hosted file sharing and collaboration platform. The best Owncloud fork, EU-based and without useless features, what nextcloud should be.
- **qbittorrent** – Torrent client.
