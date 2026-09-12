# Visual Editors

> Premium visual editors for game data — loot tables, spawns, portals, safezones,
> ore cutting, and 3D map geometry. Save behavior differs by editor;
> start with [Backups & Recovery](RECOVERY.md).

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

### Walkthrough: change one monster's drop entry

1. Copy your source folder, then choose **Load ItemLooting.xlsx** and open the copied workbook.
2. Search for the monster code. Load monster names if you need name-based navigation.
3. Select the intended pool and entry, choose **Edit**, and change one value.
4. Review the displayed item and rate, then click **Save**. Check for a success message.
5. Reload the copied workbook and confirm the change persisted. Use Parser to convert the saved workbook into a separate test output folder.

Saving here changes the workbook, not your deployed server binary. Keep a
record of the original value so you can compare or restore it.

### Drop Rate Format

Drop rates use RF Online's native integer format where `0x7FFF7FFF` (2,147,450,879) = 100%.
The editor shows both the raw value and the calculated percentage.

---

## Monster Editor

Manage monster spawn blocks per map — positions, counts, respawn timers, and spawn rates.

**Open:** Press **Ctrl+Shift+M** or use **Tools > Monster Editor**.

### Walkthrough: inspect and edit one spawn

1. Choose **Browse Map Folder** and select a copied map folder containing its related DAT and SPT files.
2. Select the intended block/spawn. Record the monster code, count, and coordinates before editing.
3. Change one supported field and use **Edit Selected**. This operation can write to the copied map files immediately.
4. Check the status/error message, then **Refresh** and reselect the entry to confirm the value persisted.
5. Test the copied map files in a staging environment before replacing live files.

If loading fails or editing is disabled, correct the reported file problem and
reload. Do not mix spawn DAT and SPT files from different map revisions.

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

### Walkthrough: resize an existing safezone

1. Choose **Browse Map Folder** and open a copy of the complete map folder.
2. Select the intended safezone and record its original coordinates, radius, and height.
3. Change the desired size and choose **Edit Selected**. The action saves the loaded SPT data.
4. Reload the copied folder and confirm the saved size. Check the zone in your staging client/server before deploying it.

Create and delete actions also save changes. Keep an independent folder backup;
closing the tab is not an undo operation. If save verification fails, follow
[Backups & Recovery](RECOVERY.md) before continuing.

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

### Walkthrough: review and save an existing portal

1. Copy the server map folder and the matching client files before editing.
2. Choose **Browse Map/** and load the copied map root. Select the existing portal you want to inspect.
3. Verify its source, destination, and arrival/clickable role. Run **Validate Links** before changing it.
4. Change only the intended field in the portal view, then choose **Save** and check the result.
5. Run **Validate Links** again and reload the saved data to confirm the change.
6. If the operation also needs client data, load the matching copied `Map.dat` and `NDMap.dat` indicated by the workflow. A server-side save alone does not establish client synchronization.

Test both travel direction and arrival position in staging. Validation can find
link problems, but it does not replace checking the portal in your actual game.

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
