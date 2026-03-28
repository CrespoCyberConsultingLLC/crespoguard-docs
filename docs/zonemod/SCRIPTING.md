# JS Scripting Quickstart

CrespoGuard ZoneMod includes a built-in **JavaScript engine** inside the Zone Server.
You write plain `.js` files, drop them into a folder, and the server executes them — no
compiler, no external runtime, no restarts required.

| Concept | Detail |
|---------|--------|
| **Engine** | JavaScript (ES2020, single-threaded, sandboxed) |
| **Script folder** | `CrespoGuard/scripts/` |
| **Plugin folder** | `CrespoGuard/plugins/` (protected `.cgp` bytecode) |
| **Entry point** | `on()` — register a callback for any game event |
| **Hot-reload** | `/js reload` in-game or via the Dashboard |

---

## Your First Script

Create a file called `welcome.js`:

```javascript title="CrespoGuard/scripts/welcome.js"
on('player.login', function(player) {
    player.sendMessage('Welcome to the server, ' + player.name + '!');
});
```

!!! tip "Deploying"
    1. Save the file to `CrespoGuard/scripts/welcome.js`.
    2. In-game, type `/js reload` — or press **Reload Scripts** in the Dashboard.
    3. Log in with a character and you should see the welcome message.

No server restart needed.

---

## How Events Work

Every script revolves around **events**. You call `on()` with an event name and a
callback function. The engine does the rest.

```javascript
on('event.name', function(/* args depend on the event */) {
    // your logic here
});
```

Events come in three flavours:

=== "Notification"

    **Info only** — you can read data but cannot change or cancel the action.

    ```javascript title="Announce level-ups to the server"
    on('player.levelUp', function(player, newLevel) {
        GameServer.broadcast(player.name + ' reached level ' + newLevel + '!');
    });
    ```

=== "Blocking"

    **Return `false`** to cancel the action before it happens.

    ```javascript title="Chat filter"
    on('player.chat', function(player, message) {
        if (message.indexOf('badword') !== -1) {
            player.sendMessage('Watch your language!');
            return false; // block the chat message
        }
    });
    ```

=== "Value-modifying"

    **Return a number** to replace the original value.

    ```javascript title="Double EXP weekend"
    on('player.gainExp', function(player, amount) {
        return amount * 2; // double EXP
    });
    ```

!!! warning "Event name matters"
    The chat event is `player.chat` — **not** `player.chatCircle`.
    Check the Event Reference for exact names.

---

## File Structure

```
CrespoGuard/
├── plugins/              # Protected .cgp bytecode (don't edit)
├── scripts/              # Your custom .js scripts
│   ├── welcome.js
│   ├── custom_rates.js
│   ├── _disabled/        # Scripts here are skipped on load
│   └── store.dat         # Persistent data (auto-managed)
```

!!! note "Load priority"
    If a `.cgp` plugin and a `.js` script share the same base name, the `.cgp` file
    takes priority and the `.js` file is ignored.

---

## Available APIs

A quick map of what you can call from inside a script. Each heading links to its
full reference page.

### Player Object

The `player` argument passed to most event callbacks.

```javascript
player.name           // character name (string)
player.level          // current level (number)
player.race           // 0 = Bellato, 1 = Cora, 2 = Accretia
player.sendMessage(text)          // private system message
player.addBuff(code, index, dur, lvl)  // apply a buff
player.removeBuffByIndex(index)   // remove buff by effect index
```

!!! info "Buff channels"
    `addBuff` accepts an optional 5th argument `channel`: `0` = skill (default),
    `1` = force. Bellato/Cora force buffs need channel `1`; Accretia skill buffs
    use channel `0`.

### GameServer

Server-wide operations available as a global object.

```javascript
GameServer.broadcast(text)        // message to all online players
GameServer.getPlayerByName(name)  // returns a Player or null
GameServer.playerCount             // number of players online (property)
```

### Store (Persistent Data)

Key-value storage that survives server restarts. Data is saved to `store.dat`
automatically.

```javascript
Store.set('topKiller', player.name);
var killer = Store.get('topKiller');       // returns string or undefined
Store.delete('topKiller');
```

### Timers

Standard timer functions, scoped to the script lifetime.

```javascript
setTimeout(function() {
    GameServer.broadcast('Server event starting!');
}, 60000); // 60 seconds

var id = setInterval(function() {
    // runs every 5 minutes
}, 300000);

clearInterval(id);
```

### Event Reference

There are **309+ events** covering combat, trade, crafting, movement, and more.
See the full [Event Reference](EVENTS.md) for the complete list.

---

## Practical Example

A small "double rates" script that combines several APIs:

```javascript title="CrespoGuard/scripts/double_weekend.js"
// Double EXP + drop rate on weekends
function isWeekend() {
    var day = new Date().getDay();
    return day === 0 || day === 6; // Sunday or Saturday
}

on('player.gainExp', function(player, amount) {
    if (isWeekend()) return amount * 2;
});

on('player.login', function(player) {
    if (isWeekend()) {
        player.sendMessage('[EVENT] Double EXP weekend is active!');
    }
});
```

---

## Debugging Tips

| Technique | Usage |
|-----------|-------|
| `console.log(value)` | Prints to the server console window |
| `scriptLog(text)` | Writes to the dedicated script log file |
| `/js reload` | Reload all scripts without restarting |
| `/js list` | Show loaded scripts and their status |

!!! tip "Disable without deleting"
    Move a script into the `_disabled/` subfolder to skip it on load. Move it
    back when you are ready to re-enable.

---

## Common Pitfalls

!!! danger "Array.isArray vs JS_IsArray"
    Use `Array.isArray(val)` to check arrays. The internal `JS_IsArray(val)` takes
    **one** argument only — passing two arguments will cause unexpected behavior.

!!! warning "Don't block the main thread"
    Scripts run on the server's main thread. Avoid infinite loops or heavy
    computation — use `setTimeout` to break work into chunks if needed.

!!! note "Persistent data"
    `Store` is the only way to persist data across reloads and restarts. Local
    variables reset every time scripts are reloaded.

---

## Next Steps

- [API Reference](API.md) — Player, GameServer, Monster, Guild, Store, Timers
- [Event Reference](EVENTS.md) — all 309+ hookable events
