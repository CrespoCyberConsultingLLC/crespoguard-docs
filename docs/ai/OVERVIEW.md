# AI Assistant

> A built-in AI that helps server owners configure CrespoGuard using natural language.
> Ask it to change rates, enable modules, review scripts, or generate new ones — no
> manual JSON editing required.

!!! warning "Beta Feature"
    The AI Assistant is in **beta**. Suggestions may not always be accurate.
    Always review proposed changes before applying them.

---

## What It Does

The AI Assistant runs a **local language model** directly on your server machine.
No data leaves your server — all inference happens on the CPU using
[llama.cpp](https://github.com/ggerganov/llama.cpp).

| Capability | Description |
|-----------|-------------|
| **Config changes** | "Set EXP rate to 5x", "enable anti-dupe", "disable stealth fix" |
| **Script review** | Select a `.js` script and get a security + correctness analysis |
| **Script creation** | Describe what you want and the AI writes the JavaScript for you |
| **Explanation** | Every suggestion includes a plain-English explanation of what changes and why |

---

## How It Works

```
┌─────────────────────────────────────────────────────┐
│  Dashboard (AI Assistant tab)                       │
│                                                     │
│  1. You type a question                             │
│  2. AI reads your current zonemod.json              │
│  3. Local LLM generates a JSON response             │
│  4. Validator checks safety + correctness            │
│  5. You review changes and click Apply              │
│  6. zonemod.json is updated — no restart needed     │
└─────────────────────────────────────────────────────┘
```

The AI knows about:

- All **185 CrespoGuard modules** and their settings
- The **JavaScript event system** (350+ hooks, Player/GameServer/Store APIs)
- **Rate multipliers** (EXP, drop, gold, mining, PvP, stack limits)
- **Security modules** and why certain ones cannot be disabled

---

## Requirements

| Requirement | Detail |
|-------------|--------|
| **License** | Premium license with `ai_assistant` feature enabled |
| **RAM** | Minimum 2 GB available (for the default 1.5B model) |
| **Disk** | ~1 GB for model files (auto-downloaded on first use) |
| **CPU** | Any modern x64 processor — no GPU required |

!!! note "Model files"
    On first launch, the AI downloads the base model from HuggingFace (~1 GB)
    and a fine-tuned adapter from CrespoGuard servers (~30 MB). Both are cached
    locally in `CrespoGuard/models/` and only downloaded once.

---

## Model

The default model is **Qwen2.5-1.5B-Instruct** (Q4_K_M quantization), fine-tuned
specifically on CrespoGuard configuration data. It runs entirely on CPU with typical
response times of **10-20 seconds**.

The fine-tuning adapter (LoRA) is trained on 500+ question-answer pairs covering:

- Module configuration (rates, toggles, thresholds)
- JavaScript scripting (events, APIs, common patterns)
- Troubleshooting (why modules don't work, common misconfigs)
- Security guidance (what's safe to change, what isn't)

---

## Safety

The AI **cannot** disable critical security modules. The following are force-enabled
and protected from AI modification:

| Protected Module | Purpose |
|-----------------|---------|
| `equip_validation` | Equipment exploit prevention |
| `memory_patch` | Memory integrity |
| `animus_fix` | Animus exploit fix |
| `trade_validation` | Trade dupe prevention |
| `drop_protection` | Drop exploit prevention |
| `loot_protection` | Loot exploit prevention |
| `mining_validation` | Mining exploit prevention |
| `auto_ban` | Automatic violation bans |
| `sql_protection` | SQL injection prevention |
| `network_validation` | Network exploit prevention |
| `dupe_prevention` | Item duplication prevention |
| `store_validation` | Store exploit prevention |
| `combat_validation` | Combat exploit prevention |
| `crash_fix` | Server crash prevention |
| `crash_dump` | Crash diagnostics |

If you ask the AI to disable any of these, it will refuse with a clear explanation.

All other changes go through a **validator** that checks:

1. The module exists in CrespoGuard
2. The setting name is valid
3. The proposed value makes sense
4. The JSON output is well-formed

---

## Privacy

- The model runs **100% locally** on your machine
- **No queries or responses are sent to the internet**
- Usage telemetry (query count only, no content) is sent to the admin API for
  license metering — this can be disabled
- Feedback ratings (thumbs up/down) are sent to help improve the model

---

## Next Steps

- [Usage Guide](USAGE.md) — how to ask questions, review scripts, and apply changes
- [Retraining Guide](RETRAINING.md) — how to improve the model with feedback
