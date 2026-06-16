# Setup Guide

> Step-by-step guide for RF Online server owners to configure and deploy the CrespoGuard launcher for their community.

## Prerequisites

- A running RF Online 2.2.3.2 server (AccountServer + ZoneServer)
- A clean RF Online 2.2.3.2 client directory
- The CrespoGuard delivery package (provided by CrespoGuard team)
- A text editor for editing JSON files
- Windows 10/11

## What's in the Package

The neutral Community launcher package is a base operator kit. It includes launcher/client files and templates, but no server-specific `modules.json`, generated `config.bin`, or relay server binaries.

```text
CrespoGuard-Community/
|-- RFLauncher.exe
|-- RFLauncher.sig
|-- dinput8.dll
|-- CrespoGuard.ico
|-- CrespoGuardConfigTool.exe
|-- settings.ini
|-- README.txt
|-- LICENSE.txt
|-- modules.json.template
|-- modules.ru_ru.json.template
|-- System/
    |-- Launcher/
        |-- fonts/
        |   |-- Rajdhani-Regular.ttf
        |   |-- Rajdhani-Bold.ttf
        |   |-- Orbitron-Variable.ttf
        |-- Language/
            |-- en.json
            |-- en_gb.json
            |-- ru_ru.json
```

`System\Launcher\Config\config.bin` is generated after you create `modules.json`; it is not included in the neutral base package.

## Quick Start (5 Minutes)

### Step 1: Create and edit modules.json

Copy `modules.json.template` to `modules.json`, then open `modules.json` in your text editor. If you need a Russian-client starting point, copy `modules.ru_ru.json.template` instead. At minimum, update these fields:

```json
{
  "ServerConfig": {
    "ServerName": "Your Server Name",
    "LoginServerIp": "YOUR.SERVER.IP",
    "LoginServerPort": 10001,
    "ZoneServerIp": "YOUR.SERVER.IP",
    "ZoneServerPort": 27780,
    "ServerVersion": "2.2.3.2"
  }
}
```

### Step 2: Customize Your Branding

Add a `Branding` section to replace all CrespoGuard references with your server name:

```json
{
  "Branding": {
    "WindowTitle": "Your Server Name",
    "StatusBarText": "Your Server v1.0",
    "FooterText": "Powered by Your Server"
  }
}
```

### Step 3: Add Your Logo and Background

- Place your server logo at `System\Launcher\logo.png` (recommended: 340x120px, transparent PNG)
- Place a background image at `System\Launcher\background.png` (recommended: 1100x650px or larger)
- Set `"EnableCustomBackground": true` in FeatureFlags

### Step 4: Generate Encrypted Config

The launcher requires an encrypted `config.bin` — it will not load plaintext `modules.json` in release builds.

> For the full walkthrough with detailed examples, see [CONFIG_CREATION.md](CONFIG_CREATION.md).

Contact the CrespoGuard team to generate your `config.bin`, or if you have the admin build:

=== "PowerShell"

    ```powershell
    # From your client directory (where modules.json lives):
    & "path\to\admin\RFLauncher.exe" --encrypt-config
    ```

=== "Git Bash"

    ```bash
    "/c/path/to/admin/RFLauncher.exe" --encrypt-config
    ```

This reads `modules.json` from the current directory and writes `System\Launcher\Config\config.bin`.

!!! warning "Delete modules.json after encrypting"
Players should never have the plaintext config. Delete `modules.json` from player-facing archives after generating `config.bin`. Keep it only in your operator/admin workspace.

### Step 5: Copy to Client Directory

Copy the entire delivery folder contents into your players' RF Online client directory:

```text
RF Online Client/
|-- RFLauncher.exe          <- copy here
|-- RFLauncher.sig          <- copy here
|-- dinput8.dll             <- copy here
|-- CrespoGuard.ico         <- copy here
|-- settings.ini            <- optional default preferences
|-- System/
    |-- Launcher/           <- merge this folder
        |-- Config/
        |   |-- config.bin
        |-- fonts/
        |-- Language/
        |-- Music/
        |-- logo.png
        |-- background.png
```

### Step 6: Test

Launch `RFLauncher.exe` from the client directory. You should see:

- Your server name in the sidebar and window title
- Your logo in the sidebar
- Your background image
- Your branding in the status bar
- Login form connecting to your server IP

## Connection Mode

The launcher can connect directly to your LoginServer or through the CrespoGuard Relay. The relay binary/config package is delivered separately from the neutral launcher operator kit.

- **Direct login** - simplest setup; players connect to the configured LoginServer address.
- **Encrypted tunnel (CGRD, AES-256-GCM)** - routes login traffic through CrespoGuard Relay using `SecureLogin` and a shared PSK.
- **Zone proxy** - optional relay-side forwarding for enter-world traffic so the game client connects through the proxy instead of the real ZoneServer address.

Use the relay when you need encrypted login transport, rate limiting, IP bans, or origin IP protection. If you do not deploy the relay, leave `SecureLogin.EnableSecureLogin` disabled and connect directly.

!!! tip "Relay setup"
See [Relay Overview](RELAY.md) for relay deployment options and `server.json` setup. The relay package is separate from the launcher base operator kit.

## Sirin Server Setup

If your server uses Sirin, the Community launcher works out of the box:

1. Set `"IsSirin": true` in your `modules.json` configuration
2. Place `sirin-launcher.dll` in the client directory alongside `RFLauncher.exe`
3. Generate `config.bin` as normal

The relay works with Sirin out of the box when configured for the same login path.

## Next Steps

- [Config Reference](CONFIG_REFERENCE.md) - Every `modules.json` field documented
- [Theming & Branding](THEMING.md) - Colors, fonts, effects, and layout
- [Assets](ASSETS.md) - Logo, background, font, and music specs
- [Deployment](DEPLOYMENT.md) - Packaging and distributing to players
- [Relay Overview](RELAY.md) - Transparent proxy, encrypted tunnel, and zone proxy setup

## Troubleshooting

| Problem                                         | Solution                                                                                                           |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Launcher shows "Config not found"               | Ensure `System\Launcher\Config\config.bin` exists and was encrypted properly                                       |
| Launcher shows raw key names (e.g., "NAV_HOME") | Language file missing - copy `en_gb.json` or the configured full-code language file to `System\Launcher\Language\` |
| Fonts look wrong / fallback to bitmap           | Check font files exist in `System\Launcher\fonts\` with correct filenames                                          |
| Background not showing                          | Set `"EnableCustomBackground": true` in FeatureFlags                                                               |
| Can't connect to server                         | Verify `LoginServerIp` and `LoginServerPort` match your server, and firewall allows the port                       |
| Window title still says "CrespoGuard"           | Add the `Branding` section to modules.json and re-encrypt config.bin                                               |
| LNK1104 error when rebuilding                   | Close running RFLauncher.exe before rebuilding                                                                     |

## Support

Contact the CrespoGuard team for:

- Generating encrypted config.bin files
- License key generation
- Tier upgrades
- Custom font or theme assistance
