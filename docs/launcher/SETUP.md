
# Setup Guide

> Step-by-step guide for RF Online server owners to configure and deploy the CrespoGuard launcher for their community.

## Prerequisites

- A running RF Online 2.2.3.2 server (AccountServer + ZoneServer)
- A clean RF Online 2.2.3.2 client directory
- The CrespoGuard delivery package (provided by CrespoGuard team)
- A text editor for editing JSON files
- Windows 10/11

## What's in the Package

The Community package includes the launcher, anti-cheat DLL, and access to the CrespoGuard Relay (up to 30 players).

```
CrespoGuard-Community/
├── RFLauncher.exe                          # The launcher binary
├── dinput8.dll                             # Community anti-cheat (ClientGuard)
├── CrespoGuard.ico                         # Application icon
├── modules.json.template                   # Configuration template (edit this!)
├── System/
│   └── Launcher/
│       ├── fonts/
│       │   ├── Rajdhani-Regular.ttf        # Default body font
│       │   ├── Rajdhani-Bold.ttf           # Default bold font
│       │   └── Orbitron-Variable.ttf       # Default title font
│       └── Language/
│           └── en.json                     # English strings
```

## Quick Start (5 Minutes)

### Step 1: Edit modules.json

Open `modules.json` in your text editor. At minimum, update these fields:

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
    Players should never have the plaintext config. Delete `modules.json` from the client directory after generating `config.bin`.

### Step 5: Copy to Client Directory

Copy the entire delivery folder contents into your players' RF Online client directory:

```
RF Online Client/
├── RFLauncher.exe          ← copy here
├── RFLauncher.sig          ← copy here
├── CrespoGuard.ico         ← copy here
└── System/
    └── Launcher/           ← merge this folder
        ├── Config/
        │   └── config.bin
        ├── fonts/
        ├── Language/
        ├── Music/
        ├── logo.png
        └── background.png
```

### Step 6: Test

Launch `RFLauncher.exe` from the client directory. You should see:

- Your server name in the sidebar and window title
- Your logo in the sidebar
- Your background image
- Your branding in the status bar
- Login form connecting to your server IP

## Connection Mode

The Community Edition includes the CrespoGuard Relay for up to 30 concurrent players — encrypted transport, IP masking, DDoS protection, and dashboard access are all included for free. You can also use direct connection if you prefer (skip the relay setup).

For servers with more than 30 players, or if you need HWID bans, kick, announcements, and bin-dependent gameplay features, upgrade to Guard+ tier ($19/mo).

!!! tip "Recommended: Set Up the Relay"
    See [Relay Overview](RELAY.md) for what the relay provides and how to set it up.
    The relay is free for up to 30 players.

## Sirin Server Setup

If your server uses Sirin, the Community launcher works out of the box:

1. Set `"IsSirin": true` in your `modules.json` configuration
2. Place `sirin-launcher.dll` in the client directory alongside `RFLauncher.exe`
3. Generate `config.bin` as normal

The Community relay (30 players) works with Sirin out of the box.

## Next Steps

- [Config Reference](CONFIG_REFERENCE.md) — Every `modules.json` field documented
- [Theming & Branding](THEMING.md) — Colors, fonts, effects, and layout
- [Assets](ASSETS.md) — Logo, background, font, and music specs
- [Deployment](DEPLOYMENT.md) — Packaging and distributing to players
- [Relay Overview](RELAY.md) — Protect your server IP with the relay (Guard+ tier)

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Launcher shows "Config not found" | Ensure `System\Launcher\Config\config.bin` exists and was encrypted properly |
| Launcher shows raw key names (e.g., "NAV_HOME") | Language file missing — copy `en_gb.json` to `System\Launcher\Language\` |
| Fonts look wrong / fallback to bitmap | Check font files exist in `System\Launcher\fonts\` with correct filenames |
| Background not showing | Set `"EnableCustomBackground": true` in FeatureFlags |
| Can't connect to server | Verify `LoginServerIp` and `LoginServerPort` match your server, and firewall allows the port |
| Window title still says "CrespoGuard" | Add the `Branding` section to modules.json and re-encrypt config.bin |
| LNK1104 error when rebuilding | Close running RFLauncher.exe before rebuilding |

## Support

Contact the CrespoGuard team for:
- Generating encrypted config.bin files
- License key generation
- Premium feature activation
- Custom font or theme assistance
