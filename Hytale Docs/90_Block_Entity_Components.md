# Block Entity Components

Learn how to add entity components to blocks for special functionality like spawners and custom behaviors.

## Overview

Block entity components add special functionality to blocks. Blocks with entity components can have custom behaviors, spawn entities, track state, and perform actions beyond standard block interactions.

## Location
Block entity components are configured in `BlockType.BlockEntity.Components` in block definitions.

## Example from Game Files

### Chicken Coop Block Entity Component

From `Server/Farming/Coops/Coop_Chicken.json`:

```1:23:Server/Farming/Coops/Coop_Chicken.json
{
  "MaxResidents": 6,
  "ProduceDrops": {
    "Chicken": "Drop_Chicken_Produce",
    "Chicken_Desert": "Drop_Chicken_Produce",
    "Skrill": "Drop_Chicken_Produce"
  },
  "ResidentRoamTime": [
    6, 18
  ],
  "ResidentSpawnOffset": {
    "X": 0,
    "Y": 0,
    "Z": 3
  },
  "AcceptedNpcGroups": [
    "Chicken",
    "Chicken_Desert",
    "Skrill"
  ],
  "CaptureWildNPCsInRange": true,
  "WildCaptureRadius": 10
}
```

This shows a block entity component (Coop) configuration for a chicken coop with resident limits, produce drops, roam times, and wild NPC capture settings.

## Basic Block Entity Component Structure

```json
{
  "BlockType": {
    "BlockEntity": {
      "Components": {
        "BlockSpawner": {}
      }
    }
  }
}
```

## Block Entity Component Types

### BlockSpawner

```json
{
  "BlockEntity": {
    "Components": {
      "BlockSpawner": {}
    }
  }
}
```

Block spawner component enables block spawning functionality. Used by `Block_Spawner_Block` for world editing.

### Custom Components

Block entity components can define custom behaviors:

```json
{
  "BlockEntity": {
    "Components": {
      "MyCustomComponent": {
        "Property1": "Value1",
        "Property2": 100
      }
    }
  }
}
```

## Example: Block Spawner Block

`Server/Item/Items/MISC/Block_Spawner_Block.json`:

```json
{
  "BlockType": {
    "BlockEntity": {
      "Components": {
        "BlockSpawner": {}
      }
    }
  }
}
```

Enables block spawning for world editing tools.

## Tips for Block Entity Components

1. **Special functionality** - Use for blocks that need custom server-side logic
2. **Spawners** - BlockSpawner enables world generation tools
3. **State tracking** - Components can track block-specific state
4. **Performance** - Entity components add overhead, use sparingly

---

**Previous:** [State Evaluators](89_State_Evaluators.md) | **Next:** [Gathering Types](91_Gathering_Types.md)
