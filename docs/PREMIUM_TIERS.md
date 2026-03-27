
# CrespoGuard Tier Reference

> Every tier includes every feature. The only difference is how many players your server supports.

## Tier Overview

| Tier | Price | Max Players | Edge Routing |
|------|-------|-------------|--------------|
| **Community** | Free | 30 | +$10/mo optional |
| **Guard** | $19/mo | 200 | +$10/mo optional |
| **Shield** | $49/mo | 500 | +$10/mo optional |
| **Fortress** | $99/mo | 1000 | +$10/mo optional |

All tiers include the complete CrespoGuard feature set: self-hosted relay, AES-256-GCM encrypted tunnel, HWID bans, kick/announce from dashboard, all DDoS protection layers, Prometheus metrics, auto-update, TCP fingerprinting, threat intel, zone proxy, and every other feature. Edge routing is an optional add-on. See [Features](FEATURES.md) for the full list.

## Edge Routing Add-on (+$10/mo)

Edge routing is an optional add-on available at any tier. It puts the CrespoGuard Relay on CrespoGuard's edge infrastructure instead of your own machine.

**What it does:**
- CrespoGuard hosts the relay for you — no VPS to provision, configure, or maintain
- Full IP masking out of the box — players connect to `edge.crespoguard.com` with your route code and never see your game server IP
- All relay features (DDoS protection, encrypted tunnel, dashboard, HWID bans) work identically

**What it costs:**
- +$10/mo on top of your tier price, at any tier
- Community + Edge = $10/mo (30 players, hosted relay)
- Guard + Edge = $29/mo (200 players, hosted relay)
- Shield + Edge = $59/mo (500 players, hosted relay)
- Fortress + Edge = $109/mo (1000 players, hosted relay)

**How it works:**
1. Purchase the Edge Routing add-on
2. Receive a route code for your server
3. Configure your launcher's `SecureLoginHost` to `edge.crespoguard.com` with your route code
4. Done — no relay binary to run, no VPS to manage

**Do you need it?** Not necessarily. If you already run the relay on a separate machine or VPS from your game server, you already have IP masking and all features. Edge routing is a convenience add-on for server owners who don't want to manage a separate VPS. Self-hosted relay with all DDoS protection features is free at every tier.

## Choosing Your Tier

Pick the tier that matches your player count. Optionally add Edge Routing if you want hosted relay with zero setup.

| Your Situation | Recommended Tier |
|---------------|-----------------|
| Starting out, testing, or running a small community (up to 30 players) | **Community** (Free) |
| Growing server, up to 200 concurrent players | **Guard** ($19/mo) |
| Mid-size server, up to 500 concurrent players | **Shield** ($49/mo) |
| Large server, up to 1000 concurrent players | **Fortress** ($99/mo) |
| Want hosted relay with IP masking and zero setup at any tier | Add **Edge Routing** (+$10/mo) |

Start free. Upgrade when your server outgrows 30 players. Add edge routing whenever you want hosted relay without managing a VPS.

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
