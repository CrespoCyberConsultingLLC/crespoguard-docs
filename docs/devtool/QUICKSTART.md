# Quick Start Guide

> Set up a working copy, convert one supported file, and check the result.

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
    UI preferences are stored in `crespoguard.ini` beside the executable.
    License storage also uses `settings.json` and `license_cache.json`.
    Do not share these files with screenshots or support reports.

Work on a copy of your source data. Keep server and client output in separate
test folders, away from the running server/client and away from your inputs.
See [Backups & Recovery](RECOVERY.md) before editing or replacing files.

---

## Step 2: Prepare Your Files

Create an `input/` folder next to the executable and place your `.xlsx` files in it.

In **Parser**, use the input-folder **Browse** button to select that folder.
Click **Refresh** and confirm your files appear. Preserve the workbook sheet
names, metadata rows, and folder layout required by your existing project.
Do not build a production workbook by guessing column names from this example.

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

On first launch, click **Activate** in the header bar:

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

Choose the profile matching your source package, not simply the label used by
your server's branding. Custom layouts need separate verification. AoP remains
experimental and is not a substitute for GU or BSB support.

---

## Step 5: Convert

### Single File (Community + Premium)

1. Set the server and client output paths using their **Browse** buttons. For your first test, use empty folders in your working copy.
2. Select exactly one supported file in the file list.
3. Click one of the conversion buttons:

| Button | Output |
|--------|--------|
| **Run Server** | Server output in the configured server folder |
| **Run Client** | Client output in the configured client folder; composite outputs require their supported source set |
| **Run Both** | Both server and client output |

### Batch Conversion (Premium Only)

1. Select the intended files. With no selection, the tool targets visible files; filters affect that scope.
2. Click any conversion button
3. Read any scope confirmation before starting. A selected file or sheet and an unselected filtered list are different scopes.

!!! warning "Dry Run"
    Enable **Dry Run** to preview the planned conversion without writing output.
    A preview is not proof that a full conversion or an in-game test will pass.

---

## Step 6: Check Output

After conversion:

- Click **Open Server Output** to open the configured server output folder
- Click **Open Client Output** to open the configured client output folder
- Check the log panel at the bottom for any warnings or errors

For a first example, use a known supported server workbook from your package,
select only that file, and choose **Run Server**. Confirm the log reports a
successful conversion and that the expected output was created in your empty
test folder. A pre-existing file in an output folder is not evidence of success.
Test the result in your staging server/client before deploying it.

---

## Reverse Import (DAT to Excel)

To edit existing server files:

1. Press **Ctrl+M** or click **DAT to Excel**
2. Select a `.dat` file from your server
3. The tool creates an `.xlsx` spreadsheet with all the data
4. Edit in Excel, then convert back

!!! warning "Verify your layout"
    Import support and round-trip preservation depend on the format and profile.
    Keep the original binary and first test an unchanged import/export cycle in
    separate folders. Do not assume every custom DAT layout is byte-identical.

---

## CGEF Encryption (Premium Only)

To generate encrypted client files:

1. Check the **CGEF Encrypt** checkbox in the settings bar
2. Click **Run Client** or **Run Both**
3. Output `.edf` files are encrypted with CrespoGuard's format

Encrypted files require the CrespoGuard client to decrypt at runtime.

---

## Auto-Updates

The tool checks for updates in the background on startup. Open
**Help > Updates** to review the available update and follow its prompts.
Keep backups of your working files before updating.

---

## Next Steps

- [Editor Guide](EDITORS.md) — Drop tables, monsters, portals, safezones
- [Item Explorer](ITEM_EXPLORER.md) — Find definitions and supported references
- [Backups & Recovery](RECOVERY.md) — Protect inputs and recover from failed writes
- [Reference](REFERENCE.md) — Keyboard shortcuts, file types, troubleshooting
