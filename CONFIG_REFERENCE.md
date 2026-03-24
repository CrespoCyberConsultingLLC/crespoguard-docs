---
title: Configuration Reference
layout: default
nav_order: 3
---

# Configuration Reference

> Complete reference for every field in `modules.json`. All sections are optional — missing sections use defaults.

## Table of Contents

- [ServerConfig](#serverconfig)
- [Branding](#branding)
- [ThemeConfig](#themeconfig)
- [FeatureFlags](#featureflags)
- [ExternalLinks](#externallinks)
- [AuthLimits](#authlimits)
- [ClientLimiter](#clientlimiter)
- [Localization](#localization)
- [SecureLogin](#securelogin)
- [SecurityCheck](#securitycheck)
- [UpdateServers](#updateservers)
- [NetworkRoutes](#networkroutes)
- [CustomCryptKeyRequest](#customcryptkeyrequest)

---

## ServerConfig

Core server connection settings.

```json
{
  "ServerConfig": {
    "ServerName": "RF Solus",
    "ServerVersion": "2.2.3.2",
    "LoginServerIp": "192.168.1.100",
    "LoginServerPort": 10001,
    "ZoneServerIp": "192.168.1.100",
    "ZoneServerPort": 27780,
    "IsSirin": false,
    "Key": "0000000000000000-20270101-abcdef1234567890abcdef1234567890"
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `ServerName` | string | `"RF Server"` | Displayed in sidebar, window title, and tray tooltip |
| `ServerVersion` | string | `"GU"` | Shown below server name in sidebar |
| `LoginServerIp` | string | `"127.0.0.1"` | Login server IP address |
| `LoginServerPort` | int | `10001` | Login server port |
| `ZoneServerIp` | string | `"127.0.0.1"` | Zone server IP (used for direct connect) |
| `ZoneServerPort` | int | `27780` | Zone server port |
| `IsSirin` | bool | `false` | Enable Sirin SDK integration (requires sirin-launcher.dll) |
| `Key` | string | — | License key (provided by CrespoGuard team) |

---

## Branding

White-label configuration. Replace all CrespoGuard references with your own branding.

```json
{
  "Branding": {
    "WindowTitle": "My Server",
    "StatusBarText": "My Server Launcher v1.0",
    "FooterText": "Powered by My Server",
    "FontBody": "Rajdhani-Regular.ttf",
    "FontBold": "Rajdhani-Bold.ttf",
    "FontTitle": "Orbitron-Variable.ttf",
    "FontSizeBody": 16.0,
    "FontSizeTitle": 24.0,
    "FontSizeSmall": 13.0,
    "LogoMaxWidth": 170.0,
    "LogoMaxHeight": 60.0
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `WindowTitle` | string | `"CrespoGuard"` | Window title (taskbar name). Server name is appended automatically. |
| `StatusBarText` | string | `"CrespoGuard v0.1.0"` | Bottom-left text in status bar |
| `FooterText` | string | `"Powered by CrespoGuard"` | Bottom-right text in status bar |
| `FontBody` | string | `"Rajdhani-Regular.ttf"` | Body font filename (in `System\Launcher\fonts\`) |
| `FontBold` | string | `"Rajdhani-Bold.ttf"` | Bold font filename |
| `FontTitle` | string | `"Orbitron-Variable.ttf"` | Title/heading font filename |
| `FontSizeBody` | float | `16.0` | Body text size in pixels |
| `FontSizeTitle` | float | `24.0` | Title text size in pixels |
| `FontSizeSmall` | float | `13.0` | Small/caption text size in pixels |
| `LogoMaxWidth` | float | `170.0` | Maximum logo width in sidebar (px) |
| `LogoMaxHeight` | float | `60.0` | Maximum logo height in sidebar (px) |

---

## ThemeConfig

Visual theme — colors, effects, and layout sizing.

### Colors

All colors are RGBA arrays `[R, G, B, A]` with values 0–255.

```json
{
  "ThemeConfig": {
    "AccentColor": [0, 198, 255, 255],
    "AccentDimColor": [0, 127, 178, 255],
    "PanelBgColor": [6, 8, 16, 220],
    "SidebarBgColor": [6, 6, 14, 235],
    "TitleBarBgColor": [4, 4, 10, 230],
    "ErrorColor": [255, 89, 89, 255],
    "SuccessColor": [0, 204, 76, 255],
    "ButtonBaseColor": [15, 70, 120, 255],
    "ButtonHoverColor": [20, 100, 160, 255]
  }
}
```

| Field | Default | Description |
|-------|---------|-------------|
| `AccentColor` | Cyan `[0, 199, 255, 255]` | Primary brand color — sidebar highlights, glow, links |
| `AccentDimColor` | Dark cyan `[0, 128, 179, 255]` | Dimmed accent for hover states |
| `PanelBgColor` | `[6, 8, 16, 220]` | Main content panel background |
| `SidebarBgColor` | `[6, 6, 14, 235]` | Left sidebar background |
| `TitleBarBgColor` | `[4, 4, 10, 230]` | Top bar background |
| `ErrorColor` | Red `[255, 89, 89, 255]` | Error messages and warnings |
| `SuccessColor` | Green `[0, 204, 77, 255]` | Success messages and online indicators |
| `ButtonBaseColor` | Deep blue `[15, 70, 120, 255]` | Button default state |
| `ButtonHoverColor` | Light blue `[20, 100, 160, 255]` | Button hover state |

### Effects

```json
{
  "ThemeConfig": {
    "EnableScanLines": true,
    "EnableParticles": true,
    "EffectIntensity": 1.0,
    "GlowIntensity": 1.0
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `EnableScanLines` | bool | `true` | Horizontal scan line overlay (sci-fi effect) |
| `EnableParticles` | bool | `true` | Floating background particles |
| `EffectIntensity` | float | `1.0` | Master effect opacity multiplier (0.0 = invisible, 1.0 = full) |
| `GlowIntensity` | float | `1.0` | Glow/border effect multiplier (0.0–1.0) |

### Layout

```json
{
  "ThemeConfig": {
    "SidebarWidth": 200.0,
    "WindowWidth": 1024,
    "WindowHeight": 600
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `SidebarWidth` | float | `200.0` | Left sidebar width in pixels |
| `WindowWidth` | int | `1024` | Launcher window width |
| `WindowHeight` | int | `600` | Launcher window height |

---

## FeatureFlags

Toggle launcher features on/off.

```json
{
  "FeatureFlags": {
    "EnableWorldStatus": false,
    "EnableAutoUpdate": true,
    "EnableNoticeForm": true,
    "EnableSlidingNews": true,
    "EnableSavingCredentials": true,
    "EnableCustomBackground": false,
    "LauncherFullscreen": false,
    "UpdateSigningKey": "",
    "RemoteConfigUrl": ""
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `EnableWorldStatus` | bool | `false` | Show world status page (requires WorldStatusUrl) |
| `EnableAutoUpdate` | bool | `true` | Auto-update client files from patch server |
| `EnableNoticeForm` | bool | `true` | Show notice/announcement panel |
| `EnableSlidingNews` | bool | `true` | Show sliding news carousel on home panel |
| `EnableSavingCredentials` | bool | `true` | Allow "Remember Me" checkbox + account saving |
| `EnableCustomBackground` | bool | `false` | Load background.png/jpg from Launcher folder |
| `LauncherFullscreen` | bool | `false` | Start launcher in fullscreen mode |
| `UpdateSigningKey` | string | `""` | HMAC signing key for verifying patch manifests (empty = skip verification) |
| `RemoteConfigUrl` | string | `""` | URL to fetch updated config.bin remotely (empty = disabled) |

---

## ExternalLinks

URLs for sidebar buttons and registration.

```json
{
  "ExternalLinks": {
    "RegisterUrl": "https://yourserver.com/register",
    "RegistrationApiUrl": "https://yourserver.com/api/register",
    "WebsiteUrl": "https://yourserver.com",
    "FacebookUrl": "https://facebook.com/yourserver",
    "DiscordUrl": "https://discord.gg/yourserver",
    "WorldStatusUrl": "https://yourserver.com/status",
    "NewsUrl": "https://yourserver.com/api/news"
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `RegisterUrl` | string | Opens in browser when "Register" button is clicked (external registration) |
| `RegistrationApiUrl` | string | API endpoint for in-launcher registration form (POST JSON) |
| `WebsiteUrl` | string | Sidebar "Website" button link |
| `FacebookUrl` | string | Sidebar "Facebook" button link (hidden if empty) |
| `DiscordUrl` | string | Sidebar "Discord" button link (hidden if empty) |
| `WorldStatusUrl` | string | World status page URL (requires `EnableWorldStatus`) |
| `NewsUrl` | string | News API endpoint (GET JSON, displayed on home panel) |

> **Note**: If `RegistrationApiUrl` is set, the launcher shows an in-app registration form. If only `RegisterUrl` is set, clicking Register opens the browser instead.

---

## AuthLimits

Username and password length constraints for the login form.

```json
{
  "AuthLimits": {
    "MinUsernameLength": 3,
    "MaxUsernameLength": 12,
    "MinPasswordLength": 3,
    "MaxPasswordLength": 12
  }
}
```

---

## ClientLimiter

Control how many launcher instances can run simultaneously.

```json
{
  "ClientLimiter": {
    "EnableClientLimit": false,
    "MaxInstanceCount": 3
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `EnableClientLimit` | bool | `false` | Enable instance limiting |
| `MaxInstanceCount` | int | `3` | Maximum concurrent launcher instances (system-wide) |

---

## Localization

Language settings.

```json
{
  "Localization": {
    "EnforceNation": false,
    "NationCode": "en_gb"
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `EnforceNation` | bool | `false` | Lock language to specified nation (prevent user override) |
| `NationCode` | string | `"en_gb"` | Language code. Supported: `en_gb`, `en_us`, `ko_kr`, `zh_tw`, `ja_jp`, `ru_ru`, `pt_br`, `id_id` |

---

## SecureLogin

Encrypted relay tunnel configuration. Requires CrespoGuard Server running on the server side.

```json
{
  "SecureLogin": {
    "EnableSecureLogin": false,
    "SecureLoginType": 1,
    "SecureLoginHost": "your.server.ip",
    "SecureLoginIP": "your.server.ip",
    "SecureLoginPort": 10001,
    "SecureLoginPSK": "YOUR_PRESHARED_KEY_HERE",
    "EnableZoneProxy": false,
    "ZoneProxyIP": "your.server.ip",
    "ZoneProxyPort": 27780,
    "LicenseServerUrl": "",
    "RoutingCode": "",
    "EnableEdgeRelays": false,
    "AutoSelectRelay": true,
    "EdgeRelays": []
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `EnableSecureLogin` | bool | `false` | Route login through AES-256-GCM encrypted tunnel |
| `SecureLoginPSK` | string | — | Pre-shared encryption key (must match server.json) |
| `EnableZoneProxy` | bool | `false` | Route zone traffic through proxy (hides real zone IP) |
| `EnableEdgeRelays` | bool | `false` | Enable geographic edge relay selection |
| `EdgeRelays` | array | `[]` | List of `{Label, Host, Port, ZonePort}` edge relay nodes |

> **Community Edition**: SecureLogin is available but requires CrespoGuard Server deployment. See [SETUP.md](SETUP.md#server-side-setup-optional) for server setup.

---

## SecurityCheck

Client-side security features. Community edition includes basic checks.

```json
{
  "SecurityCheck": {
    "EnableHelperCheck": true,
    "EnableVMCheck": true,
    "EnableOtherChecks": true,
    "EnableHWIDCheck": false,
    "EnableProxyDllCheck": true
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `EnableHelperCheck` | bool | `true` | Scan for known cheat tools |
| `EnableOtherChecks` | bool | `true` | Cheat window title scanning |
| `EnableProxyDllCheck` | bool | `true` | Detect proxy DLL injection |
| `EnableVMCheck` | bool | `true` | Detect virtual machines (Wine/Linux safe) |
| `EnableHWIDCheck` | bool | `false` | Enforce HWID-locked license keys |

---

## UpdateServers

Patch server configuration for auto-updates.

```json
{
  "UpdateServers": [
    {
      "UpdateServerName": "Primary",
      "Link": "https://patches.yourserver.com/"
    }
  ]
}
```

The launcher downloads `filelist.txt` from the patch URL, compares CRC32 checksums, and downloads changed files.

---

## NetworkRoutes

Advanced multi-route configuration for servers with multiple connection paths.

```json
{
  "NetworkRoutes": {
    "OverrideWorld": 0,
    "Routes": [
      {
        "RouteName": "US East",
        "PingAddress": "us-east.yourserver.com",
        "LoginServer": { "Ip": "10.0.1.1", "Port": 10001 },
        "ZoneServers": [
          { "Ip": "10.0.1.2", "Port": 27780 }
        ]
      }
    ]
  }
}
```

When routes are configured, the launcher measures latency to each and selects the fastest.

---

## CustomCryptKeyRequest

Override the RF Online login protocol encryption parameters.

```json
{
  "CustomCryptyKeyRequest": {
    "EnableCustomCryptKeyRequest": false,
    "byPlus": 1,
    "wCryptKey": 3
  }
}
```

> **Warning**: Only change these if your server uses non-standard crypt parameters. Default values match standard RF Online 2.2.3.2.

---

## Complete Example

See [CONFIG_CREATION.md](CONFIG_CREATION.md) for a step-by-step guide to building your configuration from scratch.
