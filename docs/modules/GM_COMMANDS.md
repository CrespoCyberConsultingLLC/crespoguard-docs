# GM & Player Command Reference

Complete reference of every chat command available in CrespoGuard ZoneMod scripts.

---

## Overview

- **GM commands** require `gmDegree >= 2` (configurable per module via `gm_grade`)
- Commands are typed in **circle chat** (local chat) with a `.` prefix
- Commands are **consumed** — they are not visible to other players
- The `%zonemod` prefix uses the cheat command system (requires GM degree >= 1)
- All GM commands support `.prefix help` for inline documentation

---

## GM Toolkit (`.gm`)

The core GM administration suite. Requires `gmDegree >= 2`.

**Script:** `admin/gm_toolkit.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.gm info` | `<name>` | Show detailed player info (level, race, HP/FP/SP, position, guild, currency) |
| `.gm heal` | `<name>` | Fully heal a player (HP/FP/SP to max) |
| `.gm healall` | | Heal all online players with server broadcast |
| `.gm kill` | `<name>` | Set target player's HP to 0 |
| `.gm tp` | `<name>` | Teleport yourself to a player |
| `.gm bring` | `<name>` | Teleport a player to your position |
| `.gm item` | `<name> <table> <index> <count>` | Give an item to a player |
| `.gm dalant` | `<name> <amount>` | Give or remove dalant (negative values supported) |
| `.gm gold` | `<name> <amount>` | Give or remove gold (negative values supported) |
| `.gm exp` | `<name> <amount>` | Give EXP to a player |
| `.gm level` | `<name> <level>` | Set a player's level (1-65) |
| `.gm kick` | `<name> [reason]` | Kick a player with optional reason |
| `.gm broadcast` | `<message>` | Send a server-wide `[GM]` announcement |
| `.gm online` | | List all online players with race breakdown |
| `.gm race` | `<0\|1\|2>` | List online players by race (0=Bellato, 1=Cora, 2=Accretia) |
| `.gm buff` | `<name> <code> <idx> <dur> <lvl>` | Apply a buff to a player |
| `.gm unbuff` | `<name> <effectIndex>` | Remove a buff by effect index |
| `.gm spawn` | `<monsterCode> <count>` | Spawn monsters at your position (max 20) |
| `.gm pos` | | Show your current position and map code |
| `.gm help` | | List all GM Toolkit commands |

---

## Cheat Command System (`%zonemod`)

Uses the engine's `%` cheat command system. Requires GM degree >= 1.

**Script:** `gm_commands.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `%zonemod status` | | Show your level, HP, map, and position |
| `%zonemod online` | | Show online player count |
| `%zonemod say` | `<message>` | Broadcast a `[GM]` message to all players |
| `%zonemod kick` | `<name>` | Kick a player by name |
| `%zonemod reload` | | Prompt for server console script reload |
| `%zonemod help` | | List available subcommands |

---

## Rate & Multiplier Configuration

### EXP Configuration (`.exp`)

**Script:** `exp_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.exp status` | | Show all EXP multipliers, level curve, race/map rates |
| `.exp set` | `<rate>` | Set global kill EXP multiplier at runtime |
| `.exp reload` | | Reload config from dashboard (resets runtime overrides) |
| `.exp help` | | Show available commands |

### Critical Hit Configuration (`.crit`)

**Script:** `crit_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.crit status` | | Show crit rate, damage, cap, PvP modifier, race rates |
| `.crit set` | `<stat> <value>` | Set: `rate`, `damage`, `cap`, `pvp` |
| `.crit reload` | | Reload config from dashboard |
| `.crit help` | | Show available commands |

### Drop Rate Configuration (`.drop`)

**Script:** `drop_rate_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.drop status` | | Show global/boss rates, happy hour, lucky player, map rates |
| `.drop set` | `<global\|boss> <mult>` | Set global or boss drop multiplier at runtime |
| `.drop lucky` | `<name> <minutes>` | Grant lucky drop boost to a specific player |
| `.drop reload` | | Reload config from dashboard |
| `.drop help` | | Show available commands |

### Attack Speed Configuration (`.aspd`)

**Script:** `attack_speed_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.aspd status` | | Show delay multiplier, equip speed, clamps, race adjustments |
| `.aspd set` | `<mult>` | Set global delay multiplier (lower = faster) |
| `.aspd reload` | | Reload config from dashboard |
| `.aspd help` | | Show available commands |

### Movement Speed Configuration (`.speed`)

**Script:** `speed_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.speed status` | | Show global/race/map multipliers, level scaling, VIP, sprint state |
| `.speed set` | `<mult>` | Set global GM override multiplier (0.1-10.0) |
| `.speed boost` | `<name> <mult> <seconds>` | Give a temporary speed boost to a player |
| `.speed reload` | | Reload config from dashboard |

### Mastery Configuration (`.mastery`)

**Script:** `mastery_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.mastery status` | | Show global/race multipliers, cap, bypass flags |
| `.mastery set` | `<setting> <value>` | Change: `mastery_mult`, `mult_bellato`, `mult_cora`, `mult_accretia`, `mastery_cap`, `bypass_level_gate`, `bypass_usable` |
| `.mastery reload` | | Reload config from file |
| `.mastery help` | | Show available commands |

### Player Stat Configuration (`.stat`)

**Script:** `stat_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.stat status` | | Show stat gain, HP/FP/SP/DP multipliers, cap, level curve |
| `.stat set` | `<stat> <value>` | Set: `gain`, `hp`, `fp`, `sp`, `dp`, `cap` |
| `.stat reload` | | Reload config from dashboard |
| `.stat help` | | Show available commands |

### Regeneration Configuration (`.regen`)

**Script:** `regen_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.regen status` | | Show global/HP/FP/SP rates, combat regen, sitting bonus, DP, level scaling |
| `.regen set` | `<stat> <value>` | Set: `global`, `hp`, `fp`, `sp`, `combat` (on/off), `sitting`, `dp` |
| `.regen reload` | | Reload config from dashboard |
| `.regen help` | | Show available commands |

### Potion Configuration (`.potcfg`)

**Script:** `potion_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.potcfg status` | | Show global/HP/FP/SP multipliers, cooldown, max per minute, level curve |
| `.potcfg set` | `<stat> <value>` | Set: `global`, `hp`, `fp`, `sp`, `cooldown`, `maxpm` |
| `.potcfg reload` | | Reload config from dashboard |
| `.potcfg help` | | Show available commands |

### Quest Rate Configuration (`.questrate`)

**Script:** `quest_rates.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.questrate status` | | Show current quest rate multipliers |
| `.questrate set` | `<stat> <value>` | Set quest rate parameters at runtime |
| `.questrate reload` | | Reload config from dashboard |
| `.questrate help` | | Show available commands |

### Skill Cost Configuration (`.skillcost`)

**Script:** `skill_cost_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.skillcost status` | | Show current skill cost multipliers |
| `.skillcost set` | `<stat> <value>` | Set skill cost parameters at runtime |
| `.skillcost reload` | | Reload config from dashboard |
| `.skillcost help` | | Show available commands |

### MAU/Animus EXP (`.mau`)

**Script:** `mau_exp.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.mau status` | | Show MAU/animus EXP multiplier settings |
| `.mau set` | `<stat> <value>` | Set MAU EXP parameters at runtime |
| `.mau reload` | | Reload config from dashboard |
| `.mau help` | | Show available commands |

### Max Level Configuration (`.maxlvl`)

**Script:** `max_level_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.maxlvl status` | | Show current max level settings |
| `.maxlvl set` | `<stat> <value>` | Set max level parameters |
| `.maxlvl reload` | | Reload config from dashboard |
| `.maxlvl help` | | Show available commands |

### EXP Level Difference (`.expdiff`)

**Script:** `exp_level_diff.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.expdiff status` | | Show level difference EXP penalty settings |
| `.expdiff set` | `<stat> <value>` | Set EXP difference parameters |
| `.expdiff reload` | | Reload config from dashboard |
| `.expdiff help` | | Show available commands |

### Currency Configuration (`.currency`)

**Script:** `currency_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.currency status` | | Show currency multiplier settings (dalant, gold, PvP cash) |
| `.currency set` | `<stat> <value>` | Set currency parameters at runtime |
| `.currency reload` | | Reload config from dashboard |
| `.currency help` | | Show available commands |

### Accuracy Effect (`.accuracy`)

**Script:** `accuracy_effect.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.accuracy status` | | Show current accuracy/dodge multiplier settings |
| `.accuracy set` | `<stat> <value>` | Set accuracy parameters at runtime |
| `.accuracy reload` | | Reload config from dashboard |
| `.accuracy help` | | Show available commands |

### Action Point (`.ap`)

**Script:** `action_point.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.ap status` | | Show action point multiplier settings |
| `.ap set` | `<stat> <value>` | Set action point parameters at runtime |
| `.ap reload` | | Reload config from dashboard |
| `.ap help` | | Show available commands |

### Resell Configuration (`.resell`)

**Script:** `resell_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.resell status` | | Show resell price multiplier |
| `.resell set` | `<mult>` | Set resell multiplier at runtime |
| `.resell reload` | | Reload config from dashboard |

### Custom Shop Prices (`.shop`)

**Script:** `custom_shop_prices.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.shop status` | | Show shop price override settings |
| `.shop set` | `<stat> <value>` | Set shop price parameters |
| `.shop reload` | | Reload config from dashboard |
| `.shop help` | | Show available commands |

### Recovery Configuration (`.recover`)

**Script:** `recover_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.recover status` | | Show recovery rate settings |
| `.recover set` | `<stat> <value>` | Set recovery parameters at runtime |
| `.recover reload` | | Reload config from dashboard |
| `.recover help` | | Show available commands |

### Race Balance (`.balance`)

**Script:** `race_balance.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.balance status` | | Show per-race balance multipliers |
| `.balance set` | `<stat> <value>` | Set race balance parameters at runtime |
| `.balance reload` | | Reload config from dashboard |
| `.balance help` | | Show available commands |

---

## Monster & Combat Configuration

### Monster Configuration (`.mob`)

**Script:** `monster_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.mob status` | | Show all global/boss/aggro/skill multipliers and level ranges |
| `.mob set` | `<stat> <mult>` | Set: `hp`, `atk`, `def`, `speed`, `range`, `aggroreset`, `aggroshort`, `skilldelay`, `spprob`, `bosshp`, `bossatk`, `bossdef`, `bossspeed`, `bossrange` |
| `.mob info` | `<monsterCode>` | Look up monster data by code |
| `.mob reload` | | Reload config from dashboard |
| `.mob help` | | Show available commands |

### Monster Sub-Config Commands

| Prefix | Subcommands | Script | Description |
|--------|-------------|--------|-------------|
| `.aggro` | `status`, `set`, `reload`, `help` | `monster_aggro_config.js` | Monster aggro range and timing |
| `.mobdef` | `status`, `set`, `reload`, `help` | `monster_defense_config.js` | Monster defense multipliers |
| `.mobelem` | `status`, `set`, `reload`, `help` | `monster_elemental_config.js` | Monster elemental tolerance |
| `.mobskill` | `status`, `set`, `reload`, `help` | `monster_skill_config.js` | Monster skill probability and delay |
| `.mobsp` | `status`, `set`, `reload`, `help` | `monster_sp_config.js` | Monster SP action settings |

### Boss HP Scaling (`.bosshp`)

**Script:** `boss_hp_scale.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.bosshp status` | | Show scaling curve, online count, current scale, overrides |
| `.bosshp reload` | | Reload config from dashboard |
| `.bosshp help` | | Show available commands |

### Player Combat Configuration (`.combat`)

**Script:** `player_combat_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.combat status` | | Show PvP/PvE damage multipliers |
| `.combat set` | `<stat> <value>` | Set combat parameters at runtime |
| `.combat reload` | | Reload config from dashboard |
| `.combat help` | | Show available commands |

### Defence Formula (`.def`)

**Script:** `defence_formula.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.def status` | | Show defence formula parameters |
| `.def set` | `<setting> <value>` | Change defence formula values |
| `.def reload` | | Reload config from file |

### PvP Configuration (`.pvpcfg`)

**Script:** `pvp_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.pvpcfg status` | | Show point/cash/damage multipliers, chaos mode, kill stats |
| `.pvpcfg set` | `<stat> <value>` | Set: `points`, `cash`, `damage`, `chaos` (on/off), `minlevel`, `streak` |
| `.pvpcfg reload` | | Reload config from dashboard |
| `.pvpcfg help` | | Show available commands |

### PvP Potion (`.pvppot`)

**Script:** `pvp_potion.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.pvppot status` | | Show PvP potion settings |
| `.pvppot set` | `<stat> <value>` | Set PvP potion parameters |
| `.pvppot reload` | | Reload config |

### Safe Zone / PvP Zone (`.safezone`, `.pvpzone`)

**Scripts:** `pvp_zone.js`, `pvp/pvp_zones.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.safezone status` | | Show safe zone configuration |
| `.safezone set` | `<stat> <value>` | Set safe zone parameters |
| `.safezone reload` | | Reload config |
| `.pvpzone status` | | Show PvP zone configuration |
| `.pvpzone set` | `<stat> <value>` | Set PvP zone parameters |
| `.pvpzone reload` | | Reload config |

---

## Equipment & Items

### Durability Configuration (`.durability`)

**Script:** `durability_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.durability status` | | Show loss multipliers, PvP/PvE rates, newbie/low-level thresholds |
| `.durability set` | `<setting> <value>` | Change: `loss_mult`, `pvp_loss_mult`, `pve_loss_mult`, `newbie_max_level`, `newbie_loss_mult`, `low_level_max`, `low_level_loss_mult`, `notify_threshold` |
| `.durability reload` | | Reload config from file |
| `.durability help` | | Show available commands |

### Auto-Loot Configuration (`.autoloot`)

**Script:** `auto_loot_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.autoloot status` | | Show stack limit, scatter, boss skip, MAU, siege, delay, radius |
| `.autoloot set` | `<key> <value>` | Set: `stack`, `scatter`, `scatterBoss`, `skipBoss`, `mau`, `siege`, `enabled`, `delay`, `radius`, `notify` |
| `.autoloot reload` | | Reload config from dashboard |
| `.autoloot help` | | Show available commands and settings |

### Loot Name Replacement (`.lootname`)

**Script:** `replace_loot_name.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.lootname list` | | List current loot name replacements |
| `.lootname set` | `<code> <name>` | Set a loot name replacement |
| `.lootname reload` | | Reload config |

### Radius Drop / Scatter (`.scatter`)

**Script:** `radius_drop_loot.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.scatter status` | | Show scatter radius settings |
| `.scatter set` | `<stat> <value>` | Set scatter parameters |
| `.scatter reload` | | Reload config |

---

## Buff & Effect Management

### Buff Tracker (`.buff` GM prefix)

**Script:** `buff_tracker.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.buff list` | `<name>` | Show all active buffs on a player |
| `.buff add` | `<name> <code> <idx> <dur> <lvl> [ch]` | Apply a buff to a player |
| `.buff remove` | `<name> <effectIndex>` | Remove a specific buff from a player |
| `.buff clearall` | `<name>` | Remove all non-protected buffs from a player |
| `.buff reload` | | Reload buff tracker config |
| `.buff help` | | Show available commands |

### Buff Duration Configuration (`.buffdur`)

**Script:** `buff_duration_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.buffdur status` | | Show buff duration multiplier settings |
| `.buffdur set` | `<stat> <value>` | Set buff duration parameters |
| `.buffdur reload` | | Reload config from dashboard |
| `.buffdur help` | | Show available commands |

### Chipwar Buff Configuration (`.cwbuff`)

**Script:** `chipwar_buff_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.cwbuff status` | | Show chip war buff settings |
| `.cwbuff set` | `<stat> <value>` | Set chip war buff parameters |
| `.cwbuff reload` | | Reload config from dashboard |
| `.cwbuff help` | | Show available commands |

### NPC Buff (`.npcbuff`)

**Script:** `npc_buff.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.npcbuff status` | | Show NPC buff zone settings |
| `.npcbuff reload` | | Reload config |

### Infinity Potions (`.ipots`)

**Script:** `infinity_pots.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.ipots status` | | Show infinity potion settings |
| `.ipots set` | `<stat> <value>` | Set infinity potion parameters |
| `.ipots reload` | | Reload config |
| `.ipots help` | | Show available commands |

---

## Map & Zone Configuration

### Holy Stone Configuration (`.stone`)

**Script:** `stone_hp.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.stone status` | | Show stone/keeper multipliers, chip war result, destruction stats |
| `.stone set` | `<stat> <mult>` | Set: `stone_hp`, `stone_atk`, `stone_def`, `stone_fire/water/soil/wind`, `keeper_hp`, `keeper_atk`, `keeper_range`, `keeper_def`, `keeper_fire/water/soil/wind`, `chipwar_result` |
| `.stone reload` | | Reload config from dashboard |
| `.stone help` | | Show available commands and stat list |

### Building Configuration (`.building`)

**Script:** `building_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.building status` | | Show building stat multipliers |
| `.building set` | `<stat> <value>` | Set building parameters |
| `.building reload` | | Reload config from dashboard |
| `.building help` | | Show available commands |

### Portal Restriction (`.portal`)

**Script:** `portal_restriction.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.portal status` | | Show portal restriction settings |
| `.portal set` | `<stat> <value>` | Set portal parameters |
| `.portal reload` | | Reload config |
| `.portal help` | | Show available commands |

### Mining Configuration (`.minecfg`)

**Script:** `mine_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.minecfg status` | | Show mining rate multipliers |
| `.minecfg set` | `<stat> <value>` | Set mining parameters |
| `.minecfg reload` | | Reload config from dashboard |
| `.minecfg help` | | Show available commands |

### Trap Stat Configuration (`.trapstat`)

**Script:** `trap_stat_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.trapstat status` | | Show trap stat multipliers |
| `.trapstat set` | `<stat> <value>` | Set trap stat parameters |
| `.trapstat reload` | | Reload config |

### Animus Configuration (`.animus`)

**Script:** `animus_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.animus status` | | Show animus stat multipliers |
| `.animus set` | `<stat> <value>` | Set animus parameters |
| `.animus reload` | | Reload config from dashboard |
| `.animus help` | | Show available commands |

### Animus Stat Configuration (`.animstat`)

**Script:** `animus_stat_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.animstat status` | | Show detailed animus stat multipliers |
| `.animstat set` | `<stat> <value>` | Set animus stat parameters |
| `.animstat reload` | | Reload config from dashboard |
| `.animstat help` | | Show available commands |

---

## Death & Respawn

### Respawn Control (`.respawn`)

**Script:** `respawn_control.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.respawn status` | | Show PvE/PvP penalty, newbie/low-level thresholds, cooldowns |
| `.respawn set` | `<setting> <value>` | Change: `pve_penalty_mult`, `pvp_penalty_mult`, `newbie_max_level`, `newbie_penalty_mult`, `low_level_max`, `low_level_penalty_mult`, `resurrect_cooldown_ms`, `respawn_timer_sec` |
| `.respawn reload` | | Reload config from file |
| `.respawn help` | | Show available commands |

---

## Events & Economy

### Double EXP Weekend (`.wkndexp`)

**Script:** `event_double_exp.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.wkndexp status` | | Show weekend double EXP event settings |
| `.wkndexp set` | `<stat> <value>` | Set double EXP parameters |
| `.wkndexp reload` | | Reload config |
| `.wkndexp help` | | Show available commands |

### Double EXP Event (`.dblexp`)

**Script:** `events/double_exp.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.dblexp status` | | Show double EXP event state |
| `.dblexp start` | `[duration_min]` | Start a double EXP event |
| `.dblexp stop` | | End the current double EXP event |
| `.dblexp reload` | | Reload config |

### Boss Spawn Event (`.boss`)

**Script:** `events/boss_spawn.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.boss spawn` | | Manually trigger a boss spawn at your position |
| `.boss kill` | | Kill the current event boss |
| `.boss status` | | Show boss event state and timer |
| `.boss reload` | | Reload config |

### Race War Event (`.racewar`)

**Script:** `events/race_war.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.racewar status` | | Show race war event state and scores |
| `.racewar start` | | Start a race war event |
| `.racewar stop` | | End the race war event |
| `.racewar reload` | | Reload config |

### Chipwar Rewards (`.cw`)

**Script:** `events/chipwar_rewards.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.cw status` | | Show chip war reward settings |
| `.cw set` | `<stat> <value>` | Set chip war reward parameters |
| `.cw reload` | | Reload config |

### Mining Event (`.minevent`)

**Script:** `mining_event.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.minevent status` | | Show mining event state |
| `.minevent start` | | Start a mining event |
| `.minevent stop` | | End the mining event |
| `.minevent reload` | | Reload config |

### Bonus Start (`.bonus`)

**Script:** `bonus_start.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.bonus status` | | Show new-server bonus event settings |
| `.bonus set` | `<stat> <value>` | Set bonus parameters |
| `.bonus reload` | | Reload config |
| `.bonus help` | | Show available commands |

### Economy Configuration (`.econ`)

**Script:** `economy.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.econ status` | | Show economy settings (gold sinks, trade tax, etc.) |
| `.econ set` | `<stat> <value>` | Set economy parameters |
| `.econ reload` | | Reload config |
| `.econ help` | | Show available commands |

### Tax System (`.tax`)

**Script:** `economy/tax_system.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.tax status` | | Show tax rates and revenue |
| `.tax set` | `<stat> <value>` | Set tax parameters |
| `.tax reload` | | Reload config |

### Auction Configuration (`.auction`)

**Script:** `auction_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.auction status` | | Show auction house settings |
| `.auction set` | `<stat> <value>` | Set auction parameters |
| `.auction reload` | | Reload config |
| `.auction help` | | Show available commands |

### Daily Login Rewards (`.dlogin`)

**Script:** `daily_login.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.dlogin check` | `<name>` | Check a player's login streak |
| `.dlogin reset` | `<name>` | Reset a player's login streak |
| `.dlogin reload` | | Reload config |

### Daily Rewards (`.daily`)

**Script:** `economy/daily_rewards.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.daily status` | | Show daily reward settings |
| `.daily reset` | `<name>` | Reset a player's daily reward |
| `.daily reload` | | Reload config |

### Level Rewards (`.levelreward`)

**Script:** `level_rewards.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.levelreward list` | | List all configured level rewards |
| `.levelreward check` | `<name>` | Check a player's claimed rewards |
| `.levelreward reset` | `<name>` | Reset a player's reward claims |
| `.levelreward reload` | | Reload config |
| `.levelreward help` | | Show available commands |

---

## Guild Management

### Guild Configuration (`.guildcfg`)

**Script:** `guild_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.guildcfg status` | | Show guild configuration settings |
| `.guildcfg set` | `<setting> <value>` | Change guild parameters |
| `.guildcfg reload` | | Reload config |

### Guild Skill (`.gskill`)

**Script:** `guild_skill.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.gskill status` | | Show guild skill settings |
| `.gskill set` | `<setting> <value>` | Change guild skill parameters |
| `.gskill reload` | | Reload config |

### Guild Activity (`.gactivity`)

**Script:** `guild/guild_activity.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.gactivity status` | | Show guild activity tracking stats |
| `.gactivity set` | `<stat> <value>` | Set guild activity parameters |
| `.gactivity reload` | | Reload config |

### Guild Treasury (`.guild`)

**Script:** `guild/guild_treasury.js` (GM prefix)

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.guild status` | | Show guild treasury system settings |
| `.guild set` | `<stat> <value>` | Set guild treasury parameters |
| `.guild reload` | | Reload config |

---

## Announcements & Chat

### Advert System (`.advert`)

**Script:** `advert.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.advert status` | | Show active advertisements and schedule |
| `.advert add` | `<message>` | Add a new broadcast advertisement |
| `.advert clear` | | Clear all advertisements |
| `.advert reload` | | Reload config |

### Boss Announcements (`.bossann`)

**Script:** `boss_announce.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.bossann status` | | Show boss announcement settings |
| `.bossann set` | `<stat> <value>` | Set announcement parameters |
| `.bossann reload` | | Reload config |
| `.bossann help` | | Show available commands |

### Enchant Announcer (`.enchann`)

**Script:** `enchant_announcer.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.enchann status` | | Show enchant announcement settings |
| `.enchann set` | `<stat> <value>` | Set announcement parameters |
| `.enchann reload` | | Reload config |

### Kill Announcements (`.killann`)

**Script:** `chat/kill_announcements.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.killann status` | | Show kill announcement settings |
| `.killann set` | `<stat> <value>` | Set announcement parameters |
| `.killann reload` | | Reload config |

### Chat Moderation (`.chat`)

**Script:** `chat/chat_moderation.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.chat mute` | `<name> <minutes>` | Mute a player |
| `.chat unmute` | `<name>` | Unmute a player |
| `.chat status` | | Show chat moderation settings |
| `.chat reload` | | Reload config |

### Global Chat (`.gchat`)

**Script:** `chat/global_chat_welcome.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.gchat status` | | Show global chat welcome settings |
| `.gchat set` | `<stat> <value>` | Set global chat parameters |
| `.gchat reload` | | Reload config |

### Trade Chat Logger (`.tradechat`)

**Script:** `chat/trade_chat_logger.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.tradechat status` | | Show trade chat logging settings |
| `.tradechat set` | `<stat> <value>` | Set trade chat parameters |
| `.tradechat reload` | | Reload config |

### Shop Discount Announcements (`.discount`)

**Script:** `npc/shop_discount_announce.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.discount status` | | Show discount announcement settings |
| `.discount set` | `<stat> <value>` | Set discount parameters |
| `.discount reload` | | Reload config |

### Stat Report (`.report`)

**Script:** `stat_report.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.report status` | | Show server stat report settings |
| `.report set` | `<stat> <value>` | Set report parameters |
| `.report reload` | | Reload config |

---

## Moderation & Protection

### Anti-AFK (`.afk`)

**Script:** `anti_afk.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.afk status` | | Show AFK detection settings and active AFK players |
| `.afk set` | `<stat> <value>` | Set AFK parameters |
| `.afk reload` | | Reload config |
| `.afk help` | | Show available commands |

### Anti-Farm (`.farm`)

**Script:** `anti_farm.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.farm status` | | Show farm detection settings and flagged players |
| `.farm check` | `<name>` | Check a player's farm detection stats |
| `.farm reload` | | Reload config |

### Newbie Protection (`.newbie`)

**Script:** `newbie_protection.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.newbie reload` | | Reload newbie protection config |
| `.newbie resetkit` | `<name>` | Reset a player's starter kit claim |

### Trade Logger (`.trades`)

**Script:** `trade_logger.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.trades status` | | Show trade logging settings and recent trades |
| `.trades set` | `<stat> <value>` | Set trade logging parameters |
| `.trades reload` | | Reload config |

### Time Limit Configuration (`.timelimit`)

**Script:** `time_limit_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.timelimit status` | | Show play time limit settings |
| `.timelimit set` | `<stat> <value>` | Set time limit parameters |
| `.timelimit reload` | | Reload config |

### Playtime Tracking (`.playtime` GM)

**Script:** `utility/online_tracker.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.playtime check` | `<name>` | Check a specific player's total playtime |
| `.playtime reset` | `<name>` | Reset a player's playtime data |
| `.playtime top` | | Show top players by playtime |
| `.playtime reload` | | Reload config |
| `.playtime help` | | Show available commands |

### Buddy Tracker (`.buddy` GM)

**Script:** `utility/buddy_tracker.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.buddy stats` | `[name]` | View buddy/friend system stats |
| `.buddy reload` | | Reload config |

### Webhook Alerts (`.webhook`)

**Script:** `utility/webhook_alerts.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.webhook status` | | Show webhook alert settings |
| `.webhook set` | `<stat> <value>` | Set webhook parameters |
| `.webhook reload` | | Reload config |

---

## Event-Specific GM Commands

### Trap War (`.trapwar`)

**Script:** `traps/trap_war.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.trapwar status` | | Show trap war event state |
| `.trapwar start` | | Start a trap war event |
| `.trapwar stop` | | End the trap war event |
| `.trapwar reload` | | Reload config |

### Trap Alerts (`.trapalert`)

**Script:** `traps/trap_alerts.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.trapalert status` | | Show trap alert settings |
| `.trapalert set` | `<stat> <value>` | Set trap alert parameters |
| `.trapalert reload` | | Reload config |

### Custom Quest System (`.customquest`)

**Script:** `events/quest_system.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.customquest list` | | List all custom quests |
| `.customquest set` | `<stat> <value>` | Set quest parameters |
| `.customquest reload` | | Reload config |

### Patriarch Election Monitor (`.election`)

**Script:** `patriarch/election_monitor.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.election status` | | Show election state and candidates |
| `.election set` | `<stat> <value>` | Set election parameters |
| `.election reload` | | Reload config |

### Roving NPC (`.rovenpc`)

**Script:** `npc/roving_npc.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.rovenpc status` | | Show roving NPC positions and schedule |
| `.rovenpc set` | `<stat> <value>` | Set roving NPC parameters |
| `.rovenpc reload` | | Reload config |

### NPC Visit Tracking (`.npcvisits`)

**Script:** `npc/npc_greetings.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.npcvisits` | `<name>` | Check a player's NPC visit history |

### Weapon Skins (`.wskin`)

**Script:** `skins/weapon_skins.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.wskin status` | | Show weapon skin settings |
| `.wskin set` | `<stat> <value>` | Set weapon skin parameters |
| `.wskin reload` | | Reload config |

### Starter Kit (`.kit`)

**Script:** `welcome/starter_kit.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.kit status` | | Show starter kit settings |
| `.kit set` | `<stat> <value>` | Set starter kit parameters |
| `.kit reload` | | Reload config |

### Party Configuration (`.party`)

**Script:** `party_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.party status` | | Show party EXP distribution settings |
| `.party set` | `<stat> <value>` | Set party parameters |
| `.party reload` | | Reload config |
| `.party help` | | Show available commands |

### Party Level Difference (`.partylvl`)

**Script:** `party_level_diff_config.js`

| Command | Arguments | Description |
|---------|-----------|-------------|
| `.partylvl status` | | Show party level difference settings |
| `.partylvl set` | `<stat> <value>` | Set party level diff parameters |
| `.partylvl reload` | | Reload config |

---

## Player Commands (All Players)

These commands are available to **all players** (no GM degree required). They are consumed from chat.

### Basic Info Commands

**Script:** `player_commands.js`

| Command | Description |
|---------|-------------|
| `.online` | Show number of online players |
| `.stats` | Show your level, HP, FP, and dalant |
| `.pvp` | Show your PvP points and PvP cash |
| `.where` | Show your current map code and position |
| `.help` | List available player commands |

### Buff Management

**Script:** `buff_tracker.js`

| Command | Description |
|---------|-------------|
| `.buffs` | List all your active buffs with details |
| `.buffs clear` | Remove active debuffs (with cooldown) |

### Buff Cleanse

**Script:** `buff_cleanse.js`

| Command | Description |
|---------|-------------|
| `.cleanse` | Remove specific cleansable buffs (5s cooldown, blocked in combat) |

### Sprint

**Script:** `speed_config.js`

| Command | Description |
|---------|-------------|
| `.sprint` | Activate temporary speed boost (+50% for 10s, 60s cooldown) |

### PvP & Bounty

**Script:** `pvp/pvp_rewards_extended.js`

| Command | Description |
|---------|-------------|
| `.kdr` | Show your kill/death ratio and stats |
| `.bounty list` | View active bounties |
| `.bounty top` | Show top bounty targets |
| `.bounty territory` | Show territory control status |

### Loot Exchange

**Script:** `loot_exchange.js`

| Command | Description |
|---------|-------------|
| `.sell` | Sell loot items for currency |
| `.exchange` | Exchange items via configured recipes |

### Quest Tracking

**Script:** `quest/quest_tracker.js`

| Command | Description |
|---------|-------------|
| `.quest stats` | View your quest completion statistics |
| `.quest active` | Show your currently active quests |
| `.quest help` | List quest-related commands |

### Mining

**Script:** `mining/mining_rewards.js`

| Command | Description |
|---------|-------------|
| `.mine stats` | View your mining statistics and rewards |

### Guild Banking

**Script:** `guild/guild_treasury.js`

| Command | Description |
|---------|-------------|
| `.gbank balance` | Check your guild's bank balance |
| `.gbank deposit` | Deposit funds into the guild bank |
| `.gbank withdraw` | Withdraw funds from the guild bank |
| `.gbank history` | View recent guild bank transactions |

### Daily Login

**Script:** `daily_login.js`

| Command | Description |
|---------|-------------|
| `.loginstreak` | Check your current login streak and rewards |

### PvP Arena

**Script:** `pvp/pvp_zones.js`

| Command | Description |
|---------|-------------|
| `.arena join` | Join the PvP arena queue |
| `.arena leave` | Leave the PvP arena queue |
| `.arena status` | Show arena state and queue count |

### Playtime

**Script:** `utility/online_tracker.js`

| Command | Description |
|---------|-------------|
| `.playtime` | Check your total playtime |

### Potion System

**Script:** `potions/infinity_potion.js`

| Command | Description |
|---------|-------------|
| `.potion status` | Check your infinity potion status |

---

## Quick Reference: All Command Prefixes

| Prefix | Module | Type |
|--------|--------|------|
| `.gm` | GM Toolkit | GM |
| `%zonemod` | Cheat Commands | GM |
| `.exp` | EXP Config | GM |
| `.crit` | Critical Hit Config | GM |
| `.drop` | Drop Rate Config | GM |
| `.aspd` | Attack Speed Config | GM |
| `.speed` | Movement Speed Config | GM |
| `.mastery` | Mastery Config | GM |
| `.stat` | Player Stat Config | GM |
| `.regen` | Regeneration Config | GM |
| `.potcfg` | Potion Config | GM |
| `.questrate` | Quest Rate Config | GM |
| `.skillcost` | Skill Cost Config | GM |
| `.mau` | MAU/Animus EXP | GM |
| `.maxlvl` | Max Level Config | GM |
| `.expdiff` | EXP Level Difference | GM |
| `.currency` | Currency Config | GM |
| `.accuracy` | Accuracy Effect | GM |
| `.ap` | Action Point | GM |
| `.resell` | Resell Config | GM |
| `.shop` | Custom Shop Prices | GM |
| `.recover` | Recovery Config | GM |
| `.balance` | Race Balance | GM |
| `.mob` | Monster Config | GM |
| `.aggro` | Monster Aggro | GM |
| `.mobdef` | Monster Defense | GM |
| `.mobelem` | Monster Elemental | GM |
| `.mobskill` | Monster Skill | GM |
| `.mobsp` | Monster SP Action | GM |
| `.bosshp` | Boss HP Scaling | GM |
| `.combat` | Player Combat Config | GM |
| `.def` | Defence Formula | GM |
| `.pvpcfg` | PvP Config | GM |
| `.pvppot` | PvP Potion | GM |
| `.safezone` | Safe Zone Config | GM |
| `.pvpzone` | PvP Zone Config | GM |
| `.durability` | Durability Config | GM |
| `.autoloot` | Auto-Loot Config | GM |
| `.lootname` | Loot Name Replace | GM |
| `.scatter` | Radius Drop Scatter | GM |
| `.buff` | Buff Tracker (GM) | GM |
| `.buffdur` | Buff Duration Config | GM |
| `.cwbuff` | Chipwar Buff Config | GM |
| `.npcbuff` | NPC Buff Config | GM |
| `.ipots` | Infinity Potions Config | GM |
| `.stone` | Holy Stone Config | GM |
| `.building` | Building Config | GM |
| `.portal` | Portal Restriction | GM |
| `.minecfg` | Mining Config | GM |
| `.trapstat` | Trap Stat Config | GM |
| `.animus` | Animus Config | GM |
| `.animstat` | Animus Stat Config | GM |
| `.respawn` | Respawn Control | GM |
| `.wkndexp` | Weekend Double EXP | GM |
| `.dblexp` | Double EXP Event | GM |
| `.boss` | Boss Spawn Event | GM |
| `.racewar` | Race War Event | GM |
| `.cw` | Chipwar Rewards | GM |
| `.minevent` | Mining Event | GM |
| `.bonus` | Bonus Start Event | GM |
| `.econ` | Economy Config | GM |
| `.tax` | Tax System | GM |
| `.auction` | Auction Config | GM |
| `.dlogin` | Daily Login (GM) | GM |
| `.daily` | Daily Rewards (GM) | GM |
| `.levelreward` | Level Rewards | GM |
| `.guildcfg` | Guild Config | GM |
| `.gskill` | Guild Skill | GM |
| `.gactivity` | Guild Activity | GM |
| `.guild` | Guild Treasury (GM) | GM |
| `.advert` | Advert System | GM |
| `.bossann` | Boss Announcements | GM |
| `.enchann` | Enchant Announcer | GM |
| `.killann` | Kill Announcements | GM |
| `.chat` | Chat Moderation | GM |
| `.gchat` | Global Chat Config | GM |
| `.tradechat` | Trade Chat Logger | GM |
| `.discount` | Shop Discount | GM |
| `.report` | Stat Report | GM |
| `.afk` | Anti-AFK | GM |
| `.farm` | Anti-Farm | GM |
| `.newbie` | Newbie Protection | GM |
| `.trades` | Trade Logger | GM |
| `.timelimit` | Time Limit Config | GM |
| `.playtime` | Playtime (GM check) | GM |
| `.buddy` | Buddy Tracker | GM |
| `.webhook` | Webhook Alerts | GM |
| `.trapwar` | Trap War Event | GM |
| `.trapalert` | Trap Alerts | GM |
| `.customquest` | Custom Quest System | GM |
| `.election` | Election Monitor | GM |
| `.rovenpc` | Roving NPC | GM |
| `.npcvisits` | NPC Visit Stats | GM |
| `.wskin` | Weapon Skins | GM |
| `.kit` | Starter Kit | GM |
| `.party` | Party Config | GM |
| `.partylvl` | Party Level Diff | GM |
| `.online` | Player Count | Player |
| `.stats` | Player Stats | Player |
| `.pvp` | PvP Stats | Player |
| `.where` | Location Info | Player |
| `.help` | Help | Player |
| `.buffs` | Buff List/Clear | Player |
| `.cleanse` | Buff Cleanse | Player |
| `.sprint` | Speed Boost | Player |
| `.kdr` | Kill/Death Ratio | Player |
| `.bounty` | Bounty System | Player |
| `.sell` | Loot Sell | Player |
| `.exchange` | Item Exchange | Player |
| `.quest` | Quest Tracking | Player |
| `.mine` | Mining Stats | Player |
| `.gbank` | Guild Banking | Player |
| `.loginstreak` | Login Streak | Player |
| `.arena` | PvP Arena | Player |
| `.playtime` | Playtime Check | Player |
| `.potion` | Potion Status | Player |
