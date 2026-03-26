# Module Configuration Guide

All CrespoGuard modules are configured through a single JSON file and can be
managed via the web dashboard or in-game GM commands. This guide covers both
approaches and the most common configuration patterns.

---

## How Configuration Works

Every module's settings live in **`zonemod.json`**, located in the
`RF_Bin/CrespoGuard/` directory alongside the DLL.

The file follows a straightforward structure:

```json
{
    "modules": {
        "module_name": {
            "enabled": true,
            "settings": {
                "key": "value"
            }
        }
    }
}
```

Key points:

- **`modules.<name>.enabled`** toggles the module on or off.
- **`modules.<name>.settings`** holds every configurable value the module
  exposes.
- The server **hot-reloads** `zonemod.json` automatically — changes take effect
  within **5 seconds** without a restart.
- Always place `"enabled"` as the **first key** inside each module block.
  The parser finds the first `"enabled"` it encounters, so nested keys with the
  same name can shadow the top-level toggle.

!!! warning "JSON Syntax"
    `zonemod.json` must be valid JSON. A single misplaced comma or missing quote
    will prevent the entire file from loading. Use a JSON validator if you edit
    the file by hand.

---

## Using the Dashboard

The CrespoGuard Dashboard runs as a local web UI and provides the easiest way
to manage modules.

### Opening the Modules Tab

1. Launch the CrespoGuard Loader.
2. Click the **Modules** tab in the top navigation bar.
3. Modules are grouped by **category** (Rates, PvP, Security, etc.).

### Editing a Module

1. **Find the module** — scroll through the list or use the search bar at the
   top.
2. Click the **Settings** button next to the module name to expand its
   configuration panel.
3. Edit values directly in the input fields.
4. Click **Save**. The dashboard writes changes to `zonemod.json` and the
   server picks them up on its next reload cycle.

### Understanding Module States

| State       | Meaning                                                  |
|-------------|----------------------------------------------------------|
| **Enabled** | Module is active and processing game events.             |
| **Disabled**| Module is loaded but all logic is skipped.               |
| **LOCKED**  | Security module — always enabled, toggle is read-only.   |

!!! note "Locked Modules"
    Modules marked **LOCKED** are security-critical and cannot be disabled from
    the dashboard or config file. Their settings panel is read-only.

---

## Using GM Commands

Every module registers a **GM command prefix** (e.g., `.exp`, `.pvp`, `.buff`).
Commands are typed in the in-game chat window and require **GM degree 2 or
higher**.

### Universal Sub-Commands

All modules support a common set of sub-commands:

| Command                          | Description                            |
|----------------------------------|----------------------------------------|
| `.prefix status`                 | Display current settings and state.    |
| `.prefix set <key> <value>`      | Change a setting at runtime.           |
| `.prefix reload`                 | Force an immediate reload from file.   |
| `.prefix help`                   | List every command the module supports. |

### Examples

```
.exp status
```

> Shows the current EXP multiplier, per-race overrides, and active schedule.

```
.exp set exp_multiplier 3.0
```

> Sets the global EXP multiplier to 3x immediately.

```
.pvp reload
```

> Re-reads PvP module settings from `zonemod.json` without waiting for the
> auto-reload timer.

```
.buff help
```

> Prints every sub-command available for the buff module.

!!! tip "Runtime vs. Persistent"
    `.prefix set` changes the value **in memory only**. If the server restarts
    before the next config write, the old value from `zonemod.json` is restored.
    To make a change permanent, also update `zonemod.json` (via the dashboard or
    by hand).

---

## Common Configuration Patterns

### Rate Multipliers

Most rate modules accept a simple floating-point multiplier.

```json title="zonemod.json"
{
    "modules": {
        "server_rates": {
            "enabled": true,
            "settings": {
                "exp_multiplier": 2.0,
                "drop_multiplier": 1.5,
                "mining_multiplier": 3.0,
                "dalant_multiplier": 2.0
            }
        }
    }
}
```

| Value | Effect         |
|-------|----------------|
| `1.0` | No change (1x) |
| `2.0` | Double (2x)    |
| `0.5` | Halved (0.5x)  |

### Per-Race Settings

Modules that support per-race tuning nest values under a `races` object.

```json title="zonemod.json"
{
    "modules": {
        "race_rates": {
            "enabled": true,
            "settings": {
                "races": {
                    "bellato": {
                        "exp_mult": 1.0,
                        "drop_mult": 1.0
                    },
                    "cora": {
                        "exp_mult": 1.2,
                        "drop_mult": 1.1
                    },
                    "accretia": {
                        "exp_mult": 1.1,
                        "drop_mult": 1.0
                    }
                }
            }
        }
    }
}
```

Race keys are always lowercase: `bellato`, `cora`, `accretia`.

### Reward Definitions

Modules like daily login or event rewards use ordered arrays.

```json title="zonemod.json"
{
    "modules": {
        "daily_rewards": {
            "enabled": true,
            "settings": {
                "rewards": [
                    {
                        "dalant": 10000,
                        "gold": 0,
                        "item_code": "",
                        "message": "Day 1 — Welcome back!"
                    },
                    {
                        "dalant": 20000,
                        "gold": 100,
                        "item_code": "",
                        "message": "Day 2 — Keep it up!"
                    },
                    {
                        "dalant": 50000,
                        "gold": 500,
                        "item_code": "iw30a",
                        "message": "Day 3 — Bonus weapon!"
                    }
                ]
            }
        }
    }
}
```

Array order matters — index 0 is Day 1, index 1 is Day 2, and so on.

### Scheduled Events

Time-based modules accept day-of-week and hour ranges.

```json title="zonemod.json"
{
    "modules": {
        "weekend_event": {
            "enabled": true,
            "settings": {
                "schedule_days": [0, 5, 6],
                "start_hour": 18,
                "end_hour": 23,
                "exp_multiplier": 5.0,
                "announcement": "Weekend EXP event is LIVE!"
            }
        }
    }
}
```

| Day Value | Day       |
|-----------|-----------|
| `0`       | Sunday    |
| `1`       | Monday    |
| `2`       | Tuesday   |
| `3`       | Wednesday |
| `4`       | Thursday  |
| `5`       | Friday    |
| `6`       | Saturday  |

Hours use **24-hour format** in the server's local timezone.

### Boolean Toggles

Many modules expose simple on/off flags for individual features.

```json title="zonemod.json"
{
    "modules": {
        "auto_loot": {
            "enabled": true,
            "settings": {
                "auto_pickup": true,
                "radius_scatter": true,
                "scatter_radius": 5.0,
                "skip_boss_drops": false
            }
        }
    }
}
```

---

## Enabling and Disabling Modules

### Standard Modules

Set `"enabled"` to `true` or `false`:

```json
"module_name": {
    "enabled": false,
    "settings": { ... }
}
```

A disabled module is still **loaded** by the server — it registers its hooks and
config keys — but all gameplay logic is bypassed. This means re-enabling it does
not require a restart.

### Security Modules (LOCKED)

Twenty-six modules are marked as **force-enabled** in the DLL. These cover
anti-cheat, anti-dupe, and exploit protection. You cannot disable them via
`zonemod.json` or the dashboard.

Attempting to set `"enabled": false` on a locked module has no effect — the
server overrides it to `true` at load time.

To identify locked modules, look for the **LOCKED** badge in the dashboard
Modules tab.

---

## Full Example: zonemod.json

Below is a minimal but realistic configuration covering several modules:

```json title="zonemod.json"
{
    "modules": {
        "server_rates": {
            "enabled": true,
            "settings": {
                "exp_multiplier": 2.0,
                "drop_multiplier": 1.5,
                "dalant_multiplier": 2.0,
                "mining_multiplier": 3.0
            }
        },
        "auto_loot": {
            "enabled": true,
            "settings": {
                "auto_pickup": true,
                "radius_scatter": true,
                "scatter_radius": 5.0,
                "skip_boss_drops": false
            }
        },
        "pvp_balance": {
            "enabled": true,
            "settings": {
                "damage_cap": 9999,
                "heal_cap": 5000
            }
        },
        "daily_rewards": {
            "enabled": false,
            "settings": {
                "rewards": []
            }
        }
    }
}
```

---

## Troubleshooting

### Settings Not Taking Effect

1. **Check the logs.** Open `CrespoGuard/logs/` and look for JSON parse errors
   or module warnings.
2. **Verify with a GM command.** Run `.prefix status` in-game to see what the
   server actually loaded.
3. **Force a reload.** Run `.prefix reload` to bypass the 5-second auto-reload
   timer.
4. **Validate JSON syntax.** Paste your `zonemod.json` into a JSON validator
   (e.g., [jsonlint.com](https://jsonlint.com)) to catch missing commas,
   trailing commas, or unquoted keys.

### Module Shows as Disabled When It Should Be Enabled

- Make sure `"enabled"` is the **first key** in the module block. If a nested
  object also contains an `"enabled"` key, the parser may read that one instead.

### Locked Module Settings Are Read-Only in Dashboard

- This is expected. Security modules accept no configuration changes from the
  UI. If you need to adjust a locked module's internal thresholds, contact the
  server operator.

### GM Commands Return "Permission Denied"

- Your account must have **GM degree 2 or higher**. Ask the server
  administrator to promote your account.

### Changes Lost After Restart

- If you used `.prefix set` without also updating `zonemod.json`, the in-memory
  value is discarded on restart. Always save changes to the config file for
  persistence.
