# Quick Start Guide

> From download to your first converted files in under 5 minutes.

---

## Step 1: Download

Download `CrespoGuardRFDevTool.exe` from the CrespoGuard distribution package.
Place it in any folder — no installation required.

```
MyServer/
├── CrespoGuardRFDevTool.exe
├── input/                    # Your Excel spreadsheets go here
└── output/                   # Converted files appear here
```

!!! tip "Portable"
    The tool creates `settings.json` and `license_cache.json` next to the
    executable. Keep everything in one folder for easy backup.

---

## Step 2: Prepare Your Files

Create an `input/` folder next to the executable and place your `.xlsx` files in it.

For **EDF composite files** (Item.edf, Quest.edf, etc.), organize sheets into subfolders:

```
input/
├── Grade.xlsx              # Simple single-sheet file
├── StoreList.xlsx
├── Item.edf/               # All item sheets in one folder
│   ├── FaceItem.xlsx
│   ├── WeaponItem.xlsx
│   ├── PotionItem.xlsx
│   └── ...
├── Quest.edf/              # All quest sheets
│   ├── QuestDummyEvent.xlsx
│   ├── QuestItem.xlsx
│   └── ...
└── Character.edf/          # Class + monster data
    ├── Class.xlsx
    └── MonsterCharacter.xlsx
```

!!! note "Folder naming matters"
    Folders named `*.edf` are treated as composite file inputs. The tool merges
    all sheets inside into a single `.edf` output file.

---

## Step 3: Activate (Premium Only)

!!! info "Community Edition"
    If you don't have a license key, skip this step. The app opens in
    Community Edition mode — single-file conversion and the Map Viewer work
    without activation.

On first launch, click **Activate Premium** in the header bar:

1. Copy your **Hardware ID** (displayed in the dialog)
2. Enter your **Activation Code** (format: `XXXX-XXXX-XXXX-XXXX`)
3. Click **ACTIVATE**

The license is bound to your machine and cached locally. You can work offline
for up to 7 days before re-validation is needed.

---

## Step 4: Select Version

Choose your RF Online server version from the dropdown:

| Version | Description |
|---------|-------------|
| **GU** | Global Uprising — includes localization (nd files) and additional quest formats |
| **BSB** | Standard 2.2.3 — classic server format |

---

## Step 5: Convert

### Single File (Community + Premium)

1. Select one file in the file list
2. Click one of the conversion buttons:

| Button | Output |
|--------|--------|
| **Convert SERVER** | `.dat` files in `output/ServerScript/` |
| **Convert CLIENT** | `.edf` files in `output/Client/` |
| **Convert ALL** | Both server and client output |

### Batch Conversion (Premium Only)

1. Select multiple files, or select nothing to convert everything
2. Click any conversion button
3. All visible files are processed in sequence

!!! warning "Dry Run"
    Enable **Dry Run (Preview)** to test conversion without writing output files.
    Useful for catching Excel format errors before committing to a full conversion.

---

## Step 6: Check Output

After conversion:

- Click **Server Output** to open the `output/ServerScript/` folder
- Click **Client Output** to open the `output/Client/` folder
- Check the log panel at the bottom for any warnings or errors

---

## Reverse Import (DAT to Excel)

To edit existing server files:

1. Press **Ctrl+M** or click **DAT to Excel**
2. Select a `.dat` file from your server
3. The tool creates an `.xlsx` spreadsheet with all the data
4. Edit in Excel, then convert back

!!! tip "Round-trip verified"
    The import/export pipeline is byte-perfect — converting a `.dat` to `.xlsx`
    and back produces an identical binary file.

---

## CGEF Encryption (Premium Only)

To generate encrypted client files:

1. Check the **CGEF Encrypt (Client)** checkbox in the settings bar
2. Run a CLIENT or ALL conversion
3. Output `.edf` files are encrypted with CrespoGuard's format

Encrypted files require the CrespoGuard client to decrypt at runtime.

---

## Auto-Updates

The tool checks for updates automatically on startup (after 4 seconds).
When a new version is available, a yellow **Update Available** badge appears
in the status bar. Click it to download and install.

---

## Next Steps

- [Editor Guide](EDITORS.md) — Drop tables, monsters, portals, safezones
- [Reference](REFERENCE.md) — Keyboard shortcuts, file types, troubleshooting
