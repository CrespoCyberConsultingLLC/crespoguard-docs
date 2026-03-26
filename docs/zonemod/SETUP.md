# ZoneMod Setup Guide

> Deploy CrespoGuard ZoneMod to your RF Online server — anti-cheat, game hooks,
> JavaScript scripting, and a web dashboard. One executable replaces your ZoneServer
> startup script.

---

## Prerequisites

| Requirement | Detail |
|-------------|--------|
| **OS** | Windows Server 2012 R2+ or Windows 10/11 |
| **RF Online** | ZoneServerUD_x64.exe v2.2.3.2 |
| **SQL Server** | Express or Standard with RF_User, RF_World databases |
| **Visual C++ Runtime** | 2019+ x64 redistributable |
| **ODBC Driver** | SQL Server or ODBC Driver 17 |
| **WebView2 Runtime** | Microsoft Edge Chromium (optional, for dashboard) |
| **License** | CrespoGuard Premium (Guard+ tier) |

!!! note "Administrator required"
    CrespoGuard.exe requires admin privileges for DLL injection into the ZoneServer process.

---

## What's in the Package

```
RF_Bin/
├── CrespoGuard.exe              # Loader — launches ZoneServer + injects DLL
├── CrespoGuardMod.dll           # The mod DLL (injected into ZoneServer)
├── crespoguard.json             # Auth & loader configuration
├── zonemod.json                 # Module enable/disable + settings
├── ZoneServerUD_x64.exe         # Your RF Online ZoneServer
│
└── CrespoGuard/
    ├── scripts/                 # JavaScript plugin scripts (.js)
    ├── plugins/                 # Compiled bytecode plugins (.cgp)
    └── logs/                    # Log output
```

!!! warning "DLL location"
    `CrespoGuardMod.dll` goes **directly in RF_Bin/**, not in the `CrespoGuard/` subfolder.

---

## Quick Start

### Step 1: Copy Files

Copy `CrespoGuard.exe`, `CrespoGuardMod.dll`, and the `CrespoGuard/` folder into your
`RF_Bin/` directory alongside `ZoneServerUD_x64.exe`.

### Step 2: Configure crespoguard.json

Create `crespoguard.json` in `RF_Bin/`:

```json
{
  "ServerName": "My RF Online Server",
  "LogLevel": 1,

  "Auth": {
    "ListenIP": "0.0.0.0",
    "ListenPort": 10001,
    "DashboardPort": 8080,
    "DashboardEnabled": true
  },

  "Database": {
    "Server": ".\\SQLEXPRESS",
    "Database": "RF_User",
    "Username": "sa",
    "Password": "your_password",
    "Driver": "SQL Server"
  },

  "ZoneServerPath": "ZoneServerUD_x64.exe",
  "ZoneServerArgs": "cjdrnrwkd",
  "ModDllPath": "CrespoGuardMod.dll",
  "ModConfigPath": "zonemod.json",
  "DashboardPort": 8081
}
```

### Step 3: Configure zonemod.json

Create `zonemod.json` in `RF_Bin/` to enable/disable modules:

```json
{
  "OdbcFixEnabled": true,
  "modules": {
    "server_rates": {
      "enabled": true,
      "settings": {
        "exp_rate": 5.0,
        "drop_rate": 2.0,
        "gold_rate": 3.0,
        "mining_rate": 2.0
      }
    },
    "movement_validation": {
      "enabled": true,
      "settings": {
        "speed_threshold": 1.5,
        "teleport_distance": 500
      }
    },
    "js_scripting": {
      "enabled": true
    },
    "js_event_hooks": {
      "enabled": true
    }
  }
}
```

!!! tip "enabled must be first"
    The `"enabled"` key must be the **first key** in each module object.
    The JSON parser reads keys in order.

### Step 4: Start

Run `CrespoGuard.exe` from the `RF_Bin/` directory:

```
cd RF_Bin
CrespoGuard.exe
```

This single command:

1. Starts the Auth Server (port 10001)
2. Launches ZoneServer in suspended state
3. Injects CrespoGuardMod.dll
4. Resumes ZoneServer with all hooks active
5. Opens the web dashboard (ports 8080/8081)

### Step 5: Verify

Open `http://localhost:8081` in your browser to access the ZoneMod dashboard.
You should see all enabled modules listed with their current settings.

---

## Ports

| Port | Service | Required |
|------|---------|----------|
| 10001 | CrespoGuard Auth | Yes |
| 27780 | ZoneServer (direct) | Yes |
| 8080 | Auth Dashboard | Optional |
| 8081 | ZoneMod Dashboard | Optional |

!!! note "Firewall"
    Open ports 10001 and 27780 in your firewall for player connections.
    Dashboard ports (8080/8081) should only be accessible from your admin machine.

---

## Adding JavaScript Scripts

1. Create `.js` files in `CrespoGuard/scripts/`
2. Use `/js reload` in-game or click **Reload Scripts** in the dashboard
3. No server restart needed

See [JS Scripting](SCRIPTING.md) for the quickstart guide.

---

## Module Configuration

Modules are configured in `zonemod.json` and can be changed live through the
dashboard — no server restart required.

See [Module Catalog](CATALOG.md) for the full list of 100+ available modules
and [Configuration Guide](CONFIGURATION.md) for detailed settings.

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "DLL not found" | Ensure `CrespoGuardMod.dll` is in `RF_Bin/` (not in `CrespoGuard/`) |
| Dashboard won't load | Install WebView2 Runtime, check ports 8080/8081 not in use |
| ZoneServer crashes on start | Check `CrespoGuard/logs/` for error details |
| Modules not loading | Verify `"enabled": true` is the first key in each module |
| Scripts not running | Enable both `js_scripting` and `js_event_hooks` in zonemod.json |
| "Access denied" | Run CrespoGuard.exe as Administrator |
| Database connection fails | Check ODBC driver installed and credentials in crespoguard.json |

---

## Next Steps

- [Module Catalog](CATALOG.md) — All 100+ modules with descriptions
- [Configuration Guide](CONFIGURATION.md) — Detailed module settings
- [GM Commands](GM_COMMANDS.md) — In-game admin commands
- [JS Scripting](SCRIPTING.md) — Write custom game logic
- [AI Assistant](../ai/OVERVIEW.md) — AI-powered configuration helper
