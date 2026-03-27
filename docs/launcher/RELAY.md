
# CrespoGuard Relay

> TCP relay for RF Online servers — transparent proxy or AES-256-GCM encrypted tunnel, DDoS protection, rate limiting, and a real-time dashboard. Free for up to 30 players.

## Overview

The CrespoGuard Relay sits between your players and your game server. It provides DDoS protection, rate limiting, and a management dashboard. Two connection modes are available, both included in all tiers (including Community):

**Transparent Proxy (any RF client)** — Plain TCP proxy. Works with any vanilla RF client. No launcher required.

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

**Encrypted Tunnel (CrespoGuard Launcher)** — AES-256-GCM encrypted tunnel. Requires CrespoGuard Launcher. Encrypts player credentials and game data in transit. Available in all tiers including Community.

```
Player                          Your Server
┌──────────┐    AES-256-GCM    ┌──────────────────┐
│ CrespoGd ├───────────────────┤ CrespoGuard      │    ┌──────────────┐
│ Launcher  │   (encrypted)     │ Relay             ├────┤ LoginServer  │
│           │                   │ (port 10001)      │    │ (port 10001) │
└──────────┘                   │                   │    └──────────────┘
                               │ Everything above  │
                               │ + Encrypted tunnel│    ┌──────────────┐
                               │                   ├────┤ ZoneServer   │
                               │                   │    │ (port 27780) │
                               └──────────────────┘    └──────────────┘
```

Both modes are available in all tiers, including Community. The transparent proxy works with any vanilla RF client; the encrypted tunnel requires the CrespoGuard Launcher. Players connect to the relay, not your game server — your game server IP stays hidden when the relay runs on a separate machine.

## What's Included

| Feature | Community (Free) | Guard ($19/mo) | Shield ($49/mo) | Fortress ($79/mo) |
|---------|:----------------:|:--------------:|:---------------:|:------------------:|
| Transparent TCP proxy | Yes | Yes | Yes | Yes |
| CGRD encrypted tunnel (requires launcher) | Yes | Yes | Yes | Yes |
| IP masking (requires separate machine) | Yes | Yes | Yes | Yes |
| Dashboard (view + IP bans) | Yes | Yes | Yes | Yes |
| Dashboard (kick, HWID bans, announce) | — | Yes | Yes | Yes |
| Rate limiting + IP bans | Yes | Yes | Yes | Yes |
| Auto-ban (progressive backoff) | Yes | Yes | Yes | Yes |
| GeoIP country filtering | Yes | Yes | Yes | Yes |
| Threat Intel blocklist (200K+ IPs) | Yes | Yes | Yes | Yes |
| ASN-based filtering (block datacenter/VPN IPs) | Yes | Yes | Yes | Yes |
| TCP fingerprinting (passive OS detection) | Yes | Yes | Yes | Yes |
| Tor exit node blocklist | Yes | Yes | Yes | Yes |
| Adaptive rate limiter (burst-tolerant) | Yes | Yes | Yes | Yes |
| Anti-replay protection | Yes | Yes | Yes | Yes |
| Prometheus /metrics endpoint | Yes | Yes | Yes | Yes |
| Async rotating logger (structured output) | Yes | Yes | Yes | Yes |
| Optimized proxying (zero-copy on Linux) | Yes | Yes | Yes | Yes |
| Auto-update | Yes | Yes | Yes | Yes |
| Anonymous telemetry | Yes | Yes | Yes | Yes |
| Auto-generated API key | Yes | Yes | Yes | Yes |
| HWID bans | — | Yes | Yes | Yes |
| Announcements | — | Yes | Yes | Yes |
| SOC telemetry (detailed) | — | Yes | Yes | Yes |
| Max players | **30** | 75 | 250 | 500 |
| Multi-zone proxy | — | — | Yes | Yes |
| File logging | — | — | Yes | Yes |
| Edge relay mode | — | — | — | Yes |
| PROXY protocol v1 | — | — | — | Yes |
| Health check endpoint | — | — | — | Yes |

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
- A CrespoGuard license (Community for up to 30 players, Guard+ for higher caps)
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
| 27780 TCP | Inbound | ZoneServer (or zone proxy at Shield+) |
| 8081 TCP | Inbound | Dashboard (restrict to your admin IP!) |

**Block your real LoginServer port** from external access — only the relay (localhost) should reach it.

## Upgrading Tiers

Tier upgrades are instant — purchase the higher tier, activate the new code, and restart the relay. No client changes needed. See [Premium Tiers](../PREMIUM_TIERS.md) for full tier documentation.

## Community Edition (Free Relay — 30 Players)

The Community Edition includes the CrespoGuard Relay for up to 30 concurrent players. You get both connection modes (transparent proxy and AES-256-GCM encrypted tunnel), DDoS protection (rate limiting, GeoIP filtering, Threat Intel blocklist, auto-ban with progressive backoff), and full dashboard access — all at no cost. The encrypted tunnel requires the CrespoGuard Launcher; the transparent proxy works with any vanilla RF client. When the relay runs on a separate machine from your game server, players connect to the relay address and never see your game server IP.

The 30-player cap is a **hard limit enforced at the binary level**. It cannot be bypassed by editing the config file — the relay binary itself rejects connections beyond 30. When you need more capacity, HWID bans, kick/announce from the dashboard, or higher player caps, upgrade to Guard tier ($19/mo, 75 players).

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
