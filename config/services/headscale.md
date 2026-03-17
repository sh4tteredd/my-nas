# Headscale (Control Server) + Tailscale (Subnet Router) on TrueNAS SCALE

This guide describes how I set up a self-hosted **Headscale** instance to manage a private WireGuard mesh network, using the **Tailscale** App on TrueNAS SCALE as a **Subnet Router** to access my entire home LAN (`192.168.178.0/24`) from remote devices.

I found a lot of gatekeeping and outdated documentation regarding the v0.28.x CLI and how to properly "bridge" the TrueNAS networking into the Headscale mesh.

The setup ensures:
- **Total Privacy**: Your coordination plane (keys and node discovery) is hosted by you, not Tailscale.com.
- **Full LAN Access**: Access printers, IP cameras, and other NAS services from your iPhone/Laptop via 4G/5G.
- **No Port Forwarding**: High security with no need to expose internal services to the public internet.

---

## Architecture Overview

The stack consists of two main parts:

1. **Headscale (Container)**: The "Control Plane." It manages security keys and tells nodes how to find each other.
2. **Tailscale (TrueNAS App)**: The "Data Plane." It connects to Headscale and acts as a **Subnet Router**, advertising your home network to the rest of the mesh.

---

## Tailscale App Configuration (TrueNAS UI)

To ensure the Tailscale client talks to your Headscale server instead of the official ones, use these parameters in the TrueNAS App config:

| Parameter | Value (Example) | Note |
| :--- | :--- | :--- |
| **Hostname** | `truenas-scale` | How the NAS appears in the VPN list |
| **Auth Key** | `hskey-auth-xxx...` | Generated via Headscale CLI (see below) |
| **Advertise Routes** | `192.168.178.0/24` | Your local home network range |
| **Extra Arguments** | `--login-server=http://your.server.ip:30210` | **Required** to bypass official servers |
| **Host Network** | `Enabled` | **Required** for Subnet Routing to work |

---

## Working CLI Commands (v0.28.0+)

Since Headscale on TrueNAS usually runs in a Docker container (e.g., `ix-headscale-headscale-1`), use these exact commands in the TrueNAS Shell.

### 1. User Management
Headscale requires a user (namespace) before you can register any device.
```bash
# Create the user for your devices
sudo docker exec ix-headscale-headscale-1 headscale users create USER

# List users to confirm the ID (needed for keys)
sudo docker exec ix-headscale-headscale-1 headscale users list
```

### 2. Generating the Auth Key
Use this key in the TrueNAS Tailscale App config to automate the login.
```bash
# Replace '-u 1' with your actual User ID from the list command
sudo docker exec ix-headscale-headscale-1 headscale preauthkeys create -u 1 --reusable -e 24h
```

### 3. Approving the Subnet Route (The "Bridge")
Even if you configured "Advertise Routes" in the UI, Headscale will keep the route "Pending" for security. You must manually approve it.

```bash
# 1. Find the Node ID for your 'truenas-scale' (usually ID 2)
sudo docker exec ix-headscale-headscale-1 headscale nodes list

# 2. Verify the route is 'Available' but not yet approved
sudo docker exec ix-headscale-headscale-1 headscale nodes list-routes -i 2

# 3. Officially approve the route (The Final Step)
sudo docker exec ix-headscale-headscale-1 headscale nodes approve-routes -i 2 -r 192.168.178.0/24 #change 192.168.178.0/24 with your network address
```

---

## Troubleshooting

### "Invalid Key: unable to validate API key"
This means the Tailscale app is trying to reach `tailscale.com`. Double-check your **Extra Arguments** in the TrueNAS UI. It must contain `--login-server=http://your-ip:port`.

### Subnet route is "Approved" but LAN is unreachable
1. **On iOS/Android**: Open the Tailscale App settings and ensure **"Use Tailscale Subnets"** is toggled **ON**.
2. **Firewall**: Ensure your TrueNAS or Router isn't blocking traffic from the `tailscale0` interface.
3. **Primary Check**: Run `list-routes` again. The route should now appear under the **Serving** column.
