# Interaction Type: PlacementCountCondition

Check how many instances of a block type are placed before allowing more.

## Overview

`PlacementCountCondition` checks if the number of placed blocks of a specific type is below a threshold. Useful for limiting placements (portals, special blocks, etc.).

## Example from Game Files

### Placement Count Condition Interaction

Placement count condition interactions check how many blocks of a type are placed before proceeding. These are used for building requirements, placement limits, and block-based conditions.

## Basic Structure

```json
{
  "Type": "PlacementCountCondition",
  "Block": "Teleporter",
  "Value": 2,
  "Next": {
    "Type": "PlaceBlock",
    "RunTime": 0.125
  },
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.interactions.teleporter.failedCollectMore"
  }
}
```

## Properties

### Block

```json
{
  "Block": "Teleporter"
}
```

Block ID to count. Can be block name or full block ID.

### Value

```json
{
  "Value": 2
}
```

Maximum number of blocks allowed. Condition passes if count < Value.

### Next

```json
{
  "Next": {
    "Type": "PlaceBlock"
  }
}
```

Interaction to execute if condition passes (count < Value).

### Failed

```json
{
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.interactions.teleporter.failedCollectMore"
  }
}
```

Interaction to execute if condition fails (count >= Value).

## Complete Examples

### Example 1: Limit Portal Placement

```json
{
  "Type": "PlacementCountCondition",
  "Block": "Teleporter",
  "Value": 2,
  "Next": {
    "Type": "PlaceBlock",
    "RunTime": 0.125
  },
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.interactions.teleporter.failedCollectMore"
  }
}
```

Only allows 2 teleporters to be placed.

### Example 2: Limit Special Blocks

```json
{
  "Type": "PlacementCountCondition",
  "Block": "Beacon",
  "Value": 5,
  "Next": {
    "Type": "PlaceBlock",
    "BlockTypeToPlace": "Beacon"
  },
  "Failed": {
    "Type": "SendMessage",
    "Message": "Maximum 5 beacons allowed!"
  }
}
```

Limits beacon placement to 5.

## Tips

1. **Block ID** - Use block name or full ID, must match exactly
2. **Value** - Maximum allowed count (condition passes if count < Value)
3. **Failed feedback** - Always provide message for better UX
4. **Limiting** - Use for special blocks that should be limited (portals, beacons, etc.)

---

**Previous:** [EffectCondition](143_Interaction_Type_EffectCondition.md) | **Next:** [MemoriesCondition](145_Interaction_Type_MemoriesCondition.md)
