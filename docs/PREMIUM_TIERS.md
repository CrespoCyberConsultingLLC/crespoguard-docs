
# CrespoGuard Tier Reference

> Every tier includes every feature. The only difference is how many players your server supports.

## Tier Overview

| Tier | Price | Max Players |
|------|-------|-------------|
| **Community** | Free | 30 |
| **Guard** | $19/mo | 200 |
| **Shield** | $49/mo | 500 |
| **Fortress** | $99/mo | 1000 |

All tiers include the complete CrespoGuard feature set: hosted edge relay, full IP masking, AES-256-GCM encrypted tunnel, HWID bans, kick/announce from dashboard, all DDoS protection layers, Prometheus metrics, auto-update, TCP fingerprinting, threat intel, zone proxy, and every other feature. See [Features](FEATURES.md) for the full list.

## Choosing Your Tier

Pick the tier that matches your player count. That's it.

| Your Situation | Recommended Tier |
|---------------|-----------------|
| Starting out, testing, or running a small community (up to 30 players) | **Community** (Free) |
| Growing server, up to 200 concurrent players | **Guard** ($19/mo) |
| Mid-size server, up to 500 concurrent players | **Shield** ($49/mo) |
| Large server, up to 1000 concurrent players | **Fortress** ($99/mo) |

Start free. Upgrade when your server outgrows 30 players.

## Activation

1. Purchase a subscription from CrespoGuard
2. Receive an activation code (`XXXX-XXXX-XXXX-XXXX`)
3. On your server machine:
   ```
   CrespoGuardRelay.exe --activate YOUR-ACTIVATION-CODE
   ```
4. The license key is auto-saved to your config JSON
5. Restart the relay — the player cap increases immediately

**License validation** runs on every startup. If the license server is unreachable, a 24-hour local cache allows continued operation.

## Upgrading

Tier upgrades are instant:

1. Purchase the higher tier
2. Receive a new activation code
3. Run `--activate` with the new code
4. Restart the relay

No config changes needed. The player cap increases immediately. No launcher changes — upgrades are server-side only.

## Downgrading

If you downgrade or your subscription lapses:

- Player cap adjusts to match your current tier
- No data loss — bans, config, and logs are preserved
- All features remain available at every tier
- License validation is required on startup; a 24-hour offline cache allows brief connectivity interruptions
