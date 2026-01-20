# Using Commands In-Game

A comprehensive guide to using console commands in Hytale.

> **Note:** This guide is for **players** using commands in-game. If you want to **create custom commands**, see the [Creating Commands](41_Commands.md) guide.

## Overview

Console commands allow you to control various aspects of the game, from managing your character to manipulating the world. This guide covers all available commands and how to use them in-game.

## Opening the Console

To open the console in-game, press the **`~`** (tilde) or **`/`** key. Type your command and press **Enter** to execute it.

## Command Syntax

Commands follow this basic format:

```
/command <required_argument> [optional_argument] --flag=value
```

- **`/`** - Command prefix (may be optional depending on console mode)
- **`<argument>`** - Required parameter (must provide)
- **`[argument]`** - Optional parameter (can omit)
- **`--flag=value`** - Optional flags for modifying command behavior

## Quick Reference Commands

### Most Common Commands

| Command | Description | Example |
|---------|-------------|---------|
| `/help` | Display command help | `/help` |
| `/heal` | Fully heal health and stamina | `/heal` |
| `/whereami` | Show your current location | `/whereami` |
| `/whoami` | Show your player information | `/whoami` |
| `/give <item> [qty]` | Give yourself an item | `/give Weapon_Sword_Iron 1` |
| `/tp <x> <y> <z>` | Teleport to coordinates | `/tp 0 100 0` |
| `/time set <hour>` | Set time of day | `/time set 12` |
| `/gamemode <mode>` | Change game mode | `/gamemode creative` |
| `/kill` | Kill and respawn yourself | `/kill` |

### Custom Macro Commands

These are pre-configured custom commands:

| Command | Description |
|---------|-------------|
| `/heal` | Heals stamina and health to maximum |
| `/noon` | Sets time to noon and pauses time |
| `/delete` or `/del` or `/d` | Clears current selection (creative mode) |
| `/unstuck` | Teleports you slightly to help when stuck |
| `/neardeath` | Sets health to near-death (for testing) |
| `/resetrotation` | Resets camera rotation |
| `/fillsignature` | Fill signature command |

## Player Commands

### Health and Stats

**Set stat to maximum:**
```
/player stat settomax <StatName>
```

Examples:
```
/player stat settomax Health
/player stat settomax Stamina
/player stat settomax Mana
```

**Set stat to specific value:**
```
/player stat set <StatName> <value>
```

Example:
```
/player stat set Health 50
```

**Add to stat:**
```
/player stat add <StatName> <value>
```

Example:
```
/player stat add Mana 20
```

### Player Information

**Get your location:**
```
/whereami
```

Shows your current coordinates and world.

**Get your player info:**
```
/whoami
```

Shows your player ID, username, and other information.

**Get current zone and biome:**
```
/player zone
```

Shows the zone and biome you're currently in.

### Respawn and Death

**Kill and respawn:**
```
/kill
```

**Respawn without dying:**
```
/player respawn
```

**Damage yourself:**
```
/damage <amount>
```

Example:
```
/damage 10
```

### Effects

**Apply an entity effect:**
```
/player effect apply <EffectId> [duration]
```

Example:
```
/player effect apply Burn 30
```

**Clear all effects:**
```
/player effect clear
```

## Inventory Commands

### Managing Items

**Give yourself an item:**
```
/give <ItemId> [quantity]
```

Examples:
```
/give Weapon_Sword_Iron 1
/give Food_Bread 64
/give Potion_Health_Greater 10
```

**Clear your entire inventory:**
```
/inventoryclear
```

**View your backpack:**
```
/inventorybackpack
```

**Resize your backpack:**
```
/inventorybackpack <size>
```

Example:
```
/inventorybackpack 100
```

**Give armor set:**
```
/givearmor <armor_type>
```

Examples:
```
/givearmor Iron
/givearmor Gold
```

## Recipe Commands

**Learn a recipe:**
```
/recipe learn <ItemId>
```

Example:
```
/recipe learn Weapon_Sword_Iron
```

**Forget (unlearn) a recipe:**
```
/recipe forget <ItemId>
```

Example:
```
/recipe forget Weapon_Sword_Iron
```

**List all known recipes:**
```
/recipe list
```

## Time Commands

### Setting Time

**Set time to specific hour (0-24):**
```
/time set <hour>
```

Examples:
```
/time set 12    # Noon
/time set 0     # Midnight
/time set 6     # Dawn
/time set 18    # Dusk
```

**Set time by period:**
```
/time set day
/time set night
/time set midday
/time set midnight
```

**Get current time:**
```
/time get
```

Shows detailed time information including day, week, month, and moon phase.

### Time Control

**Pause time:**
```
/pausetime
```

Toggles time pause/unpause.

## Teleportation Commands

**Teleport to coordinates:**
```
/tp <x> <y> <z>
```

Example:
```
/tp 100 65 -200
```

**Teleport with rotation:**
```
/tp <x> <y> <z> --pitch=<p> --yaw=<y> --roll=<r>
```

Example:
```
/tp 0 70 0 --yaw=90 --pitch=-45
```

**Teleport all players to location:**
```
/tpall <x> <y> <z>
```

Example:
```
/tpall 0 100 0
```

Teleports all online players to the specified location.

**Teleport to home:**
```
/tp home
```

**Teleport to top of world (surface):**
```
/tp top
```

**Teleport to current position (useful for adjusting rotation):**
```
/tp ~ ~ ~
```

The `~` symbol means "current position" for that coordinate.

## Camera Commands

**Reset camera to default view:**
```
/camera reset
```

**Set camera to top-down view:**
```
/camera topdown
```

**Set camera to side-scroller view:**
```
/camera sidescroller
```

**Activate camera demo mode:**
```
/camera demo activate
```

**Deactivate camera demo mode:**
```
/camera demo deactivate
```

## Game Mode Commands

**Change game mode:**
```
/gamemode <mode>
```

Modes:
- `survival` - Survival mode
- `adventure` - Adventure mode
- `creative` - Creative mode

Example:
```
/gamemode creative
```

## Sound Commands

**Play a 2D sound:**
```
/sound 2d <SoundEventId>
```

Example:
```
/sound 2d SFX_Sword_T2_Impact
```

**Play a 3D sound at position:**
```
/sound 3d <SoundEventId> <x> <y> <z>
```

Example:
```
/sound 3d SFX_Explosion 100 65 -50
```

**Play sound for all players:**
```
/sound 2d <SoundEventId> --all
```

## Emote Commands

**Play an emote:**
```
/emote <EmoteId>
```

Example:
```
/emote Wave
```

## World Commands

### World Creation and Management

**Add a new world:**
```
/addworld <name> [--gen=<generatorId>] [--storage=<storageType>]
```

Example:
```
/addworld MyNewWorld --gen=HytaleGenerator
```

**Load existing world:**
```
/loadworld <name>
```

**Remove a world:**
```
/removeworld <name>
```

**Set default world:**
```
/world setdefault <worldName>
```

**Prune empty worlds:**
```
/world prune
```

Removes all non-default worlds without online players.

**List loaded worlds:**
```
/worlds
```

**Get world seed:**
```
/seed
```

Shows the world generation seed.

**Set world spawn point:**
```
/spawn set [position] [rotation]
```

Example:
```
/spawn set          # Sets to your current position
/spawn set 0 100 0  # Sets to specific coordinates
```

**Reset spawn to default:**
```
/spawn set --default
```

**Set world ticking:**
```
/setticking <true|false>
```

Example:
```
/setticking false  # Pauses world ticking
```

**Set world PvP:**
```
/setpvp <true|false>
```

Example:
```
/setpvp true  # Enables PvP combat
```

### World Settings

**Save world:**
```
/world save
```

**Save all worlds:**
```
/world save --all
```

**Toggle chunk saving:**
```
/world settings chunkSaving <true|false>
```

**Enable/disable PvP:**
```
/world settings pvp <true|false>
```

**Pause/unpause time:**
```
/world settings timepaused <true|false>
```

**Enable/disable NPC spawning:**
```
/world settings spawningnpc <true|false>
```

**Set sanctuary radius (no NPC spawns near players):**
```
/world settings sanctuaryradius <radius>
```

Example:
```
/world settings sanctuaryradius 50
```

### World Generation

**Reload world generation configuration:**
```
/worldgen reload
```

**Reload and clear chunks:**
```
/worldgen reload --clear
```

### World Map

**Discover a zone on the world map:**
```
/worldmap discover <ZoneName>
```

**Undiscover a zone:**
```
/worldmap undiscover <ZoneName>
```

**Clear world map markers:**
```
/worldmap clearmarkers
```

**Get/set world map view radius:**
```
/worldmap viewradius get
/worldmap viewradius set <radius>
```

## Server Commands

### Server Management

**Stop the server:**
```
/stop
```

**List online players:**
```
/who
```

**Set max player count:**
```
/maxplayers <count>
```

Example:
```
/maxplayers 20
```

**Kick a player:**
```
/kick <username>
```

**Ban a player:**
```
/ban <username> [reason]
```

Example:
```
/ban PlayerName Griefing
```

**Unban a player:**
```
/unban <username>
```

### Whitelist

**Add player to whitelist:**
```
/whitelist add <username>
```

**Remove player from whitelist:**
```
/whitelist remove <username>
```

### Server Communication

**Send message to all players:**
```
/say <message>
```

Example:
```
/say Server restart in 5 minutes!
```

**Send notification to all players:**
```
/notify <message>
```

**Show event title:**
```
/eventtitle <title> [secondary]
```

Example:
```
/eventtitle "Boss Defeated!" "Victory"
```

**Show as major event:**
```
/eventtitle <title> --major
```

### Backup

**Run server backup:**
```
/backup
```

## Lighting Commands

**Get light value at position:**
```
/light get <x> <y> <z>
```

Example:
```
/light get 100 65 -50
```

**Invalidate lighting (force update):**
```
/invalidatelighting
```

**Invalidate only current chunk:**
```
/invalidatelighting --one
```

**Change lighting calculation:**
```
/lightingcalculation <type>
```

Types:
- `flood` - Normal lighting
- `fullbright` - Full brightness everywhere

Example:
```
/lightingcalculation fullbright
```

**Get lighting info:**
```
/lighting info
```

**Get detailed lighting info:**
```
/lighting info --detail
```

## View Distance Commands

**Get max view radius:**
```
/maxviewradius
```

**Set view radius in chunks:**
```
/maxviewradius setChunks <chunks>
```

Example:
```
/maxviewradius setChunks 16
```

**Set view radius in blocks:**
```
/maxviewradius setBlocks <blocks>
```

Example:
```
/maxviewradius setBlocks 256
```

## Chunk Commands

**Get chunk information:**
```
/chunk info <x> <z>
```

**Load a chunk:**
```
/chunk load <x> <z>
```

**Unload a chunk:**
```
/chunk unload <x> <z>
```

**Regenerate a chunk:**
```
/chunk regenerate <x> <z>
```

Example:
```
/chunk regenerate 10 -5
```

**Mark chunk for saving:**
```
/chunk marksave <x> <z>
```

**Force resend chunks to player:**
```
/chunk resend
```

**Clear chunk cache before resending:**
```
/chunk resend --clearcache
```

**Force all blocks in chunk to tick:**
```
/chunk forcetick <x> <z>
```

**Tint a chunk:**
```
/chunk tint <color> [--radius=<n>] [--sigma=<n>] [--blur]
```

Example:
```
/chunk tint #FF0000 --blur --radius=5
```

**Set chunk send rate:**
```
/chunk maxsendrate sec <rate>
/chunk maxsendrate tick <rate>
```

**Copy chunk:**
```
/copychunk <sourceX> <sourceZ> <destX> <destZ>
```

Example:
```
/copychunk 10 5 20 15
```

Copies chunk from source coordinates to destination.

**Fill chunk with blocks:**
```
/fillchunk <x> <z> <blockType>
```

Example:
```
/fillchunk 10 -5 Rock_Stone
```

Fills entire chunk with specified block type.

**Get chunk lighting info:**
```
/chunklighting <x> <z>
```

Prints detailed lighting information for a chunk to console.

**Select a chunk (builder mode):**
```
/selectchunk
```

**Select a chunk section:**
```
/selectchunksection
```

## Entity Commands

**Count entities in world:**
```
/entity count
```

Shows total number of entities loaded.

**Clone an entity:**
```
/entity clone [entity] [count]
```

Example:
```
/entity clone 5    # Clone entity 5 times
```

**Remove an entity:**
```
/entity remove <entity>
```

**Remove all other entities:**
```
/entity remove <entity> --others
```

**Clean all entities:**
```
/entity clean
```

Removes all entities from the world.

**Set entity LOD ratio:**
```
/entity lod <ratio>
```

**Reset entity LOD to default:**
```
/entity lod --default
```

**Dump entity data:**
```
/entity dump <entityId|UUID>
```

**Resend tracked entities:**
```
/entity resend [player]
```

**Modify entity nameplate:**
```
/entity nameplate <entity> <text>
```

**Remove entity nameplate:**
```
/entity nameplate remove <entity>
```

**Make entity interactable/non-interactable:**
```
/entity interactable [entity]
/entity interactable --disable [entity]
```

**Make entity intangible:**
```
/entity intangible [entity]
/entity intangible --remove [entity]
```

**Make entity invulnerable:**
```
/entity invulnerable [entity]
/entity invulnerable --remove [entity]
```

**Hide entity from adventure players:**
```
/entity hidefromadventureplayers [entity]
/entity hidefromadventureplayers --remove [entity]
```

**Entity effect commands:**
```
/entity effect <effect> [duration]
```

Example:
```
/entity effect Burn 30
```

**Entity snapshot commands:**
```
/entity snapshot length <milliseconds>
/entity snapshot history
```

**Entity stat commands:**
```
/entity stats dump <entity>
/entity stats get <entity> <statName>
/entity stats set <entity> <statName> <value>
/entity stats add <entity> <statName> <amount>
/entity stats settomax <entity> <statName>
/entity stats reset <entity> <statName>
```

**Entity tracker info:**
```
/entity tracker [player]
```

## Block Commands

**Set a block:**
```
/block set <x> <y> <z> <blockType>
```

Example:
```
/block set 100 65 -50 Rock_Stone
```

**Set block to held item:**
```
/block set <x> <y> <z>
```

Uses the block you're currently holding.

**Spawn a block entity:**
```
/spawnblock <blockType> [x] [y] [z]
```

Example:
```
/spawnblock Furniture_Crude_Bed
```

## Builder/Creative Mode Commands

### Selection Commands

**Set selection positions:**
```
/pos1
/pos2
```

Sets first and second position of selection to your current location.

**Deselect:**
```
/deselect
```

Clears current selection.

**Expand selection:**
```
/expand <amount> [direction]
```

Example:
```
/expand 10 up
/expand 5
```

**Contract selection:**
```
/contract <amount> [direction]
```

**Shift selection:**
```
/shift <amount> <direction>
```

**Select chunk:**
```
/selectchunk
```

### Editing Commands

**Set blocks in selection:**
```
/set <blockType>
```

Example:
```
/set Rock_Stone
```

**Replace blocks:**
```
/replace <fromBlock> <toBlock>
```

Example:
```
/replace Soil_Dirt Rock_Stone
```

**Fill air blocks:**
```
/fill <blockType>
```

Fills all air within selection with specified block.

**Clear selection:**
```
/clear
```

Sets all blocks in selection to Empty/Air.

**Walls:**
```
/walls <blockType> [thickness]
```

Example:
```
/walls Rock_Stone 1
```

**Hollow:**
```
/hollow <blockType> [thickness]
```

Creates hollow shape with specified wall thickness.

**Stack selection:**
```
/stack <count> [direction]
```

Example:
```
/stack 5 up
```

**Move selection:**
```
/move <amount> [direction]
```

Example:
```
/move 10 north
```

**Submerge selection:**
```
/submerge <fluidType>
```

Example:
```
/submerge Water_Normal
```

Use `Empty` to unsubmerge.

**Tint selection:**
```
/tint <color>
```

Example:
```
/tint #FF0000
```

### Clipboard Commands

**Copy selection:**
```
/copy
```

**Cut selection:**
```
/cut
```

**Paste clipboard:**
```
/paste
```

**Rotate clipboard:**
```
/rotate <degrees>
```

Example:
```
/rotate 90
```

**Flip clipboard:**
```
/flip [direction]
```

Example:
```
/flip horizontal
```

**Edit clipboard:**
```
/edit
```

**Clear clipboard history:**
```
/clearhistory
```

### Undo/Redo

**Undo last change:**
```
/undo
```

**Redo last change:**
```
/redo
```

**Enable/disable selection history:**
```
/selectionHistory <true|false>
```

**Set tool history size:**
```
/settoolhistorysize <size>
```

Example:
```
/settoolhistorysize 100
```

## Prefab Commands

**List prefabs:**
```
/prefab list
```

**Load prefab:**
```
/prefab load <prefabName>
```

**Save prefab:**
```
/prefab save <prefabName>
```

**Edit prefab:**
```
/editprefab <subcommands>
```

**Prefab spawner commands:**
```
/prefabspawner get
/prefabspawner set <prefabPath>
/prefabspawner weight <prefab> <weight>
```

**Convert prefabs:**
```
/convertprefabs [--blocks] [--entities] [--filler] [--relative] [--destructive]
```

## NPC and Spawning Commands

**NPC path commands:**
```
/npcpath list
/npcpath create <name>
/npcpath delete <name>
/npcpath edit <name>
```

**Spawning commands:**
```
/spawning <subcommands>
```

Controls NPC spawning behavior.

**Block spawner commands:**
```
/blockspawner <subcommands>
```

## Memory and Reputation Commands

**Memory commands:**
```
/memories <subcommands>
```

Manage NPC memories.

**Reputation commands:**
```
/reputation <subcommands>
```

Manage player reputation with NPC factions.

## Instance and World Port Commands

**Instance commands:**
```
/instances <subcommands>
```

Manage world instances and portals.

**World port (teleport between worlds):**
```
/worldport <worldName>
```

Example:
```
/worldport Zone2
```

**Fragment commands:**
```
/fragment <subcommands>
```

Portal fragment-specific commands.

**Leave fragment:**
```
/leave
```

Leaves the current portal fragment world.

**Void event commands:**
```
/voidevent <subcommands>
```

## Home and Warp Commands

**Set your home:**
```
/sethome
```

**Teleport to home:**
```
/home
```

**Teleport to spawn:**
```
/spawn
```

**Teleport to top (surface):**
```
/top
```

**Warp commands:**
```
/warp <warpName>
/warp list
/warp set <name>
/warp delete <name>
```

## Checkpoint Commands

**Checkpoint commands (parkour system):**
```
/checkpoint set
/checkpoint clear
/checkpoint list
```

## Item Commands

**Spawn item entity:**
```
/spawnitem <itemId> [quantity] [--count=<n>] [--force=<n>]
```

Example:
```
/spawnitem Weapon_Sword_Iron 1 --count=5 --force=2.0
```

Spawns 5 item entities with 1 sword each, thrown with 2x force.

**Set item state:**
```
/itemstate <state>
```

**Curse held item:**
```
/cursethis
```

Makes the held item unable to be dropped or destroyed.

**Open held item container:**
```
/inventoryitem
```

## Mount and Vehicle Commands

**Dismount from vehicle/mount:**
```
/dismount
```

**Check mounted status:**
```
/check
```

## Permission Commands

**Group permissions:**
```
/perm group list <group>
/perm group add <group> <permissions>
/perm group remove <group> <permissions>
```

**User permissions:**
```
/perm user list <uuid>
/perm user add <uuid> <permissions>
/perm user remove <uuid> <permissions>
```

**User groups:**
```
/perm user group list <uuid>
/perm user group add <uuid> <group>
/perm user group remove <uuid> <group>
```

**Test permissions:**
```
/testperm <permission>
```

**Operator commands:**
```
/op add <player>
/op remove <player>
/op list
```

## Asset and Pack Commands

**List asset packs:**
```
/packs list
```

**Asset commands:**
```
/assets tags <class> <tag>
```

Find assets with specific tags.

**Update assets from git:**
```
/updateassets
```

Runs `git pull` in assets directory.

**Update prefabs:**
```
/updateprefabs <subcommand>
```

Git commands for prefabs.

## Performance and Debug Commands

**Server TPS:**
```
/tps
```

Shows ticks per second for all worlds.

**World performance:**
```
/world perf [--all] [--delta] [--graph]
```

Example:
```
/world perf --graph --width=100 --height=10
```

**Set world TPS:**
```
/world tps <rate>
```

Example:
```
/world tps 30
```

**Reset TPS metrics:**
```
/world tps reset
```

**Network ping:**
```
/ping get [--detail]
/ping graph [--width=<n>] [--height=<n>]
/ping clear
```

**Time dilation:**
```
/time dilation <value>
```

Sets server time scaling (0.01 to 4.0).

Example:
```
/time dilation 0.5    # Slow motion
/time dilation 2.0    # Fast forward
```

**Server stats:**
```
/server stats cpu
/server stats memory
/server stats gc
```

**Force garbage collection:**
```
/server gc
```

**Server dump:**
```
/server dump [--json]
```

**Profiler commands:**
```
/profiler download
/profiler attach
/profiler status
/profiler clean
```

**Packet stats:**
```
/packetStats <packetType>
```

**Latency simulation:**
```
/latencySimulation <subcommands>
```

**Network commands:**
```
/network <subcommands>
```

**Pause/unpause game:**
```
/pause
```

## Metastore Commands

**View metastores:**
```
/metastore
/meta entity
/meta chunk
/meta world
```

## Tag Pattern Commands

**Test tag patterns:**
```
/tagpattern <blockType> <pattern>
```

Example:
```
/tagpattern Rock_Stone "Type=Rock"
```

## Ambience and Weather Commands

**Ambience commands:**
```
/ambience <subcommands>
```

Manage environmental audio.

**Weather commands:**
```
/weather <subcommands>
```

Manage weather systems.

## Player Display Name Commands

**Set display name:**
```
/player displayname set <name>
/displayname set <name>
```

**Clear display name:**
```
/player displayname clear
/displayname clear
```

## Hide/Show Player Commands

**Hide player:**
```
/hide
```

**Show player:**
```
/hide show
```

Makes you invisible/visible to other players.

## HUD Commands

**Toggle builder tools legend:**
```
/builderToolsLegend
```

**HUD test:**
```
/hudtest
```

Hides hotbar to test HUD systems.

## Special Commands

**Hub command:**
```
/hub
```

Returns to Cosmos of Creativity hub.

**LAN discovery:**
```
/landiscovery <enable|disable>
```

**Version info:**
```
/version
```

Shows server version information.

**Send player to another server:**
```
/send <player> <host> <port> <type>
```

**Wait command (for macros):**
```
/wait <seconds>
```

Waits specified time before continuing.

**Sudo (run command as another player):**
```
/sudo <player> <command>
```

## Import Commands

**Import OBJ to voxels:**
```
/importobj
```

Opens OBJ to voxel import tool.

**Import image to blocks:**
```
/importimage
```

Opens image to blocks import tool.

## Interaction Commands

**Execute interaction:**
```
/interaction <subcommands>
```

Test and debug item interactions.

## Hit Detection Commands

**Toggle hit detection debug:**
```
/hitdetection
```

**Get hitbox info:**
```
/hitbox <hitboxId>
```

**Calculate hitbox extents:**
```
/hitbox extents [--threshold=<n>]
```

## Hitbox Collision Commands

**Add/remove hitbox collision:**
```
/hitboxcollision add [entity]
/hitboxcollision remove [entity]
```

## Repulsion Commands

**Add/remove entity repulsion:**
```
/repulsion add [entity]
/repulsion remove [entity]
```

## Particle Commands

**Particle system commands:**
```
/particle <subcommands>
```

Spawn and test particle effects.

## Block Set Commands

**List block sets:**
```
/blockset
```

**Show block set contents:**
```
/blockset <setName>
```

Example:
```
/blockset Soil
```

## Drop List Commands

**Simulate drop list:**
```
/droplist <dropListId>
```

Tests loot table drops.

## Camera Shake Commands

**Camera shake commands:**
```
/camshake <subcommands>
```

Test camera shake effects.

## World Path Commands

**List world paths:**
```
/worldpath list
```

**Remove world path:**
```
/worldpath remove <name>
```

**Save world paths:**
```
/worldpath save
```

**Path builder commands:**
```
/worldpath builder stop
/worldpath builder load <name>
/worldpath builder simulate
/worldpath builder clear
/worldpath builder add
/worldpath builder set [index]
/worldpath builder goto <index>
/worldpath builder remove <index>
/worldpath builder save <name>
```

## Internationalization Commands

**i18n commands:**
```
/i18n <subcommands>
```

**Toggle TMP tags:**
```
/toggleTmpTags
```

Toggles display of [TMP] tags on temporary translation strings.

## Log Commands

**Set log level:**
```
/log <level>
```

Levels: `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`

**Open server logs:**
```
/logs
```

Opens server log viewer page.

## Stress Test Commands

**Server stress testing:**
```
/stresstest <subcommands>
```

## PID Check Commands

**Check if process is running:**
```
/pidcheck <pid>
```

**Check client PID (singleplayer):**
```
/pidcheck --singleplayer
```

## Debug Player Position

**Debug player position sync:**
```
/debugplayerposition
```

## Authentication Commands

**Auth commands:**
```
/auth dump
/auth refresh
/auth reload
```

## Plugin Commands

**List plugins:**
```
/plugin list
```

**Load plugin:**
```
/plugin load <pluginId>
```

**Unload plugin:**
```
/plugin unload <pluginId>
```

**Reload plugin:**
```
/plugin reload <pluginId>
```

## Hotbar Commands

**Set hotbar to saved value:**
```
/hotbar <slot>
```

## Block Placement Override

**Toggle block placement rules:**
```
/toggleBlockPlacementOverride
```

Allows breaking block placement rules.

## Clear Entities

**Clear entities in selection:**
```
/clearEntities
```

Removes all entities within current selection.

## Debug Commands

### Entity Information

**Dump entity data:**
```
/entity dump <entityId|UUID>
```

**Get entity metastore:**
```
/meta entity
```

**Get chunk metastore:**
```
/meta chunk
```

**Get world metastore:**
```
/meta world
```

### Model Commands

**Open model selection:**
```
/model
```

**Set player model:**
```
/model set <ModelName> [--scale=<n>] [--bypassScaleLimits]
```

Example:
```
/model set NPC/Beast/Emberwulf/Models/Model --scale=2.0
```

**Reset player model:**
```
/model reset [--scale=<n>]
```

### Item State

**Set held item state:**
```
/itemstate <state>
```

## Validate Commands

**Validate CPB files:**
```
/validatecpb [path]
```

Validates Compressed Prefab Buffer files.

## Edit Line Command

**Draw line of blocks:**
```
/editline <blockType>
```

Draws a line between two selected points.

## Environment Commands

**Set environment in selection:**
```
/environment <environmentId>
```

Example:
```
/environment Env_Zone1_Plains
```

## Edit Selection Commands

**Edit selection properties:**
```
/editselection <subcommands>
```

## Extend Face Command

**Extend target face:**
```
/extendface [amount]
```

## Global Mask Commands

**Set global block mask:**
```
/globalmask <pattern>
```

## Repair Fillers

**Repair filler blocks:**
```
/repairfillers
```

## Selection History

**Record selection changes:**
```
/selectionHistory <true|false>
```

## Update Selection

**Update current selection:**
```
/updateselection
```

## Builder Tools Commands

These commands are available in builder/creative mode:

**Clear entities in selection:**
```
/clearEntities
```

Removes all entities within your current selection.

**Import image:**
```
/importimage
```

Opens the image-to-blocks import tool for creating pixel art.

**Import OBJ model:**
```
/importobj
```

Opens the OBJ-to-voxel import tool for converting 3D models.

## Explosive Toggle

**Toggle explosives:**
```
/toggleexplosives
```

Enables or disables explosive damage in the world.

## Force Chunk Tick

**Force all blocks in chunk to tick:**
```
/forcechunktick <x> <z>
```

Example:
```
/forcechunktick 10 -5
```

Marks every block in the chunk to update/tick.

## Stash Commands

**Set droplist for container:**
```
/stash setDroplist <droplistId>
```

**Get droplist from container:**
```
/stash getDroplist
```

Look at a chest and use these commands to view or set its loot table.

## Tips and Tricks

### Command History

- Press **Up Arrow** to cycle through previous commands
- Press **Down Arrow** to cycle forward through command history

### Tab Completion

- Press **Tab** to auto-complete command names and arguments
- Press **Tab** multiple times to cycle through available options

### Common Command Patterns

**Teleport to spawn:**
```
/tp 0 100 0
```

**Full heal:**
```
/heal
```

Or manually:
```
/player stat settomax Health
/player stat settomax Stamina
/player stat settomax Mana
```

**Set time to noon:**
```
/time set 12
```

Or use the macro:
```
/noon
```

**Clear inventory and give starter kit:**
```
/inventoryclear
/give Tool_Pickaxe_Crude 1
/give Tool_Hatchet_Crude 1
/give Weapon_Sword_Iron 1
```

**Test weapon with infinite health:**
```
/player stat settomax Health
/player stat set Health 99999
```

## Advanced Usage

### Chaining Commands with Macros

You can create custom macro commands to execute multiple commands. See the [Creating Commands](41_Commands.md) guide for details on creating custom macros.

### Using Flags

Many commands support optional flags to modify behavior:

```
--world=<WorldName>    # Specify world
--all                  # Apply to all
--clear                # Clear before applying
--force                # Force operation
```

Example:
```
/tp home --world=MyWorld
/world save --all
```

### Relative Coordinates

Use `~` for relative positioning:

```
/tp ~ ~10 ~      # Teleport 10 blocks up
/tp ~5 ~ ~-5     # Teleport 5 blocks east and 5 blocks north
```

### Finding Item IDs

To find item IDs for commands like `/give` and `/recipe`:

1. Look in `Server/Item/Items/` folders
2. The filename (without `.json`) is the Item ID
3. Example: `Weapon_Sword_Iron.json` → `Weapon_Sword_Iron`

Common prefixes:
- `Weapon_` - Weapons
- `Tool_` - Tools
- `Food_` - Food items
- `Potion_` - Potions
- `Ingredient_` - Crafting ingredients
- `Furniture_` - Furniture blocks
- `Plant_` - Plants and seeds

### Finding Effect IDs

Effect IDs are found in `Server/EntityEffect/` folders. Common effects:

- `Burn` - Fire damage over time
- `Poison` - Poison damage
- `Regeneration` - Health regeneration
- `Speed` - Movement speed boost
- `Slowness` - Movement speed reduction

### Finding Sound Event IDs

Sound event IDs are found in `Client/Sound/` folders and typically start with `SFX_` or `Music_`.

Examples:
- `SFX_Sword_T2_Impact` - Sword impact sound
- `SFX_Explosion` - Explosion sound
- `Music_Theme_Main` - Main theme music

## Troubleshooting

### Command Not Found

If you get a "command not found" error:

1. Check spelling and capitalization
2. Type `/help` to see available commands
3. Ensure you have permission to use the command
4. Check if the command requires admin/operator privileges

### Invalid Arguments

If you get "invalid argument" errors:

1. Check the command syntax with `/help <command>`
2. Ensure Item IDs are spelled exactly correct (case-sensitive)
3. Use quotes for arguments with spaces: `/say "Hello World"`
4. Check that numerical values are in valid ranges

### Permission Denied

Some commands require operator/admin permissions. If you're running a server:

1. Grant yourself operator status in server configuration
2. Check permission settings
3. Contact server administrator

---

**Related Guides:**
- [Creating Commands](41_Commands.md) - Creating custom macro commands
- [Macro Commands](167_Macro_Commands.md) - Advanced macro command reference
- [Items](02_Items.md) - Item ID reference

---

**Previous:** [Offhand Item Stats](170_Offhand_Item_Stats.md) | **Next:** [Builder Tools Guide](172_Builder_Tools_Guide.md)
