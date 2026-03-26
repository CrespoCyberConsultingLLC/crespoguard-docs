
# Creating config.bin

> Step-by-step guide to building your encrypted launcher configuration from scratch.

## Why config.bin?

The launcher refuses to load plaintext `modules.json` in release builds. All configuration is delivered as `config.bin` — an AES-256-GCM encrypted binary with tamper detection. This prevents players from reading your PSK, license key, or server internals.

```
modules.json  ──[encrypt]──>  config.bin  ──[launcher loads]──>  runtime config
  (you edit)                  (players get)                      (in memory only)
```

## Requirements

- The **admin build** of `RFLauncher.exe` (provided by CrespoGuard team, or built with `ADMIN_BUILD=ON`)
- A complete `modules.json` in the working directory
- The `System\Launcher\Config\` directory must exist (created automatically)

## Step 1: Create modules.json

Start from the template or create a new file. Minimal working example:

```json
{
  "ServerConfig": {
    "ServerName": "My RF Server",
    "LoginServerIp": "192.168.1.100",
    "LoginServerPort": 10001,
    "ZoneServerIp": "192.168.1.100",
    "ZoneServerPort": 27780,
    "ServerVersion": "2.2.3.2",
    "IsSirin": false,
    "Key": "0000000000000000-20270101-00000000000000000000000000000000"
  },
  "Branding": {
    "WindowTitle": "My RF Server",
    "StatusBarText": "My RF Server v1.0",
    "FooterText": "Powered by My RF Server"
  },
  "ThemeConfig": {
    "AccentColor": [0, 198, 255, 255]
  },
  "FeatureFlags": {
    "EnableAutoUpdate": false,
    "EnableCustomBackground": true,
    "EnableSavingCredentials": true
  },
  "ExternalLinks": {
    "DiscordUrl": "https://discord.gg/yourserver"
  },
  "AuthLimits": {
    "MinUsernameLength": 3,
    "MaxUsernameLength": 12,
    "MinPasswordLength": 3,
    "MaxPasswordLength": 12
  },
  "Localization": {
    "NationCode": "en_gb"
  },
  "SecurityCheck": {
    "EnableHelperCheck": true,
    "EnableVMCheck": true,
    "EnableOtherChecks": true,
    "EnableProxyDllCheck": true,
    "EnableHWIDCheck": false
  }
}
```

See [CONFIG_REFERENCE.md](CONFIG_REFERENCE.md) for every field and its default value.

## Step 2: Add Secure Login (Optional — All Tiers)

If using the CrespoGuard Relay with the encrypted tunnel (available in all tiers including Community), add the SecureLogin section to enable the AES-256-GCM encrypted tunnel between the launcher and relay. Servers not using the encrypted tunnel skip this step — the launcher connects directly to your LoginServer or through the transparent proxy.

```json
{
  "SecureLogin": {
    "EnableSecureLogin": true,
    "SecureLoginType": 1,
    "SecureLoginHost": "YOUR.PUBLIC.IP",
    "SecureLoginIP": "YOUR.PUBLIC.IP",
    "SecureLoginPort": 10001,
    "SecureLoginPSK": "YOUR_64_CHAR_HEX_PSK"
  }
}
```

Generate the PSK:
```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

The same PSK must appear in both `modules.json` and the relay's `server.json`.

## Step 3: Add License Key

If you have a CrespoGuard license:

```json
{
  "ServerConfig": {
    "Key": "0000000000000000-20270101-c38505adc65114f57ff7e772ba8e9cac"
  }
}
```

For Community tier without a license, use a placeholder master key:
```json
{
  "ServerConfig": {
    "Key": "0000000000000000-20270101-00000000000000000000000000000000"
  }
}
```

## Step 4: Add Auto-Update Server (Optional)

If you host a patch server:

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

## Step 5: Encrypt

Place `modules.json` in your **client root directory** (the folder with `RF_Online.bin`), then run:

```powershell
# PowerShell (Windows)
& "C:\path\to\admin\RFLauncher.exe" --encrypt-config
```

```bash
# Git Bash / MSYS2
"/c/path/to/admin/RFLauncher.exe" --encrypt-config
```

**What happens:**
1. Reads `modules.json` from the current working directory
2. Parses and validates JSON (fails on syntax errors)
3. Encrypts with AES-256-GCM (CGCB v2 format)
4. Writes to `System\Launcher\Config\config.bin`
5. Creates the directory structure if it doesn't exist

**Expected output:** No output on success. Check that `System\Launcher\Config\config.bin` exists.

## Step 6: Verify

Test that the launcher loads your config:

1. Ensure `config.bin` is at `System\Launcher\Config\config.bin`
2. Run the release `RFLauncher.exe` from the client directory
3. Verify:
   - Window title shows your server name
   - Sidebar shows your branding
   - Login connects to the correct IP

## Updating config.bin

Any time you change `modules.json`, you must re-encrypt:

1. Edit `modules.json`
2. Run `--encrypt-config` again
3. The old `config.bin` is overwritten
4. Distribute the new `config.bin` to players (via patch server or manual update)

## Common Mistakes

| Problem | Cause | Fix |
|---------|-------|-----|
| `"Config not found"` at launch | config.bin not in `System\Launcher\Config\` | Check path, re-run `--encrypt-config` from client root |
| Encrypt command does nothing | Not running from the right directory | `cd` to the folder containing `modules.json` first |
| `"JSON parse error"` | Syntax error in modules.json | Validate JSON (trailing commas, missing quotes, etc.) |
| Launcher ignores changes | Forgot to re-encrypt | Re-run `--encrypt-config` after every modules.json edit |
| PSK mismatch (relay rejects) | Different PSK in modules.json vs server.json | Copy the exact same key to both files |
| `"modules.json not found"` | Wrong working directory | Run the command from the folder where modules.json lives |
| Old launcher loads old config | Auto-updater hasn't pushed new config.bin | Upload new config.bin to patch server and update filelist.txt |

## Config Encryption Details

`config.bin` uses military-grade encryption with built-in tamper detection. If the file is modified or corrupted, the launcher rejects it immediately. The encryption key is compiled into the binary and cannot be extracted through standard means.

The PSK inside the config provides per-server authentication — even if two servers use the same launcher binary, they cannot connect to each other's relay.

## File Locations Summary

```
Client Root/
├── modules.json                        ← YOU EDIT THIS (delete after encrypting)
├── RF_Online.bin
├── RFLauncher.exe                      ← release build (loads config.bin only)
└── System/
    └── Launcher/
        └── Config/
            └── config.bin              ← GENERATED (distribute to players)
```

!!! warning "Delete modules.json after encrypting"
    Delete `modules.json` from the client directory after encrypting. Players should never have access to the plaintext config. Only keep it in your admin/build environment.
