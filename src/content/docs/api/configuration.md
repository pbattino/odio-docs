---
title: Configuration
description: Configuration reference for go-odio-api.
---

The odio installer generates `~/.config/odio-api/config.yaml`. You can also create it manually.

Configuration file locations (in order of precedence):
- Specified with `--config <path>`
- `~/.config/odio-api/config.yaml` (user-specific)
- `/etc/odio-api/config.yaml` (system-wide)

## Minimal config

```yaml
bind: lo
logLevel: info

api:
  enabled: true
  port: 8018
  ui:
    enabled: true
  cors:
    origins: ["https://odio-pwa.vercel.app"]
```

## Network binding

`bind` controls which network interfaces the API listens on.

```yaml
bind: lo                      # loopback only (default)
# bind: enp2s0                # single LAN interface
# bind: [lo, enp2s0]          # loopback + LAN
# bind: [lo, enp2s0, wlan0]   # loopback + ethernet + wifi
# bind: all                   # all interfaces (Docker, remote access)
```

> **Docker:** `bind` must be set to `all` for remote access through the bridge network.

## Backend configuration

Each backend can be enabled or disabled independently. Disabling a backend removes all its routes from the API.

### Systemd (opt-in, whitelist required)

See [Systemd](/api/systemd/) for endpoint details.

```yaml
systemd:
  enabled: true
  timeout: 90s
  system:
    - bluetooth.service
  user:
    - mpd.service
    - shairport-sync.service
    - snapclient.service
    - spotifyd.service
    - upmpdcli.service
```

### Bluetooth

See [Bluetooth](/api/bluetooth/) for endpoint details and system setup.

```yaml
bluetooth:
  enabled: true
  timeout: 5s
  pairingTimeout: 60s
  idleTimeout: 30m
```

### Power management

See [Power](/api/power/) for endpoint details.

```yaml
power:
  enabled: true
  capabilities:
    poweroff: true
    reboot: true
```

### Zeroconf / mDNS

See [Zeroconf](/api/zeroconf/) for details. Requires `bind` to be set to a real network interface.

```yaml
bind: enp2s0
zeroconf:
  enabled: true
```
