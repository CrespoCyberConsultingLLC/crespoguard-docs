
# Deployment & Distribution

> How to package, distribute, and update the launcher for your players.

## Packaging for Players

### Required Files

These files must be present in the player's RF Online client directory:

```
RF Online Client/
├── RFLauncher.exe                          # Launcher binary
├── RFLauncher.sig                          # Integrity signature (optional for community)
├── CrespoGuard.ico                         # Application icon
├── RF_Online.bin                           # Game executable (already present)
├── d3d9.dll                                # Protected server DLL (already present, DO NOT REPLACE)
├── dinput8.dll                             # CrespoGuard Client DLL (optional, for in-game features)
└── System/
    └── Launcher/
        ├── Config/
        │   └── config.bin                  # Encrypted configuration
        ├── fonts/
        │   ├── Rajdhani-Regular.ttf
        │   ├── Rajdhani-Bold.ttf
        │   └── Orbitron-Variable.ttf
        ├── Language/
        │   └── en_gb.json
        ├── Music/
        │   └── (optional .mp3)
        ├── logo.png
        └── background.png
```

### Files NOT to Distribute

These are generated locally per-player and should NOT be included in your distribution:

| File | Reason |
|------|--------|
| `modules.json` | Plaintext config — client builds refuse to load it |
| `System\Launcher\accounts.json` | Player's saved credentials (DPAPI encrypted) |
| `RFLauncher.log` | Debug log |
| `System\Launcher\cg_settings.ini` | Per-session launcher-to-game settings |
| `System\DefaultSet.tmp` | Per-session encrypted game token |
| `__cg_session\` | Clean room temporary files |

### Packaging Steps

1. Start with a clean RF Online 2.2.3.2 client directory
2. Copy all CrespoGuard files (see Required Files above)
3. Verify `config.bin` is present and was encrypted from your final `modules.json`
4. Test the launcher connects to your server
5. Create a ZIP/RAR archive or use your preferred distribution method

## Distribution Methods

### Method 1: Full Client Package

Bundle the entire RF Online client + CrespoGuard files as a single download.

**Pros**: Players get everything in one download, no manual file copying
**Cons**: Large download size (~2-4GB), must be updated for any game file change

### Method 2: Patch Package (Recommended)

Distribute CrespoGuard files separately as a "patch" that players apply to their existing client.

**Pros**: Small download (~5-10MB), easy to update
**Cons**: Players need an existing 2.2.3.2 client first

Structure your patch as:
```
CrespoGuard-Patch/
├── README.txt              # "Copy all files to your RF Online folder"
├── RFLauncher.exe
├── RFLauncher.sig
├── CrespoGuard.ico
├── dinput8.dll
└── System/
    └── Launcher/
        └── (all launcher assets)
```

### Method 3: Auto-Update (Recommended for Ongoing)

Use the built-in auto-updater to keep players current:

1. Host a patch server (any HTTPS file server)
2. Generate `filelist.txt` manifest using the admin tool
3. Configure `UpdateServers` in modules.json
4. Players get updates automatically on launcher start

See [Auto-Update Setup](#auto-update-setup) below.

## Auto-Update Setup

### Prerequisites

- An HTTPS web server (nginx, Apache, S3, Cloudflare R2, etc.)
- The admin build of RFLauncher.exe (for manifest generation)

### Step 1: Create Patch Directory

Create a directory on your web server with all files players need:

```
patches.yourserver.com/
├── filelist.txt            # Generated manifest
├── RFLauncher.exe
├── RFLauncher.sig
├── dinput8.dll
└── System/
    └── Launcher/
        └── (all assets)
```

### Step 2: Generate Manifest

From the patch directory:

```powershell
& "path\to\admin\RFLauncher.exe" --generate-filelist
```

This creates `filelist.txt` with CRC32 checksums for every file:

```
RFLauncher.exe|a1b2c3d4|985600
dinput8.dll|e5f6a7b8|34816
System\Launcher\Config\config.bin|11223344|2048
```

If you have an `UpdateSigningKey` configured, the manifest includes an HMAC signature that the launcher verifies before applying updates.

### Step 3: Configure modules.json

```json
{
  "UpdateServers": [
    {
      "UpdateServerName": "Primary",
      "Link": "https://patches.yourserver.com/"
    }
  ],
  "FeatureFlags": {
    "EnableAutoUpdate": true,
    "UpdateSigningKey": "your_signing_key_here"
  }
}
```

### Step 4: Re-encrypt and Upload

After changing modules.json:

1. Re-encrypt: `RFLauncher.exe --encrypt-config`
2. Upload the new `config.bin` to your patch server
3. Regenerate `filelist.txt`
4. Upload everything

### Update Flow

When a player launches:

```
1. Launcher loads config.bin
2. Fetches filelist.txt from UpdateServer
3. Compares CRC32 of local files vs manifest
4. Downloads changed files
5. Shows progress bar in sidebar
6. Continues to login screen
```

## Server-Side Deployment

### CrespoGuard Relay (Guard+ Tier)

The relay is a paid server-side component starting at Guard tier ($19/mo). Deploy on your server machine:

```
Server/
├── CrespoGuardRelay.exe
└── server.json
```

**Configuration:**
```json
{
  "ServerName": "Your Server",
  "ListenIP": "0.0.0.0",
  "ListenPort": 10001,
  "TargetIP": "127.0.0.1",
  "TargetPort": 10001,
  "PSK": "same_key_as_modules_json",
  "MaxClients": 75,
  "PublicIP": "YOUR_PUBLIC_IP",
  "MaskServerIP": true,
  "DashboardEnabled": true,
  "DashboardPort": 8081
}
```

Run: `CrespoGuardRelay.exe server.json`

For full relay setup details, see [CrespoGuard Relay](RELAY.md).

### Firewall Rules

| Port | Protocol | Direction | Purpose |
|------|----------|-----------|---------|
| 10001/10002 | TCP | Inbound | Login server / relay |
| 27780 | TCP | Inbound | Zone server |
| 8081 | TCP | Inbound (optional) | Relay dashboard (restrict to admin IPs!) |

## Updating Your Deployment

### Config Changes Only

If you change branding, theme, or settings:

1. Edit `modules.json`
2. Re-encrypt: `RFLauncher.exe --encrypt-config`
3. Upload new `config.bin` to your patch server
4. Update `filelist.txt`
5. Players get the new config on next launcher start

### Binary Updates

When CrespoGuard releases a new launcher version:

1. Replace `RFLauncher.exe` in your patch folder
2. Replace `dinput8.dll` if updated
3. Re-sign: `RFLauncher.exe --sign-launcher path\to\RFLauncher.exe`
4. Regenerate `filelist.txt`
5. Upload everything

### Emergency Rollback

Keep previous versions of `RFLauncher.exe` and `config.bin`. To rollback:

1. Replace the files on your patch server with the previous versions
2. Regenerate `filelist.txt`
3. Players auto-update back to the previous version

## Version Management

### Recommended Versioning

Use `StatusBarText` in the Branding config to track versions:

```json
{
  "Branding": {
    "StatusBarText": "My Server Launcher v1.2.0"
  }
}
```

Update this with each release so players can verify they're running the latest version.

### Release Checklist

- [ ] Update `StatusBarText` version in modules.json
- [ ] Re-encrypt config.bin
- [ ] Test launcher connects to server
- [ ] Test in-game features work (if dinput8.dll updated)
- [ ] Upload to patch server
- [ ] Regenerate filelist.txt
- [ ] Verify auto-update works (test with a clean client)
- [ ] Announce update to players (Discord, website, etc.)

## Troubleshooting

| Issue | Cause | Fix |
|-------|-------|-----|
| Auto-update stuck at 0% | Patch server unreachable | Check URL, SSL cert, firewall |
| "Config not found" after update | config.bin not on patch server | Upload config.bin, update filelist |
| Players see old version | filelist.txt not regenerated | Re-run `--generate-filelist` and upload |
| DLL not loading in game | dinput8.dll not in game root | Ensure it's in the same folder as RF_Online.bin |
| "FG cannot be enabled" error | d3d9.dll was replaced | Restore the original protected d3d9.dll from your server files |
| Launcher crashes on start | Corrupted config.bin | Re-encrypt from modules.json |
| Self-integrity check fails | RFLauncher.sig outdated | Re-sign with `--sign-launcher` |
