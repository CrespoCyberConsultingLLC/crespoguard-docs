# Deployment & Distribution

> How to package, distribute, and update the launcher for your players.

## Packaging for Players

### Required Files

These files must be present in the player's RF Online client directory:

```text
RF Online Client/
|-- RFLauncher.exe                          # Launcher binary
|-- RFLauncher.sig                          # Integrity signature
|-- dinput8.dll                             # CrespoGuard Client DLL required by current launcher builds
|-- CrespoGuard.ico                         # Application icon
|-- RF_Online.bin                           # Game executable (already present)
|-- d3d9.dll                                # Server/protection DLL if your RF client already requires it; do not replace blindly
|-- System/
    |-- Launcher/
        |-- Config/
        |   |-- config.bin                  # Encrypted configuration
        |-- fonts/
        |   |-- Rajdhani-Regular.ttf
        |   |-- Rajdhani-Bold.ttf
        |   |-- Orbitron-Variable.ttf
        |-- Language/
        |   |-- en.json
        |   |-- en_gb.json
        |   |-- ru_ru.json                  # English fallback/template unless translated
        |-- Music/
        |   |-- (optional .mp3)
        |-- logo.png
        |-- background.png
```

### Files NOT to Distribute

These are generated locally per-player and should NOT be included in your distribution:

| File                              | Reason                                             |
| --------------------------------- | -------------------------------------------------- |
| `modules.json`                    | Plaintext config — client builds refuse to load it |
| `System\Launcher\accounts.json`   | Player's saved credentials (DPAPI encrypted)       |
| `RFLauncher.log`                  | Debug log                                          |
| `System\Launcher\cg_settings.ini` | Per-session launcher-to-game settings              |
| `System\DefaultSet.tmp`           | Per-session encrypted game token                   |
| `__cg_session\`                   | Clean room temporary files                         |
| `RFLauncher.exe.new`              | Staged self-update executable                      |
| `System\Launcher\pending_updates.txt` | Signed local metadata for staged replacements  |
| `*.pcap`                          | Optional diagnostic packet capture                 |

### Diagnostic Logging

Normal player builds redact login packet payloads and credential encoding parameters from `RFLauncher.log`. They do not create decrypted PCAP files.

Operators can build a diagnostic launcher with `-DCG_ENABLE_PACKET_CAPTURE=ON`. That build writes decrypted CGRD frames to PCAP and can expose credentials. Use it in a controlled test client, then remove the capture files and replace the diagnostic launcher before distribution.

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

### Method 3: Auto-Update (Paid Tiers / If Enabled)

Use the built-in auto-updater only when your package/tier enables patch-server updates. Community packages should default to manual patch distribution unless CrespoGuard explicitly enables auto-update for that customer.

1. Host a patch server (any HTTPS file server)
2. Generate `filelist.txt` manifest using the admin tool
3. Configure `UpdateServers` in `modules.json`
4. Re-encrypt `config.bin`
5. Players get updates automatically on launcher start

See [Auto-Update Setup](#auto-update-setup) below.

## Auto-Update Setup

### Prerequisites

- A CrespoGuard package/tier with auto-update enabled
- An HTTPS web server (nginx, Apache, S3, Cloudflare R2, etc.)
- The admin build of RFLauncher.exe (for manifest generation)

### Step 1: Create Patch Directory

Create a directory on your web server with all files players need:

```text
patches.yourserver.com/
|-- filelist.txt            # Generated manifest
|-- RFLauncher.exe
|-- RFLauncher.sig
|-- dinput8.dll
|-- System/
    |-- Launcher/
        |-- (all assets)
```

### Step 2: Generate Manifest

From the patch directory:

```powershell
& "path\to\admin\RFLauncher.exe" --generate-filelist
```

This creates `filelist.txt` with SHA-256 hashes and file sizes. The format is `path|SHA256|file_size_bytes`:

```text
# CrespoGuard Patch Manifest v2
# Format: path|SHA256|size
RFLauncher.exe|9f64a747e1b97f131fabb6b447296c9b6f0201e79fb3c5356e6c77e89b6a806a|985600
dinput8.dll|55a3f6d8b9f91d38fdbe60ad4d7b8f61b7f7321f0db58d0d472a6f84c47d1c8f|34816
System/Launcher/Config/config.bin|43c80f2c0a7cc4ec5d29c4b7899e0b39f7e0ab61dd90937d4f4f0f037f1f5a9f|2048
# HMAC: 64_hex_characters
```

Player builds require a valid `UpdateSigningKey` and a matching HMAC signature. The admin tool reads the key from `modules.json` when it generates the manifest. An unsigned manifest can be created by admin tooling, but a player build rejects it.

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
    "UpdateSigningKey": "REPLACE_WITH_64_HEX_CHARACTERS"
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

```text
1. Launcher loads generation `0x02` config.bin
2. Fetches filelist.txt over HTTPS
3. Verifies the manifest HMAC
4. Compares SHA-256 hashes and file sizes
5. Downloads and verifies changed files
6. Continues to the login screen
```

### Launcher Self-Update

The updater downloads `RFLauncher.exe` as `RFLauncher.exe.new` and verifies its SHA-256 hash against the signed manifest. It also records the expected hash and size in `System\Launcher\pending_updates.txt` and signs that file with `UpdateSigningKey`.

On the next start, the launcher verifies the pending-list HMAC, file size, and SHA-256 hash while holding the staged executable open without write sharing. It renames that verified file handle into place, preventing a path swap between verification and replacement. Invalid staged files are discarded by handle.

## Server-Side Deployment

### CrespoGuard Relay (Optional)

The relay is deployed from the separate relay package, not from the neutral launcher base operator kit. Deploy it only if you use `SecureLogin`, zone proxying, rate limiting, or origin IP protection.

```text
Server/
|-- CrespoGuardRelay.exe
|-- server.json
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
  "MaxClients": 50,
  "DashboardEnabled": false,
  "DashboardPort": 8080
}
```

Run: `CrespoGuardRelay.exe server.json`

For full relay setup details, see [CrespoGuard Relay](RELAY.md).

### Firewall Rules

| Port        | Protocol | Direction          | Purpose                                |
| ----------- | -------- | ------------------ | -------------------------------------- |
| 10001/10002 | TCP      | Inbound            | Login server / relay                   |
| 27780       | TCP      | Inbound            | Zone server or zone proxy              |
| 8080/8081   | TCP      | Inbound (optional) | Relay dashboard; restrict to admin IPs |

## Updating Your Deployment

### Config Changes Only

If you change branding, theme, or settings:

1. Edit `modules.json`
2. Re-encrypt: `RFLauncher.exe --encrypt-config`
3. For manual packages, publish a new patch zip containing the updated `config.bin`
4. If auto-update is enabled, upload the new `config.bin` and regenerate `filelist.txt`

### Binary Updates

When CrespoGuard releases a new launcher version:

1. Replace `RFLauncher.exe` in your patch folder
2. Replace `dinput8.dll`
3. Regenerate `config.bin` with the config tool paired with that launcher
4. Confirm the config header is `43 47 43 42 02 02`
5. Re-sign: `RFLauncher.exe --sign-launcher path\to\RFLauncher.exe`
6. Regenerate the HMAC-signed `filelist.txt`
7. Upload the complete launcher/config pair

### Emergency Rollback

Keep previous versions of `RFLauncher.exe` and `config.bin`. To rollback:

1. Replace the files on your patch server with the previous versions
2. Regenerate `filelist.txt`
3. Manual packages should be republished; auto-update packages roll back on next launcher start

Rollback `RFLauncher.exe` and `config.bin` as a tested pair. Current release packages use format `0x02`, key generation `0x02`; player launchers reject format `0x02` configs that use key generation `0x01`.

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
- [ ] Re-encrypt config.bin with the config tool paired to the player launcher
- [ ] Confirm config header `43 47 43 42 02 02`
- [ ] Test launcher connects to server
- [ ] Test game launch succeeds with `dinput8.dll` present
- [ ] Publish manual patch package, or upload to patch server if auto-update is enabled
- [ ] Regenerate the HMAC-signed filelist.txt if auto-update is enabled
- [ ] Verify the selected update path with a clean client
- [ ] Announce update to players (Discord, website, etc.)

## Troubleshooting

| Issue                                              | Cause                          | Fix                                                            |
| -------------------------------------------------- | ------------------------------ | -------------------------------------------------------------- |
| Auto-update stuck at 0%                            | Patch server unreachable       | Check URL, SSL cert, firewall                                  |
| "Config not found" after update                    | config.bin not on patch server | Upload config.bin, update filelist                             |
| "Config not found or invalid" after binary update  | Legacy or mismatched config generation | Regenerate with the paired config tool and confirm `43 47 43 42 02 02` |
| Auto-update reports missing signing key or invalid manifest | Empty, malformed, or mismatched `UpdateSigningKey` | Generate a 64-character hex key, rebuild config.bin, and regenerate filelist.txt |
| Staged launcher update is discarded                | Missing or invalid signed pending metadata | Publish the launcher through the signed manifest flow; do not copy `RFLauncher.exe.new` by hand |
| Players see old version                            | filelist.txt not regenerated   | Re-run `--generate-filelist` and upload                        |
| Game launch blocked with missing anti-cheat module | dinput8.dll not in game root   | Ensure it is in the same folder as RF_Online.bin               |
| "FG cannot be enabled" error                       | d3d9.dll was replaced          | Restore the original protected d3d9.dll from your server files |
| Launcher crashes on start                          | Corrupted config.bin           | Re-encrypt from modules.json                                   |
| Self-integrity check fails                         | RFLauncher.sig outdated        | Re-sign with `--sign-launcher`                                 |
