# Gameplay Configs

Configure game rules, death penalties, world settings, and player behavior.

## Overview

GameplayConfigs define server-wide gameplay rules. They control death penalties, item durability, world timing, respawn mechanics, and more. Different configs can be used for different game modes.

## Location
`Server/GameplayConfigs/`

## Example from Game Files

### Default Config

From `Server/GameplayConfigs/Default.json`:

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
      "AllowedSleepHoursRange": [19.5, 4.79]
    }
  },
  "Player": {
    "MovementConfig": "Default",
    "HitboxCollisionConfig": "SoftCollision",
    "ArmorVisibilityOption": "All"
  },
  "CameraEffects": {
    "DamageEffects": {
      "Fall": "Damage_Fall"
    }
  },
  "CreativePlaySoundSet": "CreativePlayDefaults",
  "Spawn": {
    "FirstSpawnParticles": [
      {
        "SystemId": "PlayerSpawn_Spawn",
        "RotationOffset": { "Yaw": 0 },
        "PositionOffset": { "Y": -0.9 }
      }
    ]
  },
  "Ping": {
    "PingDuration": 5.0,
    "PingCooldown": 1.0,
    "PingBroadcastRadius": 100.0,
    "PingSound": "SFX_Ping"
  }
}
```

### Creative Hub Config

From `Server/GameplayConfigs/CreativeHub.json`:

```json
{
  "Parent": "Default",
  "World": {
    "AllowBlockBreaking": false,
    "AllowBlockGathering": false,
    "AllowBlockPlacement": false
  }
}
```

---

## Config Sections

### Gathering

Feedback for unbreakable blocks:

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

### Death

Death penalties:

```json
{
  "Death": {
    "ItemsLossMode": "Configured",
    "ItemsAmountLossPercentage": 50.0,
    "ItemsDurabilityLossPercentage": 10.0
  }
}
```

| Property | Description |
|----------|-------------|
| `ItemsLossMode` | `"Configured"`, `"None"`, `"All"` |
| `ItemsAmountLossPercentage` | % of stackable items lost |
| `ItemsDurabilityLossPercentage` | % durability lost on death |

### ItemEntity

Dropped item behavior:

```json
{
  "ItemEntity": {
    "Lifetime": 600.0
  }
}
```

- `Lifetime`: Seconds before dropped items despawn (600 = 10 minutes)

### ItemDurability

Broken item penalties:

```json
{
  "ItemDurability": {
    "BrokenPenalties": {
      "Weapon": 0.75,
      "Armor": 0.75,
      "Tool": 0.75
    }
  }
}
```

- Value = effectiveness when broken (0.75 = 75% effectiveness)

### World

Day/night cycle and world rules:

```json
{
  "World": {
    "DaytimeDurationSeconds": 1728,
    "NighttimeDurationSeconds": 1152,
    "BlockPlacementFragilityTimer": 0,
    "Sleep": {
      "WakeUpHour": 4.79,
      "AllowedSleepHoursRange": [19.5, 4.79]
    },
    "AllowBlockBreaking": true,
    "AllowBlockGathering": true,
    "AllowBlockPlacement": true
  }
}
```

| Property | Description |
|----------|-------------|
| `DaytimeDurationSeconds` | Real seconds for daytime |
| `NighttimeDurationSeconds` | Real seconds for nighttime |
| `AllowBlockBreaking` | Can players break blocks |
| `AllowBlockGathering` | Can players gather resources |
| `AllowBlockPlacement` | Can players place blocks |

**Default day cycle:** 1728 + 1152 = 2880 seconds (48 minutes real time)

### Respawn

Respawn mechanics:

```json
{
  "Respawn": {
    "RadiusLimitRespawnPoint": 500,
    "MaxRespawnPointsPerPlayer": 3
  }
}
```

### Player

Player movement and appearance:

```json
{
  "Player": {
    "MovementConfig": "Default",
    "HitboxCollisionConfig": "SoftCollision",
    "ArmorVisibilityOption": "All"
  }
}
```

### Plugin (Stamina & Memories)

Custom plugin systems:

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
      "MemoriesAmountPerLevel": [10, 25, 50, 100, 200]
    }
  }
}
```

### Ping

Player ping system:

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

---

## Inheritance

Use `Parent` to inherit from another config:

```json
{
  "Parent": "Default",
  "Death": {
    "ItemsLossMode": "None"
  }
}
```

Only overridden properties change.

---

## Available Configs

| Config | Description |
|--------|-------------|
| `Default` | Standard survival gameplay |
| `CreativeHub` | No block interaction |
| `Default_Instance` | Instance-specific defaults |
| `ForgottenTemple` | Dungeon instance rules |
| `Portal` | Portal zone rules |

---

## Creating Custom Config

`Server/GameplayConfigs/Hardcore.json`:

```json
{
  "Parent": "Default",
  "Death": {
    "ItemsLossMode": "All",
    "ItemsAmountLossPercentage": 100.0,
    "ItemsDurabilityLossPercentage": 25.0
  },
  "ItemDurability": {
    "BrokenPenalties": {
      "Weapon": 0.5,
      "Armor": 0.5,
      "Tool": 0.5
    }
  },
  "Respawn": {
    "MaxRespawnPointsPerPlayer": 1
  },
  "World": {
    "NighttimeDurationSeconds": 1728
  }
}
```

This creates a hardcore mode with:
- 100% item loss on death
- 25% durability loss
- Broken items at 50% effectiveness
- Only 1 respawn point
- Longer nights

---

## Tips

1. **Use inheritance** - Extend `Default` for consistency
2. **Test death penalties** - Balance item loss carefully
3. **Day/night cycle** - Affects mob spawning and sleep
4. **Instance configs** - Create special rules for dungeons
5. **Creative modes** - Disable block changes for hubs

---

**Related:** [Server Modding](12_Server_Modding.md) | [Game Mode Configs](60_Game_Mode_Configs.md)
