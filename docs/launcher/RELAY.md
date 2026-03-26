
# CrespoGuard Relay

> Encrypted relay transport for RF Online servers — hide your IP, ban abusers, monitor connections. Available at Guard tier and above.

## Overview

The CrespoGuard Relay is a **paid server-side component** (starting at Guard tier, $19/mo) that sits between your players and your game server. It provides encrypted transport, IP masking, a management dashboard, and hardware-based bans.

```
Player                          Your Server
┌──────────┐    AES-256-GCM    ┌──────────────────┐
│ CrespoGd ├───────────────────┤ CrespoGuard      │    ┌──────────────┐
│ Launcher  │   (encrypted)     │ Relay             ├────┤ LoginServer  │
│           │                   │ (port 10001)      │    │ (port 10001) │
└──────────┘                   │                   │    └──────────────┘
                               │ IP masking        │
                               │ Rate limiting     │    ┌──────────────┐
                               │ HWID bans         ├────┤ ZoneServer   │
                               │ Dashboard         │    │ (port 27780) │
                               └──────────────────┘    └──────────────┘
```

Players connect to the relay IP using the CrespoGuard Launcher. Your real server IP stays hidden.

## What's Included

| Feature | Guard ($19/mo) | Shield ($49/mo) | Fortress ($79/mo) |
|---------|:--------------:|:---------------:|:------------------:|
| CGRD encrypted tunnel | Yes | Yes | Yes |
| Server IP masking | Yes | Yes | Yes |
| Dashboard (full read/write) | Yes | Yes | Yes |
| Rate limiting + IP bans | Yes | Yes | Yes |
| HWID bans | Yes | Yes | Yes |
| Announcements | Yes | Yes | Yes |
| Telemetry (SOC) | Yes | Yes | Yes |
| Max players | 75 | 250 | 500 |
| Multi-zone proxy | — | Yes | Yes |
| File logging | — | Yes | Yes |
| Edge relay mode | — | — | Yes |
| PROXY protocol v1 | — | — | Yes |
| Health check endpoint | — | — | Yes |

## Requirements

- A running RF Online server (LoginServer + ZoneServer)
- A Windows or Linux machine for the relay (can be the same machine as your game server)
- The CrespoGuard Relay binary (`CrespoGuardRelay.exe` or Linux build)
- A Guard+ tier license
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

## Community Edition (No Relay)

The Community Edition is the free CrespoGuard Launcher with ClientGuard and the anti-cheat suite. It does not include the relay or any server-side component. Players connect directly to your game server without relay transport.

If you only need a branded launcher and anti-cheat, the Community Edition is all you need. When you're ready for IP masking, encrypted transport, dashboards, and HWID bans, upgrade to Guard tier.
