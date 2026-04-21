# Bridge HTTP API

The Bridge is a standalone Node.js service (`crespoguard-bridge`) that sits
between your Game Control Panel (GameCP) and the RF Online game server. It
connects to a NATS message broker and a SQL Server database to expose real-time
game data over a simple HTTP API.

---

## Configuration

The Bridge is configured entirely through **environment variables**:

| Variable                 | Default      | Purpose                                              |
| ------------------------ | ------------ | ---------------------------------------------------- |
| `PORT`                   | `3100`       | HTTP server port                                     |
| `BRIDGE_API_KEY`         | _(required)_ | API authentication key                               |
| `NATS_URL`               | _(required)_ | NATS broker connection URL                           |
| `DB_SERVER`              | `localhost`  | SQL Server hostname                                  |
| `DB_PORT`                | `1433`       | SQL Server port                                      |
| `DB_USER`                | `sa`         | SQL Server username                                  |
| `DB_PASS`                | _(empty)_    | SQL Server password                                  |
| `LOG_HISTORY_PATH`       | _(optional)_ | File path for `/bridge/history` log data             |
| `DISCORD_CW_WEBHOOK_URL` | _(optional)_ | Discord webhook URL for Chip War event notifications |

!!! warning "Required Variables"
`BRIDGE_API_KEY` and `NATS_URL` must be set before the service will start.
The API key is used for all authenticated endpoints.

---

## Authentication

All endpoints except `/bridge/health` and `/bridge/name-colors` require the API
key. The Bridge accepts credentials via **three methods**:

1. **`Authorization: Bearer <api_key>`** header
2. **`X-Bridge-Key: <api_key>`** header
3. **`?token=<api_key>`** query parameter

Key comparison is constant-time (timing-attack safe). Unauthenticated requests
receive a `401` response:

```json
{
  "error": "Unauthorized. Provide API key via Authorization header, X-API-Key header, or cg_session cookie."
}
```

---

## Endpoints

| Method | Path                  | Auth    | Description                              |
| ------ | --------------------- | ------- | ---------------------------------------- |
| GET    | `/bridge/health`      | No      | Health check                             |
| GET    | `/bridge/name-colors` | No      | Character name color map                 |
| GET    | `/bridge/snapshot`    | API key | Full server state snapshot               |
| GET    | `/bridge/events`      | API key | Delta events since a given timestamp     |
| POST   | `/bridge/command`     | API key | Execute a game command                   |
| GET    | `/bridge/history`     | API key | Character session history from log files |

---

### GET `/bridge/health`

Returns the current service health. **No authentication required.**

**Response:**

```json
{
  "ok": true,
  "nats": true,
  "uptime": 86400,
  "onlineCount": 142
}
```

| Field         | Type      | Description                           |
| ------------- | --------- | ------------------------------------- |
| `ok`          | `boolean` | Whether the service is running        |
| `nats`        | `boolean` | Whether the NATS connection is active |
| `uptime`      | `number`  | Service uptime in seconds             |
| `onlineCount` | `number`  | Current number of online players      |

---

### GET `/bridge/name-colors`

Returns the character name color map, used by the GameCP to render colored
player names. **No authentication required.**

**Query parameters:**

| Parameter | Type     | Description                      |
| --------- | -------- | -------------------------------- |
| `v`       | `number` | Version number for cache-busting |

**Response:**

```json
{
  "v": 3,
  "colors": {
    "PlayerOne": "#ff0000",
    "PlayerTwo": "#00ff00"
  }
}
```

| Field    | Type                     | Description                             |
| -------- | ------------------------ | --------------------------------------- |
| `v`      | `number`                 | Current version number                  |
| `colors` | `Record<string, string>` | Map of character name to hex color code |

!!! note "Data Source"
Name colors are read from the `gamecp_name_colors` table in the `RF_GameCP`
database and refreshed every **30 seconds**.

---

### GET `/bridge/snapshot`

Returns a complete snapshot of the current server state including all active
sessions, recent events, PvP kills, Chip War status, and faction counts.

**Response:**

```json
{
  "connected": true,
  "onlineCount": 142,
  "peakOnline": 203,
  "loginsToday": 487,
  "killsToday": 1253,
  "activeSessions": [],
  "recentEvents": [],
  "pvpKills": [],
  "chipWarState": null,
  "factionCounts": { "0": 48, "1": 52, "2": 42 }
}
```

| Field            | Type                     | Description                                                                       |
| ---------------- | ------------------------ | --------------------------------------------------------------------------------- |
| `connected`      | `boolean`                | Whether the game server connection is active                                      |
| `onlineCount`    | `number`                 | Current online player count                                                       |
| `peakOnline`     | `number`                 | Peak online count for the current day                                             |
| `loginsToday`    | `number`                 | Total logins since midnight UTC                                                   |
| `killsToday`     | `number`                 | Total PvP kills since midnight UTC                                                |
| `activeSessions` | `PlayerSession[]`        | All active player sessions (see [PlayerSession](#playersession))                  |
| `recentEvents`   | `GameEvent[]`            | Up to **50** recent game events (see [GameEvent](#gameevent))                     |
| `pvpKills`       | `PvpKill[]`              | Up to **20** recent PvP kills (see [PvpKill](#pvpkill))                           |
| `chipWarState`   | `ChipWarState \| null`   | Current Chip War state, or `null` if inactive (see [ChipWarState](#chipwarstate)) |
| `factionCounts`  | `Record<number, number>` | Online player count per faction (key = race code)                                 |

---

### GET `/bridge/events`

Returns events that occurred after the given timestamp. Use this for polling
incremental updates instead of fetching the full snapshot.

**Query parameters:**

| Parameter | Type     | Description                                                                   |
| --------- | -------- | ----------------------------------------------------------------------------- |
| `since`   | `number` | Unix timestamp in **milliseconds**. Only events after this time are returned. |

**Response:**

```json
{
  "events": [],
  "pvpKills": [],
  "chipWarState": null,
  "onlineCount": 142,
  "factionCounts": { "0": 48, "1": 52, "2": 42 }
}
```

| Field           | Type                     | Description                         |
| --------------- | ------------------------ | ----------------------------------- |
| `events`        | `GameEvent[]`            | Events since the given timestamp    |
| `pvpKills`      | `PvpKill[]`              | PvP kills since the given timestamp |
| `chipWarState`  | `ChipWarState \| null`   | Current Chip War state              |
| `onlineCount`   | `number`                 | Current online count                |
| `factionCounts` | `Record<number, number>` | Online count per faction            |

---

### POST `/bridge/command`

Executes a game command. Request body is limited to **64 KB**.

**Request:**

```json
{
  "command": "giveItemByName",
  "params": {
    "serial": 12345,
    "itemName": "Beam Saber",
    "count": 1
  }
}
```

| Field     | Type                      | Description                             |
| --------- | ------------------------- | --------------------------------------- |
| `command` | `string`                  | Command name from the allowlist below   |
| `params`  | `Record<string, unknown>` | _(optional)_ Parameters for the command |

**Allowed commands:**

| Command               | Description                                 |
| --------------------- | ------------------------------------------- |
| `get_online_players`  | Retrieve the list of online players         |
| `get_chip_war_status` | Get current Chip War state                  |
| `giveItemByName`      | Give an item to a player by item name       |
| `item_give`           | Give an item to a player                    |
| `session_create`      | Create a new game session                   |
| `terminate_session`   | Terminate an active session                 |
| `takeItemBySerial`    | Remove an item from a player by item serial |
| `pushUserSave`        | Force-save a player's data                  |
| `teleportPlayer`      | Teleport a player to coordinates or a map   |

**Success response:**

```json
{
  "success": true
}
```

**Error response:**

```json
{
  "error": "Unknown command"
}
```

---

### GET `/bridge/history`

Returns session history for a character from log files. This endpoint requires
the `LOG_HISTORY_PATH` environment variable to be configured.

**Query parameters:**

| Parameter | Type     | Description                     |
| --------- | -------- | ------------------------------- |
| `serial`  | `number` | Character serial number         |
| `from`    | `string` | Start date in `YYYYMMDD` format |
| `to`      | `string` | End date in `YYYYMMDD` format   |

**Response:**

```json
{
  "hisSessions": [],
  "lhiSessions": [],
  "lookup": {
    "items": {},
    "mobs": {}
  }
}
```

| Field          | Type     | Description                       |
| -------------- | -------- | --------------------------------- |
| `hisSessions`  | `array`  | Historical session records        |
| `lhiSessions`  | `array`  | Linked historical session records |
| `lookup.items` | `object` | Item ID to name mapping           |
| `lookup.mobs`  | `object` | Mob ID to name mapping            |

!!! warning "503 Service Unavailable"
This endpoint returns **503** if `LOG_HISTORY_PATH` is not configured on the
server.

---

## Type Schemas

### PlayerSession

Represents an active player connection to the game server.

| Field       | Type      | Description                                          |
| ----------- | --------- | ---------------------------------------------------- |
| `uuid`      | `string`  | Unique session identifier                            |
| `serial`    | `number`  | Character serial number                              |
| `username`  | `string?` | Character name (may be absent)                       |
| `race`      | `number?` | Race code: `0` = Accretia, `1` = Bellato, `2` = Cora |
| `level`     | `number?` | Character level (may be absent)                      |
| `mapId`     | `number?` | Current map ID (may be absent)                       |
| `enteredAt` | `number`  | Session start time (Unix milliseconds)               |

---

### GameEvent

Represents a game event captured by the Bridge.

| Field       | Type     | Description                    |
| ----------- | -------- | ------------------------------ |
| `id`        | `string` | Unique event identifier        |
| `type`      | `string` | Event type (see table below)   |
| `summary`   | `string` | Human-readable event summary   |
| `data`      | `object` | Event-specific payload         |
| `timestamp` | `number` | Event time (Unix milliseconds) |

**Event types:**

| Type                   | Description                                             |
| ---------------------- | ------------------------------------------------------- |
| `player_login`         | Player connected to the server                          |
| `player_logout`        | Player disconnected from the server                     |
| `pvp_kill`             | A player killed another player                          |
| `enchant_success`      | Item enchantment succeeded                              |
| `enchant_fail`         | Item enchantment failed                                 |
| `dungeon`              | Dungeon-related event                                   |
| `map`                  | Map change or map-related event                         |
| `character_created`    | A new character was created                             |
| `character_deleted`    | A character was deleted                                 |
| `cw_scene_change`      | Chip War scene transition                               |
| `cw_stone_destroyed`   | Chip War Holy Stone destroyed                           |
| `cw_destroyer_arrived` | Chip War Destroyer (Holystone Keeper Destroyer) arrived |
| `cw_kill`              | Kill during Chip War                                    |
| `unknown`              | Unclassified event                                      |

---

### PvpKill

Represents a PvP kill event.

| Field          | Type      | Description                          |
| -------------- | --------- | ------------------------------------ |
| `id`           | `string`  | Unique kill identifier               |
| `attackerUuid` | `string`  | UUID of the attacking player         |
| `attackerName` | `string?` | Name of the attacker (may be absent) |
| `victimUuid`   | `string`  | UUID of the victim                   |
| `victimName`   | `string?` | Name of the victim (may be absent)   |
| `damage`       | `number`  | Damage dealt in the killing blow     |
| `timestamp`    | `number`  | Kill time (Unix milliseconds)        |

---

### ChipWarState

Represents the current state of a Chip War battle.

| Field                  | Type             | Description                                       |
| ---------------------- | ---------------- | ------------------------------------------------- |
| `available`            | `boolean`        | Whether Chip War is currently available           |
| `sceneCode`            | `number`         | Current Chip War scene/phase code                 |
| `nextStartTime`        | `number`         | Scheduled start time for the next Chip War        |
| `masterRace`           | `number`         | Race code of the faction controlling the chip     |
| `stoneHP`              | `number[3]`      | Holy Stone HP for each faction (indexed by race)  |
| `battlePoints`         | `number[3]`      | Battle points for each faction (indexed by race)  |
| `destroyerSerial`      | `number`         | Serial number of the Destroyer unit               |
| `destroyerName`        | `string?`        | Name of the Destroyer player (may be absent)      |
| `destroyerGuildSerial` | `number`         | Guild serial of the Destroyer player              |
| `oreRemain`            | `number`         | Remaining ore in the mine                         |
| `oreTotal`             | `number`         | Total ore capacity                                |
| `stones`               | `ChipWarStone[]` | Individual Holy Stone states                      |
| `keeper`               | `object \| null` | Holystone Keeper state, or `null` if not active   |
| `keeper.hp`            | `number`         | Keeper's current HP                               |
| `keeper.masterRace`    | `number`         | Race code of the faction that controls the Keeper |
| `updatedAt`            | `number`         | Last update time (Unix milliseconds)              |

---

### ChipWarStone

Represents a single Holy Stone in Chip War.

| Field        | Type      | Description                            |
| ------------ | --------- | -------------------------------------- |
| `hp`         | `number`  | Current HP                             |
| `maxHP`      | `number`  | Maximum HP                             |
| `masterRace` | `number`  | Race code of the controlling faction   |
| `active`     | `boolean` | Whether this stone is currently active |

---

## Internal Limits

The Bridge enforces the following internal limits:

| Limit                            | Value        |
| -------------------------------- | ------------ |
| Max recent events in snapshot    | 50           |
| Max PvP kills in snapshot        | 20           |
| Max stored events in memory      | 200          |
| Max stored PvP kills in memory   | 50           |
| POST body size limit             | 64 KB        |
| Name color refresh interval      | 30 seconds   |
| Database reconciliation interval | 120 seconds  |
| Daily counter reset              | Midnight UTC |

---

## Database Tables

The Bridge reads from the following SQL Server tables:

| Database    | Table                      | Purpose                                  |
| ----------- | -------------------------- | ---------------------------------------- |
| `RF_World`  | `tbl_base`                 | Character data (names, levels, factions) |
| `RF_User`   | `tbl_Sirin_Account_Player` | Account-to-character mapping             |
| `RF_GameCP` | `gamecp_name_colors`       | Custom name colors for the GameCP        |

---

## Example: Polling for Updates

A typical GameCP integration polls `/bridge/events` on a short interval to keep
its UI in sync:

```javascript
let lastTimestamp = Date.now();

async function poll() {
  const res = await fetch(
    `http://127.0.0.1:3100/bridge/events?since=${lastTimestamp}`,
    { headers: { Authorization: "Bearer YOUR_API_KEY" } },
  );
  const data = await res.json();

  // Process new events
  for (const event of data.events) {
    console.log(`[${event.type}] ${event.summary}`);
    lastTimestamp = Math.max(lastTimestamp, event.timestamp);
  }

  // Update online count
  document.getElementById("online").textContent = data.onlineCount;
}

setInterval(poll, 3000);
```

---

## Security Notes

- API key comparison is **constant-time** (timing-attack safe)
- POST request bodies are limited to **64 KB**
- CORS headers are included for browser-based access
- The Bridge should only be exposed on `127.0.0.1` or behind a reverse proxy
  in production environments
