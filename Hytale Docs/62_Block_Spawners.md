# Block Spawners

Learn how to configure block spawners for world generation that place blocks like grass, ores, and decorative elements.

## Overview

Block spawners define rules for placing blocks during world generation. They can spawn blocks based on:
- Surface block types
- Height/Y-level
- Environment tags
- Weight probabilities

## Location
`Server/Item/Block/Spawners/New/`

## Basic Block Spawner Structure

Create `Server/Item/Block/Spawners/New/MyCustom_Grass.json`:

```json
{
  "Entries": [
    {
      "Name": "Grass_Block",
      "State": {
        "Custom": false,
        "AllowViewing": true,
        "Type": "block",
        "BlockId": "Plant_Grass"
      },
      "Weight": 100
    }
  ]
}
```

## Spawner Properties

### Entries

Array of spawnable blocks:

```json
{
  "Entries": [
    {
      "Name": "Item_Name",
      "State": {
        "Type": "block",
        "BlockId": "Plant_Grass"
      },
      "Weight": 100
    }
  ]
}
```

### Weight

```json
{
  "Weight": 100
}
```

Probability weight. Higher = more likely to spawn.

### Block State

```json
{
  "State": {
    "Custom": false,
    "AllowViewing": true,
    "Type": "block",
    "BlockId": "Plant_Grass"
  }
}
```

- **`Type`** - `"block"` for blocks, `"container"` for chests
- **`BlockId`** - Block/item ID to spawn

### Container State

For spawning containers with drops:

```json
{
  "State": {
    "Type": "container",
    "Droplist": "Zone1_Goblin_Tier1",
    "AllowViewing": true
  }
}
```

## Complete Example: Goblin Chest Spawner

`Server/Item/Block/Spawners/New/Zone1_Goblin_Tier1.json`:

```json
{
  "Entries": [
    {
      "Name": "Furniture_Goblin_Chest_Small",
      "State": {
        "Custom": false,
        "AllowViewing": true,
        "Type": "container",
        "Droplist": "Zone1_Goblin_Tier1"
      },
      "Weight": 100
    }
  ]
}
```

## Using Block Spawners

Block spawners are referenced in world generation configs and prefab definitions.

## Block Spawner Blocks

Special blocks can spawn other blocks:

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

## Tips for Block Spawners

1. **Weight balance** - Adjust weights for desired spawn frequency
2. **Container drops** - Use droplists for chests with loot
3. **World generation** - Spawners integrate with biome generation
4. **Testing** - Verify spawn rates match expectations

---

**Previous:** [Custom Connected Blocks](61_Custom_Connected_Blocks.md) | **Next:** [Block Fluids](63_Block_Fluids.md)
