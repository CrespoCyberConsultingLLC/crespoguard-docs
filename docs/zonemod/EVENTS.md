# Event Reference

Complete reference for all CrespoGuard ZoneMod JavaScript scripting events. Register handlers with `on("event.name", callback)`.

---

## Parameter Conventions

All event callbacks follow one of these signatures:

| Signature | Meaning |
|-----------|---------|
| `(player)` | Single Player object |
| `(player, value)` | Player + numeric value to modify or inspect |
| `(player, player2)` | Two Player objects (e.g. killer + victim) |
| `(null, value)` | No player context, just the value |
| `(player, string)` | Player + string argument (chat message, NPC code, etc.) |

## Return Type Badges

| Badge | Behavior |
|-------|----------|
| **Notification** | Return value is ignored. The event is informational only. |
| **Blocking** | Return `false` to cancel the action. Return `true` (or nothing) to allow. |
| **Value (double)** | Return a number to replace the original value. The returned value is used by the server. |
| **Value (int)** | Return an integer to replace the original value. |

---

## Player Combat

Combat-related events covering attacks, kills, deaths, and PvP mechanics.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `player.death` | `(victim, killer)` | Notification | Player was killed by another player |
| `player.kill` | `(killer, victim)` | Notification | Player killed another player (reverse of death) |
| `player.killPlayer` | `(killer, victim)` | Notification | PvP kill confirmed via RecvKillMessage |
| `player.killMonster` | `(player)` | Notification | Player killed a monster (PvE kill) |
| `player.attack` | `(player)` | Notification | Player performed a basic melee/ranged attack |
| `player.attackForce` | `(player)` | Notification | Player performed a force (magic) attack |
| `player.attackSkill` | `(player)` | Notification | Player performed a skill-based attack |
| `player.attackSiege` | `(player)` | Notification | Player attacked in siege mode |
| `player.attackUnit` | `(player)` | Notification | Player attacked from a MAU/unit |
| `player.selfDestruct` | `(player)` | Notification | Player triggered MAU self-destruct |
| `player.force` | `(player, forceSerial)` | Notification | Player cast a force skill |
| `player.damageable` | `(player)` | Blocking | Return `false` to make the player invulnerable |
| `player.pvpCalc` | `(player)` | Blocking | Return `false` to skip PvP calculation |
| `player.chaosMode` | `(player)` | Blocking | Return `false` to suppress chaos mode |
| `player.autoRecover` | `(player)` | Blocking | Return `false` to skip the regen tick |

```javascript
// Log all PvP kills with race info
on("player.killPlayer", function(killer, victim) {
    console.log(killer.name + " (" + killer.race + ") killed " + victim.name + " (" + victim.race + ")");
});

// Make players invulnerable in town
on("player.damageable", function(player) {
    if (player.isInTown()) return false;
    return true;
});
```

---

## Player Stats

Events that modify player stat calculations. Most are value-modifying -- return a number to override the computed stat.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `player.gainExp` | `(player, amount)` | Value (double) | Kill EXP gain (amount > 0) |
| `player.questExp` | `(player, amount)` | Value (double) | Quest EXP reward |
| `player.expLoss` | `(player, amount)` | Value (double) | Death EXP penalty (amount < 0) |
| `player.expPotion` | `(player, amount)` | Value (double) | EXP gained from potion |
| `player.maxHP` | `(player, value)` | Value (double) | Computed maximum HP |
| `player.maxFP` | `(player, value)` | Value (double) | Computed maximum FP |
| `player.maxSP` | `(player, value)` | Value (double) | Computed maximum SP |
| `player.maxDP` | `(player, value)` | Value (double) | Computed maximum DP |
| `player.moveSpeed` | `(player, speed)` | Value (double) | Player movement speed |
| `player.attackDelay` | `(null, delayMs)` | Value (double) | Attack delay in milliseconds |
| `player.attackDP` | `(player, value)` | Value (double) | Player attack damage points |
| `player.attackRange` | `(player, value)` | Value (double) | Player attack range |
| `player.critFC` | `(player, value)` | Value (double) | Critical hit factor |
| `player.defFC` | `(player, value)` | Value (double) | Defense factor |
| `player.defFacing` | `(player, value)` | Value (double) | Facing defense multiplier |
| `player.defGap` | `(player, value)` | Value (double) | Gap defense multiplier |
| `player.weaponAdjust` | `(player, value)` | Value (double) | Weapon adjustment factor |
| `player.fireTol` | `(player, value)` | Value (double) | Fire elemental tolerance |
| `player.waterTol` | `(player, value)` | Value (double) | Water elemental tolerance |
| `player.soilTol` | `(player, value)` | Value (double) | Earth elemental tolerance |
| `player.windTol` | `(player, value)` | Value (double) | Wind elemental tolerance |
| `player.totalTol` | `(player, value)` | Value (double) | Total elemental tolerance (combined) |
| `player.addDalant` | `(player, amount)` | Value (double) | Dalant currency gain amount |
| `player.addGold` | `(player, amount)` | Value (double) | Gold currency gain amount |
| `player.pvpPointGain` | `(player, amount)` | Value (double) | PvP point gain amount |
| `player.pvpCashGain` | `(player, amount)` | Value (double) | PvP cash bag gain amount |
| `player.masteryGain` | `(player, value)` | Value (double) | Mastery cumulation after attack |
| `player.calcMastery` | `(null, value)` | Value (double) | Mastery level calculation result |
| `player.statGain` | `(player, amount)` | Value (double) | Stat point gain from AlterStat |
| `player.durabilityLoss` | `(player, delta)` | Value (double) | Equipment durability loss (negative) |
| `player.maxLevel` | `(null, value)` | Value (int) | Maximum achievable level |
| `player.equipSpeed` | `(player, value)` | Value (double) | Equipment-based speed modifier |
| `player.timePenalty` | `(null, value)` | Value (double) | Time-limit penalty multiplier |
| `player.itemEffect` | `(player, value)` | Value (double) | Normal equipped item effect value |
| `player.haveItemEffect` | `(player, value)` | Value (double) | Passive (have) item effect value |

```javascript
// 2x EXP rates, halve death penalty
on("player.gainExp", function(p, v) { return v * 2.0; });
on("player.expLoss", function(p, v) { return v * 0.5; });

// +20% move speed for all players
on("player.moveSpeed", function(p, v) { return v * 1.2; });

// Set max level to 80
on("player.maxLevel", function(p, v) { return 80; });
```

---

## Player Lifecycle

Events for player connection, map transitions, leveling, and respawn.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `player.createComplete` | `(player)` | Notification | Character fully loaded on server |
| `player.login` | `(player)` | Notification | Alias for createComplete (fires immediately after) |
| `player.disconnect` | `(player)` | Notification | Player disconnecting (Billing_Logout) |
| `player.logout` | `(player)` | Notification | Alias for disconnect |
| `player.mapEnter` | `(player, mapInMode)` | Notification | Player entered a map |
| `player.mapLeave` | `(player)` | Notification | Player leaving current map |
| `player.levelUp` | `(player, newLevel)` | Notification | Player leveled up |
| `player.classChange` | `(player, classIndex)` | Notification | Player changed class/job |
| `player.classReset` | `(player)` | Notification | Player reset their class |
| `player.respawn` | `(player)` | Notification | Player respawned (via respawn button) |
| `player.revival` | `(player)` | Notification | Player revived (pc_Revival) |
| `player.resurrect` | `(player)` | Blocking | Player being resurrected by potion/skill |

```javascript
// Welcome message on login
on("player.login", function(player) {
    player.sendMessage("[Server] Welcome back, " + player.name + "!");
    player.sendMessage("[Server] Your level: " + player.level + " | Race: " + player.race);
});

// Announce level-ups server-wide
on("player.levelUp", function(player, newLevel) {
    if (newLevel >= 50) {
        GameServer.broadcast("[Level Up] " + player.name + " reached level " + newLevel + "!");
    }
});
```

---

## Chat

All chat channel events. Most are blocking -- return `false` to suppress the message.

!!! note
    Chat events receive `(player, message)` where `message` is the raw chat string. The event name `player.chat` corresponds to circle (local) chat, not the deprecated `player.chatCircle`.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `player.chat` | `(player, message)` | Blocking | Circle (local area) chat |
| `player.chatAll` | `(player, message)` | Blocking | All-channel chat |
| `player.chatWhisper` | `(player, message)` | Blocking | Private whisper message |
| `player.chatParty` | `(player, message)` | Blocking | Party chat |
| `player.chatGuild` | `(player, message)` | Blocking | Guild chat |
| `player.chatRace` | `(player, message)` | Blocking | Race (faction) chat |
| `player.chatMap` | `(player, message)` | Blocking | Map-wide chat |
| `player.chatTrade` | `(player, message)` | Blocking | Trade channel chat |
| `player.chatGlobal` | `(player, message)` | Blocking | Global server-wide chat |
| `player.chatRaceBoss` | `(player, message)` | Blocking | Race leader broadcast |
| `player.chatGuildLeader` | `(player, message)` | Blocking | Guild leader broadcast |
| `player.chatOperator` | `(player, message)` | Blocking | Operator/GM chat channel |
| `player.chatCheat` | `(player, cmdStr)` | Blocking | Cheat command entered via chat |
| `player.cheatCommand` | `(player, cmdStr)` | Blocking | ProcessCheatCommand GM command |
| `chat.manage` | `(player, playerIdx)` | Blocking | Chat management request (mute/ban) |

```javascript
// Custom chat command handler
on("player.chat", function(player, message) {
    if (message === "!online") {
        player.sendMessage("[Server] Online players: " + GameServer.getPlayerCount());
        return false; // suppress from chat
    }
    return true;
});

// Block profanity in global chat
var BLOCKED_WORDS = ["badword1", "badword2"];
on("player.chatGlobal", function(player, message) {
    var lower = message.toLowerCase();
    for (var i = 0; i < BLOCKED_WORDS.length; i++) {
        if (lower.indexOf(BLOCKED_WORDS[i]) >= 0) {
            player.sendMessage("[Filter] Your message was blocked.");
            return false;
        }
    }
    return true;
});
```

---

## Portal / Movement

Events for portal usage, teleportation, and movement mode changes.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `player.portal` | `(player, portalCode)` | Blocking | Player requesting base portal (by code) |
| `portal.move` | `(player, portalIdx)` | Blocking | Player moving through portal (by index) |
| `player.teleportTo` | `(player, targetName)` | Blocking | Player teleporting to another player |
| `player.moveToStone` | `(player)` | Notification | Player using move-to-own-stone |
| `player.moveMode` | `(player, moveType)` | Notification | Player changed movement mode (walk/run) |
| `player.siegeMode` | `(player)` | Notification | Player entered siege mode |

```javascript
// Block portal access under level 30
on("portal.move", function(player, portalIdx) {
    if (player.level < 30) {
        player.sendMessage("[Portal] You must be level 30+ to use portals.");
        return false;
    }
    return true;
});
```

---

## Items / Inventory

Events for item interactions including pickup, use, upgrade, and equipment changes.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `item.pickup` | `(player)` | Notification | Player picked up a ground item |
| `player.usePotion` | `(player)` | Notification | Player used a potion item |
| `player.upgradeItem` | `(player)` | Notification | Player initiated item upgrade |
| `player.enchant` | `(player)` | Notification | Item upgrade completed (fires after upgradeItem) |
| `player.throwItem` | `(player)` | Notification | Player threw/discarded an item |
| `player.setItem` | `(player)` | Notification | Player set-item check request |
| `player.combine` | `(player)` | Notification | Player combined items |
| `player.downgrade` | `(player)` | Notification | Player downgraded an item |
| `player.equip` | `(player)` | Notification | Player equipped an item |
| `player.unequip` | `(player)` | Notification | Player unequipped an item |
| `player.exchangeItemSlot` | `(player)` | Notification | Player swapped item slots |
| `player.equipEffect` | `(player)` | Blocking | Equipment effect being applied |
| `player.useNpcItem` | `(player)` | Notification | Player used an NPC-linked item |
| `player.useFireCracker` | `(player)` | Notification | Player used a firecracker item |
| `player.useRadar` | `(player)` | Notification | Player used a radar item |
| `player.useLossExpRecovery` | `(player)` | Notification | Player used a loss-EXP recovery item |
| `player.oreCutting` | `(player)` | Notification | Player cutting ore |
| `player.oreIntoBag` | `(player)` | Notification | Player put ore into bag |

```javascript
// Log all item pickups
on("item.pickup", function(player) {
    console.log(player.name + " picked up an item");
});

// Announce successful enchants
on("player.enchant", function(player) {
    GameServer.broadcast("[Enchant] " + player.name + " upgraded an item!");
});
```

---

## Trade / Shop

Events for player-to-player trading, NPC shops, cash shop, auction, and mail.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `player.trade` | `(player)` | Notification | Player confirmed a direct trade |
| `player.tradeAsk` | `(player)` | Notification | Player sent a trade request |
| `player.tradeAdd` | `(player)` | Notification | Player added item to trade window |
| `player.tradeBet` | `(player)` | Notification | Player placed currency in trade |
| `player.buyItem` | `(player)` | Notification | Player bought from NPC shop |
| `player.sellItem` | `(player)` | Notification | Player sold to NPC shop |
| `player.craft` | `(player)` | Notification | Player crafted an item |
| `player.exchangeDalant` | `(player, amount)` | Notification | Player exchanged dalant for gold |
| `player.exchangeGold` | `(player, amount)` | Notification | Player exchanged gold for dalant |
| `player.exchangePvP` | `(player, goldAmount)` | Notification | Player exchanged gold for PvP points |
| `shop.sellPrice` | `(null, price)` | Value (double) | NPC shop sell price calculation |
| `shop.buyPrice` | `(null, price)` | Value (double) | NPC shop buy price calculation |
| `cashShop.buy` | `(player, playerIdx)` | Blocking | Cash shop purchase attempt |
| `auction.register` | `(player, playerIdx)` | Blocking | Player registering auction item |
| `auction.adjustPrice` | `(player, playerIdx)` | Blocking | Player adjusting auction price |
| `mail.send` | `(player, playerIdx)` | Blocking | Player sending mail |

```javascript
// 50% off all NPC shop prices
on("shop.buyPrice", function(p, price) {
    return price * 0.5;
});

// Double NPC sell prices
on("shop.sellPrice", function(p, price) {
    return price * 2.0;
});
```

---

## Trunk / Storage

Events for warehouse (trunk) inventory operations.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `player.trunkMove` | `(player)` | Notification | Player moved item to/from trunk |
| `player.trunkSwap` | `(player)` | Notification | Player swapped trunk item slots |
| `player.trunkMerge` | `(player)` | Notification | Player merged trunk item stacks |
| `player.trunkSlot` | `(player)` | Notification | Player altered trunk item slot |
| `player.trunkDivide` | `(player)` | Notification | Player divided trunk item stack |
| `player.trunkSplitPotion` | `(player)` | Notification | Player split potion stack in trunk |

```javascript
// Log all trunk operations
on("player.trunkMove", function(player) {
    console.log(player.name + " accessed warehouse storage");
});
```

---

## Mining

Events for the mining/resource gathering system.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `player.mine` | `(player)` | Notification | Player started mining |
| `player.mineComplete` | `(player)` | Notification | Player completed mining action |
| `mining.startRequest` | `(player, playerIdx)` | Blocking | Mining start authorization |
| `mining.cancelRequest` | `(player, playerIdx)` | Blocking | Mining cancel authorization |
| `mining.oreYield` | `(null, count)` | Value (double) | Number of ore yielded from mining |

```javascript
// Double ore yield
on("mining.oreYield", function(p, count) {
    return count * 2;
});
```

---

## Quests

Events for quest acceptance, completion, and management.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `player.questAccept` | `(player)` | Notification | Player accepted a quest (via NPC) |
| `player.nativeQuestAccept` | `(player, questIndex)` | Notification | Player accepted a native quest |
| `player.questComplete` | `(player)` | Notification | Player completed and claimed quest reward |
| `player.questEvent` | `(player)` | Notification | Quest after-happen event triggered |
| `player.questGiveup` | `(player, questIdx)` | Notification | Player abandoned a quest |
| `quest.repeatCooldown` | `(null, seconds)` | Value (double) | Repeatable quest cooldown in seconds |

```javascript
// Halve quest repeat cooldowns
on("quest.repeatCooldown", function(p, seconds) {
    return seconds * 0.5;
});
```

---

## Skills

Events for skill usage, costs, and level-gating.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `player.skillUse` | `(player, skillIndex)` | Notification | Player used a skill |
| `player.skillCostCheck` | `(player, skillIndex)` | Blocking | Return `false` to block the skill |
| `skill.attackRequest` | `(player, playerIdx)` | Blocking | Skill-based attack authorization |
| `skill.forceRequest` | `(player, playerIdx)` | Blocking | Force skill authorization |
| `skill.classRequest` | `(player, playerIdx)` | Blocking | Class skill authorization |
| `player.expLevelGate` | `(player, monsterLevel)` | Blocking | EXP level-difference gate check |
| `player.masteryLevelGate` | `(player, targetLevel)` | Blocking | Mastery level-difference gate check |
| `player.masteryUsable` | `(player, sfType)` | Blocking | Whether mastery skill type is usable |

```javascript
// Allow EXP from any level monster (bypass level gate)
on("player.expLevelGate", function(player, monsterLevel) {
    return true; // always allow
});
```

---

## Buffs

Events for buff application, duration, and periodic ticks.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `buff.duration` | `(player, duration)` | Value (double) | Buff duration in milliseconds |
| `buff.applied` | `(player)` | Notification | A buff was applied to the player |
| `player.buffTick` | `(player)` | Notification | Periodic buff tick on the player |

```javascript
// Double all buff durations
on("buff.duration", function(p, duration) {
    return duration * 2.0;
});
```

---

## Party

Events for party (group) formation and experience distribution.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `party.join` | `(player)` | Notification | Player applied to join a party |
| `party.joinAnswer` | `(player)` | Notification | Party join response received |
| `party.leave` | `(player)` | Notification | Player left the party |
| `party.expRate` | `(player, rate)` | Value (double) | Party EXP distribution rate |
| `party.levelCheck` | `(null, level)` | Blocking | Party level-difference gate check |

```javascript
// 50% bonus party EXP
on("party.expRate", function(p, rate) {
    return rate * 1.5;
});
```

---

## Guild

Events for guild management, membership, and financial operations.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `guild.manage` | `(player, action)` | Notification | Guild management action |
| `guild.roomEnter` | `(player)` | Notification | Player entered guild room |
| `guild.roomLeave` | `(player)` | Notification | Player left guild room |
| `guild.create` | — | Notification | A new guild was created |
| `guild.battleAccept` | — | Notification | Guild battle accepted |
| `guild.memberLeave` | — | Notification | Member forced-left the guild |
| `guild.changeMaster` | — | Notification | Guild master changed |
| `guild.depositMoney` | `(player)` | Notification | Player deposited money into guild |
| `guild.vote` | `(player)` | Notification | Guild vote cast |
| `guild.suggest` | `(player)` | Notification | Guild suggestion offered |
| `guild.cancelSuggest` | `(player)` | Notification | Guild suggestion cancelled |
| `guild.joinApply` | `(player)` | Notification | Player applied to join guild |
| `guild.joinAccept` | `(player)` | Notification | Guild join application accepted |
| `guild.joinApplyCancel` | `(player)` | Notification | Guild join application cancelled |
| `guild.selfLeave` | `(player)` | Notification | Player voluntarily left guild |
| `guild.roomRent` | `(player)` | Notification | Guild room rental request |
| `guild.setHonor` | `(player)` | Notification | Guild honor setting changed |
| `guild.createCost` | `(null, cost)` | Value (double) | Guild creation cost in dalant |

```javascript
// Free guild creation
on("guild.createCost", function(p, cost) {
    return 0;
});
```

---

## Potions

Events for the potion system including heal amounts and application checks.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `potion.canApply` | `(user, target)` | Blocking | Whether potion can be applied (two players) |
| `potion.apply` | `(user, target)` | Notification | Potion applied (two players: user + target) |
| `potion.hpAmount` | `(player, amount)` | Value (double) | HP heal amount (before application) |
| `potion.fpAmount` | `(player, amount)` | Value (double) | FP heal amount (before application) |
| `potion.spAmount` | `(player, amount)` | Value (double) | SP heal amount (before application) |
| `potion.heal` | `(player)` | Notification | HP potion heal completed |
| `potion.healFP` | `(player)` | Notification | FP potion heal completed |
| `potion.healSP` | `(player)` | Notification | SP potion heal completed |
| `potion.exp` | `(player)` | Notification | EXP potion used |
| `potion.movestone` | `(player)` | Notification | Movestone potion used |

```javascript
// Double HP potion healing
on("potion.hpAmount", function(player, amount) {
    return amount * 2.0;
});
```

---

## Monsters

Events for monster spawns, AI, stats, and combat behavior.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `monster.spawn` | — | Notification | A monster was created |
| `monster.respawn` | — | Notification | A monster respawned |
| `monster.death` | `(player)` | Notification | Monster died (player is killer if available) |
| `monster.attack` | — | Notification | Monster performed basic melee attack |
| `monster.attackForce` | — | Notification | Monster performed force attack |
| `monster.attackSkill` | — | Notification | Monster performed skill attack |
| `monster.aggroChange` | `(player)` or — | Notification | Monster changed aggro target |
| `monster.damage` | `(player)` | Notification | Monster took damage (player is attacker) |
| `monster.stateChange` | — | Notification | Monster state flags changed |
| `monster.childSpawn` | — | Notification | Child monster spawned (hierarchy) |
| `monster.childDestroy` | — | Notification | Child monster destroyed (hierarchy) |
| `monster.recall` | `(player, monsterName)` | Blocking | GM recalling/summoning a monster |
| `monster.maxHP` | `(null, value)` | Value (double) | Monster maximum HP |
| `monster.moveSpeed` | `(null, speed)` | Value (double) | Monster movement speed |
| `monster.attackDP` | `(null, value)` | Value (double) | Monster attack damage |
| `monster.attackRange` | `(null, value)` | Value (double) | Monster attack range |
| `monster.defFC` | `(null, value)` | Value (double) | Monster defense factor |
| `monster.fireTol` | `(null, value)` | Value (double) | Monster fire tolerance |
| `monster.waterTol` | `(null, value)` | Value (double) | Monster water tolerance |
| `monster.soilTol` | `(null, value)` | Value (double) | Monster earth tolerance |
| `monster.windTol` | `(null, value)` | Value (double) | Monster wind tolerance |
| `monster.dropRate` | `(null, rate)` | Value (double) | Monster drop rate multiplier |
| `monster.skillRange` | `(null, value)` | Value (double) | Monster skill attack range |
| `monster.aggroResetTime` | `(null, ms)` | Value (double) | Aggro reset timeout in ms |
| `monster.aggroShortTime` | `(null, ms)` | Value (double) | Short aggro timeout in ms |
| `monster.skillDelay` | `(null, delay)` | Value (double) | Monster skill cooldown delay |
| `monster.spActionProb` | `(null, prob)` | Value (int) | SP action probability (0-10000) |
| `monster.spLimitCount` | `(null, count)` | Value (int) | Per-encounter skill use cap |

```javascript
// Double monster HP, halve drop rate
on("monster.maxHP", function(p, hp) { return hp * 2; });
on("monster.dropRate", function(p, rate) { return rate * 0.5; });

// Announce boss monster recalls
on("monster.recall", function(player, monName) {
    if (player && player.gmDegree > 0) {
        GameServer.broadcast("[Boss] " + monName + " has been summoned by GM " + player.name + "!");
    }
    return true;
});
```

---

## Animus

Events for Bellato animus (summoned companion) management and stats.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `animus.spawn` | — | Notification | An animus was created |
| `animus.recall` | `(player)` | Notification | Player recalled their animus |
| `animus.return` | `(player)` | Notification | Player returned their animus |
| `animus.command` | `(player, command)` | Notification | Player issued animus command |
| `animus.invenChange` | `(player)` | Notification | Animus inventory changed |
| `player.animusTarget` | `(player)` | Notification | Player set animus attack target |
| `animus.maxHP` | `(null, value)` | Value (double) | Animus maximum HP |
| `animus.attackDP` | `(null, value)` | Value (double) | Animus attack damage |
| `animus.attackRange` | `(null, value)` | Value (double) | Animus attack range |
| `animus.defFC` | `(null, value)` | Value (double) | Animus defense factor |
| `animus.fireTol` | `(null, value)` | Value (double) | Animus fire tolerance |
| `animus.waterTol` | `(null, value)` | Value (double) | Animus water tolerance |
| `animus.soilTol` | `(null, value)` | Value (double) | Animus earth tolerance |
| `animus.windTol` | `(null, value)` | Value (double) | Animus wind tolerance |

```javascript
// Buff animus stats by 50%
on("animus.maxHP", function(p, v) { return v * 1.5; });
on("animus.attackDP", function(p, v) { return v * 1.5; });
```

---

## MAU / Units

Events for MAU (Massive Armor Unit) and vehicle management.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `unit.mount` | `(player)` | Notification | Player mounted a MAU |
| `unit.dismount` | `(player)` | Notification | Player dismounted from MAU |
| `unit.return` | `(player)` | Notification | Player returned MAU to storage |
| `unit.frameBuy` | `(player)` | Notification | Player purchased MAU frame |
| `unit.partTuning` | `(player)` | Notification | Player tuned MAU parts |
| `unit.bulletFill` | `(player)` | Notification | Player refilled MAU ammunition |
| `unit.sell` | `(player)` | Notification | Player sold a MAU |
| `player.unitDeliver` | `(player)` | Notification | Player delivered a MAU |
| `player.unitRepair` | `(player)` | Notification | Player repaired MAU frame |

```javascript
// Log MAU mount/dismount
on("unit.mount", function(player) {
    console.log(player.name + " mounted a MAU");
});
on("unit.dismount", function(player) {
    console.log(player.name + " dismounted from MAU");
});
```

---

## Traps / Towers

Events for trap and guard tower placement, destruction, and stats.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `trap.spawn` | — | Notification | A trap was placed |
| `trap.destroy` | — | Notification | A trap was destroyed |
| `player.makeTrap` | `(player)` | Notification | Player crafted a trap |
| `player.makeTower` | `(player)` | Notification | Player crafted a guard tower |
| `tower.spawn` | — | Notification | A guard tower was placed |
| `trap.maxHP` | `(null, value)` | Value (double) | Trap maximum HP |
| `trap.attackDP` | `(null, value)` | Value (double) | Trap attack damage |
| `trap.attackRange` | `(null, value)` | Value (double) | Trap attack range |
| `trap.defFC` | `(null, value)` | Value (double) | Trap defense factor |
| `trap.fireTol` | `(null, value)` | Value (double) | Trap fire tolerance |
| `trap.waterTol` | `(null, value)` | Value (double) | Trap water tolerance |
| `trap.soilTol` | `(null, value)` | Value (double) | Trap earth tolerance |
| `trap.windTol` | `(null, value)` | Value (double) | Trap wind tolerance |
| `tower.maxHP` | `(null, value)` | Value (double) | Tower maximum HP |
| `tower.attackDP` | `(null, value)` | Value (double) | Tower attack damage |
| `tower.attackRange` | `(null, value)` | Value (double) | Tower attack range |
| `tower.defFC` | `(null, value)` | Value (double) | Tower defense factor |
| `tower.fireTol` | `(null, value)` | Value (double) | Tower fire tolerance |
| `tower.waterTol` | `(null, value)` | Value (double) | Tower water tolerance |
| `tower.soilTol` | `(null, value)` | Value (double) | Tower earth tolerance |
| `tower.windTol` | `(null, value)` | Value (double) | Tower wind tolerance |

```javascript
// Double tower HP and attack
on("tower.maxHP", function(p, v) { return v * 2; });
on("tower.attackDP", function(p, v) { return v * 2; });
```

---

## Holy Stone / Chip War

Events for the chip war system, holy stone, stone keepers, and chip war phases.

### Chip War Control

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `chipwar.start` | — | Notification | Chip war started (stones activated) |
| `chipwar.end` | — | Notification | Chip war ended (stones deactivated) |
| `chipwar.winner` | `(player)` | Notification | Player claimed the chip |
| `chipwar.phaseChange` | — | Notification | Chip war phase changed |
| `chipwar.battleStop` | — | Notification | Chip war battle stopped |
| `chipwar.keeperStart` | — | Notification | Keeper phase started |
| `chipwar.resultType` | `(null, result)` | Value (int) | Chip war result type (0-3) |
| `holystone.destroyed` | — | Notification | A holy stone was destroyed |
| `holystone.keeperDeath` | `(player)` or — | Notification | Stone keeper died (player if attacker is player) |
| `holystone.schedule` | — | Notification | Holy stone schedule altered |

### Holy Stone Stats

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `stone.maxHP` | `(null, value)` | Value (double) | Holy stone maximum HP |
| `stone.attackDP` | `(null, value)` | Value (double) | Holy stone attack damage |
| `stone.defFC` | `(null, value)` | Value (double) | Holy stone defense factor |
| `stone.fireTol` | `(null, value)` | Value (double) | Holy stone fire tolerance |
| `stone.waterTol` | `(null, value)` | Value (double) | Holy stone water tolerance |
| `stone.soilTol` | `(null, value)` | Value (double) | Holy stone earth tolerance |
| `stone.windTol` | `(null, value)` | Value (double) | Holy stone wind tolerance |

### Keeper Stats

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `keeper.maxHP` | `(null, value)` | Value (double) | Keeper maximum HP |
| `keeper.attackDP` | `(null, value)` | Value (double) | Keeper attack damage |
| `keeper.attackRange` | `(null, value)` | Value (double) | Keeper attack range |
| `keeper.defFC` | `(null, value)` | Value (double) | Keeper defense factor |
| `keeper.fireTol` | `(null, value)` | Value (double) | Keeper fire tolerance |
| `keeper.waterTol` | `(null, value)` | Value (double) | Keeper water tolerance |
| `keeper.soilTol` | `(null, value)` | Value (double) | Keeper earth tolerance |
| `keeper.windTol` | `(null, value)` | Value (double) | Keeper wind tolerance |

```javascript
// Buff chip war stone HP by 3x
on("stone.maxHP", function(p, v) { return v * 3; });

// Announce chip war events
on("chipwar.start", function() {
    GameServer.broadcast("[Chip War] The battle has begun!");
});
on("chipwar.winner", function(player) {
    GameServer.broadcast("[Chip War] " + player.name + " (" + player.race + ") claimed the Holy Stone Chip!");
});
```

---

## Economy

Events for server economy rates and tax calculations.

!!! info
    Economy events pass `raceCode` as an additional integer parameter. The callback receives `(null, value)` and should return the modified value.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `economy.exchangeRate` | `(null, rate)` | Value (double) | Dalant/gold exchange rate per race |
| `economy.oreRate` | `(null, rate)` | Value (double) | Ore rate per race |
| `economy.taxRate` | `(null, rate)` | Value (double) | Tax rate per race |
| `economy.taxRateBps` | `(null, rate)` | Value (double) | Tax rate in basis points per race |

```javascript
// Halve the tax rate
on("economy.taxRate", function(p, rate) {
    return rate * 0.5;
});
```

---

## Combat Formulas

Events that modify core combat calculation results.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `combat.calcGenAtt` | `(null, value)` | Value (double) | General (melee/ranged) attack power |
| `combat.calcForceAtt` | `(null, value)` | Value (double) | Force (magic) attack power |
| `combat.guildBattle` | — | Blocking | Return `false` to deny guild battle damage |
| `combat.damPoint` | `(player, value)` | Value (double) | Final damage point calculation |

```javascript
// +10% to all melee/ranged damage
on("combat.calcGenAtt", function(p, v) { return v * 1.1; });

// Cap maximum single-hit damage at 50000
on("combat.damPoint", function(p, v) {
    return v > 50000 ? 50000 : v;
});
```

---

## NPC

Events for NPC dialog, shop, quest, and buff interactions.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `npc.dialog` | `(player, npcCode)` | Notification | Player opened NPC dialog |
| `npc.shop` | `(player, npcCode)` | Notification | Player opened NPC shop |
| `npc.quest` | `(player)` | Notification | NPC quest request |
| `npc.questList` | `(player)` | Notification | NPC quest list request |
| `npc.buffRequest` | `(player, storeRecIndex)` | Blocking | NPC buff request authorization |

```javascript
// Log NPC interactions
on("npc.dialog", function(player, npcCode) {
    console.log(player.name + " spoke with NPC: " + npcCode);
});
```

---

## Loot

Events for loot box creation and pickup authorization.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `loot.take` | `(player, playerIdx)` | Blocking | Loot pickup authorization |
| `loot.create` | `(player)` | Notification | Loot item box created |
| `loot.boxSpawn` | — | Notification | Item box spawned on ground |
| `loot.boxFixPos` | — | Notification | Item box position finalized |

```javascript
// Log loot box spawns
on("loot.boxSpawn", function() {
    console.log("A loot box appeared on the ground");
});
```

---

## PvP

Events related specifically to PvP cash and point recovery.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `player.pvpCashRecover` | `(player)` | Notification | Player used PvP cash recovery item |

---

## Buddy

Events for the friend/buddy list system.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `player.addBuddy` | `(player)` | Notification | Player sent buddy add request |
| `buddy.addAnswer` | `(player)` | Notification | Buddy add request answered |
| `buddy.delete` | `(player)` | Notification | Player deleted a buddy |

---

## Patriarch

Events for the patriarch (race leader) election system.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `patriarch.punish` | `(player, type)` | Notification | Patriarch punishment action |
| `patriarch.proposeVote` | `(player)` | Notification | Patriarch proposed a vote |
| `patriarch.castVote` | `(player, voteCode)` | Notification | Player cast patriarch vote |

---

## Voting

Events for the in-game voting system.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `vote.cast` | `(player)` | Blocking | Player casting a vote |
| `vote.sendPaper` | `(player)` | Blocking | Vote paper sent to player |
| `vote.sendPaperAll` | — | Notification | Vote papers sent to all |
| `vote.sendScore` | `(player)` | Notification | Vote score sent to player |
| `vote.sendScoreAll` | — | Notification | Vote scores broadcast |

---

## Misc

General-purpose and uncategorized events.

| Event Name | Parameters | Return Type | Description |
|------------|-----------|-------------|-------------|
| `ui.response` | `(player, jsonString)` | Notification | Custom UI response from client |
| `player.gesture` | `(player, gestureId)` | Notification | Player performed a gesture/emote |
| `player.senseState` | `(player)` | Notification | Player sense state updated |
| `player.shapeUpdate` | `(player)` | Notification | Player appearance/shape updated |
| `player.pvpOrderUpdate` | `(player)` | Blocking | PvP order update (return `false` to block) |
| `action.eventStatus` | `(null, actionCode)` | Value (int) | Action event status code |
| `server.tick` | — | Notification | Server tick (fires once per second) |

```javascript
// Periodic server tick handler
var uptime = 0;
on("server.tick", function() {
    uptime++;
    if (uptime % 3600 === 0) {
        GameServer.broadcast("[Server] Uptime: " + (uptime / 3600) + " hour(s)");
    }
});
```

---

## Event Count Summary

| Category | Count |
|----------|-------|
| Player Combat | 15 |
| Player Stats | 35 |
| Player Lifecycle | 12 |
| Chat | 15 |
| Portal / Movement | 6 |
| Items / Inventory | 18 |
| Trade / Shop | 16 |
| Trunk / Storage | 6 |
| Mining | 5 |
| Quests | 6 |
| Skills | 8 |
| Buffs | 3 |
| Party | 5 |
| Guild | 18 |
| Potions | 10 |
| Monsters | 27 |
| Animus | 14 |
| MAU / Units | 9 |
| Traps / Towers | 22 |
| Holy Stone / Chip War | 25 |
| Economy | 4 |
| Combat Formulas | 4 |
| NPC | 5 |
| Loot | 4 |
| PvP | 1 |
| Buddy | 3 |
| Patriarch | 3 |
| Voting | 5 |
| Misc | 7 |
| **Total** | **~309** |
