# CrespoGuard Features

> Community starts with the neutral launcher package, manual updates, and the self-hosted relay path. Paid tiers add management, automation, higher caps, and hosted/edge capabilities.

## Tier Pricing

| Tier          | Price     | Max Players | Primary Additions                                                  |
| ------------- | --------- | ----------- | ------------------------------------------------------------------ |
| **Community** | Free      | 50          | Launcher, self-hosted relay, IP bans/rate limiting, manual updates |
| **Guard**     | $15/mo    | 50          | Dashboard, HWID bans, combat features, server IP masking           |
| **Shield**    | $30/mo    | 200         | Multi-zone proxy, file logging                                     |
| **Fortress**  | $50-75/mo | 500         | Edge relays, PROXY protocol, health checks                         |

## Launcher Features

| Feature                                         | Community | Paid Tiers |
| ----------------------------------------------- | :-------: | :--------: |
| Custom server branding (name, logo, background) |    Yes    |    Yes     |
| Full color theme customization                  |    Yes    |    Yes     |
| Custom fonts                                    |    Yes    |    Yes     |
| Custom localization files                       |    Yes    |    Yes     |
| Encrypted login through CrespoGuard Relay       |    Yes    |    Yes     |
| Account manager                                 |    Yes    |    Yes     |
| Manual package updates                          |    Yes    |    Yes     |
| Auto-update system (patch server)               |     -     |    Yes     |
| World selection with status indicators          |    Yes    |    Yes     |
| System tray minimization                        |    Yes    |    Yes     |
| Game process watchdog                           |    Yes    |    Yes     |
| Self-integrity verification                     |    Yes    |    Yes     |

## Security & Anti-Cheat

| Feature                            | Community | Paid Tiers |
| ---------------------------------- | :-------: | :--------: |
| Anti-debugger                      |    Yes    |    Yes     |
| Cheat process scanning             |    Yes    |    Yes     |
| Cheat window title scanning        |    Yes    |    Yes     |
| Proxy DLL detection                |    Yes    |    Yes     |
| VM detection (Wine/Linux safe)     |    Yes    |    Yes     |
| HWID collection                    |    Yes    |    Yes     |
| IP bans and rate limiting in relay |    Yes    |    Yes     |
| HWID ban enforcement               |     -     |    Yes     |
| Relay dashboard actions            |     -     |    Yes     |

## Client Guard (dinput8.dll)

`dinput8.dll` is included in the current launcher package and current launcher builds check for it before starting `RF_Online.bin`.

### Bin-Independent Features

These work on any RF_Online.bin version without game memory hooks.

- Multi-client support
- Locale/display compatibility helpers
- Discord Rich Presence
- Asset decryption support where configured
- FreeGuard/Cerberus compatibility handling

### Bin-Dependent Features (2.2.3.2)

These use pattern-scanned addresses from the game binary and require a compatible RF Online 2.2.3.2 client.

- Stack limit patches
- Custom name colors
- Hunter point patches
- In-game options panel
- FOV / camera distance overrides
- View distance / dynamic lighting / shadows
- Quest marker overlay
- Premium combat automation and chat overlay where licensed

## Server Components

| Component                            | Community | Paid Tiers |
| ------------------------------------ | :-------: | :--------: |
| CrespoGuard Relay (encrypted tunnel) |    Yes    |    Yes     |
| IP bans and rate limiting            |    Yes    |    Yes     |
| Relay dashboard                      |     -     |    Yes     |
| CrespoGuard LoginServer              |     -     |    Yes     |
| CrespoGuard ZoneMod                  |     -     |    Yes     |
| SOC unified dashboard                |     -     |    Yes     |
| Edge relay routing                   |     -     |  Fortress  |

## Binary Compatibility

- The launcher itself works with RF Online server/client variants as long as the configured login flow is valid.
- Bin-independent Client Guard features work without RF_Online.bin memory hooks.
- Bin-dependent Client Guard features are validated for RF Online 2.2.3.2 and deactivate when required patterns are unavailable.

## Upgrading

Contact the CrespoGuard team to:

1. Receive the correct package/tier license for your server.
2. Enable paid relay/dashboard/HWID/automation features where needed.
3. Increase player caps or add hosted edge routing.

The neutral Community launcher package remains server-neutral; server-specific settings belong in `modules.json` and generated `config.bin`.
