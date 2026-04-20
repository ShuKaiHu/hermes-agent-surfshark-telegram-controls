# Setup Guide

## Goal

Set up Surfshark manual WireGuard configs so Hermes can control the connection from Telegram.

## Requirements

- Hermes Agent with Telegram gateway enabled
- `wg` and `wg-quick` installed
- Surfshark manual WireGuard configs exported from Surfshark

## Recommended config directories

Hermes can search common locations including:

- `/opt/homebrew/etc/wireguard`
- `/usr/local/etc/wireguard`
- `/etc/wireguard`

## Recommended file names

### Japan
- `jp-tok.conf`

### Taiwan
- `tw-tpe.conf`
- `tw-taipei.conf`
- `tw-tai.conf`

### Hong Kong
- `hk-hkg.conf`
- `hk-hongkong.conf`
- `hongkong.conf`
- `hk.conf`

## Permissions

```bash
chmod 600 /path/to/config.conf
```

## Telegram usage

Open the control panel:

```text
/surfshark
```

Direct commands:

```text
/surfshark connect-japan
/surfshark connect-taiwan
/surfshark connect-hongkong
/surfshark disconnect
/surfshark status
```

## macOS / Homebrew note

If `wg-quick` comes from Homebrew, explicit Bash 5 invocation is safer than relying on plain `sudo wg-quick`.

Preferred pattern:

```bash
/opt/homebrew/bin/bash /opt/homebrew/bin/wg-quick up /opt/homebrew/etc/wireguard/jp-tok.conf
/opt/homebrew/bin/bash /opt/homebrew/bin/wg-quick down /opt/homebrew/etc/wireguard/jp-tok.conf
```

## Privacy reminder

Never commit:

- real private keys
- full exported personal `.conf` files
- tokens
- chat IDs
- machine-identifying local paths
