# Interaction Type: RefillContainer

Fill containers with fluids (water, milk, etc.).

## Overview

`RefillContainer` fills containers with fluids from blocks or entities. Used for buckets, bottles, and other fluid containers.

## Basic Structure

```json
{
  "Type": "RefillContainer",
  "Effects": {
    "ItemAnimationId": "Interact",
    "ClearSoundEventOnFinish": true,
    "ClearAnimationOnFinish": true,
    "LocalSoundEventId": "SFX_WATER_MoveIn"
  },
  "States": {
    "Filled_Water": {
      "AllowedFluids": [
        "Water_Source",
        "Water"
      ]
    }
  },
  "RunTime": 0.1
}
```

## Properties

### States

```json
{
  "States": {
    "Filled_Water": {
      "AllowedFluids": [
        "Water_Source",
        "Water"
      ]
    }
  }
}
```

Map of container state IDs to fluid requirements:
- Key: State ID when filled
- **`AllowedFluids`** - Array of fluid block IDs that can fill this state

### Effects

```json
{
  "Effects": {
    "ItemAnimationId": "Interact",
    "ClearSoundEventOnFinish": true,
    "ClearAnimationOnFinish": true,
    "LocalSoundEventId": "SFX_WATER_MoveIn"
  }
}
```

Effects while filling (animations, sounds).

### RunTime

```json
{
  "RunTime": 0.1
}
```

Duration of fill interaction.

## Complete Examples

### Example 1: Water Bucket

```json
{
  "Type": "Simple",
  "RunTime": 0.1,
  "Next": {
    "Type": "RefillContainer",
    "Effects": {
      "ItemAnimationId": "Interact",
      "ClearSoundEventOnFinish": true,
      "ClearAnimationOnFinish": true,
      "LocalSoundEventId": "SFX_WATER_MoveIn"
    },
    "States": {
      "Filled_Water": {
        "AllowedFluids": [
          "Water_Source",
          "Water"
        ]
      }
    },
    "RunTime": 0.1
  }
}
```

Fills bucket with water from water source.

### Example 2: Milk Bucket

```json
{
  "Type": "RefillContainer",
  "Effects": {
    "ItemAnimationId": "Milk",
    "LocalSoundEventId": "SFX_Milk"
  },
  "States": {
    "Filled_Milk": {
      "AllowedFluids": [
        "Milk_Source"
      ]
    }
  }
}
```

Fills bucket with milk from cow.

## Tips

1. **AllowedFluids** - Specify all valid fluid block IDs
2. **State IDs** - Match container item state definitions
3. **Clear effects** - Use `ClearSoundEventOnFinish`/`ClearAnimationOnFinish` for clean transitions
4. **Block check** - May need BlockCondition to ensure targeting correct fluid block

---

**Previous:** [ModifyInventory](135_Interaction_Type_ModifyInventory.md) | **Next:** [Consume](137_Interaction_Type_Consume.md)
