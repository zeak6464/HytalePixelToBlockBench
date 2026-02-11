# NPC Pathfinding

Learn how to configure NPC pathfinding and patrol paths for NPC movement behavior.

## Overview

NPC pathfinding allows NPCs to follow predefined paths, patrol routes, and move intelligently through the world. Paths can be defined in prefabs or world paths.

## Location
NPC pathfinding is configured in NPC Role Instructions, using Path sensors and BodyMotion.

## Example from Game Files

### NPC Path Following

From `Server/NPC/Roles/_Core/Templates/Template_Intelligent.json`:

```571:582:Server/NPC/Roles/_Core/Templates/Template_Intelligent.json
                  "$Comment": "Prefer to follow a path if it exists",
                  "Enabled": { "Compute": "FollowPatrolPath" },
                  "Sensor": {
                    "Type": "Path",
                    "Path": { "Compute": "PatrolPathName" },
                    "Range": 20
                  },
                  "BodyMotion": {
                    "Type": "Path",
                    "MinRelSpeed": 0.18,
                    "MaxRelSpeed": 0.25
                  }
```

This shows NPC pathfinding configuration using a Path sensor to detect paths and Path BodyMotion to follow them at specified speeds.

### Square Patrol Path

From `Server/NPC/Roles/_Core/Components/Paths/Component_Path_Square.json`:

```1:30:Server/NPC/Roles/_Core/Components/Paths/Component_Path_Square.json
{
  "Class": "Path",
  "Parameters": {
    "Scale": {
      "Value": 1,
      "Description": "Overall path scale"
    }
  },
  "Content": {
    "Scale": { "Compute": "Scale" },
    "Waypoints": [
      {
        "Distance": 5
      },
      {
        "Rotation": 90,
        "Distance": 5
      },
      {
        "Rotation": 90,
        "Distance": 5
      },
      {
        "Rotation": 90,
        "Distance": 5
      }
    ]
  },
  "Type": "Component"
}
```

This shows a path definition for NPCs to patrol in a square pattern with waypoints at 90-degree rotations.

## Path Types

### WorldPath

```json
{
  "Sensor": {
    "Type": "Path",
    "PathType": "WorldPath",
    "Path": "p1,p2,p3"
  },
  "BodyMotion": {
    "Type": "Path",
    "Shape": "Points",
    "Direction": "Random"
  }
}
```

Follows world-defined path nodes.

### AnyPrefabPath

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

Follows paths defined in prefabs.

## Path Motion Properties

### Shape

```json
{
  "Shape": "Loop"  // or "Points", "Line"
}
```

- **`Loop`** - Loops the path
- **`Points`** - Goes through points once
- **`Line`** - Linear path

### Direction

```json
{
  "Direction": "Forward"  // or "Random", "Backward"
}
```

Path traversal direction.

### Speed

```json
{
  "MinRelSpeed": 0.8,
  "MaxRelSpeed": 1.0
}
```

Relative speed range for path following.

### Node Behavior

```json
{
  "StartAtNearestNode": true,
  "UseNodeViewDirection": true,
  "PickRandomAngle": true,
  "MinNodeDelay": 2,
  "MaxNodeDelay": 6
}
```

- **`StartAtNearestNode`** - Start at closest path node
- **`UseNodeViewDirection`** - Face node direction
- **`PickRandomAngle`** - Randomize facing angle
- **`MinNodeDelay/MaxNodeDelay`** - Wait time at nodes

### Distance

```json
{
  "MinWalkDistance": 2,
  "MaxWalkDistance": 5
}
```

Distance range for path following.

## Complete Example: Patrol Path

```json
{
  "Instructions": [
    {
      "Instructions": [
        {
          "Sensor": {
            "Type": "Path",
            "PathType": "AnyPrefabPath"
          },
          "BodyMotion": {
            "Type": "Path",
            "UseNodeViewDirection": true,
            "PickRandomAngle": true
          }
        },
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

NPC follows prefab paths when available, otherwise wanders.

## Path Components

NPC components can handle pathfinding:

```json
{
  "Class": "Instruction",
  "Parameters": {
    "FollowPathRange": {
      "Value": 30,
      "Description": "Search radius for patrol paths"
    },
    "PathShape": {
      "Value": "Line",
      "Description": "Path shape to follow"
    }
  },
  "Content": {
    "Sensor": {
      "Type": "Path",
      "Range": { "Compute": "FollowPathRange" }
    },
    "BodyMotion": {
      "Type": "Path",
      "UseNodeViewDirection": true,
      "Shape": { "Compute": "PathShape" }
    }
  }
}
```

## Debug Commands

Use these commands to visualize pathfinding and leashing:

```
/npc debug set VisLeash
```

Shows the NPC's leash position and max distance. Useful for debugging:
- Why an NPC stops chasing
- Where the "home" position is set
- Leash boundary visualization

---

## Known Limitations

NPC pathfinding has documented limitations:

1. **Complex Terrain** - NPCs may struggle with cliffs, water, or irregular surfaces
2. **Multi-Level Structures** - Vertical navigation can be problematic
3. **Dynamic Obstacles** - Moving blocks or entities may not be avoided properly
4. **Path Updates** - Paths are not always recalculated when the world changes

### Mitigation Strategies

- Design paths that avoid complex terrain
- Use `MaxClimbHeight` appropriately in motion controllers
- Set reasonable `LeashDistance` values to prevent NPCs wandering too far
- Test patrol routes thoroughly before deployment

---

## Official Resources

- **Generated NPC Documentation:** https://hytalemodding.dev/en/docs/official-documentation/npc-doc
- **NPC Tutorial:** https://hytalemodding.dev/en/docs/official-documentation/npc/1-know-your-enemy
- **Video Tutorials:**
  - [Part 1](https://youtu.be/vsbYytAI-_o) - [Part 2](https://youtu.be/Za_wipUM-i8) - [Part 3](https://youtu.be/n44o2ABVhy4)
  - [Part 4](https://youtu.be/jg-IUZopAi8) - [Part 5](https://youtu.be/tSVeuUguCwA) - [Part 6](https://youtu.be/hgelFDVhmYw)

---

## Tips for NPC Pathfinding

1. **Prefab paths** - Define paths in prefabs for structured patrols
2. **World paths** - Use world paths for dynamic routes
3. **Node delays** - Add delays for realistic patrol behavior
4. **Fallback behavior** - Use "Any" sensor for fallback (wander)
5. **Path shapes** - Use Loop for continuous patrols
6. **Debug visualization** - Use `/npc debug set VisLeash` to see leash boundaries

---

**Previous:** [Entity Damage Types](73_Entity_Damage_Types.md) | **Next:** Back to [README](../README.md)
