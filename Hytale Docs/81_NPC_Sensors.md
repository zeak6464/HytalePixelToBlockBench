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

## Example from Game Files

### NPC Sensors from Template

From `Server/NPC/Roles/_Core/Templates/Template_Intelligent.json`:

```17:32:Server/NPC/Roles/_Core/Templates/Template_Intelligent.json
    "ViewRange": {
      "Value": 15,
      "Description": "The range from which the target will be seen. If zero, sight will be disabled."
    },
    "ViewSector": {
      "Value": 180,
      "Description": "The view sector within which the target needs to be to be seen."
    },
    "HearingRange": {
      "Value": 8,
      "Description": "The range from which the target will be heard. If zero, hearing will be disabled."
    },
    "AbsoluteDetectionRange": {
      "Value": 2,
      "Description": "The range at which a target is guaranteed to be detected. If zero, abosulte detection will be disabled."
    },
```

This shows NPC sensor parameters for sight (ViewRange, ViewSector) and hearing (HearingRange), along with absolute detection range.

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

### Beacon Sensor (NPC-to-NPC messages)

The **Beacon** sensor lets NPCs react to messages sent by other NPCs (or systems), optionally capturing a target into a slot.

From `Template_Edible_Critter.json` (used by the `Edible_Rat` flow):

```63:73:Server/NPC/Roles/_Core/Templates/Template_Edible_Critter.json
              "Sensor": {
                "Type": "Beacon",
                "Message": "Approach_Target",
                "TargetSlot": "LockedTarget"
              },
              "Actions": [
                {
                  "Type": "State",
                  "State": "Seek"
                }
              ]
```

Another example from `Template_Goblin_Ogre.json` (reacting while asleep):

```414:425:Server/NPC/Roles/Intelligent/Aggressive/Goblin/Templates/Template_Goblin_Ogre.json
            {
              "Sensor": {
                "Type": "Beacon",
                "Message": "Annoy_Ogre",
                "Range": 5
              },
              "Actions": [
                {
                  "Type": "Attack",
                  "Attack": { "Compute": "SleepingAttack" },
                  "AttackPauseRange": [ 1, 2 ]
                }
              ]
            }
```

Common fields:

- **`Message`**: String message ID.
- **`Range`**: Optional max distance for receiving the message.
- **`TargetSlot`**: Optional slot to store the received target (commonly `LockedTarget`).

### Target Sensor

Use the **Target** sensor to check distance/validity against a stored target slot.

From `Template_Edible_Critter.json`:

```96:105:Server/NPC/Roles/_Core/Templates/Template_Edible_Critter.json
              "Sensor": {
                "Type": "Target",
                "TargetSlot": "LockedTarget",
                "Range": { "Compute": "SeekRange" }
              },
              "BodyMotion": {
                "Type": "Seek",
                "SlowDownDistance": 0.1,
                "StopDistance": 0.1
              }
```

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

Combines absolute detection, sight, and sound sensors. Here's the actual structure:

```json
{
  "Type": "Component",
  "Class": "Sensor",
  "Parameters": {
    "ViewRange": {
      "Value": 10,
      "Description": "The range from which the target will be seen"
    },
    "ViewSector": {
      "Value": 120,
      "Description": "The view sector within which the target needs to be"
    },
    "HearingRange": {
      "Value": 10,
      "Description": "The range from which the target will be heard"
    },
    "AbsoluteDetectionRange": {
      "Value": 2,
      "Description": "Range at which target is guaranteed detected"
    },
    "ThroughWalls": {
      "Value": true,
      "Description": "Whether NPC can hear through obstacles"
    },
    "Attitudes": {
      "Value": ["Hostile", "Revered", "Friendly", "Neutral"],
      "Description": "Prioritized list of attitudes to respond to"
    }
  },
  "Content": {
    "Type": "Or",
    "Sensors": [
      // Absolute detection (close range, guaranteed)
      {
        "Type": "Mob",
        "Range": { "Compute": "AbsoluteDetectionRange" },
        "LockOnTarget": true
      },
      // Sight (with line of sight and view sector)
      {
        "Type": "Mob",
        "Range": { "Compute": "ViewRange" },
        "Filters": [
          { "Type": "LineOfSight" },
          { "Type": "ViewSector", "ViewSector": { "Compute": "ViewSector" } }
        ]
      },
      // Sound (can hear crouching players)
      {
        "Type": "Mob",
        "Range": { "Compute": "HearingRange" },
        "Filters": [
          {
            "Type": "Not",
            "Filter": { "Type": "MovementState", "State": "Crouching" }
          }
        ]
      }
    ]
  }
}
```

### Available Sensor Components

| Component | Description |
|-----------|-------------|
| `Component_Sensor_Standard_Detection` | Combined sight, sound, absolute |
| `Component_Sensor_Sight` | Vision-only detection |
| `Component_Sensor_Sight_By_Attitude` | Vision filtered by attitude |
| `Component_Sensor_Sound` | Hearing-only detection |
| `Component_Sensor_Sound_By_Attitude` | Hearing filtered by attitude |
| `Component_Sensor_Day` | Triggers during daytime |
| `Component_Sensor_Night` | Triggers during nighttime |
| `Component_Sensor_Is_Indoors` | Detects if NPC is indoors |
| `Component_Sensor_Leads_Flock` | Flock leadership check |
| `Component_Sensor_Timed_Nap` | Sleep cycle sensor |
| `Component_Sensor_Threatened` | Detects threat level |
| `Component_Sensor_Lost_Target_Detection` | Target tracking lost |

## Debug Commands

Use these debug commands to visualize NPC sensors in-game:

```
/npc debug set <flag>
/npc debug presets    -- lists all available flags
```

### VisSensorRanges

```
/npc debug set VisSensorRanges
```

Visualizes all currently checked sensor ranges as rings and view sectors:
- Detection ranges shown as circles
- View cones displayed as sectors
- Entities matched by sensors are highlighted

**Example:** A sleeping Bear shows only one sensor active. When awake, it displays hearing, vision, and absolute detection ranges.

### VisMarkedTargets

```
/npc debug set VisMarkedTargets
```

Shows all current marked targets locked onto by the NPC. Useful for understanding:
- When NPCs change focus
- What the current target is in a crowd
- How the `LockedTarget` slot works

### VisAiming

```
/npc debug set VisAiming
```

Shows what NPCs are actually aiming at. Works for NPCs that have aim instructions in their logic (ranged attackers, etc.).

---

## Official Resources

- **Generated NPC Documentation:** https://hytalemodding.dev/en/docs/official-documentation/npc-doc
- **NPC Tutorial:** https://hytalemodding.dev/en/docs/official-documentation/npc/1-know-your-enemy
- **Video Tutorials:**
  - [Part 1](https://youtu.be/vsbYytAI-_o) - [Part 2](https://youtu.be/Za_wipUM-i8) - [Part 3](https://youtu.be/n44o2ABVhy4)
  - [Part 4](https://youtu.be/jg-IUZopAi8) - [Part 5](https://youtu.be/tSVeuUguCwA) - [Part 6](https://youtu.be/hgelFDVhmYw)

---

## Tips for NPC Sensors

1. **Combined sensors** - Use `Type: "Or"` to combine multiple sensors
2. **Filters** - Use filters to refine detection (line-of-sight, groups, etc.)
3. **Range balance** - Too large = too sensitive, too small = misses targets
4. **View sector** - Control vision cone angle
5. **Component reuse** - Create sensor components for reuse across NPCs
6. **Debug visualization** - Use `/npc debug set VisSensorRanges` to test sensors

---

**Previous:** [NPC State Transitions](80_NPC_State_Transitions.md) | **Next:** [NPC Instructions](82_NPC_Instructions.md)
