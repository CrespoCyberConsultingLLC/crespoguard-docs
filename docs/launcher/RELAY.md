
# CrespoGuard Relay

> TCP relay for RF Online servers — DDoS protection, IP masking, rate limiting, and a real-time dashboard. Free for up to 30 players.

## Overview

The CrespoGuard Relay sits between your players and your game server. It provides DDoS protection, rate limiting, IP masking, and a management dashboard. Two modes are available:

**Community Edition (Free, up to 30 players)** — Transparent TCP proxy. Works with any vanilla RF client. No launcher required.

```
Player                          Your Server
┌──────────┐     plain TCP     ┌──────────────────┐
│ Any RF    ├───────────────────┤ CrespoGuard      │    ┌──────────────┐
│ Client    │   (transparent)   │ Relay             ├────┤ LoginServer  │
│           │                   │ (port 10002)      │    │ (port 10001) │
└──────────┘                   │                   │    └──────────────┘
                               │ DDoS protection   │
                               │ Rate limiting     │    ┌──────────────┐
                               │ Auto-ban          ├────┤ ZoneServer   │
                               │ GeoIP + Threat DB │    │ (port 27780) │
                               │ Dashboard         │
                               └──────────────────┘
```

**Guard+ Tier ($19/mo+)** — AES-256-GCM encrypted tunnel. Requires CrespoGuard Launcher. Full IP protection + HWID bans.

```
Player                          Your Server
┌──────────┐    AES-256-GCM    ┌──────────────────┐
│ CrespoGd ├───────────────────┤ CrespoGuard      │    ┌──────────────┐
│ Launcher  │   (encrypted)     │ Relay             ├────┤ LoginServer  │
│           │                   │ (port 10001)      │    │ (port 10001) │
└──────────┘                   │                   │    └──────────────┘
                               │ Everything above  │
                               │ + Encrypted tunnel│    ┌──────────────┐
                               │ + HWID bans       ├────┤ ZoneServer   │
                               │ + Kick/Announce   │    │ (port 27780) │
                               └──────────────────┘    └──────────────┘
```

Community players connect to the relay IP directly. Guard+ players connect via the CrespoGuard Launcher. Your real server IP stays hidden when the relay runs on a separate machine.

## What's Included

| Feature | Community (Free) | Guard ($19/mo) | Shield ($49/mo) | Fortress ($79/mo) |
|---------|:----------------:|:--------------:|:---------------:|:------------------:|
| Transparent TCP proxy | Yes | Yes | Yes | Yes |
| CGRD encrypted tunnel | — | Yes | Yes | Yes |
| IP masking (separate machine) | Yes | Yes | Yes | Yes |
| Dashboard (view + IP bans) | Yes | Yes | Yes | Yes |
| Dashboard (kick, HWID bans, announce) | — | Yes | Yes | Yes |
| Rate limiting + IP bans | Yes | Yes | Yes | Yes |
| Auto-ban (progressive backoff) | Yes | Yes | Yes | Yes |
| GeoIP country filtering | Yes | Yes | Yes | Yes |
| Threat Intel blocklist (200K+ IPs) | Yes | Yes | Yes | Yes |
| Auto-update (SHA-256 verified) | Yes | Yes | Yes | Yes |
| Anonymous telemetry | Yes | Yes | Yes | Yes |
| Auto-generated API key (CSPRNG) | Yes | Yes | Yes | Yes |
| HWID bans | — | Yes | Yes | Yes |
| Announcements | — | Yes | Yes | Yes |
| SOC telemetry (detailed) | — | Yes | Yes | Yes |
| Max players | **30** | 75 | 250 | 500 |
| Multi-zone proxy | — | — | Yes | Yes |
| File logging | — | — | Yes | Yes |
| Edge relay mode | — | — | — | Yes |
| PROXY protocol v1 | — | — | — | Yes |
| Health check endpoint | — | — | — | Yes |

## Requirements

- A running RF Online server (LoginServer + ZoneServer)
- A Windows or Linux machine for the relay (can be the same machine as your game server)
- The CrespoGuard Relay binary (`CrespoGuardRelay.exe` or Linux build)
- A CrespoGuard license (Community for up to 30 players, Guard+ for higher caps)
- The CrespoGuard Launcher configured with SecureLogin (PSK must match)

## Quick Setup

### Step 1: Activate Your License

```
CrespoGuardRelay.exe --activate YOUR-ACTIVATION-CODE
```

### Step 2: Generate Config

```
CrespoGuardRelay.exe --generate-config server.json
```

### Step 3: Configure

Edit `server.json`:

```json
{
    "ServerName": "Your Server",
    "ListenIP": "0.0.0.0",
    "ListenPort": 10001,
    "TargetIP": "127.0.0.1",
    "TargetPort": 10001,
    "PSK": "same_key_as_modules_json",
    "MaxClients": 75,
    "PublicIP": "YOUR_PUBLIC_IP",
    "MaskServerIP": true,
    "DashboardEnabled": true,
    "DashboardPort": 8081,
    "DashboardApiKey": "YOUR_GENERATED_KEY",
    "RateLimitPerIP": 15,
    "RateLimitWindowSec": 60,
    "MaxConnectionsPerIP": 5,
    "IPBansFile": "ipbans.json"
}
```

The PSK must match the `SecureLoginPSK` in your launcher's `modules.json`.

### Step 4: Start the Relay

```
CrespoGuardRelay.exe server.json
```

### Step 5: Configure the Launcher

Add `SecureLogin` to your launcher's `modules.json` and re-encrypt `config.bin`:

```json
{
  "SecureLogin": {
    "EnableSecureLogin": true,
    "SecureLoginType": 1,
    "SecureLoginHost": "YOUR.PUBLIC.IP",
    "SecureLoginIP": "YOUR.PUBLIC.IP",
    "SecureLoginPort": 10001,
    "SecureLoginPSK": "YOUR_64_CHAR_HEX_PSK"
  }
}
```

See [Creating config.bin](CONFIG_CREATION.md) for the full encryption walkthrough.

## Firewall Rules

| Port | Direction | Purpose |
|------|-----------|---------|
| 10001 TCP | Inbound | Relay listen (encrypted tunnel) |
| 27780 TCP | Inbound | ZoneServer (or zone proxy at Shield+) |
| 8081 TCP | Inbound | Dashboard (restrict to your admin IP!) |

**Block your real LoginServer port** from external access — only the relay (localhost) should reach it.

## Upgrading Tiers

Tier upgrades are instant — purchase the higher tier, activate the new code, and restart the relay. No client changes needed. See [Premium Tiers](../PREMIUM_TIERS.md) for full tier documentation.

## Community Edition (Free Relay — 30 Players)

The Community Edition includes the CrespoGuard Relay for up to 30 concurrent players. You get encrypted transport, IP masking, DDoS protection (rate limiting, GeoIP filtering, Threat Intel blocklist, auto-ban with progressive backoff), and full dashboard access — all at no cost.

The 30-player cap is a **hard limit enforced at the binary level**. It cannot be bypassed by editing the config file — the relay binary itself rejects connections beyond 30. When you need more capacity, HWID bans, kick, announcements, or bin-dependent gameplay features, upgrade to Guard tier ($19/mo, 75 players).

## Auto-Update

The relay checks for new versions on startup. When a newer version is available, it downloads the update via HTTPS (curl), verifies the SHA-256 checksum of the downloaded binary, replaces itself, and restarts automatically. No manual intervention required.

```json
{
    "AutoUpdateEnabled": true
}
```

`AutoUpdateEnabled` defaults to `true`. Set to `false` in `server.json` to disable.

## Telemetry

The relay sends anonymous usage statistics on startup and every 24 hours. **No personally identifiable information (PII) is collected.** Telemetry data includes:

- Relay version
- Operating system
- Player cap (max clients)
- Enabled features
- Peak player count
- Connection statistics
- DDoS protection statistics

```json
{
    "TelemetryEnabled": true
}
```

`TelemetryEnabled` defaults to `true`. Opt out by setting it to `false` in `server.json`.

## Auto-Generated API Key

On first run, if no `DashboardApiKey` is set in `server.json`, the relay automatically generates a secure 128-bit random key using the operating system's cryptographically secure pseudorandom number generator (CSPRNG) and saves it to the config file. This ensures the dashboard API is protected from the moment the relay starts — no manual key generation needed.
