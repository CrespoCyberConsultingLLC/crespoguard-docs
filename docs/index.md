<div align="center">
  <img src="assets/banner.png" alt="CrespoGuard" width="700">
  <p><strong>The professional launcher and security suite for RF Online private servers.</strong></p>
</div>

CrespoGuard replaces the stock RF Online launcher with a modern, fully customizable client that your players will actually want to use — and a security layer that protects your server from day one.

## Why CrespoGuard?

### Your Brand, Not Ours

Every server has its own identity. CrespoGuard is built from the ground up to disappear behind your brand:

- **Your name** in the window title, status bar, and footer — zero CrespoGuard references visible to players
- **Your colors** — 9 configurable theme colors turn the same binary into a completely different launcher per server
- **Your fonts** — swap the default sci-fi look for medieval, modern, Korean, or anything else with custom TTF fonts
- **Your logo and background** — full sidebar logo with configurable sizing, custom background image
- **Your effects** — scan lines, particles, and glow can be dialed up for cyberpunk or turned off completely for a clean, professional feel
- **Your layout** — configurable window size and sidebar width to fit your branding

One binary, infinite identities. See [Theming & Branding](launcher/THEMING.md) for 6 ready-made color presets.

### Security That Works Without the Game Binary

Most anti-cheat solutions hook deeply into the game executable — which means they break on different versions, conflict with existing protections, and require constant maintenance. CrespoGuard's launcher-side security is **completely version-independent**:

- **Anti-debugger** — multiple detection methods running continuously
- **Cheat process scanning** — extensive signature database, re-checked every 10 seconds
- **Cheat window title scanning** — detects known cheat tool windows
- **Proxy DLL detection** — catches known injection methods
- **VM detection** — blocks virtual machines while keeping Wine/Linux players safe
- **IP protection** — transparent relay hides your real server IP from players (Community); encrypted AES-256-GCM tunnel available at Guard+

All of this runs in the launcher process. No game binary patching. No conflicts with existing protections. Works on any RF Online version.

### How CrespoGuard Protects Your Server

CrespoGuard uses a **layered defense model** — multiple independent security systems that reinforce each other. Bypassing one layer doesn't compromise the others.

**Layer 1: Relay Transport**

All login traffic between players and your server travels through the CrespoGuard Relay. At Community tier, this is a transparent TCP proxy that hides your real server IP — players never see your actual address. At Guard+ tier, the relay uses AES-256-GCM authenticated encryption with a pre-shared key, meaning traffic can't be intercepted, replayed, or tampered with in transit.

**Layer 2: Config Tamper Protection**

The launcher configuration (server IP, license, encryption keys) is stored in an encrypted binary format with integrity verification. If the file is modified — even a single byte — the launcher rejects it immediately. Players cannot extract your server details, modify connection parameters, or forge configurations. The encryption key is compiled into the binary, not stored alongside the config.

**Layer 3: Continuous Runtime Scanning**

The launcher doesn't just check once at startup — it runs continuous background scans every 10 seconds while the game is running:

- **Process scanning** compares running processes against an extensive database of known cheat tools, trainers, memory editors, and packet sniffers. The database is signature-based, not name-based — renaming a tool doesn't bypass detection.
- **Window scanning** detects cheat tool windows even when processes are disguised or renamed, catching tools that evade process-level detection.
- **DLL injection detection** identifies known proxy and injection DLLs that cheat tools use to attach to the game process.
- **Debugger detection** uses multiple independent methods to detect debugging tools. Using a single detection method would be easy to bypass — CrespoGuard uses several that cross-validate, making bypasses significantly harder.

When any scan detects a violation, the session is terminated immediately. There's no warning period for cheaters to disconnect their tools.

**Layer 4: Hardware Identification**

Every player's machine is fingerprinted using multiple hardware identifiers (CPU, disk, motherboard, network adapter) combined into a single hash. This enables:

- **HWID bans** (Guard+ tier) — ban the hardware, not just the IP. Players can't evade by changing IP address or creating new accounts.
- **License binding** — optionally lock license keys to specific machines, preventing key sharing.
- **Audit trail** — track which hardware connected, when, and what violations occurred.

The HWID is computed using system-level APIs that are difficult to spoof without kernel-level tools.

**Layer 5: Server-Side Enforcement**

Critical security decisions happen on the server, not the client:

- **License validation** is performed by the relay server before the player reaches your login server. Invalid or expired licenses are rejected at the network edge.
- **Rate limiting** prevents brute-force attacks — configurable per-IP connection throttling and concurrent connection limits.
- **Ban enforcement** is server-authoritative. Even if someone patches the client, the relay blocks banned HWIDs and IPs before they can connect.
- **Premium feature gating** is verified server-side. Client-side patches cannot unlock premium features — the server must authorize them.

**Layer 6: VM and Environment Detection**

CrespoGuard detects virtual machines used to run multiple cheat-testing instances or evade hardware bans. The detection checks multiple system indicators — not just a single registry key or process name — making it resilient to basic spoofing. Wine/Linux users are specifically excluded from VM detection, so legitimate Linux players aren't affected.

**Why This Matters**

Most RF Online anti-cheat tools rely on a single layer — usually a game binary hook that checks for CheatEngine once at startup. That's trivially bypassed. CrespoGuard's approach means:

- A cheater who bypasses process scanning still hits debugger detection
- A cheater who spoofs their HWID still gets caught by DLL injection detection
- A cheater who patches the client still gets blocked by server-side enforcement
- A cheater who replays network traffic still fails authenticated encryption

No single bypass defeats the system. That's the point of layered defense.

### Built for Server Owners

CrespoGuard isn't a one-size-fits-all tool — it's a platform designed around how private server communities actually work:

- **Encrypted config** — server IP, keys, and settings are encrypted with tamper detection. Players can't extract or modify your server details.
- **Auto-updates** (Premium) — manifest-based patching from your HTTPS server. Push updates without players reinstalling.
- **Multi-language** — 8 languages out of the box (English, Korean, Chinese, Japanese, Russian, Portuguese, Indonesian). Every UI string is customizable.
- **Account manager** — saved credentials with DPAPI encryption. Remember Me. Multi-account switching.
- **Compatibility checker** — 11 automated checks on first launch (Game Mode, Game DVR, HAGS, AutoHDR, power plan, etc.) with auto-fixes where possible.
- **Clean room mode** — per-session registry and INI isolation so the game doesn't pollute the player's system.
- **Borderless windowed** — real borderless mode with resolution selection, not the game's broken windowed implementation.

### Free to Start, Scales When You Need It

The Community Edition is **free** and includes everything above. No trial period, no feature crippling, no nag screens. Your players get a professional launcher experience from day one.

When your server grows, Premium tiers add combat automation for your players, server management dashboards, HWID bans, and multi-region edge relays:

| Tier | Price | Players | What It Adds |
|------|-------|---------|-------------|
| **Community** | Free | 30 | Full launcher + transparent TCP relay + IP masking + read-only dashboard + anti-cheat |
| **Guard** | $19/mo | 75 | CGRD encrypted tunnel, dashboard write ops, HWID bans, combat features |
| **Shield** | $49/mo | 250 | Multi-zone proxy, file logging |
| **Fortress** | $79/mo | 500 | Edge relays, PROXY protocol, health checks |

See [Features](FEATURES.md) for the full comparison and [Premium Tiers](PREMIUM_TIERS.md) for detailed documentation.

### What Premium Gives Your Players

Premium isn't just server management — it unlocks in-game features your players will love:

- **Auto-loot (two tiers)** — basic auto-loot for all players (~500ms pickup delay), instant auto-loot for VIP players (items skip the ground entirely). The most powerful monetization lever in RF Online private servers. See [Features](FEATURES.md#auto-loot-two-tiers-for-player-monetization) for the full breakdown.
- **Auto-target** — tab cycling through nearby mobs with smart skip logic
- **Auto-attack** — spacebar automation on valid targets
- **Combat assist** — combined targeting + attack with name filter for focused farming
- **Chat overlay** — modern ImGui chat window with message history, filtering, and input cycling

These appear in the in-game options panel. On Community tier they're visible but grayed out with a `[Premium]` tag — so players know the features exist and can ask their server owner to upgrade.

## Getting Started

| Step | Guide |
|------|-------|
| 1. Configure your server | [Setup Guide](launcher/SETUP.md) — running in 5 minutes |
| 2. Brand your launcher | [Theming & Branding](launcher/THEMING.md) — colors, fonts, effects |
| 3. Create config.bin | [Creating config.bin](launcher/CONFIG_CREATION.md) — encryption walkthrough |
| 4. Prepare assets | [Assets](launcher/ASSETS.md) — logo, background, font, music specs |
| 5. Deploy to players | [Deployment](launcher/DEPLOYMENT.md) — packaging, auto-updates, versioning |

**Setting up the relay?** See [Community Relay Setup](launcher/RELAY.md) for the free transparent proxy guide, or [Premium Tiers](PREMIUM_TIERS.md) for encrypted tunnel setup.

## Documentation

| Document | Description |
|----------|-------------|
| [Setup Guide](launcher/SETUP.md) | Quick start — configure and test in 5 minutes |
| [Configuration Reference](launcher/CONFIG_REFERENCE.md) | Every `modules.json` field documented (12 sections) |
| [Creating config.bin](launcher/CONFIG_CREATION.md) | Step-by-step encryption guide with examples |
| [Theming & Branding](launcher/THEMING.md) | Colors, fonts, effects, layout — 6 ready-made presets |
| [Features](FEATURES.md) | Community vs Premium comparison tables |
| [Assets](launcher/ASSETS.md) | Logo, background, font, music, and language file specs |
| [Deployment](launcher/DEPLOYMENT.md) | Packaging, distribution, auto-updates, and versioning |
| [Community Relay Setup](launcher/RELAY.md) | Free transparent proxy — hide your IP in 5 minutes |
| [Premium Tiers](PREMIUM_TIERS.md) | Guard / Shield / Fortress detailed reference |

## Requirements

- RF Online server (any version for launcher; 2.2.3.2 for Client Guard DLL features)
- Windows 10/11
- CrespoGuard delivery package

## Frequently Asked Questions

### Compatibility

**Does CrespoGuard work on Linux / Wine / Proton?**
Yes. The launcher and game run under Wine/Proton. VM detection is specifically designed to allow Wine — it only blocks actual virtual machines (VMware, VirtualBox, etc.), not compatibility layers. Linux players can use CrespoGuard without issues.

**Does it work with any RF Online version?**
The launcher itself (login, branding, relay, anti-cheat) works with any RF Online version. The Client Guard DLL features that interact with game memory (FOV overrides, stack patches, combat features) are validated for version 2.2.3.2. On other versions, those features gracefully deactivate — no crash, no errors.

**Does it conflict with FreeGuard / existing server protections?**
No. CrespoGuard's security runs in the launcher process and uses system-level APIs — it doesn't modify the game binary or conflict with existing DLL protections. Your server's existing anti-cheat chain remains untouched.

**Will it work with my custom/modified RF_Online.bin?**
The launcher will work with any binary. Client Guard DLL features depend on finding known code patterns in the game executable. If your binary is heavily modified, some features may not activate. They fail safely — never a crash.

**Does it work with Sirin servers?**
The Community Edition's relay runs in transparent TCP proxy mode (Sirin-compatible) — it works with any RF Online server, including Sirin servers. Guard+ tiers add full Sirin SDK integration in the launcher, bridging Sirin's authentication through the CGRD encrypted relay for enhanced security.

### Multi-Player Households

**Will the anti-cheat flag multiple players in the same house?**
No. CrespoGuard identifies machines by hardware fingerprint, not by IP address. Two players on the same network with different computers have different hardware IDs and are treated as separate individuals. They can both play simultaneously without triggering any detection.

**What about two accounts on the same computer?**
Multi-client mode allows multiple game instances on one machine. Each instance shares the same hardware ID but has a separate login session. This is fully supported — CrespoGuard's instance limiter (configurable, default 3) controls how many launchers can run at once, not how many accounts exist.

**Does IP-based rate limiting affect shared connections?**
Rate limiting applies to connection attempts, not active sessions. The default allows 15 login attempts per IP within 60 seconds and 5 concurrent connections. For internet cafes or shared networks, server owners can increase these limits in the relay config. Active players are not affected — rate limiting only throttles the login handshake.

### Security Concerns

**Can players bypass the anti-cheat by patching the launcher?**
Critical security decisions are enforced server-side. Even if a player modifies the launcher binary, the relay server independently validates licenses, enforces bans, and verifies connections. A patched client cannot bypass server-side enforcement.

**Can players extract my server IP from the launcher?**
No. The launcher configuration is encrypted with tamper detection. Players cannot read the plaintext contents. When using the encrypted relay, your real server IP is never transmitted to the client — players only see the relay address.

**What happens if a player is caught cheating?**
The session is terminated immediately with no warning period. The event is logged with the player's hardware ID, IP, and what was detected. Server owners with Guard+ tier can review events in the dashboard and issue HWID bans to prevent the player from reconnecting on any account.

**How often is the cheat detection database updated?**
The detection database is compiled into the launcher binary. Updates are delivered through the auto-update system — server owners push a new `RFLauncher.exe` to their patch server, and players receive it automatically on next launch. No manual intervention required.

### Performance

**Does CrespoGuard affect game performance?**
The launcher minimizes to the system tray while the game runs. Background security scans use negligible CPU (a brief check every 10 seconds). The Client Guard DLL runs in the game process but performs no continuous heavy operations — its features are event-driven, not polling-based. Players will not notice any performance impact.

**Does the encrypted relay add latency?**
The relay adds minimal overhead — typically under 1ms for the encryption/decryption layer. For servers where players are geographically distant, the Fortress tier's edge relay network actually *reduces* latency by routing players to the nearest relay node instead of connecting directly across continents.

### Server Administration

**Do I need to restart the relay to update bans?**
No. The ban file is automatically reloaded every 30 seconds. Add a ban to the file and it takes effect within half a minute — no downtime.

**Can I run CrespoGuard alongside my existing launcher?**
Yes. CrespoGuard doesn't modify your server infrastructure. Players who use CrespoGuard connect through the relay; players using other launchers can still connect directly to your login server if you leave the port open. You can migrate gradually.

**What happens if the CrespoGuard license server is down?**
The relay caches the last successful license validation locally. If the license server is temporarily unreachable, the relay continues operating normally using the cached validation. Extended outages (beyond the cache period) will prevent the relay from starting, but your game server itself is completely unaffected — players can still connect directly.

**Can I white-label CrespoGuard so players don't know what's behind it?**
Absolutely. That's the entire point of the branding system. Every visible reference — window title, status bar, footer text, fonts, colors, logo — is configurable. Players see your server name, your brand, your design. CrespoGuard is invisible unless you choose to mention it.

## Support

Contact the CrespoGuard team for:
- Config encryption (config.bin generation)
- License key activation
- Premium tier upgrades
- Custom theming assistance
