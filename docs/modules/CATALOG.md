# Module Catalog

Complete reference of every CrespoGuard ZoneMod module available for your server. All modules listed below are production-ready and included with your license.

!!! tip "How Modules Work"

    - **Dashboard-configurable** -- all settings managed from the web UI or `crespoguard.json`, no code editing needed
    - **Hot-reload** -- configuration changes auto-apply within **5 seconds**, no server restart required
    - **GM commands** -- every module exposes `/zonemod <module>` commands for runtime control in-game
    - **Independent toggle** -- enable or disable each module individually without affecting others
    - **Security modules** are force-enabled and cannot be turned off (marked :lock:)

!!! info "Command Syntax"

    All GM commands use the prefix `/zonemod`. For example, `/zonemod exp info` shows current EXP multipliers. Commands shown in tables below omit the `/zonemod` prefix for brevity.

!!! warning "Player Commands"

    Some modules expose commands for regular players (e.g., `/daily`, `/stats`, `/bounty`). These are noted in the module descriptions below. Player commands do **not** use the `/zonemod` prefix.

### Module Count by Category

| Category | Modules | Category | Modules |
|----------|:-------:|----------|:-------:|
| Combat & PvP | 9 | Economy & Trade | 9 |
| Events & Scheduling | 9 | Player Progression | 12 |
| Guild System | 5 | Monster & PvE | 10 |
| Skills & Buffs | 8 | Mining & Resources | 4 |
| Chat & Communication | 10 | Cosmetics | 3 |
| Admin Tools | 8 | Maps & Portals | 4 |
| Structures | 4 | Units | 4 |
| Party | 2 | Quests | 3 |
| Misc | 10 | **Total** | **114** |

---

## 1. Combat & PvP

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **kill_rewards** | Grant items/EXP/currency on player kills | `kill_rewards reload`, `status` |
| **pvp_config** | Master PvP toggle, damage scaling, faction multipliers | `pvp_config set <k> <v>`, `info` |
| **pvp_zones** | Define custom PvP-enabled map regions with boundaries | `pvp_zones list`, `add`, `remove` |
| **pvp_rewards_extended** | Streak bonuses, bounty system, K/D ratio rewards | `pvp_rewards status`, `reset <player>` |
| **pvp_potion** | Restrict or allow specific potions during PvP | `pvp_potion list`, `block <code>` |
| **player_combat_config** | PvP damage caps, minimum floors, cooldowns | `player_combat set <k> <v>` |
| **crit_config** | Critical hit rate and damage multiplier per class | `crit_config set <class> <rate> <dmg>` |
| **attack_speed_config** | Attack speed caps and per-weapon-type modifiers | `attack_speed set <type> <cap>` |
| **defence_formula** | Override defense-to-damage-reduction calculation | `defence_formula set <mode>` |

**kill_rewards** -- Configurable reward tables by level range and race matchup. Anti-farm protection with diminishing returns on repeated kills. Supports item codes, gold, CP, and EXP.

**pvp_config** -- Global PvP damage multiplier (0.1x--10.0x). Per-race and per-class scaling overrides. Toggle friendly-fire, cross-race, and siege damage separately.

**pvp_zones** -- Define rectangular or circular PvP regions with coordinates. Per-zone rules: FFA, faction-only, or guild-war. Entry/exit notifications for players.

**pvp_rewards_extended** -- Kill streak multipliers at configurable thresholds (3/5/10/20+). Auto-bounty on high-streak players. Player command: `/bounty`.

**pvp_potion** -- Whitelist/blacklist potions by item code. Separate rules for open-world PvP vs Chip War vs Guild Battle. Cooldown overrides for PvP-only usage.

**player_combat_config** -- Max/min damage caps per level bracket. Damage reduction for large level gaps. Separate tuning for melee, ranged, and force.

**crit_config** -- Per-class critical rate bonus/penalty. Damage multiplier configurable 1.0x--5.0x. Option to cap maximum crit rate percentage.

**attack_speed_config** -- Global and per-weapon speed caps. Buff stacking rules. Separate caps for PvP and PvE contexts.

**defence_formula** -- Linear, logarithmic, or custom curve modes. Per-level effectiveness scaling. Separate physical and force defense tuning.

---

## 2. Economy & Trade

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **tax_system** | Taxes on NPC sales, trades, and auctions | `tax set <type> <rate>`, `info` |
| **currency_config** | Custom currency types, conversion rates, display names | `currency list`, `set <name> <rate>` |
| **economy** | Global gold multipliers, inflation controls, sink mechanics | `economy set <k> <v>`, `info` |
| **custom_shop_prices** | Override NPC buy/sell prices per item or category | `shop_prices set <code> <buy> <sell>` |
| **resell_config** | Player resale prices and vendor buyback rates | `resell set <grade> <percent>` |
| **auction_config** | Auction house fees, listing limits, duration | `auction set <k> <v>` |
| **loot_exchange** | NPC that converts loot items into currency or items | `loot_exchange list`, `reload` |
| **trade_logger** | Log all player-to-player trades with full details | `trade_logger search <player>` |
| **custom_combine** | Custom item combination recipes beyond default crafting | `combine list`, `add`, `remove` |

**tax_system** -- Configurable rates for NPC purchases, trades, and auction sales. Per-race tax revenue tracking. Exempt specific items or ranks.

**currency_config** -- Create named currencies backed by item codes. Conversion rates between gold, CP, and custom currencies. Custom display names in notifications.

**economy** -- Global gold drop and NPC sale multipliers. Gold sink events and inflation monitoring. Automatic dampening thresholds.

**custom_shop_prices** -- Per-item buy/sell price overrides. Category-wide multipliers (weapons, armor, consumables). Race-specific faction shop pricing.

**resell_config** -- Global resale percentage (1--100%). Per-item-grade multipliers (normal, rare, unique, epic). Disable resale for specific categories.

**auction_config** -- Listing fee percentage and fixed minimum. Max concurrent listings per player. Duration options and auto-expiry.

**loot_exchange** -- Exchange recipes: N items of type A = 1 item of type B. Tiered exchanges supported. Per-map exchange NPC.

**trade_logger** -- Logs both sides of every trade: items, gold, quantities. Webhook integration. Searchable via dashboard.

**custom_combine** -- Multi-input recipes with specific output. Success rate per recipe (0--100%). Optional gold cost and level requirement.

---

## 3. Events & Scheduling

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **server_event** | Schedule timed server-wide events with auto start/stop | `event list`, `start <name>`, `stop` |
| **race_war** | Race War rewards, timing, participation rules | `racewar info`, `reward set` |
| **boss_spawn** | Schedule and announce world boss spawns | `boss_spawn list`, `force <id>` |
| **chipwar_rewards** | Chip War victory and participation reward tables | `chipwar set <tier> <reward>` |
| **trap_war** | Custom trap-based PvP event with scoring | `trapwar start`, `stop`, `score` |
| **mining_event** | Boosted mining with rare ore spawn increases | `mining_event start <map>`, `stop` |
| **double_exp_event** | Scheduled EXP multiplier windows | `double_exp start <mult> <min>` |
| **weekend_exp_bonus** | Automatic EXP boost on configurable days | `weekend_exp set <day> <bonus>` |
| **shop_discount** | Timed NPC shop discount events | `shop_discount start <pct> <min>` |

**server_event** -- Cron-style scheduling. Chain multiple effects per event (EXP + drops + announcement). Dashboard calendar view.

**race_war** -- Victory/defeat/participation reward tiers. Duration, cooldown, minimum participants. Bonus for outnumbered factions.

**boss_spawn** -- Cron-based timers with randomization window. Server-wide spawn announcements. HP scaling by online player count.

**chipwar_rewards** -- Separate rewards for winners, losers, and MVPs. Contribution-based tiers. Consecutive victory bonuses.

**trap_war** -- Teams score by holding trapped zones. Configurable trap damage and point values. Auto-scoring and leaderboard.

**mining_event** -- Increase ore spawn rates and rare ore probability. Per-map activation. Auto start/stop with announcements.

**double_exp_event** -- Any custom multiplier value. Scheduled or manual trigger with duration. Stacks with other EXP bonuses.

**weekend_exp_bonus** -- Select bonus days and hours. Per-day bonus percentage. Configurable start/end times.

**shop_discount** -- Global or per-shop percentage. Scheduled or manual. Player notifications on start/end.

---

## 4. Player Progression

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **exp_config** | Global EXP multipliers for combat, quests, mining | `exp set <type> <mult>`, `info` |
| **exp_level_diff** | EXP penalty/bonus by monster-player level gap | `exp_diff set <gap> <pct>` |
| **level_rewards** | Auto rewards at milestone levels | `level_rewards list`, `set` |
| **mastery_config** | Mastery gain rates and skill unlock overrides | `mastery set <tree> <rate>` |
| **max_level_config** | Override max player and class levels | `max_level set <type> <lvl>` |
| **stat_config** | Base stat growth and per-class modifiers | `stat_config set <cls> <stat> <v>` |
| **daily_rewards** | Login reward calendar with escalating prizes | `daily_rewards reload`, `reset` |
| **daily_login** | Consecutive login streak tracking and bonuses | `daily_login info`, `reset` |
| **bonus_start** | Starting bonus items/gold for new characters | `bonus_start set <race> <items>` |
| **newbie_protection** | PvP immunity and damage reduction for low levels | `newbie set <level>` |
| **starter_kit** | Race-specific starter equipment packages | `starter_kit reload` |
| **online_tracker** | Track and reward accumulated online time | `online_tracker info <player>` |

**exp_config** -- Separate multipliers for monster, quest, and mining EXP. Per-race and per-level-range modifiers. Force/skill EXP overrides.

**exp_level_diff** -- Penalty curve for killing lower-level monsters. Bonus for higher-level targets. Configurable zero-EXP cutoff.

**level_rewards** -- Reward packages at level milestones (10, 20, 30...). Items, gold, CP, and buff grants. One-time or repeatable.

**mastery_config** -- Per-tree gain rate multiplier. Override mastery requirements for unlocks. Max mastery caps per class.

**max_level_config** -- Separate caps for base, class, and force level. EXP table extension for extended levels. Stat scaling rules.

**stat_config** -- Per-class stat growth rates per level. Flat bonuses at thresholds. HP/FP/SP formula overrides.

**daily_rewards** -- 30-day calendar with prize tiers. Catch-up mechanic for missed days. Monthly reset. Player command: `/daily`.

**daily_login** -- Streak milestones at 3, 7, 14, 30 days. Configurable break penalty. Dashboard streak leaderboard.

**bonus_start** -- Gold, items, and buffs on first login. Per-race packages. One-time flag prevents recreation abuse.

**newbie_protection** -- PvP immunity until level threshold. Graduated damage reduction tapering near threshold. Visual indicator.

**starter_kit** -- Full gear set per race and class. Consumables bundle. Configurable level requirement.

**online_tracker** -- Reward tiers at time milestones (1h, 4h, 8h). AFK detection. Player command: `/playtime`.

---

## 5. Guild System

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **guild_config** | Max members, level requirements, creation cost | `guild_config set <k> <v>` |
| **guild_skill** | Guild-wide passive skills and activation rules | `guild_skill grant <guild> <skill>` |
| **guild_treasury** | Guild bank with deposit/withdraw logging | `guild_treasury info <guild>` |
| **guild_activity** | Member activity tracking and auto-kick | `guild_activity report <guild>` |
| **election_monitor** | Archon election monitoring and rule enforcement | `election info`, `log` |

**guild_config** -- Max members per guild level. Creation cost and requirements. Battle entry fees and conditions.

**guild_skill** -- Passive buffs for all online guild members. Unlock tied to guild level or activity. Scaling per guild level.

**guild_treasury** -- Deposit/withdraw limits per rank. Full transaction log. Auto-deposit from guild territories.

**guild_activity** -- Track logins, kills, quests, war participation. Auto-kick inactive members. Weekly webhook reports.

**election_monitor** -- Vote logging with identity and timestamp. Minimum participation threshold. One-vote-per-account enforcement.

---

## 6. Monster & PvE

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **monster_config** | Global monster stat multipliers (HP, ATK, DEF, EXP) | `monster set <stat> <mult>` |
| **monster_aggro_config** | Aggro range, priority rules, threat decay | `aggro set <k> <v>` |
| **monster_defense_config** | Per-type defense overrides and resistances | `monster_def set <id> <p> <f>` |
| **monster_elemental_config** | Elemental damage/resistance tables | `elemental set <id> <elem> <v>` |
| **monster_skill_config** | Skill usage frequency, cooldowns, targeting | `monster_skill set <id> <k> <v>` |
| **monster_sp_config** | Special attack triggers and configuration | `monster_sp set <k> <v>` |
| **boss_hp_scale** | Dynamic boss HP scaling by player count | `boss_scale set <mult>` |
| **boss_announce** | World boss spawn/death announcements | `boss_announce test <id>` |
| **boss_announcer** | Extended boss tracking with timer predictions | `boss_announcer list`, `timer` |
| **drop_rate_config** | Global and per-monster drop rate multipliers | `drop_rate set <type> <mult>` |

**monster_config** -- HP, ATK, DEF, EXP multipliers. Per-map difficulty scaling. Level-range overrides.

**monster_aggro_config** -- Detection radius and vertical range. Priority: closest, lowest HP, highest damage, healer-first. Threat decay and reset timer.

**monster_defense_config** -- Per-ID defense overrides. Category multipliers (normal, elite, boss). Physical vs force split.

**monster_elemental_config** -- Per-monster weakness/resistance percentages. All elements: fire, water, earth, wind, dark, holy.

**monster_skill_config** -- Skill probability and cooldown per type. AoE vs single-target targeting. HP-threshold triggers (enrage at 30%).

**monster_sp_config** -- SP trigger conditions (timer, HP, player count). Damage multipliers and AoE radius. Cooldown and max usage per encounter.

**boss_hp_scale** -- HP multiplier per additional nearby player. Min/max HP caps. Configurable detection radius.

**boss_announce** -- Spawn and death announcements with name/location. Death message includes killer name. Discord webhook.

**boss_announcer** -- Respawn timer predictions. Historical kill log. Discord rich embed notifications.

**drop_rate_config** -- Global multiplier. Per-monster and per-item overrides. Separate rates for equipment, consumables, materials.

---

## 7. Skills & Buffs

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **skill_cost_config** | Override FP/SP costs per skill or class | `skill_cost set <id> <fp> <sp>` |
| **buff_tracker** | Monitor all buff applications and removals | `buff_tracker search <player>` |
| **buff_duration_config** | Override buff durations globally or per ID | `buff_duration set <id> <sec>` |
| **chipwar_buff_config** | Buff rules during Chip War events | `cw_buff allow/block <id>` |
| **npc_buff** | NPC buff service with configurable menus | `npc_buff list`, `reload` |
| **infinity_potion** | Non-consumable potions that never run out | `infinity_potion add <code>` |
| **potion_config** | Potion effectiveness, cooldowns, stacking | `potion set <k> <v>` |
| **hq_speed_buff** | Auto speed buff inside HQ/town areas | `hq_speed set <mult>` |

**skill_cost_config** -- Per-skill FP/SP cost overrides. Class-wide reduction percentages. Level-scaled cost curves.

**buff_tracker** -- Log every application: source, target, ID, duration. Track removals. Dashboard analytics.

**buff_duration_config** -- Global duration multiplier. Per-ID overrides. Separate rules for self-buffs vs party buffs.

**chipwar_buff_config** -- Whitelist/blacklist buffs in Chip War. Auto-apply faction buffs on start. Death persistence rules.

**npc_buff** -- Per-NPC buff menus with cost and duration. Level gates. Race-specific availability. Players interact with NPCs.

**infinity_potion** -- Designate item codes as infinite-use. Cooldown between uses. Works with HP, FP, SP potions.

**potion_config** -- Healing amount multipliers. Global cooldown override. Multi-type stacking rules.

**hq_speed_buff** -- Movement speed multiplier in HQ zones. Per-map and per-race configuration. Auto-remove on exit.

---

## 8. Mining & Resources

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **mine_config** | Mining success rates, ore yields, tool requirements | `mine set <k> <v>` |
| **mining_rewards** | Bonus rewards and rare drops while mining | `mining_rewards set <k> <v>` |
| **durability_config** | Equipment durability loss and repair costs | `durability set <k> <v>` |
| **mau_exp** | MAU experience gain rates and level scaling | `mau_exp set <rate>` |

**mine_config** -- Success rate and yield multipliers. Per-ore-type rates. Tool grade requirements.

**mining_rewards** -- Rare item chance per action. Bonus EXP from mining. Lucky strike jackpot events.

**durability_config** -- Loss rate multiplier (0 = no loss). Per-activity rates: PvE, PvP, mining. Repair cost multiplier.

**mau_exp** -- EXP gain rate multiplier. Per-level stat scaling. Max MAU level override.

---

## 9. Chat & Communication

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **chat_moderation** | Word filters, spam detection, auto-mute | `chat_mod mute <player> <min>` |
| **enchant_announcer** | Broadcast rare upgrade successes server-wide | `enchant_announce set <threshold>` |
| **kill_announcements** | Broadcast notable PvP kills | `kill_announce set <k> <v>` |
| **advert** | Scheduled announcement rotation | `advert list`, `add`, `remove` |
| **global_chat_welcome** | Public welcome when players log in | `global_welcome set <template>` |
| **trade_chat_logger** | Log all trade chat for moderation | `trade_log search <keyword>` |
| **welcome_message** | Private greeting on login | `welcome set <message>` |
| **npc_greetings** | Custom NPC dialogue and greetings | `npc_greet set <npc> <msg>` |
| **roving_npc** | NPC that moves between locations announcing | `roving_npc start`, `stop`, `route` |
| **stat_report** | Player stat summaries and leaderboards | `stat_report player`, `leaderboard` |

**chat_moderation** -- Word blacklist with regex. Repeated message throttling and auto-mute. Severity levels.

**enchant_announcer** -- Broadcast on configurable upgrade threshold (+5, +6, +7). Custom templates. Discord webhook.

**kill_announcements** -- Announce streaks, first blood, streak endings. Custom templates. Min streak threshold.

**advert** -- Multiple messages in rotation. Configurable interval. Per-message time-of-day scheduling.

**global_chat_welcome** -- Template with player name and race. Configurable channel. Cooldown on rapid re-logins.

**trade_chat_logger** -- Full logging with timestamps. Webhook forwarding. Dashboard search.

**welcome_message** -- Custom multi-line message with color codes. Different messages for new vs returning players.

**npc_greetings** -- Per-NPC messages. Race-specific variations. Randomized message pools.

**roving_npc** -- Waypoint routes per map. Broadcast at each arrival. Configurable speed and pause.

**stat_report** -- Player stat cards: kills, deaths, playtime, wealth. Leaderboards by category. Player command: `/stats`.

---

## 10. Cosmetics

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **preset_skins** | Pre-defined visual skin sets for equipment | `skins grant <player> <id>` |
| **weapon_skins** | Custom weapon visuals without stat changes | `weapon_skin grant <player> <skin>` |
| **nametag_color** | Custom nametag colors by rank or achievement | `nametag set <player> <color>` |

**preset_skins** -- Curated skin packages (VIP, event rewards). Per-race restrictions. Visual only, no stats. Player command: `/skin`.

**weapon_skins** -- Override weapon appearance per item code. Temporary or permanent grants.

**nametag_color** -- Color by GM rank, VIP, or achievement. Custom hex codes. Client-side patch integration.

---

## 11. Admin Tools

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **gm_toolkit** | Extended GM commands for administration | `gm help` for full list |
| **script_commands** | Execute JS scripts via chat commands | `script run <name> [args]` |
| **custom_commands** | Custom chat commands mapped to actions | `commands add/remove <cmd>` |
| **webhook_alerts** | Server events to Discord/Slack via webhooks | `webhook list`, `test` |
| **item_use_logger** | Log all item usage with full details | `item_log search <code>` |
| **anti_farm** | Detect and penalize repetitive kill farming | `anti_farm whitelist <player>` |
| **anti_afk** | Detect and handle AFK players in combat zones | `anti_afk set <timeout> <action>` |
| **auto_loot_config** | Auto pickup with configurable radius and filters | `auto_loot set <k> <v>` |

**gm_toolkit** -- Teleport, summon, kick, ban, item grant, stat modification. Bulk operations. Command audit logging.

**script_commands** -- Execute `RF_Bin/scripts/` JS files by name. Pass arguments via chat. Output as private message.

**custom_commands** -- Define `/command` aliases. Per-command permission levels (player, VIP, GM). Variable substitution.

**webhook_alerts** -- Events: login, kills, boss spawns, wars. Discord rich embeds. Multiple endpoints per category.

**item_use_logger** -- Every item use logged: player, code, time, location. Dashboard filters. Suspicious pattern alerts.

**anti_farm** -- Track same-target kills within time window. Configurable threshold. Penalties: reward reduction, PvP ban, warning.

**anti_afk** -- Idle detection threshold. Actions: warning, teleport to HQ, kick. Safe zone exemption.

**auto_loot_config** -- Pickup radius configuration. Grade/type/code filters. Boss loot exemption.

---

## 12. Maps & Portals

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **portal_restriction** | Block portal access by level, race, or condition | `portal restrict <id> <rules>` |
| **pvp_zone** | Define map areas with custom PvP rules | `pvp_zone add <map> <coords>` |
| **respawn_control** | Custom respawn locations, timers, death penalties | `respawn set <map> <x> <y> <z>` |
| **speed_config** | Movement speed multipliers by map or condition | `speed set <ctx> <mult>` |

**portal_restriction** -- Level range per portal. Race-specific access. Time/event-based locking.

**pvp_zone** -- Custom zones with FFA, faction, or guild rules. Entry warnings. Per-zone damage multipliers.

**respawn_control** -- Per-map respawn locations. Level-scaled timers. Death penalties: EXP loss, item drop chance.

**speed_config** -- Global multiplier. Per-map overrides (faster in towns, slower in dungeons). Mount and MAU speed.

---

## 13. Structures

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **building_config** | Guard tower HP, damage, placement rules | `building set <k> <v>` |
| **trap_stat_config** | Trap damage, duration, radius, deploy limits | `trap_stat set <type> <k> <v>` |
| **trap_alerts** | Notifications on trap placement/trigger | `trap_alert set <k> <v>` |
| **stone_hp** | Holy Stone HP scaling and regen rates | `stone_hp set <map> <hp> <regen>` |

**building_config** -- HP and damage multipliers. Max concurrent structures per guild/race. Placement restriction zones.

**trap_stat_config** -- Per-type damage, radius, duration overrides. Max traps per player/area. Visibility rules.

**trap_alerts** -- GM alerts for sensitive area traps. Webhook notifications. Per-map activity summaries.

**stone_hp** -- HP multiplier for all stones. Regen rate per tick. Per-map overrides.

---

## 14. Units

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **animus_config** | Animus summon rules, duration, availability | `animus set <k> <v>` |
| **animus_stat_config** | Animus stat multipliers per type | `animus_stat set <type> <stat> <v>` |
| **animus_tracker** | Animus usage, kills, lifetime stats | `animus_track report` |
| **mau_tracker** | MAU usage, fuel consumption, combat stats | `mau_track report` |

**animus_config** -- Duration and cooldown overrides. Level/class requirements. Max concurrent summons.

**animus_stat_config** -- Per-type HP, ATK, DEF multipliers. Level scaling curves. Elemental bonuses.

**animus_tracker** -- Summon frequency, active time, kills. Per-player reports. Dashboard popularity analytics.

**mau_tracker** -- Deployment time, fuel usage, combat kills. Per-player stats. Fuel rate monitoring.

---

## 15. Party

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **party_config** | Party size, EXP sharing mode, formation bonuses | `party set <k> <v>` |
| **party_level_diff_config** | EXP penalties for level gaps in parties | `party_diff set <gap> <penalty>` |

**party_config** -- Max party size override. EXP distribution: equal, proportional, hybrid. Full party bonuses.

**party_level_diff_config** -- Max level diff before penalty. Gradual or hard cutoff. Option to block party formation.

---

## 16. Quests

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **quest_tracker** | Track progress, completion rates, popular quests | `quest_track report` |
| **quest_rates** | Quest EXP and reward multipliers | `quest_rates set <type> <mult>` |
| **quest_system** | Custom quest definitions and chain quests | `quest create`, `remove <id>` |

**quest_tracker** -- Per-player completion history. Server-wide popularity stats. Dashboard funnel visualization.

**quest_rates** -- Global quest EXP multiplier. Per-quest overrides. Separate gold, EXP, item multipliers.

**quest_system** -- Custom quests: kill, collect, delivery objectives. Chain quest unlocks. Custom reward tables.

---

## 17. Misc

| Module | Description | GM Commands |
|--------|-------------|-------------|
| **client_patches** | Runtime client memory patches for bug fixes | `client_patches list`, `reload` |
| **action_point** | AP gain rates and cap configuration | `ap set <k> <v>` |
| **time_limit_config** | Session time limits and playtime restrictions | `time_limit set <daily> <weekly>` |
| **recover_config** | HP/FP/SP natural recovery multipliers | `recover set <type> <mult>` |
| **regen_config** | Sitting/standing regeneration overrides | `regen set <state> <type> <mult>` |
| **accuracy_effect** | Accuracy and dodge scaling from equipment | `accuracy set <effect> <mult>` |
| **radius_drop_loot** | Scatter loot drops in a radius around monsters | `radius_loot set <radius>` |
| **replace_loot_name** | Override displayed names for dropped items | `loot_name set <code> <name>` |
| **infinity_pots** | Alternative infinite potion system (custom items) | `infinity_pots add <code>` |
| **buddy_tracker** | Friend list activity tracking and notifications | `buddy info <player>` |

**client_patches** -- Memory patches on login. Fix CPU spin, gamma crash, DPI scaling. Per-patch enable/disable.

**action_point** -- Per-activity AP gain multiplier. Max AP cap override. Decay rate and daily reset.

**time_limit_config** -- Daily/weekly playtime limits. Warning notifications at thresholds. Soft or hard enforcement.

**recover_config** -- Standing and combat recovery multipliers. Per-class overrides. Suppression during PvP.

**regen_config** -- Sitting regen multiplier (HP, FP, SP). Standing idle rate. Interruption rules on damage/movement.

**accuracy_effect** -- Accuracy bonus multiplier from equipment. Dodge rate scaling. Per-effect-index tuning.

**radius_drop_loot** -- Scatter radius in game units. Min/max distance. Boss loot exemption.

**replace_loot_name** -- Map item codes to custom names. Color code support. Useful for events and translations.

**infinity_pots** -- Define item codes as non-consumable. Separate from infinity_potion for custom items. Per-type cooldown.

**buddy_tracker** -- Online/offline notifications for friends. Activity summaries. Player command: `/friends`.

---

## Quick Reference: Player-Facing Commands

These commands are available to all players (no GM rank required):

| Command | Module | What It Does |
|---------|--------|--------------|
| `/daily` | daily_rewards | Claim today's login reward |
| `/bounty` | pvp_rewards_extended | View active player bounties |
| `/stats` | stat_report | View personal statistics |
| `/playtime` | online_tracker | Check accumulated online time |
| `/skin` | preset_skins | Browse and apply available skins |
| `/friends` | buddy_tracker | Check friend activity and status |

## Quick Reference: Most Common GM Commands

| Task | Command |
|------|---------|
| Check EXP rates | `/zonemod exp info` |
| Set EXP multiplier | `/zonemod exp set combat 2.0` |
| Check drop rates | `/zonemod drop_rate info` |
| Start double EXP | `/zonemod double_exp start 2 60` |
| Force boss spawn | `/zonemod boss_spawn force <id>` |
| Mute a player | `/zonemod chat_mod mute <player> 30` |
| Check guild treasury | `/zonemod guild_treasury info <guild>` |
| List active events | `/zonemod event list` |
| Reload module config | `/zonemod <module> reload` |
