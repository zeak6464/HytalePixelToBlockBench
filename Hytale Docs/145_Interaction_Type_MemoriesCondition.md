# Interaction Type: MemoriesCondition

Check interaction memory/history with branch paths for different counts.

## Overview

`MemoriesCondition` checks how many times a specific interaction has been executed and branches based on count. Useful for progressive unlocks, tiered systems, or count-based behaviors.

## Example from Game Files

### Memories Condition Interaction

Memories condition interactions check player memories before proceeding. These are used for memory-based quests, achievements, and progression mechanics.

## Basic Structure

```json
{
  "Type": "MemoriesCondition",
  "Next": {
    "0": {
      "Parent": "Teleporter_Try_Place",
      "Value": 2
    },
    "1": {
      "Parent": "Teleporter_Try_Place",
      "Value": 2
    },
    "2": {
      "Parent": "Teleporter_Try_Place",
      "Value": 3
    }
  },
  "Failed": {
    "Parent": "Teleporter_Try_Place",
    "Value": 8,
    "Failed": {
      "Type": "SendMessage",
      "Key": "server.interactions.teleporter.failed"
    }
  }
}
```

## Properties

### Next

```json
{
  "Next": {
    "0": {
      "Parent": "Interaction_Id",
      "Value": 2
    },
    "1": {
      "Parent": "Interaction_Id",
      "Value": 2
    },
    "2": {
      "Parent": "Interaction_Id",
      "Value": 3
    }
  }
}
```

Map of count indices (as strings) to interactions:
- Key: Count index (0, 1, 2, etc.)
- **`Parent`** - Parent interaction to reference
- **`Value`** - Value for that interaction

### Failed

```json
{
  "Failed": {
    "Parent": "Interaction_Id",
    "Value": 8,
    "Failed": {
      "Type": "SendMessage"
    }
  }
}
```

Interaction to execute if count exceeds all defined indices.

## Complete Examples

### Example 1: Progressive Portal Placement

```json
{
  "Type": "MemoriesCondition",
  "Next": {
    "0": {
      "Parent": "Teleporter_Try_Place",
      "Value": 2
    },
    "1": {
      "Parent": "Teleporter_Try_Place",
      "Value": 2
    },
    "2": {
      "Parent": "Teleporter_Try_Place",
      "Value": 3
    },
    "3": {
      "Parent": "Teleporter_Try_Place",
      "Value": 4
    },
    "4": {
      "Parent": "Teleporter_Try_Place",
      "Value": 6
    },
    "5": {
      "Parent": "Teleporter_Try_Place",
      "Value": 8
    }
  },
  "Failed": {
    "Parent": "Teleporter_Try_Place",
    "Value": 8,
    "Failed": {
      "Type": "SendMessage",
      "Key": "server.interactions.teleporter.failed"
    }
  }
}
```

Different behaviors based on interaction count (0-5 uses).

### Example 2: Tiered System

```json
{
  "Type": "MemoriesCondition",
  "Next": {
    "0": {
      "Parent": "Unlock_Tier1",
      "Value": 1
    },
    "1": {
      "Parent": "Unlock_Tier2",
      "Value": 2
    },
    "2": {
      "Parent": "Unlock_Tier3",
      "Value": 3
    }
  }
}
```

Unlocks different tiers based on use count.

## Tips

1. **Count indices** - Use string keys ("0", "1", "2") for count-based branching
2. **Progressive systems** - Use for systems that change behavior over time
3. **Parent references** - Reference other interactions by ID for cleaner code
4. **Failed branches** - Provide Failed for when count exceeds all indices

---

**Previous:** [PlacementCountCondition](144_Interaction_Type_PlacementCountCondition.md) | **Next:** [MovementCondition](146_Interaction_Type_MovementCondition.md)
