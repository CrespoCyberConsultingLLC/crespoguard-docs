# API Reference

Complete reference for the CrespoGuard ZoneMod JavaScript API. All objects, properties,
and methods exposed to scripts running in the CrespoGuard scripting engine.

---

## Player Object

Player objects are passed to event callbacks and returned by `GameServer` lookup methods.
All properties are **read-only**. Mutations go through methods.

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `name` | `string` | Character name |
| `level` | `int` | Current level |
| `race` | `int` | Race code: `0` = Bellato, `1` = Cora, `2` = Accretia |
| `hp` | `int` | Current hit points |
| `maxHp` | `int` | Maximum hit points |
| `fp` | `int` | Current force points |
| `maxFp` | `int` | Maximum force points |
| `sp` | `int` | Current stamina points |
| `maxSp` | `int` | Maximum stamina points |
| `x` | `float` | World X position |
| `y` | `float` | World Y position |
| `z` | `float` | World Z position |
| `online` | `bool` | Whether the player is currently connected |
| `dalant` | `int` | Dalant currency balance |
| `gold` | `int` | Gold currency balance |
| `exp` | `int` | Current experience points |
| `pvpPoint` | `int` | PvP points |
| `pvpCash` | `int` | PvP cash |
| `serial` | `int` | Unique character serial number |
| `mapCode` | `int` | Current map code |
| `maxLevel` | `int` | Maximum attainable level |
| `battleMode` | `int` | Current battle mode flag |
| `isMoving` | `bool` | Whether the player is currently moving |
| `moveSpeed` | `float` | Current movement speed |
| `index` | `int` | Player array index (internal slot) |
| `guildName` | `string` | Name of the player's guild (empty if none) |
| `guildRole` | `int` | Guild rank/class code |
| `hasGuild` | `bool` | Whether the player belongs to a guild |
| `isRidingUnit` | `bool` | Whether the player is riding a MAU/mount |
| `attackRange` | `float` | Attack range |
| `attackDP` | `float` | Attack damage points |
| `maxDp` | `float` | Maximum defense points |
| `fireTol` | `int` | Fire tolerance |
| `waterTol` | `int` | Water tolerance |
| `earthTol` | `int` | Earth tolerance |
| `windTol` | `int` | Wind tolerance |
| `isChaos` | `bool` | Whether the player has Chaos status |
| `isPatriarch` | `bool` | Whether the player is a Patriarch/Archon |
| `isStealth` | `bool` | Whether the player is stealthed |
| `stateFlags` | `int` | Raw state bitmask |
| `gmDegree` | `int` | GM authority level (`0` = normal player) |
| `inParty` | `bool` | Whether the player is in a party |
| `classId` | `int` | Character class ID |
| `mapIndex` | `int` | Current map instance index |
| `guild` | `Guild` | Guild object (or `null` if no guild) |
| `skinSet` | `object` | Current skin override map (read-only) |

### Methods

#### Communication

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `sendMessage` | `msg` | `void` | Send a private system message to the player |
| `sendRawChat` | `msg, msgType, senderName` | `void` | Send a raw chat packet (msgType: 0=System, 1=Circle, 2=Race, 3=Party) |

```javascript
on('player.login', function(player) {
    player.sendMessage('Welcome back, ' + player.name + '!');
});
```

#### Stats & Resources

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `setHp` | `value` | `void` | Set current HP |
| `setFp` | `value` | `void` | Set current FP |
| `setSp` | `value` | `void` | Set current SP |
| `heal` | | `void` | Fully restore HP, FP, and SP |
| `addDalant` | `amount` | `void` | Add dalant to the player |
| `addGold` | `amount` | `void` | Add gold to the player |
| `subtractDalant` | `amount` | `void` | Remove dalant from the player |
| `subtractGold` | `amount` | `void` | Remove gold from the player |
| `addExp` | `amount` | `void` | Grant experience points |
| `setLevel` | `level` | `void` | Set the player's level |
| `addPvpPoint` | `amount` | `void` | Add PvP points |
| `addPvpCash` | `amount` | `void` | Add PvP cash |

```javascript
on('player.chat', function(player, msg) {
    if (msg === '!heal') {
        player.heal();
        player.sendMessage('You have been fully healed.');
    }
});
```

#### Movement & State

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `kick` | `reason` | `void` | Disconnect the player with an optional reason |
| `teleport` | `x, y, z` | `void` | Teleport to world coordinates |
| `resurrect` | | `void` | Resurrect a dead player |
| `isInTown` | | `bool` | Check if the player is in a town safe zone |
| `breakStealth` | | `void` | Force the player out of stealth |

```javascript
on('player.chat', function(player, msg) {
    if (msg === '!home') {
        player.teleport(2500.0, 100.0, 2500.0);
        player.sendMessage('Teleported to home base.');
    }
});
```

#### Inventory

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `giveItem` | `itemCode, count, storageType` | `void` | Give items to the player |
| `removeItem` | `itemCode, count, storageType` | `void` | Remove items from inventory |
| `hasItem` | `itemCode, storageType` | `bool` | Check if the player has the item |
| `getItemCount` | `itemCode, storageType` | `int` | Get quantity of an item |

```javascript
on('player.chat', function(player, msg) {
    if (msg === '!kit') {
        player.giveItem('iw01a001', 1, 0); // Give a weapon
        player.sendMessage('Starter kit granted.');
    }
});
```

#### Buffs

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `addBuff` | `effectCode, effectIndex, duration, level [, channel]` | `void` | Apply a buff. Channel: `0` = skill (default), `1` = force |
| `removeBuff` | `channel, slotIndex` | `void` | Remove a buff by channel and slot |
| `removeBuffByIndex` | `effectIndex` | `void` | Remove a buff by scanning all slots for the effect index |
| `hasBuff` | `effectIndex` | `bool` | Check if the player has a specific buff |
| `getBuffs` | | `array` | Return an array of active buff objects |

!!! warning "Force buffs require channel 1"
    Bellato and Cora force buffs must use `channel: 1`. Accretia skill buffs use `channel: 0`.

```javascript
// Apply a 60-second level-5 buff via force channel
player.addBuff(120, 42, 60, 5, 1);

// Check and remove
if (player.hasBuff(42)) {
    player.removeBuffByIndex(42);
}
```

#### GM Commands

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `execGMCommand` | `command` | `void` | Execute a GM command as this player |
| `sendCombineResult` | `a, b, c, d, e` | `void` | Send a combine result packet |

#### Skin / Visual Overrides

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `equipSkin` | `slot, itemCode, level` | `void` | Apply a visual skin to an equipment slot using an item code |
| `equipItemByCode` | `slot, itemCode` | `void` | Equip item visuals by code |
| `equipItem` | `slot, itemCode, level` | `void` | Equip item visuals with level |
| `lookupItemCode` | `itemCode` | `object\|null` | Look up item data by code |
| `lookupItemEffects` | `itemCode, level` | `object\|null` | Look up item effect data |
| `setSkinByCode` | `slot, itemCode` | `void` | Set skin override by item code string |
| `setSkin` | `slot, meshId` | `void` | Set skin override by raw mesh ID |
| `clearSkin` | `slot` | `void` | Clear skin override for a slot |
| `clearAllSkins` | | `void` | Clear all skin overrides |
| `getSkin` | `slot` | `object\|null` | Get current skin override for a slot |
| `applySkinSet` | `skinSetObj` | `void` | Apply a full set of skin overrides from an object |
| `setSkinBuff` | `effectIndex, level` | `void` | Apply a cosmetic buff tied to a skin |
| `clearSkinBuffs` | | `void` | Remove all skin-linked buffs |
| `removeSkin` | | `void` | Remove all skins and broadcast the change |

#### UI / Dialog

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `showDialog` | `dialogData` | `void` | Send a custom dialog/UI packet to the player |
| `closeDialog` | `dialogId` | `void` | Close an open dialog by ID |
| `updateHUD` | `hudData` | `void` | Send a HUD update packet |
| `clearHUD` | | `void` | Clear all custom HUD elements |
| `setQuestMarker` | `x, z` | `void` | Show a quest waypoint marker on the minimap |
| `clearQuestMarker` | | `void` | Remove the quest waypoint marker |
| `getActiveQuests` | | `array` | Get the player's active quest list |

---

## GameServer Object

Global singleton for server-wide queries and operations. Available as `GameServer` in all scripts.

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `playerCount` | `int` | Number of currently online players |

### Player Lookup

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `getPlayer` | `index` | `Player\|null` | Get a player by array slot index |
| `getPlayerByName` | `name` | `Player\|null` | Find an online player by character name (case-insensitive) |
| `getPlayerBySerial` | `serial` | `Player\|null` | Find an online player by unique serial |
| `getPlayerByIndex` | `playerIdx` | `Player\|null` | Get a player by ObjID array index |
| `getOnlinePlayers` | | `Player[]` | Get all online players |
| `getPlayersByRace` | `raceCode` | `Player[]` | Get all online players of a race (0/1/2) |
| `getPlayersInRange` | `x, y, z, radius` | `Player[]` | Get all players within radius of a point |

```javascript
var bellato = GameServer.getPlayersByRace(0);
GameServer.broadcast('Bellato online: ' + bellato.length);
```

### Communication

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `broadcast` | `message` | `void` | Send a GM notice to all online players |

### Monster Operations

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `getMonster` | `index` | `Monster\|null` | Get a monster by array index |
| `getAliveMonsters` | | `Monster[]` | Get all alive monsters |
| `monsterCount` | | `int` | Count of alive monsters on the map |
| `spawnMonster` | `mapCode, monsterCode, x, y, z` | `Monster\|null` | Spawn a monster at coordinates |
| `monsterExists` | `code` | `bool` | Check if a monster code exists in data |
| `getMonsterInfo` | `code` | `object\|null` | Get monster data record (code, isBoss, maxHP, attackDP) |
| `getMonsterCodes` | | `string[]` | Get all registered monster codes |

```javascript
var boss = GameServer.spawnMonster(200, 'mn_headhunter01', 2500, 100, 2500);
if (boss) {
    GameServer.broadcast('A boss has spawned!');
}
```

### Guild Operations

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `getGuild` | `index` | `Guild\|null` | Get a guild by array index |

### Quest

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `getQuestInfo` | `index` | `object\|null` | Get quest data (code, killTarget, killCount) |

### Chip War Control

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `chipWarStart` | | `void` | Start the Chip War battle phase |
| `chipWarPreStart` | | `void` | Enter the Chip War pre-start (countdown) phase |
| `chipWarStop` | | `void` | Force-stop the current Chip War |
| `chipWarKeeperStart` | | `void` | Activate Chip War keeper spawn |

### Monster Events

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `startMonsterEvent` | `name` | `{success, message}` | Start a named monster event set |
| `stopMonsterEvent` | `name` | `{success, message}` | Stop a named monster event set |

### Utility

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `getTime` | | `int` | Server time in seconds (timeGetTime / 1000) |
| `getTickCount` | | `int` | Raw millisecond tick counter |

### Auto-Loot Configuration

Access via `GameServer.autoLoot`.

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `autoLoot.get` | | `object` | Get current config: `{ stackLimit, scatterRange, scatterOnlyBoss, skipBoss, mau, siege }` |
| `autoLoot.set` | `configObj` | `void` | Update auto-loot settings. Only provided keys are changed. |

```javascript
var cfg = GameServer.autoLoot.get();
console.log('Stack limit: ' + cfg.stackLimit);

GameServer.autoLoot.set({ stackLimit: 500, scatterRange: 8 });
```

### Skin Remaps

Global item visual replacements applied to **all** players.

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `addSkinRemap` | `fromCode, toCode` | `void` | Replace all visuals of one item code with another |
| `removeSkinRemap` | `fromCode` | `bool` | Remove a remap entry |
| `clearSkinRemaps` | | `void` | Remove all remap entries |

---

## Monster Object

Returned by `GameServer.getMonster()`, `GameServer.getAliveMonsters()`, and monster-related
event callbacks.

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `hp` | `int` | Current hit points |
| `maxHp` | `int` | Maximum hit points |
| `x` | `float` | World X position |
| `y` | `float` | World Y position |
| `z` | `float` | World Z position |
| `isAlive` | `bool` | Whether the monster is currently alive |
| `isBoss` | `bool` | Whether this is a boss-tier monster |
| `moveSpeed` | `float` | Current movement speed |
| `attackRange` | `float` | Attack range |
| `attackDP` | `int` | Attack damage points |
| `fireTol` | `int` | Fire tolerance |
| `waterTol` | `int` | Water tolerance |
| `earthTol` | `int` | Earth tolerance |
| `windTol` | `int` | Wind tolerance |
| `index` | `int` | Array slot index |
| `code` | `string` | Monster code from data record |
| `serial` | `int` | Unique object serial |
| `level` | `int` | Monster level |
| `name` | `string` | Display name |
| `race` | `int` | Race code from data record |

### Methods

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `moveTo` | `x, y, z [, checkPath]` | `bool` | Command the monster to walk to a position. Optional path validation. |
| `teleport` | `x, y, z` | `void` | Instantly teleport the monster |
| `setSpeed` | `speed` | `void` | Override the monster's movement speed |
| `despawn` | | `void` | Remove the monster from the world |

```javascript
on('monster.death', function(monster, killer) {
    if (monster.isBoss) {
        GameServer.broadcast(killer.name + ' slew ' + monster.name + '!');
    }
});
```

---

## Guild Object

Returned by `GameServer.getGuild()` or the `player.guild` property.

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `name` | `string` | Guild name |
| `serial` | `int` | Unique guild serial |
| `grade` | `int` | Guild grade/tier |
| `race` | `int` | Guild race code |
| `dalant` | `float` | Guild dalant treasury |
| `gold` | `float` | Guild gold treasury |
| `memberCount` | `int` | Number of guild members |

### Methods

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `removeMember` | `serial` | `void` | Force-remove a member by character serial |
| `changeMaster` | `memberSerial` | `bool` | Transfer guild leadership to the member with the given serial |

---

## Store (Persistent Key-Value Storage)

Global `Store` object for persisting data across script reloads and server restarts.
Data is saved to a JSON file on disk.

### Methods

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `get` | `key` | `any` | Retrieve a value by key. Returns `undefined` if not found. |
| `set` | `key, value` | `void` | Store a value. Accepts strings, numbers, booleans, and objects (serialized as JSON). |
| `delete` | `key` | `bool` | Delete a key. Returns `true` if the key existed. |
| `has` | `key` | `bool` | Check if a key exists |
| `keys` | | `string[]` | Get all stored keys |
| `save` | | `void` | Force an immediate flush to disk (auto-saved periodically) |
| `size` | | `int` | Number of stored keys (property, not method) |

```javascript
on('player.login', function(player) {
    var visits = Store.get('visits_' + player.serial) || 0;
    visits++;
    Store.set('visits_' + player.serial, visits);
    player.sendMessage('Login #' + visits + ' recorded.');
});

on('player.logout', function(player) {
    Store.save(); // ensure data is flushed before next restart
});
```

---

## Timers

Standard timer functions available in the global scope.

| Function | Parameters | Returns | Description |
|----------|-----------|---------|-------------|
| `setTimeout` | `callback, delayMs` | `int` | Execute callback once after `delayMs` milliseconds. Returns timer ID. |
| `setInterval` | `callback, intervalMs` | `int` | Execute callback repeatedly every `intervalMs` milliseconds. Returns timer ID. |
| `clearTimeout` | `timerId` | `void` | Cancel a pending timeout |
| `clearInterval` | `timerId` | `void` | Cancel a repeating interval |

```javascript
// Announce every 5 minutes
var announcerId = setInterval(function() {
    GameServer.broadcast('Server rates: 5x EXP, 3x Drop!');
}, 300000);

// One-time delayed message
setTimeout(function() {
    GameServer.broadcast('The Chip War begins in 1 minute!');
}, 60000);
```

---

## Console

Standard `console` object for logging. Output goes to the server log and the per-script
`debug.log` file.

| Method | Parameters | Description |
|--------|-----------|-------------|
| `console.log` | `...args` | General log message |
| `console.warn` | `...args` | Warning-level message |
| `console.error` | `...args` | Error-level message |
| `console.debug` | `...args` | Debug-level message |
| `console.info` | `...args` | Info-level message |
| `console.trace` | | Print a stack trace |
| `console.table` | `data` | Log tabular data |
| `console.time` | `[label]` | Start a named timer |
| `console.timeEnd` | `[label]` | End a named timer and log elapsed time |
| `console.count` | `[label]` | Increment and log a named counter |
| `console.countReset` | `[label]` | Reset a named counter |
| `console.assert` | `condition, ...args` | Log an error if `condition` is falsy |

---

## HTTP

Async HTTP client. Requests run on a background thread; callbacks fire on the next
engine tick. URLs must be whitelisted in the module config.

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `http.get` | `url, callback` | `void` | Perform an HTTP GET request |
| `http.post` | `url, body, callback` | `void` | Perform an HTTP POST request (Content-Type: application/json) |

Callbacks receive three arguments: `(error, body, statusCode)`.

- On success: `error` is `null`, `body` is the response string, `statusCode` is the HTTP status.
- On failure: `error` is an error string, `body` is `null`, `statusCode` is `0`.

```javascript
// Send a webhook notification when a boss dies
on('monster.death', function(monster, killer) {
    if (!monster.isBoss) return;

    var payload = JSON.stringify({
        content: killer.name + ' killed ' + monster.name + '!'
    });

    http.post('https://discord.com/api/webhooks/YOUR_HOOK', payload,
        function(err, body, status) {
            if (err) console.error('Webhook failed: ' + err);
        }
    );
});
```

!!! note "Concurrency & Limits"
    Maximum 4 concurrent HTTP requests. Timeout is 5 seconds. Response body capped at 1 MB.

---

## NATS

Lightweight pub/sub messaging for cross-server communication via a NATS server.
Connection is configured in the module config.

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `nats.publish` | `subject, payload` | `bool` | Publish a message. Returns `true` on success. |
| `nats.subscribe` | `subject, callback` | `number` | Subscribe to a subject. Returns a subscription ID. |
| `nats.unsubscribe` | `subscriptionId` | `bool` | Unsubscribe by ID. Returns `true` if found. |
| `nats.connected` | | `bool` | Check if the NATS client is connected |

Subscribe callbacks receive `(subject, payload, replySubject)`.

```javascript
// Cross-server global chat relay
nats.subscribe('chat.global', function(subject, payload, reply) {
    var data = JSON.parse(payload);
    GameServer.broadcast('[' + data.server + '] ' + data.name + ': ' + data.msg);
});

on('player.chat', function(player, msg) {
    if (msg.startsWith('!global ')) {
        nats.publish('chat.global', JSON.stringify({
            server: 'Zone1',
            name: player.name,
            msg: msg.substring(8)
        }));
    }
});
```

---

## ClientPatches

Server-driven client visual patches. Register named patch groups and push them to
players on login or on demand. Requires the CrespoGuard client module.

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `ClientPatches.register` | `name, entries[]` | `void` | Register a named patch group |
| `ClientPatches.unregister` | `name` | `void` | Remove a registered patch group |
| `ClientPatches.list` | | `string[]` | Get names of all registered patch groups |
| `ClientPatches.pushTo` | `player, name` | `void` | Send a specific patch group to one player |
| `ClientPatches.pushAll` | `name` | `void` | Send a patch group to all online players |
| `ClientPatches.pushAllTo` | `player` | `void` | Send all registered patches to one player |

!!! info "Auto-push on login"
    All registered patches are automatically sent to each player when they log in.

!!! note "Patch definitions"
    Patch entries are provided by CrespoGuard support. Contact us for custom client patches.

---

## Global Functions

Available in every script's global scope.

| Function | Parameters | Returns | Description |
|----------|-----------|---------|-------------|
| `on` | `eventName, callback` | `void` | Register a callback for a game event |
| `off` | `eventName` | `void` | Unregister all callbacks for an event |
| `setTimeout` | `callback, delayMs` | `int` | Schedule a one-shot timer |
| `setInterval` | `callback, intervalMs` | `int` | Schedule a repeating timer |
| `clearTimeout` | `timerId` | `void` | Cancel a timeout |
| `clearInterval` | `timerId` | `void` | Cancel an interval |
| `scriptLog` | `message` | `void` | Write a timestamped line to the script's `debug.log` |
| `scriptLog.clear` | | `void` | Truncate the script's `debug.log` file |

!!! tip "Event name reference"
    The event name is `player.chat`, **not** `player.chatCircle`. See the
    [Events Reference](EVENTS.md) for a complete list of available events.
