# Interaction Type: ChangeBlock

Transform blocks into other block types (tilling, conversion).

## Overview

`ChangeBlock` transforms one block type into another at the target location. Useful for tilling soil, converting blocks, or transformation mechanics.

## Basic Structure

```json
{
  "Type": "ChangeBlock",
  "RunTime": 0.15,
  "Changes": {
    "Empty": "Rock_Stone"
  },
  "Effects": {
    "ItemAnimationId": "Till"
  },
  "Next": {
    "Type": "Simple"
  },
  "Failed": {
    "Type": "Simple"
  }
}
```

## Properties

### Changes

```json
{
  "Changes": {
    "Empty": "Rock_Stone",
    "Soil_Dirt": "Soil_Tilled"
  }
}
```

Map of source block IDs to target block IDs. Key is current block, value is new block.

### RunTime

```json
{
  "RunTime": 0.15
}
```

Delay before block changes.

### Effects

```json
{
  "Effects": {
    "ItemAnimationId": "Till",
    "WorldSoundEventId": "SFX_Till"
  }
}
```

Effects to play during transformation.

### Next

```json
{
  "Next": {
    "Type": "Simple"
  }
}
```

Interaction to execute after successful change.

### Failed

```json
{
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.interactions.changeblock.failed"
  }
}
```

Interaction to execute if change fails (block doesn't match any key in Changes).

## Complete Examples

### Example 1: Tilling Soil

```json
{
  "Type": "ChangeBlock",
  "RunTime": 0.15,
  "Changes": {
    "Soil_Dirt": "Soil_Tilled"
  },
  "Effects": {
    "ItemAnimationId": "Till",
    "WorldSoundEventId": "SFX_Till"
  }
}
```

Transforms dirt to tilled soil.

### Example 2: Multiple Conversions

```json
{
  "Type": "ChangeBlock",
  "Changes": {
    "Empty": "Rock_Stone",
    "Soil_Dirt": "Soil_Tilled",
    "Grass": "Soil_Dirt"
  },
  "Effects": {
    "ItemAnimationId": "Convert",
    "WorldSoundEventId": "SFX_Convert"
  }
}
```

Multiple block transformations based on source block.

### Example 3: With Validation

```json
{
  "Type": "ChangeBlock",
  "RunTime": 0.2,
  "Changes": {
    "Wood_Log": "Wood_Stripped_Log"
  },
  "Effects": {
    "ItemAnimationId": "Strip",
    "WorldSoundEventId": "SFX_Strip_Wood"
  },
  "Next": {
    "Type": "Simple",
    "Effects": {
      "WorldParticles": [
        {
          "SystemId": "Wood_Chips"
        }
      ]
    }
  },
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.interactions.cannot.strip"
  }
}
```

Strips wood log, shows particles on success, message on failure.

## Tips

1. **Change maps** - Use maps for multiple source->target conversions
2. **Validation** - Provide `Failed` for better UX when change can't happen
3. **Effects** - Always include visual/audio feedback
4. **RunTime** - Use delays for transformation feel (0.15-0.2s typical)

---

**Previous:** [PlaceBlock](127_Interaction_Type_PlaceBlock.md) | **Next:** [ChangeState](129_Interaction_Type_ChangeState.md)
