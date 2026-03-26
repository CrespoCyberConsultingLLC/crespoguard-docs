# AI Assistant Usage Guide

> How to ask questions, review scripts, apply configuration changes, and provide
> feedback to improve the model.

---

## Getting Started

1. Open the **CrespoGuard Dashboard** in your browser
2. Navigate to the **AI Assistant** tab
3. The status badge shows the current state:

| Status | Meaning |
|--------|---------|
| **Ready** | Model loaded, ready to accept queries |
| **Loading** | Model is being loaded into memory (first query may take 30-60s) |
| **Downloading** | Model files are being downloaded (~1 GB, one-time) |
| **Not Licensed** | Your license doesn't include the AI feature |
| **Error** | Something went wrong — check the loader log |

!!! tip "First launch"
    On first launch, you'll be prompted to download the AI model. This is a
    one-time ~1 GB download. The model is cached locally and loads automatically
    on subsequent starts.

---

## Asking Questions

Type a natural language question in the input box and press **Ask** or hit Enter.

### Configuration Changes

The AI understands many ways to phrase the same request:

```
set exp rate to 5x
increase experience to 500%
make exp five times higher
double the drop rate
enable movement validation
turn on anti-dupe protection
set mining rate to 3x and enable auto-ban
disable stealth fix
```

The AI responds with:

1. **Explanation** — what it understood and why it's making these changes
2. **Changes table** — module, setting, current value, and proposed value
3. **Warnings** — non-critical issues (e.g., setting not found in current config)
4. **Errors** — critical issues that block the change

### Example

**You ask:** "set exp to 5x and enable movement validation"

**AI responds:**

| Module | Setting | Current | Proposed |
|--------|---------|---------|----------|
| `server_rates` | `exp_rate` | `1.0` | `5.0` |
| `movement_validation` | `enabled` | `false` | `true` |

> I've set the EXP multiplier to 5x and enabled movement validation to detect
> speed hacks, teleport exploits, and fly hacks.

---

## Applying Changes

After reviewing the AI's suggestions:

1. Check the changes table — make sure everything looks correct
2. Click **Apply Changes**
3. The changes are written to `zonemod.json` immediately
4. No server restart required — modules hot-reload the new config

!!! danger "Always review first"
    The AI is a tool, not an authority. Always verify that the proposed changes
    make sense for your server before applying them.

---

## Reviewing Scripts

The AI can analyze your JavaScript scripts for security issues, bugs, and
best practices.

1. Select a script from the **Review Script** dropdown
2. Click **Review**
3. The AI reads the script and provides:
    - Security issues (e.g., missing input validation, unsafe operations)
    - Logic bugs (e.g., wrong event name, missing return value)
    - Performance concerns (e.g., blocking the main thread)
    - Improvement suggestions

!!! note "Script size limit"
    Scripts larger than 32 KB cannot be reviewed. Break large scripts into
    smaller, focused modules.

### Script Sources

The dropdown lists all `.js` files in your `CrespoGuard/scripts/` folder.
Protected `.cgp` plugins are not available for review (their source is compiled).

---

## Creating Scripts

Click **Create JS Script** to have the AI write a new script for you.

1. Describe what you want: "a script that doubles EXP on weekends"
2. The AI generates the JavaScript code
3. Review the generated code
4. Enter a filename (e.g., `weekend_exp.js`)
5. Click **Save** to write it to `CrespoGuard/scripts/`

The new script is loaded automatically on the next `/js reload` or dashboard
reload.

!!! warning "Review generated code"
    AI-generated scripts should always be reviewed before deployment. The AI
    may use incorrect event names or API calls. Cross-check with the
    [Event Reference](../scripting/EVENTS.md) and [API Reference](../scripting/API.md).

---

## Providing Feedback

After every AI response, you'll see thumbs up (👍) and thumbs down (👎) buttons.

| Action | When to Use |
|--------|-------------|
| **👍 Thumbs up** | The response was correct and helpful |
| **👎 Thumbs down** | The response was wrong, confusing, or unhelpful |

Feedback is sent to the CrespoGuard admin portal where it's reviewed and used
to improve the model. See [Retraining Guide](RETRAINING.md) for details.

!!! tip "Negative feedback is valuable"
    When you thumbs-down a response, the admin team can see exactly what went
    wrong and add a corrected version to the training data. This directly
    improves future responses.

---

## Example Queries

### Rate Configuration

```
set exp to 5x
double drop rate
set gold rate to 3x
increase mining rate to 200%
set pvp share to 50%
raise stack limit to 9999
```

### Module Toggles

```
enable movement validation
turn on anti-dupe
disable stealth fix
enable auto-ban with 3 strike threshold
enable chat logging
turn off portal redirect
```

### Multi-Change Requests

```
set exp to 5x and drops to 2x
enable movement validation and combat validation
double all rates and enable auto-ban
```

### JavaScript Help

```
write a script that announces level-ups server-wide
create a chat filter that blocks offensive words
make a double exp event script for weekends
how do I use the Store API to save player data?
```

### Troubleshooting

```
why isn't movement validation detecting speed hacks?
my scripts aren't loading, what could be wrong?
how do I configure auto-ban thresholds?
what's the difference between drop_protection and loot_protection?
```

---

## Limitations

| Limitation | Detail |
|-----------|--------|
| **Response time** | 10-20 seconds per query (CPU inference) |
| **Context window** | The AI sees module names and enabled status, not full config details |
| **Hallucination** | May suggest settings that don't exist — the validator catches these |
| **Complex logic** | Better at simple config changes than multi-step reasoning |
| **Script size** | Cannot review scripts larger than 32 KB |
| **One query at a time** | Wait for the current response before asking another |

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Status shows "Not Licensed" | Check that your license includes `ai_assistant: true` |
| Status shows "Error" | Check `CrespoGuard/logs/` for error details |
| Model download fails | Ensure internet access and ~1 GB free disk space |
| "No pending changes" on Apply | Ask a config question first — review/create don't produce changes |
| Script dropdown empty | Check that `.js` files exist in `CrespoGuard/scripts/` |
| Response seems wrong | Use 👎 feedback and try rephrasing your question |
| Very slow responses (>60s) | Check available RAM — model needs ~2 GB free |

---

## Next Steps

- [AI Overview](OVERVIEW.md) — architecture, safety, and requirements
- [Retraining Guide](RETRAINING.md) — how the feedback loop improves the model
- [JS Scripting Quickstart](../scripting/QUICKSTART.md) — write your own scripts
