# Block Variants

Learn how to configure block rotation variants and placement variants for blocks that can face different directions.

## Overview

Block variants allow blocks to be placed in different orientations. Variants include rotation options like cardinal directions (NESW), all rotations, and special rotation types.

## Location
Block variants are configured in `BlockType.VariantRotation` property.

## Variant Rotation Types

### NESW (Cardinal Directions)

```json
{
  "BlockType": {
    "VariantRotation": "NESW"
  }
}
```

Allows rotation to North, East, South, or West (4 directions).

### All Rotations

```json
{
  "BlockType": {
    "VariantRotation": "All"
  }
}
```

Allows rotation in all directions (360 degrees).

### Pipe Rotation

```json
{
  "BlockType": {
    "VariantRotation": "Pipe"
  }
}
```

Rotation along axis (for pipes, logs, etc.).

### UpDown Rotation

```json
{
  "BlockType": {
    "VariantRotation": "UpDown"
  }
}
```

Two states: up or down orientation.

### DoublePipe

```json
{
  "BlockType": {
    "VariantRotation": "DoublePipe"
  }
}
```

Rotation for double-pipe blocks.

### YawStep90 / YawStep1

```json
{
  "BlockType": {
    "RandomRotation": "YawStep90"  // 90-degree steps
    "RandomRotation": "YawStep1"   // 1-degree steps
  }
}
```

Random rotation at placement in specified increments.

## Connected Block Variants

Blocks using connected block templates can have variants based on connections:

```json
{
  "BlockType": {
    "ConnectedBlockRuleSet": {
      "Type": "CustomTemplate",
      "TemplateShapeAssetId": "ChestConnectedBlockTemplate",
      "TemplateShapeBlockPatterns": {
        "Default": "Chest_Small",
        "Double": "Chest_Large"
      }
    }
  }
}
```

## Item Variant Property

Some items have a `Variant` property:

```json
{
  "Variant": true
}
```

Indicates the item supports variant states.

## Tips for Block Variants

1. **Cardinal directions** - Use `NESW` for most furniture and directional blocks
2. **All rotations** - Use `All` when precise rotation matters
3. **Random rotation** - Use `RandomRotation` for natural variation
4. **Connected blocks** - Variants work with connected block templates

---

**Previous:** [NPC Pathfinding](74_NPC_Pathfinding.md) | **Next:** [Block States](76_Block_States.md)
