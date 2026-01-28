# Server Modding

## Overview

Server modding in Hytale involves creating custom **server instances** (separate game worlds) and **gameplay configs** (rules and behaviors). Unlike asset-based modding (items, NPCs), server modding focuses on configuring how the game world behaves and what rules apply.

## Creating Server Instances

### Overview

Server instances are separate game worlds with their own settings, world generation, and behaviors. Each instance is a self-contained server that players can join.

### Location
`Server/Instances/YourInstanceName/`

## Example from Game Files

### Default Gameplay Config

From `Server/GameplayConfigs/Default.json`:

```1:60:Server/GameplayConfigs/Default.json
{
  "Gathering": {
    "UnbreakableBlock": {
      "ParticleSystemId": "Block_Break_Dust",
      "SoundEventId": "SFX_Unbreakable_Block"
    },
    "IncorrectTool": {
      "ParticleSystemId": "Block_Break_Dust",
      "SoundEventId": "SFX_Unbreakable_Block"
    }
  },
  "Death": {
    "ItemsLossMode": "Configured",
    "ItemsAmountLossPercentage": 50.0,
    "ItemsDurabilityLossPercentage": 10.0
  },
  "ItemEntity": {
    "Lifetime": 600.0
  },
  "ItemDurability": {
    "BrokenPenalties": {
      "Weapon": 0.75,
      "Armor": 0.75,
      "Tool": 0.75
    }
  },
  "Plugin": {
    "Stamina": {
      "SprintRegenDelay": {
        "EntityStatId": "StaminaRegenDelay",
        "Value": -0.75
      }
    },
    "Memories": {
      "MemoriesAmountPerLevel": [10, 25, 50, 100, 200],
      "MemoriesRecordParticles": "MemoryRecordedStatue",
      "MemoriesCatchItemId": "Memory_Particle",
      "MemoriesCatchEntityParticle": {
        "SystemId": "Memory_Catch_Rune",
        "TargetNodeName": "Head"
      },
      "MemoriesCatchParticleViewDistance": 64
    }
  },
  "Respawn": {
    "RadiusLimitRespawnPoint": 500,
    "MaxRespawnPointsPerPlayer": 3
  },
  "World": {
    "DaytimeDurationSeconds": 1728,
    "NighttimeDurationSeconds": 1152,
    "BlockPlacementFragilityTimer": 0,
    "Sleep": {
      "WakeUpHour": 4.79,
      "AllowedSleepHoursRange": [ 19.5, 4.79 ]
    }
  },
  "Player": {
    "MovementConfig": "Default",
    "HitboxCollisionConfig": "SoftCollision"
```

This shows a complete gameplay config with gathering settings, death mechanics, item durability, stamina system, and world configuration.

### Basic Instance Structure

Create a folder `Server/Instances/MyCustomServer/` with `instance.bson` or `config.json`:

**Using JSON format (`config.json`):**

```json
{
  "Version": 4,
  "UUID": {
    "$binary": "YourUUIDHere",
    "$type": "04"
  },
  "Seed": 123456789,
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
    "Type": "HytaleGenerator",
    "WorldStructure": "Basic",
    "PlayerSpawn": {
      "X": 0,
      "Y": 400,
      "Z": 0
    }
  },
  "GameMode": "Adventure",
  "GameplayConfig": "Default",
  "IsPvpEnabled": false,
  "IsSpawningNPC": true,
  "IsTicking": true,
  "IsBlockTicking": true,
  "IsSavingPlayers": true,
  "IsSavingChunks": true
}
```

### Instance Properties

| Property | Type | Description | Example Values |
|----------|------|-------------|----------------|
| `GameMode` | String | Game mode type | `"Adventure"`, `"Creative"` |
| `GameplayConfig` | String | Config file ID | `"Default"`, `"CreativeHub"` |
| `IsPvpEnabled` | Boolean | Enable PvP combat | `true`, `false` |
| `IsSpawningNPC` | Boolean | Spawn NPCs naturally | `true`, `false` |
| `IsTicking` | Boolean | World time progresses | `true`, `false` |
| `IsBlockTicking` | Boolean | Blocks update (crops, redstone) | `true`, `false` |
| `IsGameTimePaused` | Boolean | Pause in-game time | `true`, `false` |
| `IsSavingPlayers` | Boolean | Save player data | `true`, `false` |
| `IsSavingChunks` | Boolean | Save world chunks | `true`, `false` |
| `DeleteOnRemove` | Boolean | Delete instance when removed | `true`, `false` |

### World Generation Types

#### 1. Hytale Generator (Procedural World)

```json
{
  "WorldGen": {
    "Type": "HytaleGenerator",
    "WorldStructure": "Basic",
    "PlayerSpawn": {
      "X": 0,
      "Y": 400,
      "Z": 0,
      "Pitch": 0,
      "Yaw": 0,
      "Roll": 0
    }
  }
}
```

**WorldStructure options:**
- `"Basic"` - Standard procedural world
- Other structure types defined in `Server/HytaleGenerator/WorldStructures/`

#### 2. Flat World

```json
{
  "WorldGen": {
    "Type": "Flat",
    "Tint": "#578a24",
    "Layers": [
      {
        "BlockType": "Rock_Stone",
        "Environment": "Env_Zone1_Plains",
        "To": 75,
        "From": 0
      },
      {
        "BlockType": "Soil_Dirt",
        "Environment": "Env_Zone1_Plains",
        "To": 79,
        "From": 75
      },
      {
        "BlockType": "Soil_Grass",
        "Environment": "Env_Zone1_Plains",
        "To": 80,
        "From": 79
      },
      {
        "BlockType": "Empty",
        "Environment": "Env_Zone1_Plains",
        "To": 320,
        "From": 80
      }
    ]
  }
}
```

**Layers define:**
- `From` / `To` - Y coordinate range
- `BlockType` - Block to fill with
- `Environment` - Biome environment

#### 3. Void World

```json
{
  "WorldGen": {
    "Type": "Void",
    "Tint": "#00aa00",
    "Environment": "Default"
  }
}
```

#### 4. Hytale World (Pre-made)

```json
{
  "WorldGen": {
    "Type": "Hytale",
    "Name": "Instance_Creative_Hub",
    "Environment": "Env_Creative_Hub"
  }
}
```

### Spawn Configuration

#### Global Spawn Point

```json
{
  "SpawnProvider": {
    "Id": "Global",
    "SpawnPoint": {
      "X": 0.0,
      "Y": 100.0,
      "Z": 0.0,
      "Pitch": 0.0,
      "Yaw": 90.0,
      "Roll": 0.0
    }
  }
}
```

### Instance Discovery (Plugin Settings)

Configure how instances appear to players:

```json
{
  "Plugin": {
    "Instance": {
      "RemovalConditions": [],
      "PreventReconnection": false,
      "Discovery": {
        "TitleKey": "server.instances.my_custom_server.title",
        "SubtitleKey": "server.instances.my_custom_server.subtitle",
        "Display": true,
        "AlwaysDisplay": false,
        "Icon": "MyCustomServer.png",
        "Major": true,
        "Duration": 4.0,
        "FadeInDuration": 1.5,
        "FadeOutDuration": 1.5
      }
    }
  }
}
```

**RemovalConditions** can include:
```json
{
  "Type": "WorldEmpty",
  "TimeoutSeconds": 300
}
```

### Required Plugins

Specify plugins that must be loaded:

```json
{
  "RequiredPlugins": {
    "PluginName1": {},
    "PluginName2": {}
  }
}
```

### Instance Resources Folder

Instances can have a `resources/` folder with additional data:

```
Server/Instances/MyCustomServer/
├── instance.bson (or config.json)
└── resources/
    ├── InstanceData.json
    ├── PrefabEditSession.json
    ├── ReputationData.json
    ├── SpawnSuppressionController.json
    └── Time.json
```

### Complete Example: PvP Arena Instance

```json
{
  "Version": 4,
  "Seed": 987654321,
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
    "Type": "Flat",
    "Tint": "#444444",
    "Layers": [
      {
        "BlockType": "Rock_Stone",
        "Environment": "Env_Zone1_Plains",
        "To": 100,
        "From": 0
      }
    ]
  },
  "WorldMap": {
    "Type": "Disabled"
  },
  "ChunkStorage": {
    "Type": "Hytale"
  },
  "GameMode": "Adventure",
  "GameplayConfig": "PvPArena",
  "IsPvpEnabled": true,
  "IsSpawningNPC": false,
  "IsTicking": false,
  "IsBlockTicking": false,
  "IsGameTimePaused": true,
  "IsObjectiveMarkersEnabled": false,
  "IsSavingPlayers": false,
  "IsSavingChunks": false,
  "DeleteOnRemove": true,
  "Plugin": {
    "Instance": {
      "RemovalConditions": [
        {
          "Type": "WorldEmpty",
          "TimeoutSeconds": 60
        }
      ],
      "PreventReconnection": true,
      "Discovery": {
        "TitleKey": "server.instances.pvp_arena.title",
        "SubtitleKey": "server.instances.pvp_arena.subtitle",
        "Display": true,
        "Major": false
      }
    }
  }
}
```

---

## Creating Gameplay Configs

### Overview

Gameplay configs define rules and behaviors for your server instances. They control death penalties, respawn behavior, world settings, and more.

### Location
`Server/GameplayConfigs/`

### Basic Gameplay Config Structure

Create `Server/GameplayConfigs/MyCustomConfig.json`:

```json
{
  "Parent": "Default",
  "World": {
    "AllowBlockBreaking": true,
    "AllowBlockGathering": true,
    "AllowBlockPlacement": true
  },
  "Death": {
    "ItemsLossMode": "Configured",
    "ItemsAmountLossPercentage": 25.0,
    "ItemsDurabilityLossPercentage": 10.0
  }
}
```

### Inheritance

Gameplay configs can inherit from parent configs and override specific settings:

```json
{
  "Parent": "Default",
  "Death": {
    "ItemsLossMode": "None"
  }
}
```

### Gameplay Config Categories

#### World Settings

```json
{
  "World": {
    "AllowBlockBreaking": true,
    "AllowBlockGathering": true,
    "AllowBlockPlacement": true,
    "BlockPlacementFragilityTimer": 0,
    "DaytimeDurationSeconds": 1728,
    "NighttimeDurationSeconds": 1152,
    "Sleep": {
      "WakeUpHour": 4.79,
      "AllowedSleepHoursRange": [21, 4.79]
    }
  }
}
```

#### Death & Respawn

```json
{
  "Death": {
    "ItemsLossMode": "Configured",
    "ItemsAmountLossPercentage": 50.0,
    "ItemsDurabilityLossPercentage": 10.0,
    "RespawnController": {
      "Type": "WorldSpawnPoint"
    }
  },
  "Respawn": {
    "RadiusLimitRespawnPoint": 500,
    "MaxRespawnPointsPerPlayer": 3
  }
}
```

**ItemsLossMode options:**
- `"None"` - No item loss on death
- `"Configured"` - Use percentage loss settings
- `"All"` - Drop all items

#### Combat Settings

```json
{
  "Combat": {
    "DisableNPCIncomingDamage": false,
    "DisablePlayerIncomingDamage": false
  }
}
```

#### Gathering Configuration

```json
{
  "Gathering": {
    "UnbreakableBlock": {
      "ParticleSystemId": "Block_Break_Dust",
      "SoundEventId": "SFX_Unbreakable_Block"
    },
    "IncorrectTool": {
      "ParticleSystemId": "Block_Break_Dust",
      "SoundEventId": "SFX_Unbreakable_Block"
    }
  }
}
```

#### Item Settings

```json
{
  "ItemEntity": {
    "Lifetime": 600.0
  },
  "ItemDurability": {
    "BrokenPenalties": {
      "Weapon": 0.75,
      "Armor": 0.75,
      "Tool": 0.75
    }
  }
}
```

#### Player Settings

```json
{
  "Player": {
    "MovementConfig": "Default",
    "HitboxCollisionConfig": "SoftCollision"
  }
}
```

#### Plugin Configuration

Configure built-in plugin systems:

```json
{
  "Plugin": {
    "Stamina": {
      "SprintRegenDelay": {
        "EntityStatId": "StaminaRegenDelay",
        "Value": -0.75
      }
    },
    "Memories": {
      "MemoriesAmountPerLevel": [10, 25, 50, 100, 200],
      "MemoriesRecordParticles": "MemoryRecordedStatue",
      "MemoriesCatchItemId": "Memory_Particle"
    },
    "ForgottenTemple": {
      "MinYRespawn": 110,
      "RespawnSound": "SFX_Divine_Respawn"
    }
  }
}
```

#### Spawn Settings

```json
{
  "Spawn": {
    "FirstSpawnParticles": [
      {
        "SystemId": "PlayerSpawn_Spawn",
        "RotationOffset": {
          "Yaw": 0
        },
        "PositionOffset": {
          "Y": -0.9
        }
      }
    ]
  }
}
```

#### Ping System

```json
{
  "Ping": {
    "PingDuration": 5.0,
    "PingCooldown": 1.0,
    "PingBroadcastRadius": 100.0,
    "PingSound": "SFX_Ping"
  }
}
```

#### Camera Effects

```json
{
  "CameraEffects": {
    "DamageEffects": {
      "Fall": "Damage_Fall"
    }
  }
}
```

### Complete Example: Hardcore Config

```json
{
  "Parent": "Default",
  "Death": {
    "ItemsLossMode": "All",
    "RespawnController": {
      "Type": "WorldSpawnPoint"
    }
  },
  "World": {
    "AllowBlockBreaking": true,
    "AllowBlockGathering": true,
    "AllowBlockPlacement": true,
    "DaytimeDurationSeconds": 2400,
    "NighttimeDurationSeconds": 2400
  },
  "Respawn": {
    "RadiusLimitRespawnPoint": 0,
    "MaxRespawnPointsPerPlayer": 1
  }
}
```

### Complete Example: Creative Build Server

```json
{
  "Parent": "Default",
  "World": {
    "AllowBlockBreaking": true,
    "AllowBlockGathering": true,
    "AllowBlockPlacement": true,
    "BlockPlacementFragilityTimer": 0
  },
  "Death": {
    "ItemsLossMode": "None"
  },
  "Combat": {
    "DisableNPCIncomingDamage": true,
    "DisablePlayerIncomingDamage": true
  },
  "CreativePlaySoundSet": "CreativePlayDefaults"
}
```

---

## Server Mod Workflow

### Step 1: Create Gameplay Config

1. Create `Server/GameplayConfigs/MyServerConfig.json`
2. Inherit from `"Default"` or another config
3. Override settings you want to change

### Step 2: Create Server Instance

1. Create folder `Server/Instances/MyCustomServer/`
2. Create `config.json` or `instance.bson`
3. Set `GameplayConfig` to your config ID
4. Configure world generation and spawn points
5. Set instance-specific settings (PvP, NPCs, etc.)

### Step 3: Configure Discovery (Optional)

1. Add `Plugin.Instance.Discovery` to your instance config
2. Add translation keys to language files
3. Set icon and display settings

### Step 4: Test Your Server

1. Load your instance in-game
2. Verify settings work as expected
3. Adjust configs as needed

---

## Tips for Server Modding

1. **Start with Default** - Inherit from `"Default"` config and override only what you need
2. **Test incrementally** - Change one setting at a time to see effects
3. **Use JSON format** - `config.json` is easier to read/edit than `.bson`
4. **Unique UUIDs** - Each instance needs a unique UUID
5. **Resource folders** - Instance resources are automatically created when needed
6. **GameplayConfig inheritance** - Use parent configs to share common settings
7. **World types** - Choose appropriate world generation for your server type
8. **Instance cleanup** - Set `DeleteOnRemove` based on whether instance should persist

---

## Common Server Types

| Type | GameMode | PvP | NPCs | WorldGen | Use Case |
|------|----------|-----|------|----------|----------|
| **Adventure** | Adventure | false | true | HytaleGenerator | Standard survival |
| **Creative** | Creative | false | false | Flat | Building servers |
| **PvP Arena** | Adventure | true | false | Flat | Combat servers |
| **Hardcore** | Adventure | false | true | HytaleGenerator | High-stakes survival |
| **Mini-game** | Adventure | varies | varies | Flat/Void | Game modes |

---

**Previous:** [Creating Accessories](11_Creating_Accessories.md) | **Next:** [Getting Started](01_Getting_Started.md)

**See also:** [Java Plugins](183_Java_Server_Plugins.md) — Java dev setup, plugin template, and building plugins (commands, events, custom logic). [Hytale Modding](https://hytalemodding.dev/en/docs/guides/plugin/setting-up-env) for the setup guide.
