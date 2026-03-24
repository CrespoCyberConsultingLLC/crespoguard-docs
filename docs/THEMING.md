
# Theming & Branding

> How to make the launcher look like YOUR server, not a generic template.

## Overview

The launcher is fully white-label. Every visual element — colors, fonts, effects, layout, text, images — is configurable through `modules.json`. No code changes required.

## Color Schemes

### How Colors Work

The launcher uses 9 configurable colors in the `ThemeConfig` section. All values are `[R, G, B, A]` arrays (0–255).

```
┌─────────────────────────────────────────────────────────┐
│ TitleBarBgColor                                         │
├───────────────┬─────────────────────────────────────────┤
│               │                                         │
│  SidebarBg    │  PanelBgColor                          │
│               │                                         │
│  AccentColor  │  ButtonBaseColor / ButtonHoverColor     │
│  (highlights) │  ErrorColor / SuccessColor              │
│  AccentDim    │                                         │
│  (hover)      │                                         │
│               │                                         │
├───────────────┴─────────────────────────────────────────┤
│ StatusBar (AccentColor tint)                            │
└─────────────────────────────────────────────────────────┘
```

### Example Themes

#### Cyberpunk (Default)
```json
{
  "AccentColor": [0, 198, 255, 255],
  "AccentDimColor": [0, 127, 178, 255],
  "PanelBgColor": [6, 8, 16, 220],
  "SidebarBgColor": [6, 6, 14, 235],
  "TitleBarBgColor": [4, 4, 10, 230],
  "ButtonBaseColor": [15, 70, 120, 255],
  "ButtonHoverColor": [20, 100, 160, 255]
}
```

#### Warm Gold
```json
{
  "AccentColor": [255, 180, 50, 255],
  "AccentDimColor": [200, 140, 30, 255],
  "PanelBgColor": [12, 10, 8, 230],
  "SidebarBgColor": [10, 8, 6, 240],
  "TitleBarBgColor": [8, 6, 4, 235],
  "ButtonBaseColor": [120, 70, 15, 255],
  "ButtonHoverColor": [160, 100, 20, 255]
}
```

#### Crimson Red
```json
{
  "AccentColor": [220, 40, 40, 255],
  "AccentDimColor": [160, 30, 30, 255],
  "PanelBgColor": [14, 6, 6, 225],
  "SidebarBgColor": [12, 4, 4, 240],
  "TitleBarBgColor": [10, 3, 3, 235],
  "ButtonBaseColor": [100, 20, 20, 255],
  "ButtonHoverColor": [140, 35, 35, 255]
}
```

#### Emerald Green
```json
{
  "AccentColor": [40, 200, 120, 255],
  "AccentDimColor": [30, 150, 90, 255],
  "PanelBgColor": [6, 14, 10, 225],
  "SidebarBgColor": [4, 12, 8, 240],
  "TitleBarBgColor": [3, 10, 6, 235],
  "ButtonBaseColor": [15, 90, 55, 255],
  "ButtonHoverColor": [20, 120, 75, 255]
}
```

#### Royal Purple
```json
{
  "AccentColor": [160, 80, 255, 255],
  "AccentDimColor": [120, 60, 200, 255],
  "PanelBgColor": [10, 6, 16, 225],
  "SidebarBgColor": [8, 4, 14, 240],
  "TitleBarBgColor": [6, 3, 12, 235],
  "ButtonBaseColor": [60, 25, 110, 255],
  "ButtonHoverColor": [85, 40, 150, 255]
}
```

#### Clean Minimal (no tint)
```json
{
  "AccentColor": [180, 180, 190, 255],
  "AccentDimColor": [140, 140, 150, 255],
  "PanelBgColor": [18, 18, 22, 235],
  "SidebarBgColor": [14, 14, 18, 245],
  "TitleBarBgColor": [10, 10, 14, 240],
  "ButtonBaseColor": [50, 50, 60, 255],
  "ButtonHoverColor": [70, 70, 85, 255]
}
```

### Color Design Tips

- **AccentColor** is the most visible — it colors the sidebar highlights, glow borders, status indicators, and links
- **Panel backgrounds** should have high alpha (220–245) for text readability over the background image
- **Button colors** should be darker shades of your accent — too bright and they compete with text
- Keep **ErrorColor** red-ish and **SuccessColor** green-ish for universal recognition

## Effects

The default launcher has a sci-fi aesthetic with scan lines, floating particles, and glow effects. These can be dialed down or disabled entirely.

```json
{
  "ThemeConfig": {
    "EnableScanLines": false,
    "EnableParticles": false,
    "EffectIntensity": 0.0,
    "GlowIntensity": 1.0
  }
}
```

| Setting | What It Does |
|---------|-------------|
| `EnableScanLines: false` | Removes the horizontal line overlay |
| `EnableParticles: false` | Removes floating background dots |
| `EffectIntensity: 0.5` | Halves the opacity of all effects (scan lines + particles) |
| `GlowIntensity: 0.0` | Removes the neon glow around panels and borders |

**For a clean, modern look**: disable scan lines and particles, keep glow at 0.5–1.0.

**For maximum sci-fi**: leave everything enabled at 1.0.

**For a professional/corporate feel**: disable everything, use the Clean Minimal color scheme.

## Fonts

### Using Custom Fonts

1. Place your `.ttf` font files in `System\Launcher\fonts\`
2. Reference them in the `Branding` section:

```json
{
  "Branding": {
    "FontBody": "NotoSans-Regular.ttf",
    "FontBold": "NotoSans-Bold.ttf",
    "FontTitle": "Cinzel-Bold.ttf",
    "FontSizeBody": 15.0,
    "FontSizeTitle": 22.0,
    "FontSizeSmall": 12.0
  }
}
```

### Font Recommendations by Style

| Aesthetic | Body Font | Title Font |
|-----------|-----------|------------|
| Sci-fi / Cyberpunk | Rajdhani, Exo 2, Orbitron | Orbitron, Audiowide, Rajdhani Bold |
| Fantasy / Medieval | Cinzel, EB Garamond | Cinzel Decorative, MedievalSharp |
| Modern / Clean | Noto Sans, Source Sans 3, Rubik | Montserrat, Poppins Bold |
| Korean MMO | Noto Sans KR, Spoqa Han Sans | Black Han Sans |
| Retro / Classic | IBM Plex Sans, JetBrains Mono | Press Start 2P, VT323 |

### Font Size Guidelines

| Use Case | Body | Title | Small |
|----------|------|-------|-------|
| Default (1024x600) | 16.0 | 24.0 | 13.0 |
| Larger window (1100x650) | 17.0 | 26.0 | 13.0 |
| Dense UI | 14.0 | 20.0 | 11.0 |
| Large text (accessibility) | 18.0 | 28.0 | 15.0 |

!!! note "Font slots"
    The launcher uses 3 font slots: body (all UI text), bold (emphasized text), and title (panel headers, server name). The body font is also used at a smaller size for captions and status text.

## Layout

### Window Size

```json
{
  "ThemeConfig": {
    "WindowWidth": 1100,
    "WindowHeight": 650
  }
}
```

The default 1024x600 works well for most setups. Increase for:
- Larger logos that need more sidebar space
- Servers with long news content
- Higher DPI displays

### Sidebar Width

```json
{
  "ThemeConfig": {
    "SidebarWidth": 220.0
  }
}
```

Adjust based on your logo width and button text length. Range: 180–280px.

### Logo Sizing

```json
{
  "Branding": {
    "LogoMaxWidth": 180.0,
    "LogoMaxHeight": 70.0
  }
}
```

The logo maintains aspect ratio within these bounds. If your sidebar is wider, increase `LogoMaxWidth` to match.

## Branding Text

### Status Bar

The status bar shows two text elements:

```
┌──────────────────────────────────────────────┐
│ "RF Solus Launcher v2.2"    "Powered by ..." │
└──────────────────────────────────────────────┘
  ↑ StatusBarText                ↑ FooterText
```

```json
{
  "Branding": {
    "StatusBarText": "RF Solus Launcher v2.2",
    "FooterText": "Powered by RF Solus"
  }
}
```

### Window Title

The taskbar/title shows: `{WindowTitle} - {ServerName}`

```json
{
  "Branding": {
    "WindowTitle": "RF Solus"
  }
}
```

Result: `RF Solus - RF Solus` (or just `RF Solus` if ServerName matches).

**Tip**: If your WindowTitle already contains the server name, the result may look redundant. Set WindowTitle to something short like `"Launcher"` to get `"Launcher - RF Solus"`.

## Localization

All UI strings can be customized per-language. Edit `System\Launcher\Language\en_gb.json`:

```json
{
  "NAV_HOME": "HOME",
  "NAV_LOGIN": "[ LOGIN ]",
  "NAV_REGISTER": "REGISTER",
  "REG_CREATE_ACCOUNT": "CREATE ACCOUNT",
  "REG_BACK_TO_LOGIN": "BACK TO LOGIN",
  "SIDEBAR_PLAYERS": "Players",
  "WORLDLIST_STATUS_ONLINE": "ONLINE",
  "WORLDLIST_STATUS_OFFLINE": "OFFLINE",
  "MAIN_BUTTON_LOGIN": "LOGIN",
  "MAIN_LABEL_MEMBER_LOGIN": "MEMBER LOGIN",
  "BUTTON_SETTINGS": "SETTINGS",
  "MAIN_BUTTON_MYACCOUNT": "ACCOUNTS"
}
```

You can rename every button, label, and status message to match your server's language and tone.

## Putting It All Together

Here's a complete themed config for a fantasy-styled server:

```json
{
  "Branding": {
    "WindowTitle": "Realm of Aether",
    "StatusBarText": "Realm of Aether v3.1",
    "FooterText": "aether-rf.com",
    "FontBody": "NotoSans-Regular.ttf",
    "FontBold": "NotoSans-Bold.ttf",
    "FontTitle": "Cinzel-Bold.ttf",
    "FontSizeBody": 15.0,
    "FontSizeTitle": 22.0,
    "FontSizeSmall": 12.0,
    "LogoMaxWidth": 190.0,
    "LogoMaxHeight": 80.0
  },
  "ThemeConfig": {
    "AccentColor": [200, 160, 80, 255],
    "AccentDimColor": [160, 120, 60, 255],
    "PanelBgColor": [14, 12, 10, 230],
    "SidebarBgColor": [12, 10, 8, 240],
    "TitleBarBgColor": [10, 8, 6, 235],
    "ButtonBaseColor": [90, 65, 30, 255],
    "ButtonHoverColor": [130, 95, 45, 255],
    "EnableScanLines": false,
    "EnableParticles": false,
    "EffectIntensity": 0.0,
    "GlowIntensity": 0.7,
    "SidebarWidth": 230.0,
    "WindowWidth": 1100,
    "WindowHeight": 650
  }
}
```

Combined with a parchment-toned background.png, a medieval-style logo.png, and Cinzel font — this creates a completely different identity from the default cyberpunk look, using the same binary.
