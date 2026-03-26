# RF Dev Tool

> A standalone desktop tool for converting, editing, and managing RF Online game data files.
> Turn Excel spreadsheets into server-ready `.dat` and client-ready `.edf` files — no
> compiler, no scripts, no manual hex editing.

---

## What It Does

The CrespoGuard RF Dev Tool replaces the legacy PHP-based conversion scripts with a modern,
visual application. Load your Excel spreadsheets, click Convert, and get binary game files
ready for deployment.

| Feature | Description |
|---------|-------------|
| **Excel to Binary** | Convert `.xlsx` spreadsheets to RF Online `.dat` (server) and `.edf` (client) files |
| **Binary to Excel** | Reverse-import `.dat` files back to `.xlsx` for editing |
| **Visual Editors** | Edit drop tables, monsters, portals, safezones, and ore cutting visually |
| **Map Viewers** | 2D top-down and 3D OpenGL map visualization |
| **GameCP Sync** | One-click SQL Server synchronization for your Game Control Panel |
| **CGEF Encryption** | Generate encrypted `.edf` client files with CrespoGuard's proprietary format |
| **Batch Processing** | Convert entire folders of spreadsheets in one click |

---

## Community vs Premium

The RF Dev Tool is available in two tiers. The Community Edition is **free** — no license
key required.

### Community Edition (Free)

| Feature | Detail |
|---------|--------|
| Single-file conversion | Select one `.xlsx`, convert to SERVER, CLIENT, or ALL |
| DAT to Excel import | Reverse-convert `.dat` files back to editable spreadsheets |
| 2D Map Viewer | Visual map exploration with layers and coordinate tracking |
| Auto-updates | Automatic version checking and update downloads |
| Dark mode UI | Full interface customization |

### Premium Edition (Licensed)

Everything in Community, plus:

| Feature | Detail |
|---------|--------|
| Batch conversion | Convert entire folders — all `.xlsx` files at once |
| CGEF encryption | Generate encrypted `.edf` client files |
| Drop Editor | Visual monster loot table editing |
| Monster Editor | Spawn block and DMM management |
| Safezone Editor | PvP-free zone creation and sizing |
| Portal Editor | Teleport connection management with client sync |
| 3D BSP Viewer | OpenGL 3D map geometry rendering |
| Ore Cutting Editor | Ore transmutation chance editing |
| GameCP DB Sync | Direct SQL Server database synchronization |

!!! tip "Try before you buy"
    The Community Edition is fully functional for basic conversion work. Upgrade
    to Premium when you need the visual editors or batch processing.

---

## System Requirements

| Requirement | Detail |
|-------------|--------|
| **OS** | Windows 10 or 11 (64-bit) |
| **RF Version** | GU (Global Uprising) or BSB 2.2.3 |
| **Input** | `.xlsx` Excel files (OpenXML format) |
| **Internet** | Required for first activation (Premium). 7-day offline grace period after. |
| **Disk** | ~50 MB for the application |
| **Dependencies** | None — standalone portable `.exe` |

!!! note "No installation required"
    The RF Dev Tool is a single `.exe` file. Download, run, and start converting.
    Settings and license cache are stored next to the executable.

---

## Supported File Types

The tool handles all standard RF Online 2.2.3 data files:

### Item Files (46 types)

Weapons, armor, consumables, siege equipment, quest items, and more:

`FaceItem`, `UpperItem`, `LowerItem`, `GauntletItem`, `ShoeItem`, `HelmetItem`,
`WeaponItem`, `shieDItem`, `CloaKItem`, `rIngItem`, `AmuletItem`, `BulletItem`,
`MakeToolItem`, `PotionItem`, `bagItem`, `baTteryItem`, `OreItem`, `ResourceItem`,
`forCeItem`, `UnitKeyItem`, `bootYItem`, `MAPItem`, `TOWNItem`, `AnimusItem`,
`GuardTowerItem`, `TrapItem`, `SiegeKitItem`, `TicketItem`, `EventItem`,
`RecoveryItem`, `BoxItem`, `FIrecraker`, `UNmannedminer`, `RadarItem`,
`NPCLinkItem`, `CouponItem`, `UnitHead`, `UnitUpper`, `UnitLower`, `UnitArms`,
`UnitShoulder`, `UnitBack`, `UnitBullet`, `ItemMakeData`, `ItemCombine`,
`BattleDungeonItem`, `dungeon`

### Quest Files (16+ types)

All quest event types, quest items, and client-side quest data.

### Composite EDF Files (9 builders)

These are special merged output files that combine multiple sheets:

| Output | Contents |
|--------|----------|
| `Item.edf` | All 46 item types merged with per-section headers |
| `Quest.edf` | 7 event types merged with quest descriptions |
| `Character.edf` | Class definitions + monster character data |
| `SkillForce.edf` | Skills, forces, class skills, bullet effects, potion effects |
| `CashShop.edf` | Cash shop items with race bitmask encoding |
| `ItemCombine.edf` | Crafting recipes (3 separate sub-files) |
| `SetItemEffect.edf` | Set item bonus calculations |
| `PcRoom.edf` | PC room reward items |
| `Resource.edf` | 27 resource path sections (bone/mesh/animation) |

### Localization (GU only)

ND files for name/description localization: `nditem`, `ndquest`, `ndcharacter`,
`ndskillforce`, `nditemcash`, `ndstore`.

---

## Output Structure

After conversion, files are organized into two output directories:

```
output/
├── ServerScript/          # Server-side binary files
│   ├── FaceItem.dat       # Individual item types
│   ├── WeaponItem.dat
│   ├── QuestDummyEvent.dat
│   ├── MonsterCharacter.dat
│   └── ...                # One .dat per sheet
│
└── Client/                # Client-side encrypted files
    ├── item.edf           # Merged item data
    ├── character.edf      # Class + monster data
    ├── skillforce.edf     # Skills + forces
    ├── quest.edf          # All quests
    ├── itemcombine.edf    # Crafting recipes
    ├── store.edf          # Store listings
    └── en-gb/             # Localization (GU only)
        ├── nditem.edf
        ├── ndquest.edf
        └── ...
```

---

## Next Steps

- [Quick Start Guide](QUICKSTART.md) — Installation, activation, and your first conversion
- [Editor Guide](EDITORS.md) — Using the visual editors (Drop, Monster, Portal, etc.)
- [Reference](REFERENCE.md) — Keyboard shortcuts, troubleshooting, and technical details
