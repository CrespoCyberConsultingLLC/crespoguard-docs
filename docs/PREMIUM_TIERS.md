
# CrespoGuard Premium Tier Reference

> Complete breakdown of every paid tier — what unlocks, how to configure it, and when to upgrade.

## Tier Overview

| | Community | Guard | Shield | Fortress |
|---|:---------:|:-----:|:------:|:--------:|
| **Price** | Free | $19/mo | $49/mo | $79/mo |
| **Max Players** | 30 | 75 | 250 | 500 |
| **Launcher + Branding** | Yes | Yes | Yes | Yes |
| **Anti-Cheat Suite** | Yes | Yes | Yes | Yes |
| **Multi-Client** | Yes | Yes | Yes | Yes |
| **Discord Rich Presence** | Yes | Yes | Yes | Yes |
| **Locale Emulation (CP949)** | Yes | Yes | Yes | Yes |
| **Asset Decryption (CGEF)** | Yes | Yes | Yes | Yes |
| **FreeGuard/Cerberus Suppression** | Yes | Yes | Yes | Yes |
| **Account Manager** | Yes | Yes | Yes | Yes |
| **Compatibility Checker** | Yes | Yes | Yes | Yes |
| **Borderless Windowed Mode** | Yes | Yes | Yes | Yes |
| **Clean Room Mode** | Yes | Yes | Yes | Yes |
| **Encrypted Login (AES-256-GCM)** | Yes | Yes | Yes | Yes |
| **Auto-Update System** | — | Yes | Yes | Yes |
| **CrespoGuard Relay** | Yes (30 players) | Yes | Yes | Yes |
| **CGRD Encrypted Tunnel** | Yes | Yes | Yes | Yes |
| **Server IP Masking (requires separate machine)** | Yes | Yes | Yes | Yes |
| **Rate Limiting + IP Bans** | Yes | Yes | Yes | Yes |
| **DDoS Protection (GeoIP, Threat Intel, auto-ban)** | Yes | Yes | Yes | Yes |
| **ASN-Based Filtering** | Yes | Yes | Yes | Yes |
| **TCP Fingerprinting (passive OS detection)** | Yes | Yes | Yes | Yes |
| **Tor Exit Node Blocklist** | Yes | Yes | Yes | Yes |
| **Adaptive Rate Limiter (Burst-Tolerant)** | Yes | Yes | Yes | Yes |
| **Anti-Replay Protection** | Yes | Yes | Yes | Yes |
| **Prometheus /metrics Endpoint** | Yes | Yes | Yes | Yes |
| **Async Rotating Logger** | Yes | Yes | Yes | Yes |
| **Optimized Proxying (Zero-Copy on Linux)** | Yes | Yes | Yes | Yes |
| **Dashboard (view + config)** | Yes | Yes | Yes | Yes |
| **Dashboard (kick, HWID bans, announcements)** | — | Yes | Yes | Yes |
| **Relay Auto-Update** | Yes | Yes | Yes | Yes |
| **License Validation** | Yes | Yes | Yes | Yes |
| **Telemetry (anonymous usage stats)** | Yes | Yes | Yes | Yes |
| **Bin-Dependent Features ¹** | — | Yes | Yes | Yes |
| **Combat Automation ¹** | — | Yes | Yes | Yes |
| **Chat Overlay ¹** | — | Yes | Yes | Yes |
| **Multi-Zone Proxy** | — | — | Yes | Yes |
| **File Logging** | — | — | Yes | Yes |
| **Edge Relay Mode** | — | — | — | Yes |
| **PROXY Protocol v1** | — | — | — | Yes |
| **Health Check Endpoint** | — | — | — | Yes |

¹ Bin-dependent features (stack patches, name colors, FOV/camera, display fixes, hunter points, options panel, combat automation, chat overlay, bot patches) require **both** a Guard+ relay license **and** a ZoneMod license, plus the supported RF Online 2.2.3.2 binary. This is a dual-license model: the relay tier controls server infrastructure access, while ZoneMod controls gameplay feature modules. On unsupported binaries or without both licenses, these features gracefully deactivate.


## Shield Tier ($49/mo)

Everything in Guard, plus multi-zone proxy and file logging. Designed for servers with 75-250 concurrent players.

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


## Choosing Your Tier

| Your Situation | Recommended Tier |
|---------------|-----------------|
| Launcher + anti-cheat + relay (transparent proxy + encrypted tunnel, up to 30 players) + DDoS protection + auto-update + telemetry + dashboard | **Community** (Free) |
| Need HWID bans, kick/announce from dashboard, higher player cap (75), bin-dependent features | **Guard** ($19/mo) |
| 75-250 players, need zone IP masking, file logging | **Shield** ($49/mo) |
| 250+ players, multi-region, edge relays, load balancing | **Fortress** ($79/mo) |

## Activation

1. Purchase a subscription from CrespoGuard
2. Receive an activation code (`XXXX-XXXX-XXXX-XXXX`)
3. On your server machine:
   ```
   CrespoGuardRelay.exe --activate YOUR-ACTIVATION-CODE
   ```
4. The license key is auto-saved to your config JSON
5. Restart the relay — tier upgrades automatically

**License validation** runs on every startup. If the license server is unreachable, a 24-hour local cache allows continued operation.

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
