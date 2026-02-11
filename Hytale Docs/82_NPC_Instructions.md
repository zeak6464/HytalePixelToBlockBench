# NPC Instructions

Learn how NPC instructions create behavior trees and define NPC AI behavior.

## Overview

NPC instructions define behavior trees that control NPC actions. Instructions use sensors to detect conditions and execute actions or body motions based on those conditions.

### Instruction Lists vs Behavior Trees

Hytale uses "instruction lists" rather than traditional behavior trees. The key difference is in **traversal semantics**:

- **Behavior trees** may follow different semantics depending on the node type
- **Instruction lists** always follow **fallback selector node** semantics

Each instruction is evaluated in order and - if matched - executed. Unless specific flags are included (like `Continue: true`), no further instructions in the list are evaluated. This ensures the flow of logic is easy to follow, no matter how large or deep the trees grow.

> **From the official docs:** "We have more than 150 different element types (sensors, actions, motions, etc) that can be combined to build behaviors."

## Location
Instructions are configured in `Instructions` array in NPC Role definitions.

## Example from Game Files

### NPC Instructions from Template

From `Server/NPC/Roles/_Core/Templates/Template_Intelligent.json`:

NPC instructions use sensors to detect conditions and execute actions. For example, when a Path sensor detects a path within range, the NPC follows it using Path BodyMotion.

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

## Official Resources

- **Generated NPC Documentation:** https://hytalemodding.dev/en/docs/official-documentation/npc-doc
- **NPC Tutorial:** https://hytalemodding.dev/en/docs/official-documentation/npc/1-know-your-enemy
- **Video Tutorials:**
  - [Part 1](https://youtu.be/vsbYytAI-_o) - [Part 2](https://youtu.be/Za_wipUM-i8) - [Part 3](https://youtu.be/n44o2ABVhy4)
  - [Part 4](https://youtu.be/jg-IUZopAi8) - [Part 5](https://youtu.be/tSVeuUguCwA) - [Part 6](https://youtu.be/hgelFDVhmYw)

---

## Tips for NPC Instructions

1. **Nested structure** - Instructions form behavior trees with fallback selector semantics
2. **Sensor priority** - First matching sensor executes (unless `Continue: true`)
3. **Continue flag** - Use for fallback behaviors or parallel checks
4. **Component reuse** - Reference instruction components for maintainability
5. **State-based** - Combine with state transitions for complex behaviors
6. **$Comment fields** - Add comments for documentation (ignored by the game)

---

**Previous:** [NPC Sensors](81_NPC_Sensors.md) | **Next:** [NPC Motion Controllers](83_NPC_Motion_Controllers.md)
