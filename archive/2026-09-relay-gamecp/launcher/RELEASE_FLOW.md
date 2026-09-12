# Launcher Release Flow

> Operator checklist for keeping the public Community package, configured operator packages, and player-facing archives separate.

## Package Types

| Package                     | Purpose                                          | Server-specific config?                                 |
| --------------------------- | ------------------------------------------------ | ------------------------------------------------------- |
| Base operator kit           | Neutral admin/product download for server owners | No                                                      |
| Configured operator package | Server owner handoff after settings are prepared | Yes, includes `modules.json` and generated `config.bin` |
| Player-facing archive       | Files the server owner shares with players       | Yes, includes `config.bin` only                         |

## Base Operator Kit

```text
Community Package/
|-- RFLauncher.exe
|-- RFLauncher.sig
|-- dinput8.dll
|-- CrespoGuard.ico
|-- CrespoGuardConfigTool.exe
|-- settings.ini
|-- README.txt
|-- LICENSE.txt
|-- modules.json.template
|-- modules.ru_ru.json.template
|-- System/
    |-- Launcher/
        |-- Language/
        |   |-- en.json
        |   |-- en_gb.json
        |   |-- ru_ru.json
        |-- fonts/
```

The base kit intentionally does not include `modules.json`, `System\Launcher\Config\config.bin`, or relay server binaries.

## Configured Operator Package

A configured operator package can include plaintext operator files because it is for the server owner/admin, not players:

```text
Customer Package/
|-- RFLauncher.exe
|-- RFLauncher.sig
|-- dinput8.dll
|-- CrespoGuardConfigTool.exe
|-- modules.json
|-- modules.json.template
|-- modules.ru_ru.json.template
|-- System/
    |-- Launcher/
        |-- Config/
        |   |-- config.bin
        |-- Language/
        |-- fonts/
        |-- logo.png
        |-- background.png
```

## Player-Facing Archive

Player-facing archives must not include plaintext config or operator tools:

```text
Player Package/
|-- RFLauncher.exe
|-- RFLauncher.sig
|-- dinput8.dll
|-- CrespoGuard.ico
|-- settings.ini
|-- System/
    |-- Launcher/
        |-- Config/
        |   |-- config.bin
        |-- Language/
        |-- fonts/
```

Remove `modules.json`, templates, and `CrespoGuardConfigTool.exe` after `config.bin` has been generated.

## Generate config.bin

`config.bin` must live at:

```text
System\Launcher\Config\config.bin
```

A root-level `config.bin` is not loaded by release launcher builds.

From the staging folder, run the admin launcher:

```powershell
& "path\to\admin\RFLauncher.exe" --encrypt-config
```

The command reads `modules.json` from the current directory and writes `System\Launcher\Config\config.bin`.

Use the config tool shipped with the player launcher. Current release packages must use CGCB format `0x02` with key generation `0x02`; player builds reject format `0x02` configs that use legacy key generation `0x01`.

Verify the first six bytes before packaging:

```powershell
$bytes = [IO.File]::ReadAllBytes("System\Launcher\Config\config.bin")
" " -join ($bytes[0..5] | ForEach-Object { $_.ToString("X2") })
```

The output must be `43 47 43 42 02 02`. Regenerate the config after changing the player launcher or config tool.

## Sign the Exact Launcher

`RFLauncher.sig` must be generated from the exact executable that players will run:

```powershell
& "path\to\admin\RFLauncher.exe" --sign-launcher "path\to\package\RFLauncher.exe"
```

A signature generated from a different raw/protected build will fail integrity checks.

## Auto-Update Gate

Player builds require an HMAC-signed SHA-256 `filelist.txt`. Set a 64-character hexadecimal `UpdateSigningKey` in `modules.json` before generating the manifest. The key in `config.bin` must match the key used to sign `filelist.txt`.

Self-update also uses this signed metadata. Do not include `RFLauncher.exe.new` or `System\Launcher\pending_updates.txt` in a package.

## Russian-Client Setup

For Russian-client server setups, use `modules.ru_ru.json.template` as the starting `modules.json` and keep `NationCode: "ru_ru"`. The packaged `ru_ru.json` may be an English fallback template unless a real Russian translation is supplied.

## Handoff Notes

Record these values when handing off a package:

- Package path.
- Package type: base operator kit, configured operator package, or player-facing archive.
- `RFLauncher.exe` size and SHA-256.
- `RFLauncher.sig` size.
- `config.bin` path, SHA-256, and key generation, if configured.
- `modules.json` source path, if configured.
- Whether relay/direct login is configured.
- Whether auto-update uses a matching signed manifest.
- Whether full-code language files are included for the configured `NationCode`.
