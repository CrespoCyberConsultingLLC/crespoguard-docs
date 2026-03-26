
# Community Edition: Free Relay for RF Online Servers

> Protect your RF Online server in 5 minutes — hide your IP, ban abusers, rate-limit floods. Free for up to 30 players.

## Overview

The Community tier gives you a **transparent TCP proxy** that works with any vanilla RF Online client — no CrespoGuard Launcher, no ZoneMod, no encryption keys needed.

- **30 concurrent players** max
- **Server IP masking** — players never see your real server IP
- **Read-only dashboard** — see who's connected, stats, and events
- **IP bans + rate limiting** built in
- **IP ban management via dashboard**
- No license key required, no phone-home

```
Player                          Your Server
┌──────────┐     plain TCP     ┌──────────────────┐
│ RF Online ├───────────────────┤ CrespoGuard      │    ┌──────────────┐
│ Client    │   (transparent)   │ Relay             ├────┤ LoginServer  │
│           │                   │ (port 10002)      │    │ (port 10001) │
└──────────┘                   │                   │    └──────────────┘
                               │ IP masking        │
                               │ Rate limiting     │    ┌──────────────┐
                               │ IP bans           ├────┤ ZoneServer   │
                               │ Dashboard         │    │ (port 27780) │
                               └──────────────────┘    └──────────────┘
```

Players connect to the relay IP — your real server IP stays hidden.

## Prerequisites

- A running RF Online server (LoginServer on port 10001, ZoneServer on port 27780)
- A Windows or Linux machine for the relay (can be the same machine as your game server)
- The CrespoGuard Relay binary (`CrespoGuardRelay.exe` or Linux build)

## Step-by-Step Setup

### Step 1: Generate Config

```
CrespoGuardRelay.exe --generate-config server.json
```

This creates a `server.json` with all fields and a generated dashboard API key.

### Step 2: Configure for Community Edition

Edit `server.json` — here's the minimal Community config:

```json
{
    "ListenIP": "0.0.0.0",
    "ListenPort": 10002,
    "TargetIP": "127.0.0.1",
    "TargetPort": 10001,
    "MaxClients": 30,
    "SirinProxy": true,
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

**Key fields:**

| Field | Value | Why |
|-------|-------|-----|
| `SirinProxy` | `true` | **Required for Community** — enables transparent TCP mode |
| `ListenPort` | `10002` | Port players connect to |
| `TargetIP` | `127.0.0.1` | Your real LoginServer (localhost if same machine) |
| `TargetPort` | `10001` | Your LoginServer port |
| `PublicIP` | (your IP) | Used for IP masking — replaces server IP in packets |
| `MaskServerIP` | `true` | Hides real server IP from players |
| `MaxClients` | `30` | Community tier cap |
| `DashboardEnabled` | `true` | Web dashboard for monitoring (read-only at Community tier) |

!!! info "No PSK needed"
    Community edition uses transparent TCP proxying. The `PSK` field in your config is ignored when `SirinProxy` is `true`.

### Step 3: Start the Relay

```
CrespoGuardRelay.exe server.json
```

You'll see:

```
CrespoGuard Relay v3.x
  Server     CrespoGuard Relay
  Tier       Community
  Mode       SIRIN PROXY (transparent TCP + IP protection)
  Listen     0.0.0.0:10002
  Target     127.0.0.1:10001
  Rate limit 15 req/60s, max 5 conn/IP
  Dashboard  http://127.0.0.1:8081
  Ready — accepting connections
```

### Step 4: Update Your Client

Point your RF Online client to connect to the relay instead of directly to the game server:

- Change the login server IP in your client config to `YOUR_PUBLIC_IP`
- Change the login server port to `10002` (or whatever `ListenPort` you set)

That's it. Any vanilla RF Online client works — no CrespoGuard Launcher required.

### Step 5: Access the Dashboard

Open `http://YOUR_SERVER_IP:8081/?key=YOUR_API_KEY` in your browser.

The dashboard shows:

- Connected players (IP, connection time)
- Total connections, bytes relayed
- Rate limit status
- Event log (connections, disconnections, bans)
- IP ban management

!!! note "Community Dashboard is Read-Only"
    At Community tier, the dashboard shows all stats and allows IP ban management. Kick, HWID ban, and announcement features require Guard tier ($19/mo).

## IP Bans

Community tier supports IP-based bans. Create `ipbans.json` next to the relay:

```json
[
    {
        "ip": "192.168.1.100",
        "reason": "Abuse",
        "added": "2026-03-24"
    }
]
```

You can also manage IP bans from the dashboard UI.

The relay hot-reloads ban files every 30 seconds — no restart needed.

## Firewall Rules

| Port | Direction | Purpose |
|------|-----------|---------|
| 10002 TCP | Inbound | Relay listen (transparent proxy) |
| 27780 TCP | Inbound | Direct ZoneServer (or zone proxy at Shield+) |
| 8081 TCP | Inbound | Dashboard (restrict to your admin IP!) |

**Block your real LoginServer port** (10001) from external access — only the relay (localhost) should reach it.

!!! warning "Secure the Dashboard"
    Restrict port 8081 to your admin IP via firewall rules. The dashboard API key provides authentication, but limiting network access adds defense in depth.

## Deployment Layouts

### Layout A: All-in-One Server (Simplest)

Everything on one machine. Relay connects to LoginServer via localhost.

```
Your Server (single machine)
├── CrespoGuardRelay.exe    (listen 0.0.0.0:10002, SirinProxy: true)
│     └─→ LoginServer       (listen 127.0.0.1:10001)
├── AccountServer            (listen 127.0.0.1:27000)
└── ZoneServer               (listen 0.0.0.0:27780)
```

### Layout B: Split Relay + Game Server

Relay on a VPS/proxy, game server behind it.

```
VPS (public IP)                    Game Server (private IP)
├── CrespoGuardRelay.exe          ├── LoginServer
│     └─→ TCP to game server      ├── AccountServer
│         (TargetIP: 10.0.0.5)    └── ZoneServer
```

Set `TargetIP` to the game server's private/internal IP.

## Upgrading to Guard

When you outgrow 30 players or need HWID bans, kick/ban from dashboard, and announcements:

1. Purchase a Guard license ($19/mo) from CrespoGuard
2. Run `CrespoGuardRelay.exe --activate YOUR-CODE`
3. Restart the relay — tier upgrades automatically
4. Dashboard write operations, HWID bans, kick, announce commands unlock
5. Optionally switch to CGRD encrypted mode with the CrespoGuard Launcher

No client changes needed for the tier upgrade — upgrades are server-side only.

## Premium Setup (CGRD Encrypted Tunnel)

If you upgrade to Guard+ and want encrypted connections, you'll need:

- **CrespoGuard Launcher** — branded launcher with anti-cheat
- **A Pre-Shared Key (PSK)** — 64 hex character key shared between launcher and relay
- Set `SirinProxy` to `false` (or remove it) to enable CGRD mode

See [Premium Tiers](PREMIUM_TIERS.md) for full paid tier documentation.
