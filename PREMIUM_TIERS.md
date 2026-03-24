---
title: Premium Tiers
layout: default
nav_order: 9
---

# CrespoGuard Premium Tier Reference

> Complete breakdown of every paid tier — what unlocks, how to configure it, and when to upgrade.

## Tier Overview

| | Community | Guard | Shield | Fortress |
|---|:---------:|:-----:|:------:|:--------:|
| **Price** | Free | $15/mo | $30/mo | $50-75/mo |
| **Max Players** | 50 | 50 | 200 | 500 |
| **Encrypted Relay** | Yes | Yes | Yes | Yes |
| **Rate Limiting + IP Bans** | Yes | Yes | Yes | Yes |
| **Anti-Cheat Suite** | Yes | Yes | Yes | Yes |
| **Dashboard** | — | Yes | Yes | Yes |
| **HWID Bans** | — | Yes | Yes | Yes |
| **Announcements** | — | Yes | Yes | Yes |
| **Server IP Masking** | — | Yes | Yes | Yes |
| **License Validation** | — | Yes | Yes | Yes |
| **Telemetry (SOC)** | — | Yes | Yes | Yes |
| **Combat Automation** | — | Yes | Yes | Yes |
| **Chat Overlay** | — | Yes | Yes | Yes |
| **Multi-Zone Proxy** | — | — | Yes | Yes |
| **File Logging** | — | — | Yes | Yes |
| **Edge Relay Mode** | — | — | — | Yes |
| **PROXY Protocol v1** | — | — | — | Yes |
| **Health Check Endpoint** | — | — | — | Yes |

---

## Guard Tier ($15/mo)

The first paid tier. Unlocks management, security, and client-side premium features.

### Web Dashboard

A built-in web UI for monitoring and managing your relay. Accessible at `http://YOUR_IP:8080`.

**What you see:**
- Live player list with HWID prefix and license expiry
- HWID and IP ban management (add/remove bans from the browser)
- Event log (connections, disconnections, bans, rejections)
- Server metrics (uptime, total connections, rejection counts, bytes relayed)

**Configuration:**
```json
{
  "DashboardEnabled": true,
  "DashboardPort": 8080,
  "DashboardBindIP": "0.0.0.0",
  "DashboardApiKey": ""
}
```

The dashboard provides a REST API for programmatic access (status, players, bans, events, kick, ban, announce).

> **Security:** The dashboard **must** be restricted to admin IPs via firewall. Set `DashboardApiKey` to require authentication on all API requests. Never expose the dashboard port to the public internet.

### HWID Bans

Ban players by hardware ID instead of just IP (which they can change).

**Console:**
```
ban abc123def456... Exploiting
unban abc123def456...
bans
```

**Ban file (`bans.json`):**
```json
[
  {
    "hwid": "abc123def456789...",
    "reason": "Exploiting",
    "added": "2026-03-24",
    "expiry": "2026-06-24"
  }
]
```

- Temporary bans: set `expiry` date, auto-expires
- Hot-reload: file checked every 30 seconds, no restart needed
- Newly-banned connected clients are kicked immediately

### Announcements

Broadcast a message to all connected launcher clients:

```
announce Server maintenance in 30 minutes
```

Or set a persistent announcement in `server.json`:
```json
{
  "Announcement": "Welcome to our server! Join our Discord: discord.gg/..."
}
```

### Server IP Masking

Hides your real server IP from players. The relay rewrites IP addresses in outbound RF protocol packets.

```json
{
  "MaskServerIP": true,
  "PublicIP": "relay.yourserver.com"
}
```

Players see the relay IP, never the backend LoginServer/ZoneServer address.

### License Validation

Verify each connecting launcher has a valid license key:

```json
{
  "EnableLicenseCheck": true,
  "RequireHWIDMatch": true
}
```

- Validates each connecting launcher's license on CGRD handshake
- Optionally enforces HWID matching (key locked to specific machine)
- Invalid or expired licenses are rejected before reaching your login server

### Telemetry (SOC Dashboard Integration)

Pushes relay metrics to the CrespoGuard admin portal every 60 seconds:

```json
{
  "EnableTelemetry": true,
  "TelemetryIntervalSec": 60
}
```

**Metrics pushed:**
- Uptime, active/total connections, rejection counts
- Attack counters (bad magic, handshake failures, auth failures)
- Ban delta tracking
- Bytes relayed
- CPU%, memory usage
- Top 20 source IPs

Viewable in the SOC unified dashboard (CrespoGuard LoginServer).

### Client Premium Features

Guard tier unlocks premium client-side features for your players:

- **Auto-loot** — Automatic item pickup with configurable interval
- **Auto-target** — Tab cycling through nearby entities with skip set
- **Auto-attack** — Spacebar automation on valid targets
- **Attack-on-target** — Click-to-attack (select mob and instantly attack)
- **Target name filter** — Focus farming specific mob names
- **Combat assist** — Combined targeting + attack automation
- **Speed modification** — Movement speed overrides
- **Chat overlay** — Custom ImGui chat window with history and filtering

These are gated by the Premium flag in `cg_settings.ini`, set by the login server response.

### Console Commands (Guard+)

| Command | Description |
|---------|-------------|
| `status` | Relay status, connected clients, rate limits |
| `kick <id>` | Disconnect a client |
| `ban <hwid> [reason]` | Add HWID ban |
| `unban <hwid>` | Remove HWID ban |
| `ipban <ip> [reason]` | Add IP ban |
| `announce <message>` | Broadcast to all clients |
| `bans` | List all bans |
| `events` | Show recent events |
| `reload` | Reload bans + config |
| `quit` | Graceful shutdown |

---

## Shield Tier ($30/mo)

Everything in Guard, plus multi-zone proxy and file logging. Designed for servers with 50-200 concurrent players.

### Multi-Zone Proxy

Route zone traffic through the relay to hide your ZoneServer IP. Supports multiple zone servers with independent configurations.

```json
{
  "ZoneProxyEnabled": true,
  "ZoneProxies": [
    {
      "Label": "Zone 1",
      "ListenIP": "0.0.0.0",
      "ListenPort": 27780,
      "TargetIP": "10.0.0.5",
      "TargetPort": 27780,
      "MaxClients": 200
    },
    {
      "Label": "Zone 2",
      "ListenIP": "0.0.0.0",
      "ListenPort": 27781,
      "TargetIP": "10.0.0.6",
      "TargetPort": 27780,
      "MaxClients": 200
    }
  ]
}
```

**How it works:**
1. Player logs in through the CGRD relay (login traffic encrypted)
2. Launcher directs the game client to connect through the proxy
3. RF_Online.bin connects to the zone proxy port
4. Relay validates the connection (must have an active CGRD session)
5. Traffic forwarded transparently to backend ZoneServer

**Zone proxy stats** appear in the dashboard: active connections, total connections, packets/bytes relayed.

Zone proxy masks the backend ZoneServer IP from players — they only see the relay address.

### File Logging

All relay log output written to file for audit/debugging:

```json
{
  "LogToFile": true,
  "LogFile": "relay.log"
}
```

Useful for post-incident analysis and compliance. File grows without rotation — manage externally or use logrotate on Linux.

---

## Fortress Tier ($50-75/mo)

Everything in Shield, plus edge relay, PROXY protocol, and health checks. Designed for large servers (200-500 players) or multi-region deployments.

### Edge Relay Mode

Run CrespoGuard relays at the network edge (e.g., US East, EU West, Asia) to reduce latency for global player bases. Players connect to the nearest edge, which routes to your central backend.

```
Player (US East)  ──→  Edge Relay (NYC)  ──→  Backend (Chicago)
Player (EU West)  ──→  Edge Relay (AMS)  ──→  Backend (Chicago)
Player (Asia)     ──→  Edge Relay (SGP)  ──→  Backend (Chicago)
```

**Edge relay config (`server.json` on the edge node):**
```json
{
  "Mode": "edge",
  "ListenIP": "0.0.0.0",
  "ListenPort": 10001,
  "PSK": "same_as_backend_psk",
  "Routes": [
    {
      "RoutingCode": "us-east-1",
      "Label": "US East",
      "LoginIP": "10.0.0.5",
      "LoginPort": 10001,
      "ZoneIP": "10.0.0.5",
      "ZonePort": 27780
    }
  ],
  "EdgeZoneListenIP": "0.0.0.0",
  "EdgeZoneListenPort": 27781,
  "EdgeZoneMaxClients": 500
}
```

**Launcher config (modules.json):**
```json
{
  "SecureLogin": {
    "EnableEdgeRelays": true,
    "AutoSelectRelay": true,
    "EdgeRelays": [
      { "Label": "US East", "Host": "edge-us.yourserver.com", "Port": 10001, "ZonePort": 27781 },
      { "Label": "EU West", "Host": "edge-eu.yourserver.com", "Port": 10001, "ZonePort": 27781 },
      { "Label": "Asia", "Host": "edge-sg.yourserver.com", "Port": 10001, "ZonePort": 27781 }
    ]
  }
}
```

**How edge routing works:**
1. Launcher measures TCP latency to each edge relay
2. Auto-selects the fastest (or player chooses manually)
3. Connects to edge with CGRD v2 handshake
4. Sends `CMD_ROUTE { routing_code }` to identify the target backend
5. Edge opens CGRD v2 connection to backend relay
6. Forwards license validation, then bidirectional relay
7. Zone connections routed via `EdgeSessionManager` (maps client IP → route)

### PROXY Protocol v1

Standard load balancer protocol (HAProxy, AWS NLB, Cloudflare Spectrum compatible).

**Send mode** (edge relay → backend):
```json
{
  "ProxyProtocolSend": true
}
```
Edge sends `PROXY TCP4 <client_ip> <dest_ip> <client_port> <dest_port>\r\n` before forwarding traffic to backend. Backend sees the real player IP, not the edge relay IP.

**Accept mode** (load balancer → standard relay):
```json
{
  "ProxyProtocolAccept": true,
  "ProxyProtocolTrustedIPs": ["10.0.0.1", "10.0.0.2"]
}
```
Only accepts PROXY headers from trusted IPs (load balancers). All other connections are treated as direct.

### Health Check Endpoint

HTTP endpoint for load balancer health monitoring:

```json
{
  "HealthCheckEnabled": true,
  "HealthCheckPort": 8081
}
```

Returns HTTP 200 when the relay is running. Use with AWS ALB/NLB, HAProxy, or any load balancer that supports HTTP health checks.

---

## Choosing Your Tier

| Your Situation | Recommended Tier |
|---------------|-----------------|
| Small community, < 50 players, just need a launcher | **Community** (Free) |
| Want HWID bans, dashboard, combat features | **Guard** ($15/mo) |
| 50-200 players, need zone IP masking | **Shield** ($30/mo) |
| 200+ players, multi-region, load balancing | **Fortress** ($50-75/mo) |

## Activation

1. Purchase a subscription from CrespoGuard
2. Receive an activation code (`XXXX-XXXX-XXXX-XXXX`)
3. On your server machine:
   ```
   CrespoGuardRelay.exe --activate YOUR-ACTIVATION-CODE
   ```
4. The license key is auto-saved to your config JSON
5. Restart the relay — tier upgrades automatically

**License validation** runs on every startup (POST to `/api/v1/validate`). If the license server is unreachable, a 24-hour local cache allows continued operation.

## Upgrading

Tier upgrades are instant:

1. Purchase the higher tier
2. Receive a new activation code
3. Run `--activate` with the new code
4. Restart the relay

No config changes needed (unless enabling new features like edge mode). The player cap increases immediately. No launcher changes — upgrades are server-side only.

## Downgrading

If you downgrade or your subscription lapses:

- Features above your current tier require an active license and will not function
- Player cap adjusts to match your current tier
- No data loss — bans, config, and logs are preserved
- License validation is required on startup; a 24-hour offline cache allows brief connectivity interruptions
