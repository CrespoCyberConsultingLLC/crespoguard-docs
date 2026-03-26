# Visual Editors

> Premium visual editors for game data — loot tables, spawns, portals, safezones,
> ore cutting, and 3D map geometry. All editors save directly to binary format.

!!! info "Premium Feature"
    All editors except the 2D Map Viewer require a Premium license.

---

## Drop Editor

Edit monster loot tables visually instead of working with raw `ItemLooting.xlsx` spreadsheets.

**Open:** Click **Drop Editor** on the Parser tab, press **Ctrl+L**, or use **Tools > Drop Editor**.

### Features

- Select a monster to view and edit its loot table
- Add, remove, and reorder drop entries
- Set drop rates with calculated percentage display
- Multi-select items for pull operations
- Color-coded Excel export of drop configurations
- Filter monsters by name, code, or grade

### Drop Rate Format

Drop rates use RF Online's native integer format where `0x7FFF7FFF` (2,147,450,879) = 100%.
The editor shows both the raw value and the calculated percentage.

---

## Monster Editor

Manage monster spawn blocks per map — positions, counts, respawn timers, and spawn rates.

**Open:** Press **Ctrl+Shift+M** or use **Tools > Monster Editor**.

### Features

- Browse spawn blocks by map
- Create, edit, and delete spawn blocks (up to 200 per map)
- Configure DMM spawn points:
    - Monster code and name
    - Spawn count
    - Regen timer (seconds, minutes, hours, or days)
    - Spawn rate
- Filter by monster name or code
- Load coordinates from `.spt` scene files
- Visual block management with inline editing

---

## Safezone Editor

Create and manage cylindrical safe zones (PvP-disabled areas) on any map.

### Features

- Set zone position (X, Y, Z coordinates)
- Choose from size presets:

| Preset | Radius | Height |
|--------|--------|--------|
| Small | 200 | 200 |
| Medium | 500 | 500 |
| Large | 800 | 800 |
| XL | 1200 | 1200 |
| Tall | 500 | 1000 |
| Wide | 1000 | 400 |

- Custom radius and height for non-standard zones
- Browse maps and edit zones inline
- Export to binary format

---

## Portal Editor

Manage teleportation portals between maps — both server-side connections and client-side
visual data.

### Concepts

Portals have two types:

| Type | Code | Color | Description |
|------|------|-------|-------------|
| **Arrival** | 0 | Yellow | Where players appear after teleporting |
| **Clickable** | 1 | Green | Where players click to teleport |

A complete portal pair needs both: a Clickable on the source map and an Arrival on the
destination map.

### Features

- Load and edit portals across all maps
- Create portal pairs (bidirectional links in one click)
- One-way portal support for special routing
- Cross-map validation to catch broken links
- Client data sync — update `Map.dat` and `NDMap.dat`
- Edit portal display names (NDMap.dat)
- Add and delete dummy portal entries
- Reorder portals within maps
- Race filtering (Bellato, Cora, Accretia, All Races)

### Color Coding

| Color | Meaning |
|-------|---------|
| Green | Clickable portal (goto) |
| Yellow | Arrival portal (from) |
| Cyan | Special portal |
| Red | Broken link (missing destination) |

!!! tip "Full guide"
    For detailed portal editing workflows, see the Portal Editor Guide
    included with the application (`PortalEditorGuide.html`).

---

## Map Viewer (Free)

2D top-down map visualization — available in both Community and Premium editions.

**Open:** Press **Ctrl+Shift+V** or use **Tools > Map Viewer**.

### Features

- Top-down 2D map rendering from `.bsp` files
- Toggle layers:

| Layer | What it shows |
|-------|--------------|
| Grid | Coordinate grid overlay |
| Spawns | Monster spawn positions |
| Safezones | PvP-free areas |
| Portals | Teleport connections |
| Resources | Gatherable resource nodes |
| Zones | Zone boundaries |
| Map Objects | Static map objects |

- Pan (click + drag) and zoom (mouse wheel)
- **Fit All** button to reset the view
- Coordinate display in the status bar

---

## 3D BSP Viewer

OpenGL-based 3D geometry viewer for RF Online `.bsp` map files.

**Open:** Press **Ctrl+Shift+B** or use **Tools > 3D BSP Viewer**.

### Features

- 3D rendering of map geometry
- Render modes:

| Mode | Description |
|------|-------------|
| Solid | Filled polygons with vertex colors |
| Wireframe | Edge-only rendering |
| Solid + Wire | Both combined |

- Toggle options: Vertex Colors, Backface Culling, Lighting
- Camera controls: rotate, pan, zoom
- **Reset Camera** to return to default view
- Load single `.bsp` file or entire folder

!!! note "OpenGL required"
    The 3D viewer requires OpenGL support. If unavailable, the tab shows a
    fallback message — all other features continue to work normally.

---

## Ore Cutting Editor

Edit ore transmutation (cutting) drop chance tables — what players get when
processing raw ores.

### Features

- Edit drop chances per ore type
- Chance normalization (auto-adjust percentages to sum to 100%)
- Undo support for reverting changes
- Process customization per ore grade

---

## GameCP DB Sync (Premium)

Synchronize item metadata directly to your Game Control Panel's SQL Server database.

### Features

- One-click sync from Item.edf folder
- Scans numbered item subfolders for metadata
- Syncs: Code, Name, Icon ID, Level, Attack, Defense, DSR
- Auto-generates table names from item types
- Supports both `pyodbc` and `pymssql` drivers
- Connection testing before sync

### Synced Fields

| Column | Source |
|--------|--------|
| Code | Item code from binary data |
| Name | Item name from spreadsheet |
| IconID | Icon index |
| Level | Required level |
| Attack | Attack value |
| Defense | Defense value |
| DSR | Drop/Sale/Repair flags |

---

## Next Steps

- [Quick Start Guide](QUICKSTART.md) — First conversion walkthrough
- [Reference](REFERENCE.md) — Keyboard shortcuts, troubleshooting
