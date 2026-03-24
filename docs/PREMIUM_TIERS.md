
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
| **Sirin SDK Integration** | — | Yes | Yes | Yes |


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
