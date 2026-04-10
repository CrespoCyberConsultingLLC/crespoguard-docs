
# Configuration Reference

> Complete reference for every field in `modules.json`. All sections are optional — missing sections use defaults.

## Table of Contents

- [ServerConfig](#serverconfig)
- [Branding](#branding)
- [ThemeConfig](#themeconfig)
- [FeatureFlags](#featureflags)
- [ExternalLinks](#externallinks)
- [What's New Modal](#whats-new-modal)
- [AuthLimits](#authlimits)
- [ClientLimiter](#clientlimiter)
- [Localization](#localization)
- [SecureLogin](#securelogin)
- [SecurityCheck](#securitycheck)
- [UpdateServers](#updateservers)
- [NetworkRoutes](#networkroutes)
- [CustomCryptKeyRequest](#customcryptkeyrequest)


## ServerConfig

Core server connection settings. This is the only required section — the launcher needs it to know where to connect.

```json
{
  "ServerConfig": {
    "ServerName": "My RF Server",
    "LoginServerIp": "192.168.1.100",
    "LoginServerPort": 10001,
    "ZoneServerIp": "192.168.1.100",
    "ZoneServerPort": 27780,
    "ServerVersion": "2.2.3.2",
    "IsSirin": false,
    "Key": "0000000000000000-20270101-00000000000000000000000000000000"
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `ServerName` | string | `""` | Server name displayed in the sidebar and window title |
| `LoginServerIp` | string | `""` | IP address of your LoginServer (or relay, if using SecureLogin) |
| `LoginServerPort` | int | `10001` | TCP port of your LoginServer |
| `ZoneServerIp` | string | `""` | IP address of your ZoneServer |
| `ZoneServerPort` | int | `27780` | TCP port of your ZoneServer |
| `ServerVersion` | string | `"2.2.3.2"` | RF Online client version string |
| `IsSirin` | bool | `false` | Enable Sirin server compatibility (requires `sirin-launcher.dll` in client directory) |
| `Key` | string | `""` | CrespoGuard license key. Use `0000000000000000-20270101-00000000000000000000000000000000` for Community tier without a license. |


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


## ThemeConfig

Visual theme settings — colors, effects, and layout dimensions. All color values are `[R, G, B, A]` arrays (0–255). See [Theming & Branding](THEMING.md) for example themes and design tips.

```json
{
  "ThemeConfig": {
    "AccentColor": [0, 198, 255, 255],
    "AccentDimColor": [0, 127, 178, 255],
    "PanelBgColor": [6, 8, 16, 220],
    "SidebarBgColor": [6, 6, 14, 235],
    "TitleBarBgColor": [4, 4, 10, 230],
    "ButtonBaseColor": [15, 70, 120, 255],
    "ButtonHoverColor": [20, 100, 160, 255],
    "ErrorColor": [255, 89, 89, 255],
    "SuccessColor": [0, 204, 76, 255],
    "EnableScanLines": true,
    "EnableParticles": true,
    "EffectIntensity": 1.0,
    "GlowIntensity": 1.0,
    "SidebarWidth": 200.0,
    "WindowWidth": 1024,
    "WindowHeight": 600
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `AccentColor` | int[4] | `[0, 198, 255, 255]` | Primary accent color (sidebar highlights, glow borders, status indicators, links) |
| `AccentDimColor` | int[4] | `[0, 127, 178, 255]` | Dimmed accent color (hover states) |
| `PanelBgColor` | int[4] | `[6, 8, 16, 220]` | Main content panel background |
| `SidebarBgColor` | int[4] | `[6, 6, 14, 235]` | Sidebar background |
| `TitleBarBgColor` | int[4] | `[4, 4, 10, 230]` | Title bar background |
| `ButtonBaseColor` | int[4] | `[15, 70, 120, 255]` | Default button color |
| `ButtonHoverColor` | int[4] | `[20, 100, 160, 255]` | Button hover color |
| `ErrorColor` | int[4] | `[255, 89, 89, 255]` | Error message and indicator color |
| `SuccessColor` | int[4] | `[0, 204, 76, 255]` | Success message and indicator color |
| `EnableScanLines` | bool | `true` | Show horizontal scan line overlay effect |
| `EnableParticles` | bool | `true` | Show floating background particle dots |
| `EffectIntensity` | float | `1.0` | Opacity multiplier for all effects — scan lines and particles (0.0 = hidden, 1.0 = full) |
| `GlowIntensity` | float | `1.0` | Neon glow intensity around panels and borders (0.0 = none, 1.0 = full) |
| `SidebarWidth` | float | `200.0` | Sidebar width in pixels (recommended range: 180–280) |
| `WindowWidth` | int | `1024` | Launcher window width in pixels |
| `WindowHeight` | int | `600` | Launcher window height in pixels |


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


## ExternalLinks

URLs for external services shown in the launcher (social media buttons, website links, registration page, etc.).

```json
{
  "ExternalLinks": {
    "DiscordUrl": "https://discord.gg/yourserver",
    "FacebookUrl": "https://facebook.com/yourserver",
    "WebsiteUrl": "https://yourserver.com",
    "RegisterUrl": "https://yourserver.com/register",
    "RegistrationApiUrl": "https://api.yourserver.com/register",
    "NewsUrl": "https://yourserver.com/news",
    "WorldStatusUrl": "https://yourserver.com/status"
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `DiscordUrl` | string | `""` | Discord invite link (opens in browser when clicked) |
| `FacebookUrl` | string | `""` | Facebook page URL |
| `WebsiteUrl` | string | `""` | Server website URL |
| `RegisterUrl` | string | `""` | Account registration page URL (opens in browser) |
| `RegistrationApiUrl` | string | `""` | Registration API endpoint for in-launcher registration |
| `NewsUrl` | string | `""` | News feed URL for the sliding news carousel |
| `WorldStatusUrl` | string | `""` | World status API endpoint (required when `EnableWorldStatus` is `true` in FeatureFlags) |

Empty strings hide the corresponding button or feature in the launcher UI.


## What's New Modal

After an update, the launcher automatically shows a "What's New" modal to players listing the changes in the new version. The modal appears once per version — players who are already up to date never see it.

Release notes are managed through the admin portal. When you publish a new release, the portal serves the changelog for that version. The launcher fetches it from the configured release notes URL and displays it in the modal.

The status bar also shows the current launcher version using the `StatusBarText` field in [Branding](#branding). Keep this in sync with your release when you publish an update.

### News Tab

A dedicated News tab in the launcher loads content from the `NewsUrl` configured in [ExternalLinks](#externallinks). This is separate from the "What's New" modal — it shows ongoing server news and announcements rather than release-specific changelog entries.

To enable the news tab, set `NewsUrl` to a URL that returns your news content and ensure `EnableSlidingNews` is `true` in [FeatureFlags](#featureflags).


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


## ClientLimiter

Multi-client restriction settings. Prevents players from running more than a set number of game instances simultaneously.

```json
{
  "ClientLimiter": {
    "EnableClientLimit": true,
    "MaxInstanceCount": 3
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `EnableClientLimit` | bool | `false` | Enable multi-client restriction |
| `MaxInstanceCount` | int | `3` | Maximum number of simultaneous game instances allowed per machine |


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


## SecureLogin

Encrypted tunnel configuration for connecting through the CrespoGuard Relay. When enabled, the launcher establishes an AES-256-GCM encrypted tunnel to the relay instead of connecting directly to the LoginServer. See [Relay Overview](RELAY.md) for full setup details.

```json
{
  "SecureLogin": {
    "EnableSecureLogin": true,
    "SecureLoginType": 1,
    "SecureLoginHost": "YOUR.PUBLIC.IP",
    "SecureLoginIP": "YOUR.PUBLIC.IP",
    "SecureLoginPort": 10002,
    "SecureLoginPSK": "YOUR_64_CHAR_HEX_PSK",
    "EnableZoneProxy": false,
    "ZoneProxyIP": "YOUR.PUBLIC.IP",
    "ZoneProxyPort": 27780,
    "RoutingCode": "",
    "EnableEdgeRelays": false,
    "AutoSelectRelay": true,
    "EdgeRelays": []
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `EnableSecureLogin` | bool | `false` | Enable the encrypted tunnel to the relay |
| `SecureLoginType` | int | `1` | Connection mode: `0` = transparent proxy (plain TCP, no encryption), `1` = CGRD encrypted tunnel (AES-256-GCM) |
| `SecureLoginHost` | string | `""` | Relay hostname or IP (used for DNS resolution) |
| `SecureLoginIP` | string | `""` | Relay IP address (used for the TCP connection) |
| `SecureLoginPort` | int | `10002` | Relay listen port |
| `SecureLoginPSK` | string | `""` | 64-character hex pre-shared key for AES-256-GCM. Must match the `PSK` in the relay's `server.json`. Generate with `python -c "import secrets; print(secrets.token_hex(32))"` |
| `EnableZoneProxy` | bool | `false` | Route ZoneServer traffic through the relay as well |
| `ZoneProxyIP` | string | `""` | IP address for the zone proxy (when `EnableZoneProxy` is true) |
| `ZoneProxyPort` | int | `27780` | Port for the zone proxy |
| `RoutingCode` | string | `""` | Edge routing code (used with edge-hosted relays) |
| `EnableEdgeRelays` | bool | `false` | Enable CrespoGuard edge-hosted relay routing |
| `AutoSelectRelay` | bool | `true` | Automatically select the lowest-latency edge relay |
| `EdgeRelays` | array | `[]` | List of edge relay endpoints (used with edge-hosted relays) |

See [Creating config.bin](CONFIG_CREATION.md) for PSK generation and the full encryption walkthrough.


## SecurityCheck

Client-side security features. Community edition includes the full anti-cheat suite.

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


## UpdateServers

Patch server configuration for the auto-updater. This is a JSON array — each entry defines one patch server. The launcher tries servers in order and falls back to the next if one is unreachable. Requires `EnableAutoUpdate: true` in [FeatureFlags](#featureflags).

```json
{
  "UpdateServers": [
    {
      "UpdateServerName": "Primary",
      "Link": "https://patches.yourserver.com/"
    },
    {
      "UpdateServerName": "Mirror",
      "Link": "https://mirror.yourserver.com/"
    }
  ]
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `UpdateServerName` | string | `""` | Display name for this patch server (shown in update progress UI) |
| `Link` | string | `""` | Base URL of the patch server. Must host `filelist.txt` and all patch files. Include trailing slash. |

The launcher fetches `filelist.txt` from the `Link` URL, compares CRC32 checksums against local files, and downloads any changed files. See [Deployment](DEPLOYMENT.md) for the full auto-update setup guide.


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


## CustomCryptKeyRequest

Custom encryption key parameters for the RF Online client connection handshake. Only needed if your server uses non-standard encryption values. Most servers should leave this disabled.

```json
{
  "CustomCryptKeyRequest": {
    "EnableCustomCryptKeyRequest": false,
    "byPlus": 1,
    "wCryptKey": 3
  }
}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `EnableCustomCryptKeyRequest` | bool | `false` | Enable custom encryption key override for the connection handshake |
| `byPlus` | int | `1` | Additive modifier for the crypt key calculation |
| `wCryptKey` | int | `3` | Base encryption key value sent during the handshake |

!!! warning "Advanced setting"
    Only modify these values if your server uses a custom encryption handshake. Incorrect values will prevent clients from connecting. Leave disabled for standard RF Online 2.2.3.2 servers.


## Complete Example

See [CONFIG_CREATION.md](CONFIG_CREATION.md) for a step-by-step guide to building your configuration from scratch.
