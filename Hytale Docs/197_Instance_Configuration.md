# Instance Configuration

Learn how to create and configure game instances like dungeons, portals, and challenge arenas.

## Overview

Instances are separate playable areas in Hytale with their own world generation, rules, and behavior. They include dungeons, portal destinations, creative hubs, and challenge arenas. Each instance has a `config.json` that defines its properties.

## Location

`Server/Instances/{InstanceName}/`

Structure:
- **`config.json`** - Instance configuration (only some instances have this)
- **`instance.bson`** - Binary instance data
- **`meta.bson`** - Metadata (for instances with chunks)
- **`chunks/`** - Region files (`.region.bin`)
- **`resources/`** - Instance resources (InstanceData, Time, etc.)

## Available Instances

| Instance | Type | Description |
|----------|------|-------------|
| `Basic` | Template | Minimal instance template |
| `CreativeHub` | Hub | Creative mode hub area |
| `Default` | World | Default overworld |
| `Default_Flat` | World | Flat world template |
| `Default_Void` | World | Void world template |
| `Forgotten_Temple` | Dungeon | Temple dungeon instance |
| `Dungeon_1` | Dungeon | Generic dungeon |
| `Dungeon_Goblin` | Dungeon | Goblin dungeon |
| `Dungeon_Outlander` | Dungeon | Outlander dungeon |
| `Challenge_Combat_1` | Challenge | Combat challenge arena |
| `NPC_Gym` | Testing | NPC testing area |
| `NPC_Faction_Gym` | Testing | Faction testing area |
| `Portals_Henges` | Portal | Henges portal destination |
| `Portals_Hedera` | Portal | Hedera's lair destination |
| `Portals_Jungles` | Portal | Jungle portal destination |
| `Portals_Oasis` | Portal | Oasis portal destination |
| `Portals_Taiga` | Portal | Taiga portal destination |
| `Zone1_Plains1` | Zone | Plains zone instance |
| `Zone2_Desert1` | Zone | Desert zone instance |
| `Zone3_Taiga1` | Zone | Taiga zone instance |
| `Zone4_Volcanic1` | Zone | Volcanic zone instance |

## Instance config.json

### Example: Creative Hub

From `Server/Instances/CreativeHub/config.json`:

```json
{
  "Version": 4,
  "UUID": {
    "$binary": "Ds/3XgghTq2JYJNzWFqVMQ==",
    "$type": "04"
  },
  "Seed": 1618917989368,
  "SpawnProvider": {
    "Id": "Global",
    "SpawnPoint": {
      "X": 5103.5,
      "Y": 168.0,
      "Z": 4982.5,
      "Pitch": 0.0,
      "Yaw": 90.0,
      "Roll": 0.0
    }
  },
  "WorldGen": {
    "Type": "Hytale",
    "Name": "Instance_Creative_Hub",
    "Environment": "Env_Creative_Hub"
  },
  "WorldMap": {
    "Type": "Disabled"
  },
  "ChunkStorage": {
    "Type": "Hytale"
  },
  "ChunkConfig": {},
  "IsTicking": false,
  "IsBlockTicking": false,
  "IsPvpEnabled": false,
  "IsFallDamageEnabled": true,
  "IsGameTimePaused": true,
  "GameTime": "0001-01-01T16:50:44.090953998Z",
  "RequiredPlugins": {},
  "GameMode": "Creative",
  "IsSpawningNPC": false,
  "IsSpawnMarkersEnabled": true,
  "IsAllNPCFrozen": true,
  "GameplayConfig": "CreativeHub",
  "IsCompassUpdating": true,
  "IsSavingPlayers": false,
  "IsSavingChunks": true,
  "SaveNewChunks": true,
  "IsUnloadingChunks": true,
  "IsObjectiveMarkersEnabled": false,
  "DeleteOnUniverseStart": false,
  "DeleteOnRemove": false,
  "ResourceStorage": {
    "Type": "Hytale"
  },
  "Plugin": {
    "Instance": {
      "RemovalConditions": [],
      "PreventReconnection": true,
      "Discovery": {
        "TitleKey": "server.instances.creative_hub.title",
        "SubtitleKey": "server.instances.creative_hub.subtitle",
        "Display": true,
        "AlwaysDisplay": false,
        "Icon": "Forgotten_Temple.png",
        "Major": true,
        "Duration": 4.0,
        "FadeInDuration": 1.5,
        "FadeOutDuration": 1.5
      }
    }
  }
}
```

### Example: Forgotten Temple (Dungeon)

From `Server/Instances/Forgotten_Temple/config.json`:

```json
{
  "Version": 4,
  "UUID": {
    "$binary": "YDOsXrt3TOmXhNGwFrXXBA==",
    "$type": "04"
  },
  "Seed": 1618917989368,
  "SpawnProvider": {
    "Id": "Global",
    "SpawnPoint": {
      "X": 5095.0,
      "Y": 169.0,
      "Z": 4983.5,
      "Pitch": 0.0,
      "Yaw": 90.0,
      "Roll": 0.0
    }
  },
  "WorldGen": {
    "Type": "Hytale",
    "Name": "Instance_Forgotten_Temple"
  },
  "ChunkStorage": {
    "Type": "Hytale"
  },
  "ChunkConfig": {},
  "IsTicking": false,
  "IsBlockTicking": true,
  "IsPvpEnabled": false,
  "IsFallDamageEnabled": true,
  "IsGameTimePaused": true,
  "GameTime": "0001-01-04T08:00:00.000858306Z",
  "RequiredPlugins": {},
  "GameMode": "Creative",
  "IsSpawningNPC": true,
  "IsSpawnMarkersEnabled": true,
  "IsAllNPCFrozen": true,
  "GameplayConfig": "ForgottenTemple",
  "IsCompassUpdating": true,
  "IsSavingPlayers": false,
  "IsSavingChunks": true,
  "SaveNewChunks": true,
  "IsUnloadingChunks": true,
  "IsObjectiveMarkersEnabled": false,
  "DeleteOnUniverseStart": false,
  "DeleteOnRemove": false,
  "ResourceStorage": {
    "Type": "Hytale"
  },
  "Plugin": {
    "Instance": {
      "RemovalConditions": [],
      "PreventReconnection": false,
      "Discovery": {
        "TitleKey": "server.instances.forgotten_temple.title",
        "SubtitleKey": "server.instances.forgotten_temple.subtitle",
        "Display": true,
        "AlwaysDisplay": true,
        "Icon": "Forgotten_Temple.png",
        "Major": true,
        "Duration": 4.0,
        "FadeInDuration": 1.5,
        "FadeOutDuration": 1.5
      }
    }
  }
}
```

## Config Properties Reference

### Core Properties

| Property | Type | Description |
|----------|------|-------------|
| `Version` | Integer | Config version (currently 4) |
| `UUID` | Object | Unique binary identifier |
| `Seed` | Integer | World generation seed |
| `GameMode` | String | Default game mode (`"Creative"`, `"Adventure"`) |
| `GameplayConfig` | String | References `Server/GameplayConfigs/` |
| `GameTime` | String | Initial time (ISO format) |
| `RequiredPlugins` | Object | Plugins required for this instance |

### SpawnProvider

Controls where players spawn:

```json
{
  "SpawnProvider": {
    "Id": "Global",
    "SpawnPoint": {
      "X": 5103.5,
      "Y": 168.0,
      "Z": 4982.5,
      "Pitch": 0.0,
      "Yaw": 90.0,
      "Roll": 0.0
    }
  }
}
```

| Property | Description |
|----------|-------------|
| `Id` | Spawn provider type (`"Global"`) |
| `SpawnPoint.X/Y/Z` | World coordinates |
| `SpawnPoint.Pitch` | Vertical look angle |
| `SpawnPoint.Yaw` | Horizontal look angle |
| `SpawnPoint.Roll` | Roll rotation |

### WorldGen

Configures world generation:

```json
{
  "WorldGen": {
    "Type": "Hytale",
    "Name": "Instance_Creative_Hub",
    "Environment": "Env_Creative_Hub"
  }
}
```

| Property | Description |
|----------|-------------|
| `Type` | Generator type (`"Hytale"`) |
| `Name` | World structure name (from `Server/HytaleGenerator/WorldStructures/`) |
| `Environment` | Environment config (from `Server/Environments/`) |

### WorldMap

```json
{
  "WorldMap": {
    "Type": "Disabled"
  }
}
```

Set to `"Disabled"` to hide the world map for this instance.

### Behavior Flags

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `IsTicking` | Boolean | true | Whether entity updates run |
| `IsBlockTicking` | Boolean | true | Whether block updates run |
| `IsPvpEnabled` | Boolean | false | Allow player vs player combat |
| `IsFallDamageEnabled` | Boolean | true | Players take fall damage |
| `IsGameTimePaused` | Boolean | false | Freeze day/night cycle |
| `IsSpawningNPC` | Boolean | true | NPCs can spawn |
| `IsSpawnMarkersEnabled` | Boolean | true | Spawn markers active |
| `IsAllNPCFrozen` | Boolean | false | Freeze all NPC AI |
| `IsCompassUpdating` | Boolean | true | Compass updates |
| `IsObjectiveMarkersEnabled` | Boolean | false | Show objective markers |

### Chunk Settings

| Property | Type | Description |
|----------|------|-------------|
| `ChunkStorage.Type` | String | Storage type (`"Hytale"`) |
| `ChunkConfig` | Object | Chunk configuration |
| `IsSavingChunks` | Boolean | Save chunk changes |
| `SaveNewChunks` | Boolean | Save newly generated chunks |
| `IsUnloadingChunks` | Boolean | Unload distant chunks |
| `IsSavingPlayers` | Boolean | Save player data |

### Instance Lifecycle

| Property | Type | Description |
|----------|------|-------------|
| `DeleteOnUniverseStart` | Boolean | Delete when server starts |
| `DeleteOnRemove` | Boolean | Delete when instance removed |
| `ResourceStorage.Type` | String | Resource storage type |

### Plugin Settings

```json
{
  "Plugin": {
    "Instance": {
      "RemovalConditions": [],
      "PreventReconnection": true,
      "Discovery": {
        "TitleKey": "server.instances.creative_hub.title",
        "SubtitleKey": "server.instances.creative_hub.subtitle",
        "Display": true,
        "AlwaysDisplay": false,
        "Icon": "Forgotten_Temple.png",
        "Major": true,
        "Duration": 4.0,
        "FadeInDuration": 1.5,
        "FadeOutDuration": 1.5
      }
    }
  }
}
```

#### Discovery Settings

Control the UI notification when entering an instance:

| Property | Type | Description |
|----------|------|-------------|
| `TitleKey` | String | Localization key for title |
| `SubtitleKey` | String | Localization key for subtitle |
| `Display` | Boolean | Show discovery notification |
| `AlwaysDisplay` | Boolean | Show even on re-entry |
| `Icon` | String | Icon image filename |
| `Major` | Boolean | Large notification style |
| `Duration` | Float | Display time in seconds |
| `FadeInDuration` | Float | Fade in time |
| `FadeOutDuration` | Float | Fade out time |

#### Other Plugin Settings

| Property | Description |
|----------|-------------|
| `RemovalConditions` | Array of conditions to remove instance |
| `PreventReconnection` | Prevent returning after leaving |

## Instance Resources

Each instance has a `resources/` folder with runtime data:

### InstanceData.json

```json
{
  "HadPlayer": false
}
```

Tracks whether a player has visited this instance.

### Time.json

```json
{
  "Value": "0001-01-01T00:00:00Z"
}
```

Current instance time.

### SpawnSuppressionController.json

```json
{
  "Suppressions": []
}
```

Active spawn suppressions.

### ReputationData.json

```json
{
  "Stats": {}
}
```

Faction reputation data for this instance.

### PrefabEditSession.json

```json
{
  "Sessions": {}
}
```

Prefab editing session data.

## Portal Types

Portal types define destinations for portal blocks:

### Example: Henges

From `Server/PortalTypes/Henges.json`:

```json
{
  "InstanceId": "Portals_Henges",
  "Description": {
    "DisplayName": "server.portals.henges",
    "FlavorText": "server.portals.henges.description",
    "ThemeColor": "#81fdedff",
    "SplashImage": "DefaultArtwork.png"
  },
  "PlayerSpawn": {
    "Y": 120,
    "ScanHeight": 20,
    "MinRadius": 250,
    "MaxRadius": 250
  }
}
```

### Example: Hedera's Lair

From `Server/PortalTypes/Hederas_Lair.json`:

```json
{
  "InstanceId": "Portals_Hedera",
  "Description": {
    "DisplayName": "server.portals.hederas_lair",
    "FlavorText": "server.portals.hederas_lair.description",
    "ThemeColor": "#23970cec",
    "SplashImage": "DefaultArtwork.png",
    "Tips": [
      "server.portals.hederas_lair.tip1",
      "server.portals.hederas_lair.tip2"
    ]
  },
  "VoidInvasionEnabled": true,
  "PlayerSpawn": {
    "Y": 110,
    "ScanHeight": 16,
    "MinRadius": 60,
    "MaxRadius": 60
  }
}
```

### Portal Type Properties

| Property | Type | Description |
|----------|------|-------------|
| `InstanceId` | String | Target instance name |
| `Description.DisplayName` | String | Localization key |
| `Description.FlavorText` | String | Description localization key |
| `Description.ThemeColor` | String | Hex color with alpha |
| `Description.SplashImage` | String | Loading screen image |
| `Description.Tips` | Array | Loading screen tip keys |
| `VoidInvasionEnabled` | Boolean | Enable void invasion events |
| `PlayerSpawn.Y` | Integer | Spawn height to search from |
| `PlayerSpawn.ScanHeight` | Integer | Height range to scan |
| `PlayerSpawn.MinRadius` | Integer | Minimum spawn radius |
| `PlayerSpawn.MaxRadius` | Integer | Maximum spawn radius |
| `PlayerSpawn.ChunkDartThrows` | Integer | Random spawn attempts |

## Creating Custom Instances

### 1. Create Instance Folder

Create `Server/Instances/MyDungeon/`:

```
MyDungeon/
├── config.json
├── instance.bson
└── resources/
    ├── InstanceData.json
    ├── PrefabEditSession.json
    ├── ReputationData.json
    ├── SpawnSuppressionController.json
    └── Time.json
```

### 2. Create config.json

```json
{
  "Version": 4,
  "Seed": 12345,
  "SpawnProvider": {
    "Id": "Global",
    "SpawnPoint": {
      "X": 0.0,
      "Y": 100.0,
      "Z": 0.0,
      "Pitch": 0.0,
      "Yaw": 0.0,
      "Roll": 0.0
    }
  },
  "WorldGen": {
    "Type": "Hytale",
    "Name": "MyDungeon_WorldStructure"
  },
  "ChunkStorage": {
    "Type": "Hytale"
  },
  "IsTicking": true,
  "IsBlockTicking": true,
  "IsPvpEnabled": false,
  "IsFallDamageEnabled": true,
  "IsGameTimePaused": true,
  "GameMode": "Adventure",
  "IsSpawningNPC": true,
  "IsSpawnMarkersEnabled": true,
  "IsAllNPCFrozen": false,
  "GameplayConfig": "Default",
  "IsSavingChunks": true,
  "SaveNewChunks": true,
  "IsUnloadingChunks": true,
  "DeleteOnUniverseStart": false,
  "DeleteOnRemove": false,
  "ResourceStorage": {
    "Type": "Hytale"
  },
  "Plugin": {
    "Instance": {
      "RemovalConditions": [],
      "PreventReconnection": false,
      "Discovery": {
        "TitleKey": "server.instances.my_dungeon.title",
        "SubtitleKey": "server.instances.my_dungeon.subtitle",
        "Display": true,
        "AlwaysDisplay": true,
        "Icon": "MyDungeon.png",
        "Major": true,
        "Duration": 4.0,
        "FadeInDuration": 1.5,
        "FadeOutDuration": 1.5
      }
    }
  }
}
```

### 3. Create Portal Type (Optional)

Create `Server/PortalTypes/MyDungeon.json`:

```json
{
  "InstanceId": "MyDungeon",
  "Description": {
    "DisplayName": "server.portals.my_dungeon",
    "FlavorText": "server.portals.my_dungeon.description",
    "ThemeColor": "#ff5500ff",
    "SplashImage": "MyDungeon.png",
    "Tips": [
      "server.portals.my_dungeon.tip1"
    ]
  },
  "PlayerSpawn": {
    "Y": 100,
    "ScanHeight": 20,
    "MinRadius": 10,
    "MaxRadius": 50
  }
}
```

## Tips

1. **Use GameplayConfig** - Reference a GameplayConfig to set death penalties, time settings, etc.
2. **Pause time for dungeons** - Set `IsGameTimePaused: true` to keep dungeons at fixed time
3. **Freeze NPCs for hubs** - Use `IsAllNPCFrozen: true` for peaceful hub areas
4. **Disable NPC spawning** - Set `IsSpawningNPC: false` for controlled environments
5. **Discovery notifications** - Use `AlwaysDisplay: true` for important areas

## Related Documentation

- [Server Modding](12_Server_Modding.md) - Instance resources
- [Portal Types](46_Portal_Types.md) - Portal configuration
- [Gameplay Configs](189_Gameplay_Configs.md) - Game rule configuration
- [Spawn Suppression](192_Spawn_Suppression.md) - NPC spawn control
- [World Generation](38_World_Generation.md) - WorldGen configuration

---

**Previous:** [Adding Prefabs To World Generation](196_Adding_Prefabs_To_World_Generation.md) | **Next:** [View Bobbing](198_View_Bobbing.md)
