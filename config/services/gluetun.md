# qBittorrent + Gluetun (VPN) on TrueNAS SCALE

This page describes how I set up **qBittorrent** behind **Gluetun** to route all traffic through a VPN, using Docker Compose on **TrueNAS SCALE**.

I've too much gatekeeping online about this solution (i.e. people asking how to reach the VPN+torrent combo and people answering with "gluetun" without explaining anything)

The setup ensures:
- All qBittorrent traffic is forced through the VPN
- No traffic leaks if the VPN goes down (kill switch)
- Clean separation between networking (Gluetun) and application data (qBittorrent)

---

## Setup Overview

The stack is composed of two containers:

- **Gluetun**  
  Handles the VPN connection, firewall rules, DNS, and port exposure.
- **qBittorrent**  
  Runs the torrent client and shares the network stack of Gluetun.

## Docker compose

```yaml

version: "3.8"

services:
  gluetun:
    image: qmcgaw/gluetun:latest
    container_name: gluetun
    cap_add:
      - NET_ADMIN
    devices:
      - /dev/net/tun:/dev/net/tun
    environment:
      # VPN provider configuration
      - VPN_SERVICE_PROVIDER=${VPN_PROVIDER}        # es: nordvpn, mullvad, protonvpn
      - VPN_TYPE=wireguard                          # or openvpn
      - WIREGUARD_PRIVATE_KEY=${WIREGUARD_PRIVATE_KEY}

      # Server selection (optional)
      - SERVER_COUNTRIES=${VPN_COUNTRIES}           # es: Italy
      - SERVER_CATEGORIES=${VPN_CATEGORIES}         # es: P2P

      # Locale & DNS
      - TZ=${TZ}                                    # es: Europe/Rome
      - DNS_ADDRESS=${DNS_ADDRESS}                  # es: 1.1.1.1
      # - DOT=on                                    # optional: DNS over TLS

    volumes:
      - ${GLUETUN_CONFIG_PATH}:/gluetun

    ports:
      # Example: expose qBittorrent WebUI
      # - "30025:30025"

    restart: unless-stopped

  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent:latest
    container_name: qbittorrent
    network_mode: service:gluetun
    environment:
      - PUID=568
      - PGID=568
      - TZ=${TZ}
      - WEBUI_PORT=${QBIT_WEBUI_PORT}
    volumes:
      - ${QBIT_CONFIG_PATH}:/config
      - ${QBIT_DOWNLOADS_PATH}:/downloads
    restart: unless-stopped

```
## Parameter Explanation

### VPN Provider & Protocol

#### `VPN_SERVICE_PROVIDER`
Must match a provider supported by Gluetun, e.g.:

- `nordvpn`
- `mullvad`
- `protonvpn`

#### `VPN_TYPE`
`wireguard` is recommended for performance and stability.  
OpenVPN is also supported but may be slower.

#### `WIREGUARD_PRIVATE_KEY`
Provider-specific private key used to authenticate the Wireguard tunnel.  

TIP: if using **NordVPN**, you can generate your Wireguard private key following the instructions here, even if not officially supported:  
[NordLynx / How to get your private key](https://github.com/bubuntux/nordlynx?tab=readme-ov-file#how-to-get-your-private_key)
