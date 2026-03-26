
# Community vs Premium Features

> What's included in the Community Edition, and what requires a Premium license.

## Feature Comparison

### Launcher Features

| Feature | Community | Premium |
|---------|:---------:|:-------:|
| Custom server branding (name, logo, background) | Yes | Yes |
| Full color theme customization (9 colors) | Yes | Yes |
| Custom fonts (body, bold, title) | Yes | Yes |
| Effect controls (scan lines, particles, glow) | Yes | Yes |
| Configurable window size and layout | Yes | Yes |
| Multi-language support (8 languages) | Yes | Yes |
| Encrypted login (AES-256-GCM tunnel) | — | Yes |
| Account manager (save/switch accounts) | Yes | Yes |
| Auto-update system (patch server) | — | Yes |
| World selection with status indicators | Yes | Yes |
| System tray minimization | Yes | Yes |
| Game process watchdog (Job Object) | Yes | Yes |
| Compatibility checker (11 auto-fixes) | Yes | Yes |
| Borderless windowed mode | Yes | Yes |
| Background music (MP3/WAV/MIDI) | Yes | Yes |
| In-launcher registration form | Yes | Yes |
| News/announcement panel | Yes | Yes |
| Clean room mode (per-session isolation) | Yes | Yes |
| Edge relay routing (geographic) | — | Yes |
| Remote config updates (hot-reload) | Yes | Yes |
| HWID-locked licensing | Yes | Yes |
| Self-integrity verification | Yes | Yes |

### Security & Anti-Cheat

| Feature | Community | Premium |
|---------|:---------:|:-------:|
| Anti-debugger (multiple methods) | Yes | Yes |
| Cheat process scanning | Yes | Yes |
| Cheat window title scanning | Yes | Yes |
| Proxy DLL detection | Yes | Yes |
| VM detection (Wine/Linux safe) | Yes | Yes |
| HWID collection & ban support | Yes | Yes |

### Client Guard (dinput8.dll) — Bin-Independent Features

These work on any RF_Online.bin version — no game memory hooking.

| Feature | Community | Premium |
|---------|:---------:|:-------:|
| Multi-client support (run multiple instances) | Yes | Yes |
| Display fixes (VSync, cursor clipping) | Yes | Yes |
| Korean locale emulation | Yes | Yes |
| Discord Rich Presence | Yes | Yes |

### Client Guard — Bin-Dependent Features (2.2.3.2)

These use pattern-scanned addresses from the game binary. Requires RF Online 2.2.3.2.

| Feature | Community | Premium |
|---------|:---------:|:-------:|
| Stack limit patches (sell/shop) | Yes | Yes |
| Custom name colors (`__NC__` protocol) | Yes | Yes |
| Hunter point patches | Yes | Yes |
| In-game options panel (backtick key) | Yes | Yes |
| FOV / Camera distance overrides | Yes | Yes |
| View distance / Dynamic lighting / Shadows | Yes | Yes |
| Quest marker overlay | Yes | Yes |
| Chat window overlay (with history) | — | Yes |
| Auto-loot | — | Yes |
| Auto-target (tab cycling) | — | Yes |
| Auto-attack (spacebar automation) | — | Yes |
| Attack-on-target (click to attack) | — | Yes |
| Target name filter (focus farming) | — | Yes |
| Combat assist (combined targeting + attack) | — | Yes |
| Speed modification | — | Yes |
| Advanced memory features (bot patches) | — | Yes |

### Server Components

| Component | Community | Premium |
|-----------|:---------:|:-------:|
| CrespoGuard Relay (transparent proxy) | Yes | Yes |
| CrespoGuard Relay (CGRD encrypted tunnel) | — | Yes |
| CrespoGuard LoginServer (drop-in replacement) | — | Yes |
| CrespoGuard ZoneMod (52 modules, 91+ hooks) | — | Yes |
| ZoneMod web dashboard | — | Yes |
| SOC unified dashboard | — | Yes |
| HWID ban system | — | Yes |
| [AI Assistant](ai/OVERVIEW.md) (local LLM config helper) | — | Yes |

## Tier System

CrespoGuard uses a tiered licensing model for server components:

| Tier | Price | Max Players | Key Features |
|------|-------|-------------|-------------|
| **Community** | Free | 30 | Transparent TCP proxy (Sirin mode), server IP masking, read-only dashboard, IP bans, rate limiting |
| **Guard** | $19/mo | 75 | + CGRD encrypted tunnel, dashboard write ops (kick/ban), HWID bans, announcements, telemetry |
| **Shield** | $49/mo | 250 | + Multi-zone proxy, file logging |
| **Fortress** | $79/mo | 500 | + Edge relay mode, PROXY protocol, health check endpoint |

Client-side premium features (combat automation, chat overlay) require a Guard+ tier license.

Server-side tools (LoginServer, ZoneMod, SOC dashboard) are available at Guard+ tiers.

## What's Premium (Summary)

Premium features fall into four categories:

1. **Combat automation** (bin-dependent) — Auto-loot, auto-target, auto-attack, combat assist, speed modification, bot patches
2. **Chat overlay** (bin-dependent) — Custom chat window with history, message filtering
3. **Edge relay routing** (Fortress tier) — Geographic latency optimization with multi-region relay nodes
4. **Server-side tools** (Guard+) — LoginServer, ZoneMod (52 hook modules), dashboards, HWID ban system

Everything else — the launcher, branding, encrypted relay, full anti-cheat suite, security, and all non-combat client guard features — is included in the Community Edition.

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

Available through ZoneMod (Premium server component). Items never touch the ground.

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

- **Bin-independent features** (multi-client, locale, display, Discord) work on **any** RF_Online.bin version
- **Bin-dependent features** (stack patches, name colors, FOV, options panel, quest markers, and all premium features) are validated for **RF Online 2.2.3.2** only
- On other binary versions, bin-dependent features gracefully deactivate — no crash, no errors

## Premium Gating

Premium features require server-side license validation. The in-game options panel shows premium features as available but locked — players can see what's possible and ask their server owner to upgrade. Feature access is enforced server-side and cannot be bypassed locally.

## Upgrading to Premium

Contact the CrespoGuard team to:

1. Receive a Premium license key for your server
2. Get access to CrespoGuard LoginServer and ZoneMod
3. Enable premium client features (combat automation, chat overlay)
4. Get server-side anti-cheat dashboards and ban management

The upgrade is a config change + new server binaries — no client reinstall required for your players.
