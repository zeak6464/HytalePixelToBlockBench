# Block Entity Components

Learn how to add entity components to blocks for special functionality like spawners and custom behaviors.

## Overview

Block entity components add special functionality to blocks. Blocks with entity components can have custom behaviors, spawn entities, track state, and perform actions beyond standard block interactions.

## Location
Block entity components are configured in `BlockType.BlockEntity.Components` in block definitions.

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
