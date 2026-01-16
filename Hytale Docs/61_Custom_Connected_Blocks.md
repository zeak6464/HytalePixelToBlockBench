# Custom Connected Blocks

Learn how to create blocks that automatically connect to adjacent blocks, like fences, walls, and rails.

## Overview

Custom connected block templates define how blocks change their appearance based on neighboring blocks. Used for fences, walls, rails, pillars, and other structures that need to connect visually.

## Location
`Server/Item/CustomConnectedBlockTemplates/`

## Basic Connected Block Template

Create `Server/Item/CustomConnectedBlockTemplates/MyCustom_Wall.json`:

```json
{
  "ConnectsToOtherMaterials": true,
  "DefaultShape": "Straight",
  "Shapes": {
    "Straight": {
      "FaceTags": {
        "East": ["WallConnection"],
        "West": ["WallConnection"]
      },
      "PatternsToMatchAnyOf": []
    }
  }
}
```

## Template Properties

### ConnectsToOtherMaterials

```json
{
  "ConnectsToOtherMaterials": true
}
```

Whether this block connects to blocks with different materials.

### DefaultShape

```json
{
  "DefaultShape": "Straight"
}
```

Default shape when no connections match.

### Shapes

```json
{
  "Shapes": {
    "Straight": { ... },
    "Corner": { ... },
    "T_Junction": { ... }
  }
}
```

Different visual shapes based on connections.

## Face Tags

```json
{
  "FaceTags": {
    "East": ["WallConnection"],
    "West": ["WallConnection"],
    "North": ["WallConnection"],
    "South": ["WallConnection"]
  }
}
```

Defines which faces can connect. Blocks with matching face tags connect.

## Pattern Matching

```json
{
  "PatternsToMatchAnyOf": [
    {
      "Type": "Custom",
      "RulesToMatch": [
        {
          "Position": { "X": -1, "Y": 0, "Z": 0 },
          "IncludeOrExclude": "Include",
          "FaceTags": {
            "East": ["WallConnection"]
          }
        }
      ]
    }
  ]
}
```

Complex rules for when shapes appear based on neighbor positions and tags.

## Common Templates

### Wall/Fence

`Server/Item/CustomConnectedBlockTemplates/WallConnectedBlockTemplate.json`:

Straight segments, corners, T-junctions, cross-junctions.

### Rails

`Server/Item/CustomConnectedBlockTemplates/RailsConnectedBlockTemplate.json`:

Rail connections with curves and junctions.

### Pillars

`Server/Item/CustomConnectedBlockTemplates/PillarConnectedBlockTemplate.json`:

Vertical connections (top/middle/base segments).

## Using Connected Block Templates

Reference in block definitions:

```json
{
  "BlockType": {
    "ConnectedBlockRuleSet": {
      "Type": "CustomTemplate",
      "TemplateShapeAssetId": "WallConnectedBlockTemplate"
    }
  }
}
```

## Tips for Connected Blocks

1. **Face tags** - Define connection rules clearly
2. **Pattern matching** - Test complex patterns thoroughly
3. **Default shape** - Always provide a fallback
4. **Material connection** - Use `ConnectsToOtherMaterials` for versatility

---

**Previous:** [Game Mode Configs](60_Game_Mode_Configs.md) | **Next:** [Block Spawners](62_Block_Spawners.md)
