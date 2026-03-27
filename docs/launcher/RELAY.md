
# CrespoGuard Relay

> TCP relay for RF Online servers — transparent proxy or AES-256-GCM encrypted tunnel, DDoS protection, rate limiting, and a real-time dashboard. Every feature included at every tier. Self-hosted is free; edge-hosted is an optional +$10/mo add-on.

## Overview

The CrespoGuard Relay sits between your players and your game server. It provides DDoS protection, rate limiting, and a management dashboard. Two connection modes are available, both included in all tiers:

**Transparent Proxy (any RF client)** — Plain TCP proxy. Works with any vanilla RF client. No launcher required.

**Encrypted Tunnel (CrespoGuard Launcher)** — AES-256-GCM encrypted tunnel. Requires CrespoGuard Launcher. Encrypts player credentials and game data in transit.

Both modes are available in all tiers, including Community.

## Deployment Options

You choose where the relay runs. All features work identically in both options — the difference is who manages the relay infrastructure.

### Self-Hosted (Free with any tier)

You run the relay on your own machine or VPS. All features included — encrypted tunnel, DDoS protection, HWID bans, dashboard, and more. If the relay runs on a separate machine from your game server, players connect to the relay address and never see your game server IP (full IP masking). If the relay runs on the same machine as your game server, all features still work but players can see the server IP.

```
Player                          Your VPS / Machine
┌──────────┐     plain TCP     ┌──────────────────┐
│ Any RF    ├───────────────────┤ CrespoGuard      │    ┌──────────────┐
│ Client    │   (transparent)   │ Relay             ├────┤ LoginServer  │
│           │                   │ (port 10002)      │    │ (port 10001) │
└──────────┘                   │                   │    └──────────────┘
                               │ DDoS protection   │
┌──────────┐    AES-256-GCM    │ Rate limiting     │    ┌──────────────┐
│ CrespoGd ├───────────────────┤ Auto-ban          ├────┤ ZoneServer   │
│ Launcher  │   (encrypted)     │ GeoIP + Threat DB │    │ (port 27780) │
└──────────┘                   │ Dashboard         │
                               └──────────────────┘
```

### Edge-Hosted (+$10/mo add-on at any tier)

CrespoGuard hosts the relay on their edge infrastructure. You get full IP masking with zero setup — no VPS to provision, configure, or maintain. Players connect to `edge.crespoguard.com` with your route code. All relay features work identically.

```
Player                     CrespoGuard Edge           Your Server
┌──────────┐              ┌──────────────────┐
│ Any RF    ├──────────────┤ CrespoGuard      │       ┌──────────────┐
│ Client    │  plain TCP   │ Edge Relay        ├───────┤ LoginServer  │
│           │              │                   │       │ (port 10001) │
└──────────┘              │ DDoS protection   │       └──────────────┘
                          │ Rate limiting     │
┌──────────┐  AES-256-GCM │ Auto-ban          │       ┌──────────────┐
│ CrespoGd ├──────────────┤ GeoIP + Threat DB ├───────┤ ZoneServer   │
│ Launcher  │  (encrypted) │ Dashboard         │       │ (port 27780) │
└──────────┘              │                   │       └──────────────┘
                          │ edge.crespoguard  │
                          │ .com              │
                          └──────────────────┘
```

**Which should you choose?**

| | Self-Hosted (Free) | Edge-Hosted (+$10/mo) |
|---|---|---|
| **Cost** | Free with any tier | +$10/mo on top of tier price |
| **IP masking** | Yes, if relay is on a separate machine | Yes, always |
| **Setup** | You manage the relay binary and VPS | Zero setup — just configure your launcher |
| **All features** | Yes | Yes |
| **Best for** | Server owners who already have a VPS or want full control | Server owners who want IP masking without managing infrastructure |

## Pricing

| Tier | Price | Max Players | Edge Routing |
|------|-------|-------------|--------------|
| **Community** | Free | 30 | +$10/mo optional |
| **Guard** | $19/mo | 200 | +$10/mo optional |
| **Shield** | $49/mo | 500 | +$10/mo optional |
| **Fortress** | $99/mo | 1000 | +$10/mo optional |

Every feature below is included at every tier. The only difference is how many concurrent players your relay supports. Edge routing (hosted relay on CrespoGuard's infrastructure) is an optional $10/mo add-on available at any tier.

## What's Included (All Tiers — Self-Hosted)

Every feature below is included free with any tier when you run the relay yourself:

- Transparent TCP proxy
- CGRD encrypted tunnel (AES-256-GCM, requires launcher)
- IP masking (when relay runs on a separate machine from your game server)
- Dashboard (view, config, kick, HWID bans, announcements)
- Rate limiting + IP bans
- Auto-ban (progressive backoff)
- GeoIP country filtering
- Threat Intel blocklist (200K+ IPs)
- ASN-based filtering (block datacenter/VPN IPs)
- TCP fingerprinting (passive OS detection)
- Tor exit node blocklist
- Adaptive rate limiter (burst-tolerant)
- Anti-replay protection
- Prometheus /metrics endpoint
- Async rotating logger (structured output)
- Optimized proxying (zero-copy on Linux)
- Auto-update
- Anonymous telemetry
- Auto-generated API key
- HWID bans
- Announcements
- SOC telemetry (detailed)
- Multi-zone proxy
- File logging
- PROXY protocol v1
- Health check endpoint

## Edge Routing Add-on (+$10/mo)

All of the above, hosted on CrespoGuard's edge infrastructure:

- Full IP masking with zero setup — no VPS needed
- CrespoGuard manages the relay binary, updates, and uptime
- Players connect to `edge.crespoguard.com` with your route code
- All relay features work identically to self-hosted
- Available at any tier — $10/mo on top of your tier price

## v3.1 Features

### ASN-Based Filtering

Block connections from known datacenter and VPN providers by Autonomous System Number (ASN). This catches proxy/VPN traffic that GeoIP alone misses — datacenter IPs are rarely legitimate RF Online players.

```json
{
    "ASNBlocklistEnabled": true,
    "BlockedASNs": [12345, 67890]
}
```

Look up ASN numbers at ipinfo.io. Add ASN numbers to the `BlockedASNs` array. Connections from IPs belonging to those ASNs are rejected before reaching your game server.

### TCP Fingerprinting

Passive OS detection using TCP connection characteristics. Flags connections that don't match a Windows TCP stack — since RF Online only runs on Windows, non-Windows fingerprints indicate bots, proxied traffic, or attack tools.

Flagged connections are logged and optionally blocked. No client-side changes required — detection is fully passive.

```json
{
    "TCPFingerprintEnabled": true,
    "TCPFingerprintAction": "flag"
}
```

Set `TCPFingerprintAction` to `"block"` to drop non-Windows connections, or `"flag"` to log without blocking.

### Prometheus Metrics

Exposes relay metrics on a `/metrics` endpoint in Prometheus exposition format. Grafana-ready out of the box.

```json
{
    "MetricsEnabled": true,
    "MetricsPort": 9090
}
```

See the /metrics endpoint for the full list.

**Restrict the metrics port** to your monitoring infrastructure — do not expose it publicly.

### Adaptive Rate Limiter

Burst-tolerant rate limiting — allows short spikes of legitimate reconnect activity (e.g., after a zone crash) without triggering false positives, while still blocking sustained floods.

```json
{
    "RateLimitMode": "token_bucket",
    "RateLimitTokensPerSec": "<your value>",
    "RateLimitBurstSize": "<your value>"
}
```

Set `RateLimitMode` to `"token_bucket"` to enable. The fixed-window limiter (`"fixed"`) remains available for backward compatibility.

### Tor Exit Node Blocklist

Third threat intelligence source. Automatically fetches and refreshes the Tor exit node list. Connections from known Tor exit nodes are blocked alongside the existing Threat Intel and GeoIP layers.

```json
{
    "TorBlocklistEnabled": true
}
```

The list is refreshed automatically. No manual maintenance required.

### Anti-Replay Protection

Anti-replay protection for the CGRD encrypted tunnel. Detects and rejects replayed packets. Prevents replay attacks without rejecting legitimate out-of-order delivery.

### Async Rotating Logger

Structured log output with automatic rotation. Logs are written asynchronously to avoid blocking the relay's event loop.

```json
{
    "LogRotateEnabled": true,
    "LogMaxSizeMB": 10,
    "LogMaxFiles": 5
}
```

Each log file rotates at 10MB (configurable), keeping up to 5 rotations. Output is structured for easy parsing by log aggregation tools.

### Optimized Proxying

On Linux, the relay uses optimized zero-copy data transfer between sockets for improved throughput.

Enabled automatically on Linux when the kernel supports it. No configuration required. On Windows, the relay falls back to standard proxying.

## Requirements

- A running RF Online server (LoginServer + ZoneServer)
- A Windows or Linux machine for the relay (can be the same machine as your game server)
- The CrespoGuard Relay binary (`CrespoGuardRelay.exe` or Linux build)
- A CrespoGuard license (Community is free for up to 30 players)
- The CrespoGuard Launcher configured with SecureLogin (PSK must match) — required for encrypted tunnel mode; not needed for transparent proxy mode

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
| 27780 TCP | Inbound | ZoneServer (or zone proxy) |
| 8081 TCP | Inbound | Dashboard (restrict to your admin IP!) |

**Block your real LoginServer port** from external access — only the relay (localhost) should reach it.

## Upgrading Tiers

Tier upgrades are instant — purchase the higher tier, activate the new code, and restart the relay. The only thing that changes is your player cap. No client changes needed. See [Premium Tiers](../PREMIUM_TIERS.md) for full tier documentation.

## Auto-Update

The relay checks for new versions on startup. When a newer version is available, it downloads, verifies, and applies the update automatically. No manual intervention required.

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

On first run, if no `DashboardApiKey` is set in `server.json`, the relay automatically generates a secure API key and saves it to the config file. This ensures the dashboard API is protected from the moment the relay starts — no manual key generation needed.
