# Interaction Type: TeleportConfigInstance

Teleport using configuration-based instances.

## Overview

`TeleportConfigInstance` teleports the player to an instance based on configured settings. Used for portals that lead to pre-configured destinations.

## Example from Game Files

### Instance Gateway Portal

From `Server/Item/Items/Portal/Instance_Gateway.json`:

```json
{
  "CollisionEnter": {
    "Interactions": [
      {
        "Type": "TeleportConfigInstance"
      }
    ]
  }
}
```

## Basic Structure

```json
{
  "Type": "TeleportConfigInstance"
}
```

## Properties

This interaction type uses the block/entity's configuration to determine the destination instance. The destination is configured in the portal or gateway item itself.

### No Additional Properties

The interaction reads destination from the containing block/entity's configuration:

```json
{
  "BlockType": {
    "BlockEntity": {
      "Components": {
        "Portal": {
          "InstanceConfig": "Dungeon_01"
        }
      }
    }
  },
  "CollisionEnter": {
    "Interactions": [
      {
        "Type": "TeleportConfigInstance"
      }
    ]
  }
}
```

## Use Cases

- **Instance Portals** - Portals leading to dungeon instances
- **Gateway Blocks** - Blocks that teleport to configured locations
- **World Transitions** - Moving between world instances

## Difference from TeleportInstance

| Aspect | TeleportConfigInstance | TeleportInstance |
|--------|------------------------|------------------|
| Destination | From block config | Specified in interaction |
| Use Case | Configurable portals | Fixed destinations |
| Flexibility | Per-block configuration | Per-interaction |

## Tips

1. **Block Configuration** - Ensure the block has proper instance configuration
2. **Portal Component** - Use with `Portal` BlockEntity component
3. **Instance Validation** - Target instance must exist

---

**Previous:** [TeleportInstance](147_Interaction_Type_TeleportInstance.md) | **Next:** [OpenContainer](149_Interaction_Type_OpenContainer.md)
