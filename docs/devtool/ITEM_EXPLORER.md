# Item Explorer / Where Used

Use Item Explorer to find an item definition and see where supported source
tables reference it. The Explorer reads your saved files; it does not edit them.

## Before you scan

Select **GU** or **BSB** in Parser and choose the input folder containing your
source workbooks. Keep a separate working copy of your project. The scan only
covers supported sources below the selected folder, not neighboring folders or
your running game server. AoP is not supported by Item Explorer.

## Find an item and its references

1. In **Parser**, select **Item Explorer / Where Used** below the input-folder controls.
2. Wait for the scan to complete. Review **Coverage / warnings** for unreadable or unchecked sources.
3. Search for an item code or an available name. Use category and definition-status filters to narrow the results.
4. Select the item. **Definitions** shows its source representations; **Where Used** shows references found in supported tables.
5. Select a definition or reference and copy its source location, or open its source file.
6. Use the copied workbook/sheet/cell or TXT line/column to locate the entry. Opening a file does not automatically jump to that location.

For example, to investigate an item in a drop table, search its code, select
the result, and inspect its drop references. Copy the relevant source location
before opening the workbook. If no references appear, check coverage before
concluding anything about the item.

## Understand the result

| Result | Meaning and next step |
| --- | --- |
| Resolved | Checked definitions identify one canonical target. Inspect its source before editing. |
| Ambiguous | Definitions conflict or multiple representations compete. Review every source; the Explorer does not choose a winner. |
| Not checked / incomplete | Some necessary sources could not be checked. Read coverage warnings and correct the source problem before rescanning. |
| Missing within checked scope | The code is missing from a complete checked definition inventory in this folder. Investigate the reference and the selected folder. |
| Empty Where Used | No references were found in the checked domains. This does not mean the item is unused everywhere or safe to delete. |

Server and client coverage are reported separately. A checked server definition
does not prove that its client representation is valid or that the game accepts it.

## Save edits, then refresh

The Explorer is a snapshot of saved files. Save changes in the source editor,
then refresh the Explorer. Refresh scans again and resets the filters. Unsaved
Excel or editor changes are not included. Canceling a scan invalidates its results.

## Export conflicts

After a completed scan, choose **Export conflicts** and save the JSON report
outside your input files. It includes all conflicts in the completed snapshot,
including those hidden by the current filters. Exporting does not resolve
conflicts or change your sources.

## Supported scope

- GU and BSB item definitions and supported recipe and drop references.
- GU shop stock references from supported `StoreList` layouts; BSB shops are not covered.
- Recognized GU literal client TXT identities and supported direct cell references within saved GU client workbooks.
- Direct cell references are not a general Excel calculation engine. Functions, arithmetic, external workbook links, and unsupported formulas can leave a sheet unchecked.
- Missing names do not necessarily mean missing items: names depend on the available supported source fields.

Item Explorer does not validate every binary layout, automatically repair data,
or establish in-game compatibility.

## If something looks wrong

- **No results:** check the selected folder/profile, then clear filters and refresh.
- **Changes are missing:** save the source file first, then refresh.
- **Unreadable workbook:** close any editor holding the file, check the warning, and retry after correcting the issue.
- **Clipboard failure:** use the reported source location and retry copying; do not assume the clipboard contains the new location.
- **Conflicting definitions:** compare the listed sources and export the report for review. Do not delete a definition solely to remove the warning.

See [Quick Start](QUICKSTART.md) for conversion and [Visual Editors](EDITORS.md)
for editing tools.
