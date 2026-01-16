# NPC Sensors

Learn how NPC sensors work to detect players, NPCs, and environmental conditions.

## Overview

NPC sensors detect entities, conditions, and triggers in the world. Sensors can detect:
- Players and NPCs
- Sound/hearing
- Sight/vision
- Scent/smell
- Block interactions
- Damage
- Paths
- States

## Location
Sensors are configured in NPC Instructions and can be defined as reusable components in `Server/NPC/Roles/_Core/Components/Sensors/`.

## Basic Sensor Structure

```json
{
  "Sensor": {
    "Type": "Mob",
    "Range": 10,
    "GetPlayers": true,
    "GetNPCs": true
  }
}
```

## Sensor Types

### Mob Sensor

```json
{
  "Type": "Mob",
  "Range": 15,
  "GetPlayers": true,
  "GetNPCs": true,
  "LockOnTarget": true
}
```

Detects players and NPCs within range.

### Sight Sensor

```json
{
  "Type": "Mob",
  "Range": 15,
  "ViewSector": 120,
  "Filters": [
    {
      "Type": "LineOfSight"
    },
    {
      "Type": "ViewSector",
      "ViewSector": 120
    }
  ]
}
```

Detects entities within vision cone with line-of-sight.

### Sound Sensor

```json
{
  "Type": "Mob",
  "Range": 10,
  "ThroughWalls": true,
  "Filters": [
    {
      "Type": "Not",
      "Filter": {
        "Type": "MovementState",
        "State": "Crouching"
      }
    }
  ]
}
```

Detects sound/hearing (can hear through walls).

### Scent Sensor

```json
{
  "Type": "Mob",
  "Range": 10,
  "Filters": [
    {
      "Type": "LineOfSight"
    }
  ]
}
```

Detects by scent/smell.

### Damage Sensor

```json
{
  "Sensor": {
    "Type": "Damage",
    "Combat": true
  }
}
```

Triggers when NPC takes damage.

### State Sensor

```json
{
  "Sensor": {
    "Type": "State",
    "State": "Idle"
  }
}
```

Triggers when NPC is in specific state.

### Path Sensor

```json
{
  "Sensor": {
    "Type": "Path",
    "PathType": "AnyPrefabPath",
    "Range": 30
  }
}
```

Detects nearby paths for patrols.

### Block Sensor

```json
{
  "Sensor": {
    "Type": "Block",
    "BlockType": "Plant_Grass"
  }
}
```

Detects specific block types.

## Sensor Properties

### Range

```json
{
  "Range": 15
}
```

Detection range in blocks.

### GetPlayers / GetNPCs

```json
{
  "GetPlayers": true,
  "GetNPCs": true
}
```

Whether to detect players or NPCs.

### LockOnTarget

```json
{
  "LockOnTarget": true
}
```

Whether sensor locks onto detected target.

### Filters

```json
{
  "Filters": [
    {
      "Type": "LineOfSight"
    },
    {
      "Type": "ViewSector",
      "ViewSector": 120
    },
    {
      "Type": "NPCGroup",
      "ExcludeGroups": ["Livestock"]
    }
  ]
}
```

Filter conditions for detection.

### Prioritiser

```json
{
  "Prioritiser": {
    "Type": "Attitude",
    "AttitudesByPriority": ["Hostile", "Neutral"]
  }
}
```

Prioritizes targets by attitude.

## Standard Detection Component

`Server/NPC/Roles/_Core/Components/Sensors/Component_Sensor_Standard_Detection.json`:

Combines absolute detection, sight, and sound sensors.

## Tips for NPC Sensors

1. **Combined sensors** - Use `Type: "Or"` to combine multiple sensors
2. **Filters** - Use filters to refine detection (line-of-sight, groups, etc.)
3. **Range balance** - Too large = too sensitive, too small = misses targets
4. **View sector** - Control vision cone angle
5. **Component reuse** - Create sensor components for reuse across NPCs

---

**Previous:** [NPC State Transitions](80_NPC_State_Transitions.md) | **Next:** [NPC Instructions](82_NPC_Instructions.md)
