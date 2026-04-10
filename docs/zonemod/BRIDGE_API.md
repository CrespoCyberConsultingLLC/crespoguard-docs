# Bridge HTTP API

The Bridge HTTP API is built into CrespoGuard.exe and provides real-time game data access for your Game Control Panel (GameCP) website.

## Configuration

In `zonemod.json`:
```json
{
  "Bridge": {
    "Enabled": true,
    "ApiKey": "auto-generated on first run"
  }
}
```
- API key is auto-generated via secure random on first run and saved to config
- Default port: 8081

## Authentication

All endpoints except `/bridge/health` require the API key via `X-Bridge-Key` header or `?token=` query parameter (for SSE).

## Endpoints

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| GET | `/bridge/health` | No | Health check — returns `{ok, uptime, onlineCount}` |
| GET | `/bridge/snapshot` | API key | Full server state: sessions, events, PvP kills, chip war, factions |
| GET | `/bridge/events?since=<ms>` | API key | Delta events since timestamp (milliseconds) |
| POST | `/bridge/command` | API key | Execute commands: kick, give_item, broadcast, teleport |
| GET | `/bridge/live?token=<key>` | Token | SSE stream with 3-second delta updates |

## Commands

POST `/bridge/command` with JSON body:

```json
{
  "command": "kick",
  "params": { "serial": 12345 }
}
```

Available commands: `kick`, `give_item`, `broadcast`, `teleport` (and others per your allowlist).

## SSE Live Stream

Connect to `/bridge/live?token=YOUR_API_KEY` for real-time server events via Server-Sent Events. Updates every 3 seconds with player count, recent events, and state changes.

**Limits:** Maximum 4 concurrent SSE connections.

## Security Notes

- API key comparison is constant-time (timing-attack safe)
- POST bodies limited to 64KB
- Socket timeouts: 15s send, 5s receive
- CORS headers included for browser access

## Connecting Your GameCP

Set these environment variables in your GameCP website:
```
BRIDGE_URL=http://127.0.0.1:8081
BRIDGE_API_KEY=<your-api-key-from-zonemod.json>
```
