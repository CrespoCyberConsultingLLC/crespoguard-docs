
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


## Complete Example

See [CONFIG_CREATION.md](CONFIG_CREATION.md) for a step-by-step guide to building your configuration from scratch.
