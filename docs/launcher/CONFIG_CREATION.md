# Creating config.bin

> Step-by-step guide to building your encrypted launcher configuration from scratch.

## Why config.bin?

The launcher refuses to load plaintext `modules.json` in release builds. All configuration is delivered as `config.bin` — an AES-256-GCM encrypted binary with tamper detection. This prevents players from reading your PSK, license key, or server internals.

```
modules.json  ──[encrypt]──>  config.bin  ──[launcher loads]──>  runtime config
  (you edit)                  (players get)                      (in memory only)
```

## Requirements

- The **current admin build** of `RFLauncher.exe` or `CrespoGuardConfigTool.exe` paired with the player launcher
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
    "UpdateSigningKey": "REPLACE_WITH_64_HEX_CHARACTERS"
  }
}
```

Generate the signing key once and keep it in the operator workspace:

```powershell
python -c "import secrets; print(secrets.token_hex(32))"
```

Player builds require this key when auto-update runs. The admin tool uses the same value to add the HMAC signature to `filelist.txt`.

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

The current tool writes CGCB format version `0x02` with key generation `0x02`. Current release packages must use that pair; player builds reject format `0x02` configs with key generation `0x01`.

## Step 6: Verify

Test that the launcher loads your config:

1. Ensure `config.bin` is at `System\Launcher\Config\config.bin`
2. Inspect the first six bytes in PowerShell:

   ```powershell
   $bytes = [IO.File]::ReadAllBytes("System\Launcher\Config\config.bin")
   " " -join ($bytes[0..5] | ForEach-Object { $_.ToString("X2") })
   ```

   The result must be `43 47 43 42 02 02`: `CGCB`, format `0x02`, key generation `0x02`.
3. Run the release `RFLauncher.exe` from the client directory.
4. Verify:
   - Window title shows your server name
   - Sidebar shows your branding
   - Login connects to the correct IP

## Updating config.bin

Any time you change `modules.json`, you must re-encrypt:

1. Edit `modules.json`
2. Run `--encrypt-config` again
3. The old `config.bin` is overwritten
4. Confirm the header ends in `02 02`
5. Distribute the new `config.bin` through a manual package or the signed patch server

Regenerate `config.bin` whenever you pair the package with a new player launcher. Do not reuse an older generation `0x01` config with current player builds.

## Common Mistakes

| Problem                        | Cause                                        | Fix                                                                                                            |
| ------------------------------ | -------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `"Config not found"` at launch | config.bin not in `System\Launcher\Config\`  | Check path, re-run `--encrypt-config` from client root                                                         |
| Encrypt command does nothing   | Not running from the right directory         | `cd` to the folder containing `modules.json` first                                                             |
| `"JSON parse error"`           | Syntax error in modules.json                 | Validate JSON (trailing commas, missing quotes, etc.)                                                          |
| Launcher ignores changes       | Forgot to re-encrypt                         | Re-run `--encrypt-config` after every modules.json edit                                                        |
| PSK mismatch (relay rejects)   | Different PSK in modules.json vs server.json | Copy the exact same key to both files                                                                          |
| `"modules.json not found"`     | Wrong working directory                      | Run the command from the folder where modules.json lives                                                       |
| Old launcher loads old config  | Players still have the previous config.bin   | Publish a new manual patch package, or upload new config.bin and update filelist.txt if auto-update is enabled |
| "Config not found or invalid" after a launcher update | config.bin uses legacy generation `0x01` or failed authentication | Regenerate it with the current admin tool, confirm header `43 47 43 42 02 02`, and republish |

## Config Encryption Details

`config.bin` uses AES-256-GCM authenticated encryption. The launcher rejects an altered authentication tag, an unknown key generation, and legacy generation `0x01` in player builds.

The admin tool writes generation `0x02` with the key material compiled into that tool. Keep the matching player launcher and config tool together for each release. The loader retains narrow migration compatibility for legacy format `0x01` XOR configs only when their embedded HMAC validates; unsigned or HMAC-mismatched format `0x01` data is rejected. Do not create or ship format `0x01` in new packages. The PSK inside the config provides per-server relay authentication.

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
