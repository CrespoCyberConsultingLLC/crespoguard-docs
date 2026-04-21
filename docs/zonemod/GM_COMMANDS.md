# GM & Player Command Reference

Complete reference of every chat command available in CrespoGuard ZoneMod scripts.

---

## Overview

- **GM commands** require `gmDegree >= 2` (configurable per module via `gm_grade`)
- Commands are typed in **circle chat** (local chat) with a `.` prefix
- Commands are **consumed** -- they are not visible to other players
- The `%zonemod` prefix uses the cheat command system (requires GM degree >= 1)
- **Player commands** use the `!` (exclamation) prefix (e.g., `!buffs`, `!cleanse`) and require no GM rank
- All GM commands support `.prefix help` for inline documentation

---

## Standard Commands Pattern

Most GM modules share the same four subcommands:

| Subcommand | Usage                       | Description                       |
| ---------- | --------------------------- | --------------------------------- |
| `status`   | `.prefix status`            | Show current settings and state   |
| `set`      | `.prefix set <key> <value>` | Change a setting at runtime       |
| `reload`   | `.prefix reload`            | Reload config from dashboard/file |
| `help`     | `.prefix help`              | Show available commands           |

Modules that **only** use these standard commands are listed in the [Standard Modules Table](#standard-modules-table) below. Modules with additional unique commands have their own detailed sections.

---

## GM Toolkit (`.gm`)

The core GM administration suite. Requires `gmDegree >= 2`.

**Script:** `admin/gm_toolkit.js`

| Command         | Arguments                         | Description                                                                  |
| --------------- | --------------------------------- | ---------------------------------------------------------------------------- |
| `.gm info`      | `<name>`                          | Show detailed player info (level, race, HP/FP/SP, position, guild, currency) |
| `.gm heal`      | `<name>`                          | Fully heal a player (HP/FP/SP to max)                                        |
| `.gm healall`   |                                   | Heal all online players with server broadcast                                |
| `.gm kill`      | `<name>`                          | Set target player's HP to 0                                                  |
| `.gm tp`        | `<name>`                          | Teleport yourself to a player                                                |
| `.gm bring`     | `<name>`                          | Teleport a player to your position                                           |
| `.gm item`      | `<name> <table> <index> <count>`  | Give an item to a player                                                     |
| `.gm dalant`    | `<name> <amount>`                 | Give or remove dalant (negative values supported)                            |
| `.gm gold`      | `<name> <amount>`                 | Give or remove gold (negative values supported)                              |
| `.gm exp`       | `<name> <amount>`                 | Give EXP to a player                                                         |
| `.gm level`     | `<name> <level>`                  | Set a player's level (1-65)                                                  |
| `.gm kick`      | `<name> [reason]`                 | Kick a player with optional reason                                           |
| `.gm broadcast` | `<message>`                       | Send a server-wide `[GM]` announcement                                       |
| `.gm online`    |                                   | List all online players with race breakdown                                  |
| `.gm race`      | `<0\|1\|2>`                       | List online players by race (0=Bellato, 1=Cora, 2=Accretia)                  |
| `.gm buff`      | `<name> <code> <idx> <dur> <lvl>` | Apply a buff to a player                                                     |
| `.gm unbuff`    | `<name> <effectIndex>`            | Remove a buff by effect index                                                |
| `.gm spawn`     | `<monsterCode> <count>`           | Spawn monsters at your position (max 20)                                     |
| `.gm pos`       |                                   | Show your current position and map code                                      |
| `.gm help`      |                                   | List all GM Toolkit commands                                                 |

---

## Cheat Command System (`%zonemod`)

Uses the engine's `%` cheat command system. Requires GM degree >= 1.

**Script:** `gm_commands.js`

| Command               | Arguments                                                | Description                                    |
| --------------------- | -------------------------------------------------------- | ---------------------------------------------- |
| `%zonemod status`     |                                                          | Show your level, HP, map, and position         |
| `%zonemod online`     |                                                          | Show online player count                       |
| `%zonemod say`        | `<message>`                                              | Broadcast a `[GM]` message to all players      |
| `%zonemod kick`       | `<name>`                                                 | Kick a player by name                          |
| `%zonemod reload`     |                                                          | Prompt for server console script reload        |
| `%zonemod help`       |                                                          | List available subcommands                     |
| `%zonemod modules`    |                                                          | List loaded modules and their status           |
| `%zonemod version`    |                                                          | Show ZoneMod version                           |
| `%zonemod debug`      | `<tag\|all\|off\|status>`                                | Runtime debug log control                      |
| `%zonemod info`       | `<player>`                                               | Show player info (level, HP, dalant, position) |
| `%zonemod goto`       | `<player>`                                               | Teleport GM to target player                   |
| `%zonemod summon`     | `<player>`                                               | Teleport target player to GM                   |
| `%zonemod broadcast`  | `<msg>`                                                  | Server-wide broadcast message                  |
| `%zonemod event`      | `start/stop/status`                                      | Server event management                        |
| `%zonemod eventset`   | `start/stop <name>`                                      | Monster EventSet.ini control                   |
| `%zonemod chipwar`    | `start/prestart/stop/state`                              | Chip war control                               |
| `%zonemod nametag`    | `set/clear/list/broadcast`                               | Nametag color management                       |
| `%zonemod portal`     | `list/redirect/race/level/disable/enable/setspawn/clone` | Portal control                                 |
| `%zonemod mastery`    | `max/set <player>`                                       | Mastery grant commands                         |
| `%zonemod archon`     | `set/remove/clear/list`                                  | Archon appointment system                      |
| `%zonemod skin`       | `equip/set/clear/apply/list/info/lookup`                 | Skin management                                |
| `%zonemod js`         | `reload/list`                                            | JS script management                           |
| `%zonemod questreset` | `<player> <code>`                                        | Quest history reset                            |
| `%zonemod questlist`  | `<player>`                                               | Quest history dump                             |

---

## Modules with Unique Commands

These modules have commands beyond the standard status/set/reload/help pattern.

### Buff Tracker (`.buff`)

**Script:** `buff_tracker.js`

| Command          | Arguments                              | Description                                  |
| ---------------- | -------------------------------------- | -------------------------------------------- |
| `.buff list`     | `<name>`                               | Show all active buffs on a player            |
| `.buff add`      | `<name> <code> <idx> <dur> <lvl> [ch]` | Apply a buff to a player                     |
| `.buff remove`   | `<name> <effectIndex>`                 | Remove a specific buff from a player         |
| `.buff clearall` | `<name>`                               | Remove all non-protected buffs from a player |
| `.buff reload`   |                                        | Reload buff tracker config                   |
| `.buff help`     |                                        | Show available commands                      |

### Drop Rate Configuration (`.drop`)

**Script:** `drop_rate_config.js`

| Command        | Arguments               | Description                                                 |
| -------------- | ----------------------- | ----------------------------------------------------------- |
| `.drop status` |                         | Show global/boss rates, happy hour, lucky player, map rates |
| `.drop set`    | `<global\|boss> <mult>` | Set global or boss drop multiplier at runtime               |
| `.drop lucky`  | `<name> <minutes>`      | Grant lucky drop boost to a specific player                 |
| `.drop reload` |                         | Reload config from dashboard                                |

### Movement Speed Configuration (`.speed`)

**Script:** `speed_config.js`

| Command         | Arguments                 | Description                                                        |
| --------------- | ------------------------- | ------------------------------------------------------------------ |
| `.speed status` |                           | Show global/race/map multipliers, level scaling, VIP, sprint state |
| `.speed set`    | `<mult>`                  | Set global GM override multiplier (0.1-10.0)                       |
| `.speed boost`  | `<name> <mult> <seconds>` | Give a temporary speed boost to a player                           |
| `.speed reload` |                           | Reload config from dashboard                                       |

### Monster Configuration (`.mob`)

**Script:** `monster_config.js`

| Command       | Arguments       | Description                                                                                                                                             |
| ------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `.mob status` |                 | Show all global/boss/aggro/skill multipliers and level ranges                                                                                           |
| `.mob set`    | `<stat> <mult>` | Set: `hp`, `atk`, `def`, `speed`, `range`, `aggroreset`, `aggroshort`, `skilldelay`, `spprob`, `bosshp`, `bossatk`, `bossdef`, `bossspeed`, `bossrange` |
| `.mob info`   | `<monsterCode>` | Look up monster data by code                                                                                                                            |
| `.mob reload` |                 | Reload config from dashboard                                                                                                                            |

### Chat Moderation (`.chat`)

**Script:** `chat/chat_moderation.js`

| Command        | Arguments          | Description                   |
| -------------- | ------------------ | ----------------------------- |
| `.chat mute`   | `<name> <minutes>` | Mute a player                 |
| `.chat unmute` | `<name>`           | Unmute a player               |
| `.chat status` |                    | Show chat moderation settings |
| `.chat reload` |                    | Reload config                 |

### Advert System (`.advert`)

**Script:** `advert.js`

| Command          | Arguments   | Description                             |
| ---------------- | ----------- | --------------------------------------- |
| `.advert status` |             | Show active advertisements and schedule |
| `.advert add`    | `<message>` | Add a new broadcast advertisement       |
| `.advert clear`  |             | Clear all advertisements                |
| `.advert reload` |             | Reload config                           |

### Loot Name Replacement (`.lootname`)

**Script:** `replace_loot_name.js`

| Command            | Arguments       | Description                         |
| ------------------ | --------------- | ----------------------------------- |
| `.lootname list`   |                 | List current loot name replacements |
| `.lootname set`    | `<code> <name>` | Set a loot name replacement         |
| `.lootname reload` |                 | Reload config                       |

### Event Commands (Start/Stop)

These event modules have `.start` and `.stop` subcommands in addition to status/reload.

| Prefix      | Script                 | Unique Commands                                                                |
| ----------- | ---------------------- | ------------------------------------------------------------------------------ |
| `.boss`     | `events/boss_spawn.js` | `.boss spawn` (trigger spawn at your position), `.boss kill` (kill event boss) |
| `.dblexp`   | `events/double_exp.js` | `.dblexp start [duration_min]`, `.dblexp stop`                                 |
| `.racewar`  | `events/race_war.js`   | `.racewar start`, `.racewar stop` (also shows scores in status)                |
| `.minevent` | `mining_event.js`      | `.minevent start`, `.minevent stop`                                            |
| `.trapwar`  | `traps/trap_war.js`    | `.trapwar start`, `.trapwar stop`                                              |

### Player Management Commands

These modules have commands for checking/resetting individual player data.

| Prefix         | Script                      | Unique Commands                                                               |
| -------------- | --------------------------- | ----------------------------------------------------------------------------- |
| `.dlogin`      | `daily_login.js`            | `.dlogin check <name>`, `.dlogin reset <name>`                                |
| `.daily`       | `economy/daily_rewards.js`  | `.daily reset <name>` (also has status/reload)                                |
| `.levelreward` | `level_rewards.js`          | `.levelreward list`, `.levelreward check <name>`, `.levelreward reset <name>` |
| `.farm`        | `anti_farm.js`              | `.farm check <name>` (also has status/reload)                                 |
| `.newbie`      | `newbie_protection.js`      | `.newbie resetkit <name>` (also has reload)                                   |
| `.playtime`    | `utility/online_tracker.js` | `.playtime check <name>`, `.playtime reset <name>`, `.playtime top`           |
| `.npcvisits`   | `npc/npc_greetings.js`      | `.npcvisits <name>` (check NPC visit history)                                 |
| `.customquest` | `events/quest_system.js`    | `.customquest list` (also has set/reload)                                     |

---

## Standard Modules Table

All modules below use the standard command pattern: `.prefix status`, `.prefix set <key> <value>`, `.prefix reload`, `.prefix help`. Some omit `set` or `help` where not applicable.

### Rate & Multiplier Configuration

| Prefix       | Module               | Script                   | Notable Config Keys                                                                                               |
| ------------ | -------------------- | ------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| `.exp`       | EXP Config           | `exp_config.js`          | `rate` (global kill EXP multiplier)                                                                               |
| `.crit`      | Critical Hit Config  | `crit_config.js`         | `rate`, `damage`, `cap`, `pvp`                                                                                    |
| `.aspd`      | Attack Speed Config  | `attack_speed_config.js` | `mult` (delay multiplier, lower = faster)                                                                         |
| `.mastery`   | Mastery Config       | `mastery_config.js`      | `mastery_mult`, `mult_bellato`, `mult_cora`, `mult_accretia`, `mastery_cap`, `bypass_level_gate`, `bypass_usable` |
| `.stat`      | Player Stat Config   | `stat_config.js`         | `gain`, `hp`, `fp`, `sp`, `dp`, `cap`                                                                             |
| `.regen`     | Regeneration Config  | `regen_config.js`        | `global`, `hp`, `fp`, `sp`, `combat` (on/off), `sitting`, `dp`                                                    |
| `.potcfg`    | Potion Config        | `potion_config.js`       | `global`, `hp`, `fp`, `sp`, `cooldown`, `maxpm`                                                                   |
| `.questrate` | Quest Rate Config    | `quest_rates.js`         | Quest rate multipliers                                                                                            |
| `.skillcost` | Skill Cost Config    | `skill_cost_config.js`   | Skill cost multipliers                                                                                            |
| `.mau`       | MAU/Animus EXP       | `mau_exp.js`             | MAU EXP parameters                                                                                                |
| `.maxlvl`    | Max Level Config     | `max_level_config.js`    | Max level parameters                                                                                              |
| `.expdiff`   | EXP Level Difference | `exp_level_diff.js`      | Level difference EXP penalty                                                                                      |
| `.currency`  | Currency Config      | `currency_config.js`     | Dalant, gold, PvP cash multipliers                                                                                |
| `.accuracy`  | Accuracy Effect      | `accuracy_effect.js`     | Accuracy/dodge multipliers                                                                                        |
| `.ap`        | Action Point         | `action_point.js`        | Action point multipliers                                                                                          |
| `.resell`    | Resell Config        | `resell_config.js`       | `mult` (resell price multiplier)                                                                                  |
| `.shop`      | Custom Shop Prices   | `custom_shop_prices.js`  | Shop price overrides                                                                                              |
| `.recover`   | Recovery Config      | `recover_config.js`      | Recovery rate parameters                                                                                          |
| `.balance`   | Race Balance         | `race_balance.js`        | Per-race balance multipliers                                                                                      |

### Monster & Combat Configuration

| Prefix      | Module               | Script                        | Notable Config Keys                                                |
| ----------- | -------------------- | ----------------------------- | ------------------------------------------------------------------ |
| `.aggro`    | Monster Aggro        | `monster_aggro_config.js`     | Aggro range and timing                                             |
| `.mobdef`   | Monster Defense      | `monster_defense_config.js`   | Monster defense multipliers                                        |
| `.mobelem`  | Monster Elemental    | `monster_elemental_config.js` | Elemental tolerance                                                |
| `.mobskill` | Monster Skill        | `monster_skill_config.js`     | Skill probability and delay                                        |
| `.mobsp`    | Monster SP Action    | `monster_sp_config.js`        | SP action settings                                                 |
| `.bosshp`   | Boss HP Scaling      | `boss_hp_scale.js`            | Scaling curve, online count (status/reload only)                   |
| `.combat`   | Player Combat Config | `player_combat_config.js`     | PvP/PvE damage multipliers                                         |
| `.def`      | Defence Formula      | `defence_formula.js`          | Defence formula parameters                                         |
| `.pvpcfg`   | PvP Config           | `pvp_config.js`               | `points`, `cash`, `damage`, `chaos` (on/off), `minlevel`, `streak` |
| `.pvppot`   | PvP Potion           | `pvp_potion.js`               | PvP potion parameters                                              |
| `.safezone` | Safe Zone Config     | `pvp_zone.js`                 | Safe zone parameters                                               |
| `.pvpzone`  | PvP Zone Config      | `pvp/pvp_zones.js`            | PvP zone parameters                                                |

### Equipment & Items

| Prefix        | Module              | Script                 | Notable Config Keys                                                                                                                               |
| ------------- | ------------------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `.durability` | Durability Config   | `durability_config.js` | `loss_mult`, `pvp_loss_mult`, `pve_loss_mult`, `newbie_max_level`, `newbie_loss_mult`, `low_level_max`, `low_level_loss_mult`, `notify_threshold` |
| `.autoloot`   | Auto-Loot Config    | `auto_loot_config.js`  | `stack`, `scatter`, `scatterBoss`, `skipBoss`, `mau`, `siege`, `enabled`, `delay`, `radius`, `notify`                                             |
| `.scatter`    | Radius Drop Scatter | `radius_drop_loot.js`  | Scatter radius parameters                                                                                                                         |

### Buff & Effect Configuration

| Prefix     | Module               | Script                    | Notable Config Keys                         |
| ---------- | -------------------- | ------------------------- | ------------------------------------------- |
| `.buffdur` | Buff Duration Config | `buff_duration_config.js` | Buff duration multipliers                   |
| `.cwbuff`  | Chipwar Buff Config  | `chipwar_buff_config.js`  | Chip war buff parameters                    |
| `.npcbuff` | NPC Buff Config      | `npc_buff.js`             | NPC buff zone settings (status/reload only) |
| `.ipots`   | Infinity Potions     | `infinity_pots.js`        | Infinity potion parameters                  |

### Map & Zone Configuration

| Prefix      | Module             | Script                  | Notable Config Keys                                                                           |
| ----------- | ------------------ | ----------------------- | --------------------------------------------------------------------------------------------- |
| `.stone`    | Holy Stone Config  | `stone_hp.js`           | `stone_hp/atk/def`, `stone_fire/water/soil/wind`, `keeper_hp/atk/def/range`, `chipwar_result` |
| `.building` | Building Config    | `building_config.js`    | Building stat multipliers                                                                     |
| `.portal`   | Portal Restriction | `portal_restriction.js` | Portal restriction parameters                                                                 |
| `.minecfg`  | Mining Config      | `mine_config.js`        | Mining rate multipliers                                                                       |
| `.trapstat` | Trap Stat Config   | `trap_stat_config.js`   | Trap stat multipliers                                                                         |
| `.animus`   | Animus Config      | `animus_config.js`      | Animus stat multipliers                                                                       |
| `.animstat` | Animus Stat Config | `animus_stat_config.js` | Detailed animus stat multipliers                                                              |

### Death & Respawn

| Prefix     | Module          | Script               | Notable Config Keys                                                                                      |
| ---------- | --------------- | -------------------- | -------------------------------------------------------------------------------------------------------- |
| `.respawn` | Respawn Control | `respawn_control.js` | `pve_penalty_mult`, `pvp_penalty_mult`, `newbie_max_level`, `resurrect_cooldown_ms`, `respawn_timer_sec` |

### Events & Economy

| Prefix     | Module             | Script                      | Notable Config Keys               |
| ---------- | ------------------ | --------------------------- | --------------------------------- |
| `.wkndexp` | Weekend Double EXP | `event_double_exp.js`       | Weekend double EXP parameters     |
| `.cw`      | Chipwar Rewards    | `events/chipwar_rewards.js` | Chip war reward parameters        |
| `.bonus`   | Bonus Start        | `bonus_start.js`            | New-server bonus event parameters |
| `.econ`    | Economy Config     | `economy.js`                | Gold sinks, trade tax, etc.       |
| `.tax`     | Tax System         | `economy/tax_system.js`     | Tax rates and revenue             |
| `.auction` | Auction Config     | `auction_config.js`         | Auction house settings            |

### Guild Management

| Prefix       | Module              | Script                    | Notable Config Keys       |
| ------------ | ------------------- | ------------------------- | ------------------------- |
| `.guildcfg`  | Guild Config        | `guild_config.js`         | Guild parameters          |
| `.gskill`    | Guild Skill         | `guild_skill.js`          | Guild skill parameters    |
| `.gactivity` | Guild Activity      | `guild/guild_activity.js` | Guild activity tracking   |
| `.guild`     | Guild Treasury (GM) | `guild/guild_treasury.js` | Guild treasury parameters |

### Announcements & Chat

| Prefix       | Module             | Script                          | Notable Config Keys              |
| ------------ | ------------------ | ------------------------------- | -------------------------------- |
| `.bossann`   | Boss Announcements | `boss_announce.js`              | Boss announcement parameters     |
| `.enchann`   | Enchant Announcer  | `enchant_announcer.js`          | Enchant announcement parameters  |
| `.killann`   | Kill Announcements | `chat/kill_announcements.js`    | Kill announcement parameters     |
| `.gchat`     | Global Chat Config | `chat/global_chat_welcome.js`   | Global chat welcome settings     |
| `.tradechat` | Trade Chat Logger  | `chat/trade_chat_logger.js`     | Trade chat logging parameters    |
| `.discount`  | Shop Discount      | `npc/shop_discount_announce.js` | Discount announcement parameters |
| `.report`    | Stat Report        | `stat_report.js`                | Server stat report parameters    |

### Moderation & Protection

| Prefix       | Module            | Script                      | Notable Config Keys                   |
| ------------ | ----------------- | --------------------------- | ------------------------------------- |
| `.afk`       | Anti-AFK          | `anti_afk.js`               | AFK detection parameters              |
| `.trades`    | Trade Logger      | `trade_logger.js`           | Trade logging parameters              |
| `.timelimit` | Time Limit Config | `time_limit_config.js`      | Play time limit parameters            |
| `.buddy`     | Buddy Tracker     | `utility/buddy_tracker.js`  | `.buddy stats [name]` (status/reload) |
| `.webhook`   | Webhook Alerts    | `utility/webhook_alerts.js` | Webhook alert parameters              |

### Misc Configuration

| Prefix       | Module           | Script                          | Notable Config Keys               |
| ------------ | ---------------- | ------------------------------- | --------------------------------- |
| `.trapalert` | Trap Alerts      | `traps/trap_alerts.js`          | Trap alert parameters             |
| `.election`  | Election Monitor | `patriarch/election_monitor.js` | Election state and candidates     |
| `.rovenpc`   | Roving NPC       | `npc/roving_npc.js`             | Roving NPC positions and schedule |
| `.wskin`     | Weapon Skins     | `skins/weapon_skins.js`         | Weapon skin parameters            |
| `.kit`       | Starter Kit      | `welcome/starter_kit.js`        | Starter kit parameters            |
| `.party`     | Party Config     | `party_config.js`               | Party EXP distribution            |
| `.partylvl`  | Party Level Diff | `party_level_diff_config.js`    | Party level difference settings   |

---

## Player Commands (All Players)

These commands are available to **all players** (no GM degree required). Player commands use the `!` (exclamation) prefix in circle chat and are consumed from chat.

### Basic Info Commands

**Script:** `player_commands.js`

| Command   | Description                             |
| --------- | --------------------------------------- |
| `!online` | Show number of online players           |
| `!stats`  | Show your level, HP, FP, and dalant     |
| `!pvp`    | Show your PvP points and PvP cash       |
| `!where`  | Show your current map code and position |
| `!help`   | List available player commands          |

### Buff Management

| Command        | Script            | Description                                                       |
| -------------- | ----------------- | ----------------------------------------------------------------- |
| `!buffs`       | `buff_tracker.js` | List all your active buffs with details                           |
| `!buffs clear` | `buff_tracker.js` | Remove active debuffs (with cooldown)                             |
| `!cleanse`     | `buff_cleanse.js` | Remove specific cleansable buffs (5s cooldown, blocked in combat) |

### Movement

| Command   | Script            | Description                                                 |
| --------- | ----------------- | ----------------------------------------------------------- |
| `!sprint` | `speed_config.js` | Activate temporary speed boost (+50% for 10s, 60s cooldown) |

### PvP & Bounty

**Script:** `pvp/pvp_rewards_extended.js`

| Command             | Description                          |
| ------------------- | ------------------------------------ |
| `!kdr`              | Show your kill/death ratio and stats |
| `!bounty list`      | View active bounties                 |
| `!bounty top`       | Show top bounty targets              |
| `!bounty territory` | Show territory control status        |

### PvP Arena

**Script:** `pvp/pvp_zones.js`

| Command         | Description                      |
| --------------- | -------------------------------- |
| `!arena join`   | Join the PvP arena queue         |
| `!arena leave`  | Leave the PvP arena queue        |
| `!arena status` | Show arena state and queue count |

### Economy & Items

| Command           | Script                    | Description                           |
| ----------------- | ------------------------- | ------------------------------------- |
| `!sell`           | `loot_exchange.js`        | Sell loot items for currency          |
| `!exchange`       | `loot_exchange.js`        | Exchange items via configured recipes |
| `!gbank balance`  | `guild/guild_treasury.js` | Check your guild's bank balance       |
| `!gbank deposit`  | `guild/guild_treasury.js` | Deposit funds into the guild bank     |
| `!gbank withdraw` | `guild/guild_treasury.js` | Withdraw funds from the guild bank    |
| `!gbank history`  | `guild/guild_treasury.js` | View recent guild bank transactions   |

### Progression & Tracking

| Command          | Script                       | Description                                 |
| ---------------- | ---------------------------- | ------------------------------------------- |
| `!quest stats`   | `quest/quest_tracker.js`     | View your quest completion statistics       |
| `!quest active`  | `quest/quest_tracker.js`     | Show your currently active quests           |
| `!quest help`    | `quest/quest_tracker.js`     | List quest-related commands                 |
| `!mine stats`    | `mining/mining_rewards.js`   | View your mining statistics and rewards     |
| `!loginstreak`   | `daily_login.js`             | Check your current login streak and rewards |
| `!playtime`      | `utility/online_tracker.js`  | Check your total playtime                   |
| `!potion status` | `potions/infinity_potion.js` | Check your infinity potion status           |

---

## Quick Reference: All Command Prefixes

### GM Commands

| Prefix         | Module                  |
| -------------- | ----------------------- |
| `.gm`          | GM Toolkit              |
| `%zonemod`     | Cheat Commands          |
| `.exp`         | EXP Config              |
| `.crit`        | Critical Hit Config     |
| `.drop`        | Drop Rate Config        |
| `.aspd`        | Attack Speed Config     |
| `.speed`       | Movement Speed Config   |
| `.mastery`     | Mastery Config          |
| `.stat`        | Player Stat Config      |
| `.regen`       | Regeneration Config     |
| `.potcfg`      | Potion Config           |
| `.questrate`   | Quest Rate Config       |
| `.skillcost`   | Skill Cost Config       |
| `.mau`         | MAU/Animus EXP          |
| `.maxlvl`      | Max Level Config        |
| `.expdiff`     | EXP Level Difference    |
| `.currency`    | Currency Config         |
| `.accuracy`    | Accuracy Effect         |
| `.ap`          | Action Point            |
| `.resell`      | Resell Config           |
| `.shop`        | Custom Shop Prices      |
| `.recover`     | Recovery Config         |
| `.balance`     | Race Balance            |
| `.mob`         | Monster Config          |
| `.aggro`       | Monster Aggro           |
| `.mobdef`      | Monster Defense         |
| `.mobelem`     | Monster Elemental       |
| `.mobskill`    | Monster Skill           |
| `.mobsp`       | Monster SP Action       |
| `.bosshp`      | Boss HP Scaling         |
| `.combat`      | Player Combat Config    |
| `.def`         | Defence Formula         |
| `.pvpcfg`      | PvP Config              |
| `.pvppot`      | PvP Potion              |
| `.safezone`    | Safe Zone Config        |
| `.pvpzone`     | PvP Zone Config         |
| `.durability`  | Durability Config       |
| `.autoloot`    | Auto-Loot Config        |
| `.lootname`    | Loot Name Replace       |
| `.scatter`     | Radius Drop Scatter     |
| `.buff`        | Buff Tracker (GM)       |
| `.buffdur`     | Buff Duration Config    |
| `.cwbuff`      | Chipwar Buff Config     |
| `.npcbuff`     | NPC Buff Config         |
| `.ipots`       | Infinity Potions Config |
| `.stone`       | Holy Stone Config       |
| `.building`    | Building Config         |
| `.portal`      | Portal Restriction      |
| `.minecfg`     | Mining Config           |
| `.trapstat`    | Trap Stat Config        |
| `.animus`      | Animus Config           |
| `.animstat`    | Animus Stat Config      |
| `.respawn`     | Respawn Control         |
| `.wkndexp`     | Weekend Double EXP      |
| `.dblexp`      | Double EXP Event        |
| `.boss`        | Boss Spawn Event        |
| `.racewar`     | Race War Event          |
| `.cw`          | Chipwar Rewards         |
| `.minevent`    | Mining Event            |
| `.bonus`       | Bonus Start Event       |
| `.econ`        | Economy Config          |
| `.tax`         | Tax System              |
| `.auction`     | Auction Config          |
| `.dlogin`      | Daily Login (GM)        |
| `.daily`       | Daily Rewards (GM)      |
| `.levelreward` | Level Rewards           |
| `.guildcfg`    | Guild Config            |
| `.gskill`      | Guild Skill             |
| `.gactivity`   | Guild Activity          |
| `.guild`       | Guild Treasury (GM)     |
| `.advert`      | Advert System           |
| `.bossann`     | Boss Announcements      |
| `.enchann`     | Enchant Announcer       |
| `.killann`     | Kill Announcements      |
| `.chat`        | Chat Moderation         |
| `.gchat`       | Global Chat Config      |
| `.tradechat`   | Trade Chat Logger       |
| `.discount`    | Shop Discount           |
| `.report`      | Stat Report             |
| `.afk`         | Anti-AFK                |
| `.farm`        | Anti-Farm               |
| `.newbie`      | Newbie Protection       |
| `.trades`      | Trade Logger            |
| `.timelimit`   | Time Limit Config       |
| `.playtime`    | Playtime (GM check)     |
| `.buddy`       | Buddy Tracker           |
| `.webhook`     | Webhook Alerts          |
| `.trapwar`     | Trap War Event          |
| `.trapalert`   | Trap Alerts             |
| `.customquest` | Custom Quest System     |
| `.election`    | Election Monitor        |
| `.rovenpc`     | Roving NPC              |
| `.npcvisits`   | NPC Visit Stats         |
| `.wskin`       | Weapon Skins            |
| `.kit`         | Starter Kit             |
| `.party`       | Party Config            |
| `.partylvl`    | Party Level Diff        |

### Player Commands

| Prefix         | Module           |
| -------------- | ---------------- |
| `!online`      | Player Count     |
| `!stats`       | Player Stats     |
| `!pvp`         | PvP Stats        |
| `!where`       | Location Info    |
| `!help`        | Help             |
| `!buffs`       | Buff List/Clear  |
| `!cleanse`     | Buff Cleanse     |
| `!sprint`      | Speed Boost      |
| `!kdr`         | Kill/Death Ratio |
| `!bounty`      | Bounty System    |
| `!sell`        | Loot Sell        |
| `!exchange`    | Item Exchange    |
| `!quest`       | Quest Tracking   |
| `!mine`        | Mining Stats     |
| `!gbank`       | Guild Banking    |
| `!loginstreak` | Login Streak     |
| `!arena`       | PvP Arena        |
| `!playtime`    | Playtime Check   |
| `!potion`      | Potion Status    |
