# Respawn Controllers

Learn how to configure player respawn behavior after death.

## Overview

Respawn controllers determine where and how players respawn after death. Different respawn types include world spawn points, exit instance, and custom respawn logic.

## Location
Respawn controllers are configured in `Death.RespawnController` in gameplay configs.

## Basic Respawn Controller Structure

```json
{
  "Death": {
    "RespawnController": {
      "Type": "WorldSpawnPoint"
    }
  }
}
```

## Respawn Controller Types

### WorldSpawnPoint

```json
{
  "RespawnController": {
    "Type": "WorldSpawnPoint"
  }
}
```

Respawns at world spawn point or last respawn point set.

### ExitInstance

```json
{
  "RespawnController": {
    "Type": "ExitInstance"
  }
}
```

Exits current instance on death (for portals/dungeons).

## Respawn Configuration

```json
{
  "Respawn": {
    "RadiusLimitRespawnPoint": 500,
    "MaxRespawnPointsPerPlayer": 3
  }
}
```

- **`RadiusLimitRespawnPoint`** - Maximum distance from death point to respawn (blocks)
- **`MaxRespawnPointsPerPlayer`** - Maximum respawn points player can set

## Death Configuration

```json
{
  "Death": {
    "ItemsLossMode": "Configured",
    "ItemsAmountLossPercentage": 50.0,
    "ItemsDurabilityLossPercentage": 10.0,
    "RespawnController": {
      "Type": "WorldSpawnPoint"
    }
  }
}
```

## Complete Example: Instance Exit on Death

```json
{
  "Death": {
    "ItemsLossMode": "None",
    "RespawnController": {
      "Type": "ExitInstance"
    }
  }
}
```

Exits instance without item loss.

## Tips for Respawn Controllers

1. **WorldSpawnPoint** - Standard respawn for normal gameplay
2. **ExitInstance** - Use for dungeon/portal instances
3. **Radius limit** - Prevents respawning too far from death
4. **Respawn points** - Players can set custom respawn points (beds, etc.)

---

**Previous:** [Salvage Recipes](86_Salvage_Recipes.md) | **Next:** [Item Entity Config](88_Item_Entity_Config.md)
