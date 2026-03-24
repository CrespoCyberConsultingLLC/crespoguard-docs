---
title: Assets
layout: default
nav_order: 6
---

# Asset Preparation Guide

> Specifications for logo, background, fonts, music, and language files.

## Directory Structure

All assets live under `System\Launcher\` in the game client directory:

```
System/
└── Launcher/
    ├── Config/
    │   └── config.bin              # Encrypted configuration (generated)
    ├── fonts/
    │   ├── Rajdhani-Regular.ttf    # Body font
    │   ├── Rajdhani-Bold.ttf       # Bold font
    │   └── Orbitron-Variable.ttf   # Title font
    ├── Language/
    │   └── en_gb.json              # English strings (365+ keys)
    ├── Music/
    │   └── theme.mp3               # Background music
    ├── logo.png                    # Server logo
    └── background.png              # Background image
```

## Logo

### Specifications

| Property | Requirement |
|----------|-------------|
| **Format** | PNG (preferred, supports transparency) or JPG |
| **Filename** | `logo.png` or `logo.jpg` (PNG checked first) |
| **Recommended size** | 340 x 120 pixels |
| **Maximum display** | Configurable via `LogoMaxWidth` / `LogoMaxHeight` (default: 170x60px) |
| **Transparency** | Supported (PNG alpha channel) — recommended for dark backgrounds |
| **Color space** | sRGB |

### Guidelines

- The logo is displayed in the **left sidebar** on a dark background (default ~RGB 6,6,14)
- Design for dark backgrounds — light/white logos with transparent backgrounds work best
- The launcher maintains aspect ratio — it will scale down to fit, never scale up
- If your logo is wider than the sidebar, increase `SidebarWidth` and `LogoMaxWidth` in config

### Logo Sizing Config

```json
{
  "Branding": {
    "LogoMaxWidth": 190.0,
    "LogoMaxHeight": 80.0
  },
  "ThemeConfig": {
    "SidebarWidth": 230.0
  }
}
```

## Background Image

### Specifications

| Property | Requirement |
|----------|-------------|
| **Format** | PNG or JPG |
| **Filename** | `background.png` or `background.jpg` (PNG checked first) |
| **Recommended size** | Match your window size (default 1024x600, or your custom WindowWidth x WindowHeight) |
| **Minimum size** | 1024 x 600 pixels (smaller will be stretched) |
| **Feature flag** | Must set `"EnableCustomBackground": true` in FeatureFlags |

### Guidelines

- The background is rendered **full-window** behind all UI panels
- A **dark overlay** (alpha 60) is applied automatically for text readability
- Panel backgrounds add additional darkening — your image will be partially obscured
- **Dark, moody images** work best — avoid bright/high-contrast images
- Suggested content: game screenshots (darkened), abstract art, landscape scenery
- If no background image is found, a procedural dark pattern is rendered

### Enabling

```json
{
  "FeatureFlags": {
    "EnableCustomBackground": true
  }
}
```

## Fonts

### Specifications

| Property | Requirement |
|----------|-------------|
| **Format** | TrueType (`.ttf`) only |
| **Location** | `System\Launcher\fonts\` |
| **Slots** | 3 fonts: body, bold, title |
| **Fallback** | If fonts are missing, ImGui uses its built-in bitmap font |

### Default Fonts

| Slot | Default File | Size | Used For |
|------|-------------|------|----------|
| Body | `Rajdhani-Regular.ttf` | 16px | All UI text, input fields, labels |
| Bold | `Rajdhani-Bold.ttf` | 16px | Emphasized text, server name |
| Title | `Orbitron-Variable.ttf` | 24px | Panel headers ("MEMBER LOGIN", "SELECT SERVER") |
| Small | (same as Body) | 13px | Status text, captions, version info |

### Using Custom Fonts

1. Place your `.ttf` files in `System\Launcher\fonts\`
2. Update `modules.json`:

```json
{
  "Branding": {
    "FontBody": "YourFont-Regular.ttf",
    "FontBold": "YourFont-Bold.ttf",
    "FontTitle": "YourDisplayFont.ttf",
    "FontSizeBody": 16.0,
    "FontSizeTitle": 24.0,
    "FontSizeSmall": 13.0
  }
}
```

### Font Sources (Free)

- [Google Fonts](https://fonts.google.com/) — download TTF files
- [Font Squirrel](https://www.fontsquirrel.com/) — curated free fonts
- [DaFont](https://www.dafont.com/) — themed/decorative fonts

> **Important**: Only use fonts you have the license to distribute. Google Fonts are all open-source licensed (OFL/Apache).

### CJK Support

For Korean, Chinese, or Japanese text, use fonts with CJK coverage:
- **Noto Sans KR** — Korean
- **Noto Sans SC** — Simplified Chinese
- **Noto Sans TC** — Traditional Chinese
- **Noto Sans JP** — Japanese

Set the matching `NationCode` in `Localization`.

## Music

### Specifications

| Property | Requirement |
|----------|-------------|
| **Format** | MP3 (preferred), WAV, or MIDI |
| **Location** | `System\Launcher\Music\` |
| **Behavior** | First file found alphabetically is played on loop |
| **Volume** | Controlled by player via Settings > Sound |

### Guidelines

- Place exactly **one** music file in the Music folder (the launcher plays the first file found)
- MP3 is recommended for small file size (~2-5MB for a loop)
- Music loops automatically
- Music stops when the launcher minimizes to tray (game running)
- Players can adjust volume or mute in the Settings panel

### Example

```
System/Launcher/Music/
└── lobby_theme.mp3     # ~3MB, 2-minute ambient loop
```

## Language Files

### Specifications

| Property | Requirement |
|----------|-------------|
| **Format** | JSON |
| **Location** | `System\Launcher\Language\` |
| **Naming** | `{nationCode}.json` (e.g., `en_gb.json`, `ko_kr.json`) |
| **Encoding** | UTF-8 |

### Structure

Flat key-value JSON with string values:

```json
{
  "MAIN_LABEL_MEMBER_LOGIN": "MEMBER LOGIN",
  "MAIN_LABEL_ID": "Username",
  "MAIN_LABEL_PW": "Password",
  "MAIN_BUTTON_LOGIN": "LOGIN",
  "MAIN_CHECKBOX_SAVE_USER": "Remember Me",
  "NAV_HOME": "HOME",
  "NAV_LOGIN": "[ LOGIN ]",
  "NAV_REGISTER": "REGISTER",
  "SIDEBAR_PLAYERS": "Players",
  "WORLDLIST_STATUS_ONLINE": "ONLINE",
  "WORLDLIST_STATUS_OFFLINE": "OFFLINE",
  "REG_CREATE_ACCOUNT": "CREATE ACCOUNT",
  "REG_BACK_TO_LOGIN": "BACK TO LOGIN",
  "BUTTON_SETTINGS": "SETTINGS",
  "MAIN_BUTTON_MYACCOUNT": "ACCOUNTS"
}
```

### Placeholder Formatting

Some strings support `{0}`, `{1}` placeholders:

```json
{
  "MSG_USERNAME_SHORT": "Username must be at least {0} characters",
  "MSG_CLIENT_LIMIT_REACHED": "Maximum {0} launcher instances allowed"
}
```

### Supported Languages

| Code | Language |
|------|----------|
| `en_gb` | English (UK) — default |
| `en_us` | English (US) |
| `ko_kr` | Korean |
| `zh_tw` | Traditional Chinese |
| `ja_jp` | Japanese |
| `ru_ru` | Russian |
| `pt_br` | Portuguese (Brazil) |
| `id_id` | Indonesian |

Set in `modules.json`:

```json
{
  "Localization": {
    "NationCode": "en_gb",
    "EnforceNation": false
  }
}
```

## Asset Checklist

Before distributing to players, verify:

- [ ] `logo.png` — Your server logo (transparent PNG, ~340x120px)
- [ ] `background.png` — Dark background image (match window size)
- [ ] `System\Launcher\fonts\` — All 3 font files present (or defaults)
- [ ] `System\Launcher\Language\en_gb.json` — Language strings
- [ ] `System\Launcher\Config\config.bin` — Encrypted config (generated)
- [ ] `System\Launcher\Music\` — Optional background music
- [ ] `RFLauncher.exe` — Launcher binary
- [ ] `RFLauncher.sig` — Integrity signature
- [ ] `CrespoGuard.ico` — Application icon
- [ ] `EnableCustomBackground: true` set if using background image
