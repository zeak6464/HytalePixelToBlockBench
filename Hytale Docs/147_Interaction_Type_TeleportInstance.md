# Interaction Type: TeleportInstance

Teleport entities to instances, areas, or locations.

## Overview

`TeleportInstance` teleports entities to instances, areas, or specific locations. Supports position offsets, rotation, personal return points, and block removal.

## Basic Structure

```json
{
  "Type": "TeleportInstance",
  "InstanceName": "Persistent",
  "InstanceKey": "Persistent",
  "PositionOffset": {
    "X": 0,
    "Y": 0,
    "Z": 2
  },
  "Rotation": {
    "Yaw": 180
  },
  "OriginSource": "Block",
  "PersonalReturnPoint": true,
  "CloseOnBlockRemove": false,
  "RemoveBlockAfter": 5
}
```

## Properties

### InstanceName

```json
{
  "InstanceName": "Persistent"
}
```

Name of the instance to teleport to.

### InstanceKey

```json
{
  "InstanceKey": "Persistent"
}
```

Key identifier for the instance.

### PositionOffset

```json
{
  "PositionOffset": {
    "X": 0,
    "Y": 0,
    "Z": 2
  }
}
```

Position offset relative to teleport destination (blocks).

### Rotation

```json
{
  "Rotation": {
    "Yaw": 180
  }
}
```

Rotation at destination:
- **`Yaw`** - Yaw rotation (degrees)

### OriginSource

```json
{
  "OriginSource": "Block"
}
```

Source of teleportation:
- **`"Block"`** - Teleporting from a block
- **`"Entity"`** - Teleporting from an entity

### PersonalReturnPoint

```json
{
  "PersonalReturnPoint": true
}
```

If `true`, saves personal return point for returning. If `false`, no return point saved.

### CloseOnBlockRemove

```json
{
  "CloseOnBlockRemove": false
}
```

If `true`, closes instance when block is removed. If `false`, instance remains open.

### RemoveBlockAfter

```json
{
  "RemoveBlockAfter": 5
}
```

Removes teleporter block after specified number of uses. Omit to never remove.

## Complete Examples

### Example 1: Portal Teleport

```json
{
  "Type": "TeleportInstance",
  "InstanceName": "Persistent",
  "InstanceKey": "Persistent",
  "PositionOffset": {
    "X": 0,
    "Y": 0,
    "Z": 2
  },
  "Rotation": {
    "Yaw": 180
  },
  "OriginSource": "Block",
  "PersonalReturnPoint": true,
  "CloseOnBlockRemove": false,
  "RemoveBlockAfter": 5
}
```

Teleports to instance, saves return point, removes after 5 uses.

### Example 2: Simple Teleport

```json
{
  "Type": "TeleportInstance",
  "InstanceName": "Dungeon_1",
  "InstanceKey": "Dungeon_1",
  "PositionOffset": {
    "X": 0,
    "Y": 0,
    "Z": 0
  }
}
```

Teleports to dungeon instance at default position.

## Tips

1. **Instance keys** - Must match instance configuration keys
2. **PositionOffset** - Use for spawn positions within instance
3. **PersonalReturnPoint** - Use for portals that should allow return
4. **RemoveBlockAfter** - Use for one-time or limited-use teleporters
5. **Rotation** - Set Yaw for facing direction at destination

---

**Previous:** [MovementCondition](146_Interaction_Type_MovementCondition.md) | **Next:** [TeleportConfigInstance](148_Interaction_Type_TeleportConfigInstance.md)
