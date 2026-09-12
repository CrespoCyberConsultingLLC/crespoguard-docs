# Reference

> Keyboard shortcuts, menu bar, folder structure, and troubleshooting.

---

## Keyboard Shortcuts

| Shortcut | Action | Tier |
|----------|--------|------|
| **F5** | Refresh file list | Free |
| **Ctrl+Shift+S** | Run Server | Free (single-file) |
| **Ctrl+Shift+I** | Run Client | Free (single-file) |
| **Ctrl+Shift+A** | Run Both | Free (single-file) |
| **Escape** | Cancel running conversion | Free |
| **Ctrl+M** | DAT to Excel import | Free |
| **Ctrl+L** | Open Drop Editor | Premium |
| **Ctrl+Shift+M** | Open Monster Editor | Premium |
| **Ctrl+Shift+V** | Open Map Viewer | Free |
| **Ctrl+Shift+B** | Open 3D BSP Viewer | Premium |

---

## Menu Bar

### File

| Item | Action |
|------|--------|
| Refresh (F5) | Rescan input directory |
| Exit | Close the application |

### Tools

| Item | Action | Tier |
|------|--------|------|
| Drop Editor (Ctrl+L) | Open Drop Editor tab | Premium |
| Monster Editor (Ctrl+Shift+M) | Open Monster Editor tab | Premium |
| Map Viewer (Ctrl+Shift+V) | Open Map Viewer tab | Free |
| 3D BSP Viewer (Ctrl+Shift+B) | Open 3D BSP Viewer tab | Premium |

### Help

| Item | Action |
|------|--------|
| Keyboard Shortcuts | Show shortcut reference |
| About | Version, copyright, feature list |

---

## Folder Structure

The recommended layout for your working directory:

```
MyServer/
├── CrespoGuardRFDevTool.exe    # The application
├── crespoguard.ini              # UI preferences (auto-created)
├── settings.json                # License settings; do not share
├── license_cache.json           # License validation cache
│
├── input/                       # Your Excel spreadsheets
│   ├── Grade.xlsx
│   ├── StoreList.xlsx
│   ├── Item.edf/               # Composite: all item sheets
│   │   ├── FaceItem.xlsx
│   │   ├── WeaponItem.xlsx
│   │   └── ...
│   ├── Quest.edf/              # Composite: all quest sheets
│   ├── Character.edf/          # Composite: class + monster
│   └── SkillForce.edf/         # Composite: skills + forces
│
├── output/
│   ├── ServerScript/            # Server .dat files
│   └── Client/                  # Client .edf files
│       └── en-gb/              # Localization (GU only)
│
└── source-backup/               # A copy you create before editing
```

---

## Context Menu

Right-click on a file in the file list for additional options:

| Option | Action |
|--------|--------|
| Open in Excel | Open the selected `.xlsx` in your default editor |
| DAT to Excel | Import a `.dat` file to spreadsheet |
| GameCP DB Sync | Sync item data to SQL Server (Premium) |

---

## Settings Bar

The settings bar below the header controls conversion behavior:

| Setting | Description |
|---------|-------------|
| **Version** | GU or BSB — must match your server version |
| **Dry Run** | Preview conversion scope without writing files; not a full conversion validation |
| **CGEF Encrypt** | Enable encrypted `.edf` output (Premium) |
| **Server Output** | Path for `.dat` output files |
| **Client Output** | Path for `.edf` output files |

---

## Licensing

### Community Edition

No license required. The app opens immediately with core conversion features.

### Premium Activation

1. Launch the app and click **Activate**
2. Copy your **Hardware ID** from the dialog
3. Enter your **Activation Code** (`XXXX-XXXX-XXXX-XXXX`)
4. Click **ACTIVATE**

### Offline Support

After activation, the license is cached locally for up to **7 days**. If the
app cannot reach the validation server within that period, it falls back to
Community mode until connectivity is restored.

### Switching Machines

Licenses are hardware-bound (HWID = SHA-256 of MachineGuid + volume serial + MAC).
Contact CrespoGuard support to transfer a license to a new machine.

---

## Troubleshooting

For failed writes, partial output, and restoration steps, see
[Backups & Recovery](RECOVERY.md). For missing or conflicting item references,
see [Item Explorer](ITEM_EXPLORER.md#if-something-looks-wrong).

| Issue | Solution |
|-------|----------|
| **"Activation required"** | Enter a valid activation code, or use Community Edition for free |
| **Conversion produces 0 files** | Check that `.xlsx` files are in the `input/` folder and file list shows them |
| **"No data rows"** | The spreadsheet has headers but no data rows — check the sheet content |
| **Wrong output format** | Verify the Version dropdown matches your server (GU vs BSB) |
| **EDF files not encrypted** | Enable **CGEF Encrypt** checkbox (Premium only) |
| **GameCP sync fails** | Check SQL Server connection string, ensure pyodbc or pymssql driver is available |
| **3D Viewer shows black screen** | Your GPU may not support OpenGL. Try updating graphics drivers |
| **Update badge won't go away** | Click the badge to download the update, or restart the app |
| **License expired** | Re-activate with a valid code, or continue in Community mode |
| **Batch conversion blocked** | Select exactly one file for Community Edition, or activate Premium |

---

## Next Steps

- [Overview](OVERVIEW.md) — Features, file types, and editions
- [Quick Start Guide](QUICKSTART.md) — Your first conversion
- [Editor Guide](EDITORS.md) — Visual editor reference
