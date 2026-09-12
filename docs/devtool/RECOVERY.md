# Backups & Recovery

## Before a conversion or edit

1. Copy the complete source folder to a dated backup outside the working folder.
2. Work on a separate copy. Keep related map and client files together.
3. Point Parser output at separate test folders, not a live server or client.
4. Close Excel or other editors that may hold the files you will replace.

Backup behavior differs by tool. Do not assume every operation creates a
`backups/` directory or that an existing `.bak` represents your latest changes.

## Know what Save changes

| Tool | File behavior |
| --- | --- |
| Parser | Writes conversion output to configured destinations. Existing files can be replaced. |
| Drop Editor | **Save** writes changes back to the loaded `ItemLooting.xlsx`. Convert the saved workbook separately when you need binary output. |
| Monster Editor | Edit actions can write the loaded map data when applied. Work on a copied map folder. |
| Safezone Editor | Create/edit/delete actions save the loaded SPT data; there is no separate final Save step to rely on. |
| Portal Editor | Operations can affect related server and client files. Check the selected map root and loaded client files before saving or syncing. |
| Item Explorer | Reads sources only. Conflict export writes a separate report, not fixes to the source. |

## When a write fails

Read the error and the affected path. Close programs locking the file, confirm
the folder is writable, then retry from a fresh load. Do not repeatedly apply an
edit to a stale view if the file changed on disk. If verification failed or
editing was disabled, preserve the error and reload before making more changes.

If a batch fails partway through, some outputs may already exist. Review the
log for each file; do not deploy the whole folder as a successful conversion.

## Restore a working copy

Close the tool and any editors using the project. Preserve the failed working
folder separately for diagnosis, then restore the complete known-good source
set into a new working folder. Reopen that folder and verify the data before
converting again. Restore related server/client files from the same backup;
do not mix revisions to hide a mismatch.

## Ask for support

Include the app version, GU/BSB profile, operation, affected filename, exact
error, and steps to reproduce. A cropped screenshot of the error is useful.
Remove activation codes, license files, connection strings, and customer data.
Only send a source sample when you have permission to share it.
