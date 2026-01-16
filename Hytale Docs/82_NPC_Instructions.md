# NPC Instructions

Learn how NPC instructions create behavior trees and define NPC AI behavior.

## Overview

NPC instructions define behavior trees that control NPC actions. Instructions use sensors to detect conditions and execute actions or body motions based on those conditions.

## Location
Instructions are configured in `Instructions` array in NPC Role definitions.

## Basic Instruction Structure

```json
{
  "Instructions": [
    {
      "Instructions": [
        {
          "Sensor": {
            "Type": "Any"
          },
          "BodyMotion": {
            "Type": "Wander"
          }
        }
      ]
    }
  ]
}
```

## Instruction Hierarchy

Instructions are nested behavior trees:

```json
{
  "Instructions": [
    {
      "Continue": true,
      "Instructions": [
        {
          "Sensor": {
            "Type": "Damage",
            "Combat": true
          },
          "Actions": [
            {
              "Type": "State",
              "State": "Combat"
            }
          ]
        },
        {
          "Sensor": {
            "Type": "Any"
          },
          "BodyMotion": {
            "Type": "Idle"
          }
        }
      ]
    }
  ]
}
```

## Sensor-Action Pattern

```json
{
  "Sensor": {
    "Type": "Mob",
    "Range": 15
  },
  "Actions": [
    {
      "Type": "State",
      "State": "Combat"
    }
  ]
}
```

When sensor detects, execute actions.

## Sensor-BodyMotion Pattern

```json
{
  "Sensor": {
    "Type": "Path",
    "PathType": "AnyPrefabPath"
  },
  "BodyMotion": {
    "Type": "Path",
    "UseNodeViewDirection": true
  }
}
```

When sensor detects, execute movement.

## Body Motion Types

### Wander

```json
{
  "BodyMotion": {
    "Type": "Wander",
    "MaxHeadingChange": 180
  }
}
```

Random wandering movement.

### Path

```json
{
  "BodyMotion": {
    "Type": "Path",
    "Shape": "Loop",
    "UseNodeViewDirection": true
  }
}
```

Follow a path.

### Nothing

```json
{
  "BodyMotion": {
    "Type": "Nothing"
  }
}
```

Stay in place.

### Flee

```json
{
  "BodyMotion": {
    "Type": "Flee"
  }
}
```

Flee from target.

## Instruction References

```json
{
  "Instructions": [
    {
      "Reference": "Component_Instruction_Intelligent_Idle_Motion_Follow_Path",
      "Modify": {
        "FollowPathRange": {
          "Value": 50
        }
      }
    }
  ]
}
```

Reference reusable instruction components.

## Continue Flag

```json
{
  "Continue": true,
  "Instructions": [ ... ]
}
```

When `true`, continues checking child instructions even after match.

## Tips for NPC Instructions

1. **Nested structure** - Instructions form behavior trees
2. **Sensor priority** - First matching sensor executes
3. **Continue flag** - Use for fallback behaviors
4. **Component reuse** - Reference instruction components
5. **State-based** - Combine with state transitions for complex behaviors

---

**Previous:** [NPC Sensors](81_NPC_Sensors.md) | **Next:** [NPC Motion Controllers](83_NPC_Motion_Controllers.md)
