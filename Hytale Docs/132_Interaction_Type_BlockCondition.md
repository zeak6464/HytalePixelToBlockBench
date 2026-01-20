# Interaction Type: BlockCondition

Conditional logic based on block type at target location.

## Overview

`BlockCondition` checks if blocks match specific criteria and branches interactions based on the result. Useful for context-aware interactions based on what block is being targeted.

## Example from Game Files

### Block Condition Interaction

Block condition interactions check block states before proceeding. These are used for conditional block interactions and block-based logic.

## Basic Structure

```json
{
  "Type": "BlockCondition",
  "Matchers": [
    {
      "Block": {
        "Id": "Furniture_Human_Ruins_Lantern"
      }
    }
  ],
  "Next": {
    "Type": "ChangeState",
    "Changes": {
      "default": "Yellow"
    }
  },
  "Failed": "Block_Secondary"
}
```

## Properties

### Matchers

```json
{
  "Matchers": [
    {
      "Block": {
        "Id": "Furniture_Human_Ruins_Lantern"
      }
    }
  ]
}
```

Array of block matchers. All must match for condition to pass.

### Block

```json
{
  "Block": {
    "Id": "Furniture_Human_Ruins_Lantern"
  }
}
```

Block matcher:
- **`Id`** - Block ID to match

### Next

```json
{
  "Next": {
    "Type": "ChangeState"
  }
}
```

Interaction to execute if condition passes.

### Failed

```json
{
  "Failed": "Block_Secondary"
}
```

Interaction to execute if condition fails (block doesn't match).

## Complete Examples

### Example 1: Lantern Check

```json
{
  "Type": "BlockCondition",
  "Matchers": [
    {
      "Block": {
        "Id": "Furniture_Human_Ruins_Lantern"
      }
    }
  ],
  "Next": {
    "Type": "ChangeState",
    "Changes": {
      "default": "Yellow"
    }
  },
  "Failed": "Block_Secondary"
}
```

Only changes state if block is a lantern.

### Example 2: Water Check

```json
{
  "Type": "BlockCondition",
  "Matchers": [
    {
      "Block": {
        "Id": "Water_Source"
      }
    }
  ],
  "Next": {
    "Type": "SpawnNPC",
    "EntityId": "Fish",
    "SpawnOffset": {
      "X": 0,
      "Y": 0,
      "Z": 0
    }
  },
  "Failed": {
    "Type": "SpawnNPC",
    "EntityId": "Sheep",
    "SpawnOffset": {
      "X": 0,
      "Y": 0.5,
      "Z": 0
    }
  }
}
```

Spawns fish in water, sheep on land.

### Example 3: Seed Placement Check

```json
{
  "Type": "BlockCondition",
  "Matchers": [
    {
      "Block": {
        "Id": "Soil_Tilled"
      }
    }
  ],
  "Next": {
    "Type": "Replace",
    "Var": "Seed_Place",
    "DefaultValue": {
      "Interactions": ["Seed_Place"]
    },
    "DefaultOk": true
    }
  },
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.crops.must.till.first"
  }
}
```

Only allows seed placement on tilled soil.

## Tips

1. **Matchers** - All matchers must pass (AND logic)
2. **Block IDs** - Must match block IDs exactly (case-sensitive)
3. **Failed branches** - Always provide `Failed` for better UX
4. **Context-aware** - Use for conditional interactions based on block type

---

**Previous:** [UseBlock](131_Interaction_Type_UseBlock.md) | **Next:** [LaunchProjectile](133_Interaction_Type_LaunchProjectile.md)
