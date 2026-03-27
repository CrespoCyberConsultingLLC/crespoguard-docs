
# CrespoGuard Features

> Everything included with CrespoGuard. Every feature, every tier — the only difference is player count.

## Tier Pricing

| Tier | Price | Max Players | Edge Routing |
|------|-------|-------------|--------------|
| **Community** | Free | 30 | +$10/mo optional |
| **Guard** | $19/mo | 200 | +$10/mo optional |
| **Shield** | $49/mo | 500 | +$10/mo optional |
| **Fortress** | $99/mo | 1000 | +$10/mo optional |

All tiers include every feature listed below. Pick the tier that matches your server's player count.

**Edge Routing (+$10/mo add-on):** CrespoGuard hosts the relay on their edge infrastructure, giving you full IP masking with zero setup. Without the add-on, you run the relay yourself — all features still work. Self-hosted relay on a separate machine from your game server also provides IP masking. Edge routing is a convenience for server owners who don't want to manage a VPS. See [Relay](launcher/RELAY.md) for details on both deployment options.

## Launcher Features

- Custom server branding (name, logo, background)
- Full color theme customization (9 colors)
- Custom fonts (body, bold, title)
- Effect controls (scan lines, particles, glow)
- Configurable window size and layout
- Multi-language support (8 languages)
- Encrypted login (AES-256-GCM tunnel, requires relay + launcher)
- Account manager (save/switch accounts)
- Auto-update system (patch server)
- World selection with status indicators
- System tray minimization
- Game process watchdog (Job Object)
- Compatibility checker (11 auto-fixes)
- Borderless windowed mode
- Background music (MP3/WAV/MIDI)
- In-launcher registration form
- News/announcement panel
- Clean room mode (per-session isolation)
- Edge relay routing (optional +$10/mo add-on — hosted relay with full IP masking, zero setup)
- Remote config updates (hot-reload)
- HWID-locked licensing
- Self-integrity verification

## Security & Anti-Cheat

- Anti-debugger (multiple methods)
- Cheat process scanning
- Cheat window title scanning
- Proxy DLL detection
- VM detection (Wine/Linux safe)
- HWID collection & ban support

## Client Guard (dinput8.dll) — Bin-Independent Features

These work on any RF_Online.bin version — no game memory hooking.

- Multi-client support (run multiple instances)
- Korean locale emulation
- Discord Rich Presence
- Asset decryption (CGEF)
- FreeGuard/Cerberus suppression

## Client Guard — Bin-Dependent Features (2.2.3.2)

These use pattern-scanned addresses from the game binary. Requires RF Online 2.2.3.2 and a ZoneMod license.

- Stack limit patches (sell/shop)
- Custom name colors (`__NC__` protocol)
- Hunter point patches
- In-game options panel (backtick key)
- FOV / Camera distance overrides
- Display fixes (VSync, cursor clipping)
- View distance / Dynamic lighting / Shadows
- Quest marker overlay
- Chat window overlay (with history)
- Auto-loot (client-side)
- Auto-target (tab cycling)
- Auto-attack (spacebar automation)
- Attack-on-target (click to attack)
- Target name filter (focus farming)
- Combat assist (combined targeting + attack)
- Bot memory / bot patches

## Server Components

- CrespoGuard Relay (transparent proxy + encrypted tunnel)
- CrespoGuard Relay dashboard
- Auto-generated dashboard API key
- HWID ban system
- Kick & announcements from dashboard
- CrespoGuard LoginServer (drop-in replacement)
- CrespoGuard ZoneMod (52 modules, 91+ hooks)
- ZoneMod web dashboard
- SOC unified dashboard
- [AI Assistant](ai/OVERVIEW.md) (local LLM config helper)

## DDoS Protection (Relay)

- Per-IP rate limiting
- Auto-ban with progressive backoff
- GeoIP country filtering
- Threat Intel blocklist (200K+ IPs)
- ASN-based filtering (datacenter/VPN blocking)
- TCP fingerprinting (passive OS detection)
- Tor exit node blocklist
- Adaptive rate limiter (burst-tolerant)
- Anti-replay protection
- Prometheus /metrics endpoint
- Async rotating logger (structured output)
- Optimized proxying (zero-copy on Linux)
- Relay auto-update
- Anonymous telemetry

## Relay Infrastructure

- Multi-zone proxy (hide ZoneServer IPs)
- File logging
- Edge relay mode (optional +$10/mo add-on — hosted relay on CrespoGuard's infrastructure)
- PROXY protocol v1
- Health check endpoint

## Auto-Loot: Two Tiers for Player Monetization

CrespoGuard gives server owners a powerful monetization tool: **two distinct auto-loot systems** that create a clear free-to-VIP upgrade path your players can feel immediately.

### Basic Auto-Loot (Client-Side — All Players)

Available to every player through the CrespoGuard client. No server modification required.

**What players experience:**
- Monster dies → items drop on the ground → items get picked up automatically within ~0.5 seconds
- A slight delay between drop and pickup — items visibly appear then disappear
- Configurable pickup range and interval
- Smart pause detection — pauses when the player is moving so they can run away from full-inventory loot piles
- Optional advanced mode bypasses additional pickup restrictions

Auto-loot settings are configurable per-player through the in-game options panel.

### Instant Auto-Loot (Server-Side — VIP Players)

Available through ZoneMod. Items never touch the ground.

**What players experience:**
- Monster dies → items appear directly in inventory. Zero delay. No animation.
- No items on the ground at all (or scattered visually for boss kills if configured)
- Works in MAU and siege mode
- Can exclude boss drops so players still get the manual pickup moment
- Stack-aware — automatically merges into existing stacks, respects stack limits
- Server-authoritative — impossible to exploit or manipulate client-side

Instant auto-loot is configured per-server by the server owner through the ZoneMod module system.

### The Monetization Model

```
Normal Player                    VIP Player
┌────────────────────┐          ┌────────────────────┐
│ Monster dies        │          │ Monster dies        │
│       ↓             │          │       ↓             │
│ Items drop on ground│          │ Items go straight   │
│       ↓ (~500ms)    │          │ to inventory        │
│ Auto-pickup walks   │          │       ↓             │
│ to item and grabs   │          │ Done. Zero delay.   │
│       ↓             │          │                     │
│ Next item...        │          │ Already looted all. │
└────────────────────┘          └────────────────────┘
```

Players feel the difference instantly. That half-second delay and walk-to animation is just annoying enough to make instant feel like a real upgrade — but not so bad that free players quit.

**Recommended setup for server owners:**

| Player Tier | Auto-Loot Type | How to Enable |
|-------------|---------------|---------------|
| Free players | Basic (client-side, ~500ms delay) | Enable in CrespoGuard client config |
| VIP players | Instant (server-side, zero delay) | Enable ZoneMod auto_loot module, gate via VIP flag |

**How to gate instant auto-loot to VIP only:**

ZoneMod's auto-loot hooks `CreateItemBox()` globally, but you can make it VIP-only by checking the player's premium/VIP flag in your server's database or using ZoneMod's JS scripting to conditionally skip non-VIP players.

This is one of the most requested features in RF Online private servers and one of the strongest reasons for players to buy VIP. CrespoGuard gives you both systems out of the box — no custom development needed.

## Binary Compatibility

Client Guard features are split by binary dependency:

- **Bin-independent features** (multi-client, locale, Discord RPC, asset decryption, FreeGuard suppression) work on **any** RF_Online.bin version
- **Bin-dependent features** (stack patches, name colors, FOV, display fixes, options panel, quest markers, combat features, chat overlay, bot patches) are validated for **RF Online 2.2.3.2** only and require a ZoneMod license
- On other binary versions, bin-dependent features gracefully deactivate — no crash, no errors

## Upgrading

Contact the CrespoGuard team to:

1. Receive a license key for your server
2. Get access to CrespoGuard LoginServer and ZoneMod
3. Unlock higher player caps as your server grows

The upgrade is a config change + new server binaries — no client reinstall required for your players.
