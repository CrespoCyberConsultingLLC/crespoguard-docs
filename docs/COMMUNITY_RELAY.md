
# Community Edition: Launcher + Relay Integration

> How to run the free CrespoGuard Launcher with the free CrespoGuard Relay for encrypted player connections.

## Overview

The Community tier gives you:

- **CrespoGuard Launcher** — fully branded, white-label launcher with anti-cheat
- **CrespoGuard Relay** — AES-256-GCM encrypted tunnel between players and your login server
- **50 concurrent players** max
- **IP bans, rate limiting, connection limits** built in
- No license key required, no phone-home

```
Player                          Your Server
┌──────────┐    AES-256-GCM    ┌──────────────────┐
│ Launcher ├───────────────────┤ CrespoGuard      │    ┌──────────────┐
│          │   encrypted TCP   │ Relay             ├────┤ LoginServer  │
│          │                   │ (port 10001)      │    │ (port 10001) │
└──────────┘                   │                   │    └──────────────┘
                               │ Rate limiting     │
                               │ IP bans           │    ┌──────────────┐
                               │ Connection limits  ├────┤ ZoneServer   │
                               └──────────────────┘    │ (port 27780) │
                                                       └──────────────┘
```

Players never see your real login/zone server IP — the relay masks it.

## Prerequisites

- A running RF Online server (AccountServer on port 27000, LoginServer on port 10001, ZoneServer on port 27780)
- A Windows or Linux machine for the relay (can be the same machine as your game server)
- The CrespoGuard Community package

## Step-by-Step Setup

### Step 1: Generate a Pre-Shared Key

Both the launcher and relay need the same 256-bit key. Generate one:

=== "Python (any OS)"

    ```bash
    python -c "import secrets; print(secrets.token_hex(32))"
    ```

=== "PowerShell (Windows)"

    ```powershell
    -join ((1..32) | ForEach-Object { '{0:x2}' -f (Get-Random -Max 256) })
    ```

!!! info "Same key in both configs"
    Save the generated key — you'll use it in both `modules.json` and `server.json`. They must be identical.

### Step 2: Configure the Relay (server.json)

Create `server.json` next to `CrespoGuardRelay.exe`:

```json
{
  "ServerName": "Your Server Name",
  "ListenIP": "0.0.0.0",
  "ListenPort": 10001,
  "TargetIP": "127.0.0.1",
  "TargetPort": 10001,
  "PSK": "YOUR_64_CHAR_HEX_KEY_HERE",
  "MaxClients": 50,
  "LogLevel": 1,
  "EnableLicenseCheck": false,
  "RequireHWIDMatch": false,
  "AllowMasterKey": true,
  "DashboardEnabled": false,
  "DashboardPort": 8080,
  "BansFile": "bans.json",
  "Announcement": "",
  "RateLimitPerIP": 15,
  "RateLimitWindowSec": 60,
  "MaxConnectionsPerIP": 5,
  "ZoneProxyEnabled": false,
  "ZoneProxyListenIP": "0.0.0.0",
  "ZoneProxyListenPort": 27780,
  "ZoneTargetIP": "127.0.0.1",
  "ZoneTargetPort": 27780,
  "ZoneMaxClients": 50
}
```

**Key fields:**

| Field | Value | Why |
|-------|-------|-----|
| `ListenIP` | `0.0.0.0` | Accept connections from all IPs |
| `ListenPort` | `10001` | Port players connect to (same as RF standard) |
| `TargetIP` | `127.0.0.1` | Your real LoginServer (localhost if same machine) |
| `TargetPort` | `10001` | Your LoginServer port |
| `PSK` | (your key) | Must match launcher modules.json |
| `MaxClients` | `50` | Community tier cap |
| `EnableLicenseCheck` | `false` | No license validation at Community tier |
| `RateLimitPerIP` | `15` | Max login attempts per IP per window |
| `MaxConnectionsPerIP` | `5` | Max concurrent connections per IP |

### Step 3: Configure the Launcher (modules.json)

Add the `SecureLogin` section to your `modules.json`:

```json
{
  "ServerConfig": {
    "ServerName": "Your Server Name",
    "LoginServerIp": "YOUR.PUBLIC.SERVER.IP",
    "LoginServerPort": 10001,
    "ZoneServerIp": "YOUR.PUBLIC.SERVER.IP",
    "ZoneServerPort": 27780
  },
  "SecureLogin": {
    "EnableSecureLogin": true,
    "SecureLoginType": 1,
    "SecureLoginHost": "YOUR.PUBLIC.SERVER.IP",
    "SecureLoginIP": "YOUR.PUBLIC.SERVER.IP",
    "SecureLoginPort": 10001,
    "SecureLoginPSK": "YOUR_64_CHAR_HEX_KEY_HERE",
    "EnableZoneProxy": false,
    "ZoneProxyIP": "",
    "ZoneProxyPort": 27780
  }
}
```

**Critical:** The `SecureLoginPSK` must be identical to the `PSK` in `server.json`.

### Step 4: Re-encrypt Config

```powershell
& "path\to\admin\RFLauncher.exe" --encrypt-config
```

### Step 5: Start the Relay

```
CrespoGuardRelay.exe
```

You'll see:

```
CrespoGuard Relay v2.x
  Server     Your Server Name
  Tier       Community
  Listen     0.0.0.0:10001
  Target     127.0.0.1:10001
  Rate limit 15 req/60s, max 5 conn/IP
  Ready — accepting connections
```

### Step 6: Test

Launch `RFLauncher.exe` on a player machine. The sidebar should show "ONLINE" and you should be able to log in through the encrypted tunnel.

## Zone Proxy (Optional)

The zone proxy hides your real ZoneServer IP from players. Without it, players connect directly to ZoneServer after login — their game client sees the real IP.

To enable:

**server.json:**
```json
{
  "ZoneProxyEnabled": true,
  "ZoneProxyListenIP": "0.0.0.0",
  "ZoneProxyListenPort": 27780,
  "ZoneTargetIP": "127.0.0.1",
  "ZoneTargetPort": 27780
}
```

**modules.json:**
```json
{
  "SecureLogin": {
    "EnableZoneProxy": true,
    "ZoneProxyIP": "YOUR.PUBLIC.SERVER.IP",
    "ZoneProxyPort": 27780
  }
}
```

With zone proxy enabled, the launcher directs the game client to connect through the relay instead of directly to your ZoneServer. Players never see the real zone IP.

## Firewall Rules

| Port | Direction | Purpose |
|------|-----------|---------|
| 10001 TCP | Inbound | Relay listen (encrypted login) |
| 27780 TCP | Inbound | Zone proxy (if enabled) or direct ZoneServer |
| 8080 TCP | Inbound | Dashboard (Guard+ only, block for Community) |

**Block port 10001 on your real LoginServer** from external access — only the relay (localhost) should reach it.

## IP Bans

Community tier supports IP-based bans. Create `bans.json` next to the relay:

```json
[
  {
    "hwid": "192.168.1.100",
    "reason": "Abuse",
    "added": "2026-03-24"
  }
]
```

!!! note
     At Community tier, `hwid` field is used for IP addresses. HWID-based bans require Guard+ tier.

The relay hot-reloads `bans.json` every 30 seconds — no restart needed.

## Console Commands (Community Tier)

| Command | Description |
|---------|-------------|
| `status` | Show relay status, connected clients, rate limits |
| `reload` | Reload bans.json and config |
| `help` | Show available commands |
| `quit` | Graceful shutdown |

Commands like `kick`, `ban`, `unban`, `announce`, and `events` require Guard+ tier.

## Deployment Layouts

### Layout A: All-in-One Server (Simplest)

Everything on one machine. Relay connects to LoginServer via localhost.

```
Your Server (single machine)
├── CrespoGuardRelay.exe    (listen 0.0.0.0:10001)
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
└── Zone Proxy (optional)
      └─→ TCP to game server
```

Set `TargetIP` to the game server's private/internal IP.

## Upgrading to Guard

When you outgrow 50 players or need HWID bans, dashboards, and announcements:

1. Purchase a Guard license ($15/mo) from CrespoGuard
2. Run `CrespoGuardRelay.exe --activate YOUR-CODE`
3. Restart the relay — tier upgrades automatically
4. Dashboard, HWID bans, kick/ban/announce commands unlock

No launcher changes needed — the upgrade is server-side only.
