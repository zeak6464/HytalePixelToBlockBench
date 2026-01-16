# Path Definitions

Learn how to define paths for NPCs to follow, including patrol paths and custom routes.

## Overview

Paths define waypoint sequences that NPCs can follow. They're used for patrol routes, NPC movement patterns, and world navigation. Paths can be defined as prefab paths or in NPC instructions.

## Location
- Prefab paths: Defined in prefab files with path markers
- Instruction paths: Configured in NPC instructions

## Basic Path Structure

Paths are typically referenced in NPC instructions:

```json
{
  "Sensor": {
    "Type": "Path",
    "PathType": "AnyPrefabPath",
    "Range": 30
  },
  "BodyMotion": {
    "Type": "Path",
    "Shape": "Loop",
    "UseNodeViewDirection": true
  }
}
```

## Path Sensor Configuration

### AnyPrefabPath

```json
{
  "Type": "Path",
  "PathType": "AnyPrefabPath",
  "Range": 30
}
```

Detects any prefab path within range.

### Named Path

```json
{
  "Type": "Path",
  "PathType": "PrefabPath",
  "PathName": "Guard_Patrol_1",
  "Range": 30
}
```

Detects specific named path.

## Path Body Motion

### Path Following

```json
{
  "BodyMotion": {
    "Type": "Path",
    "Shape": "Loop",
    "UseNodeViewDirection": true,
    "PickRandomAngle": true,
    "StartAtNearestNode": true
  }
}
```

NPC follows detected path.

### Path Properties

- **`Shape`** - `"Loop"` (repeat), `"OneWay"` (single pass)
- **`UseNodeViewDirection`** - NPC faces path direction
- **`PickRandomAngle`** - Random facing at nodes
- **`StartAtNearestNode`** - Start at closest node

## Path Motion Controller

Path following requires appropriate motion controller:

```json
{
  "MotionControllerList": [
    {
      "Type": "Walk",
      "MaxWalkSpeed": 5,
      "MaxClimbHeight": 1,
      "JumpHeight": 1.5,
      "JumpForce": 1.5
    }
  ]
}
```

## Prefab Paths

Paths can be embedded in prefabs using path marker blocks or entities. NPCs detect these paths when prefabs are placed in the world.

## Pathfinding Configuration

See the [NPC Pathfinding](74_NPC_Pathfinding.md) guide for detailed pathfinding configuration.

## Tips for Path Definitions

1. **Prefab paths** - Embed paths in prefabs for world-placed patrol routes
2. **Range** - Set appropriate detection range (typically 20-50 blocks)
3. **Loop vs OneWay** - Use `Loop` for continuous patrols, `OneWay` for one-time routes
4. **View direction** - `UseNodeViewDirection` makes NPCs face movement direction

---

**Previous:** [Animation Slots](99_Animation_Slots.md) | **Next:** [README](README.md)

---

**🎉 Congratulations! You've reached 100 guides! 🎉**

All guides are now complete. See the [README](README.md) for the full index of all 100 guides.
