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

## Pathfinding System Attributes

These attributes control the pathfinding algorithm behavior for Seek, Flee, and Path body motions.

### Core Pathfinding Settings

```json
{
  "BodyMotion": {
    "Type": "Seek",
    "UseSteering": true,
    "UsePathfinder": true,
    "UseBestPath": true
  }
}
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `UseSteering` | Boolean | true | Enable steering behavior for smooth movement |
| `UsePathfinder` | Boolean | true | Enable A* pathfinding algorithm |
| `UseBestPath` | Boolean | true | Use the optimal path found by pathfinder |

### Throttle Settings

Controls how often pathfinding recalculates:

```json
{
  "ThrottleDelayRange": [0.5, 1.0],
  "ThrottleIgnoreCount": 3
}
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `ThrottleDelayRange` | Number[2] | [0.5, 1.0] | Min/max delay between path recalculations (seconds) |
| `ThrottleIgnoreCount` | Number | 0 | Number of recalculation requests to ignore before recalculating |

### Path Smoothing

```json
{
  "PathSmoothing": 0.5,
  "BlendHeading": true,
  "RejectionWeight": 0.3
}
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `PathSmoothing` | Number | 0 | Amount of smoothing applied to path (0-1) |
| `BlendHeading` | Boolean | false | Smoothly blend heading direction changes |
| `RejectionWeight` | Number | 0 | Weight for rejecting certain path options |

### Algorithm Limits

Control pathfinding performance and complexity:

```json
{
  "StepsPerTick": 100,
  "MaxPathLength": 50,
  "MaxOpenNodes": 1000,
  "MaxTotalNodes": 5000
}
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `StepsPerTick` | Number | 100 | Maximum pathfinding steps per game tick |
| `MaxPathLength` | Number | 50 | Maximum path length in nodes |
| `MaxOpenNodes` | Number | 1000 | Maximum open nodes in A* algorithm |
| `MaxTotalNodes` | Number | 5000 | Maximum total nodes explored |

### Movement Options

```json
{
  "DiagonalMoves": true,
  "BuildOptimisedPath": true,
  "AvoidBlockDamage": true
}
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `DiagonalMoves` | Boolean | true | Allow diagonal movement in pathfinding |
| `BuildOptimisedPath` | Boolean | true | Optimize path after calculation |
| `AvoidBlockDamage` | Boolean | false | Avoid blocks that cause damage (lava, spikes, etc.) |

### Complete Pathfinding Example

```json
{
  "BodyMotion": {
    "Type": "Seek",
    "UseSteering": true,
    "UsePathfinder": true,
    "UseBestPath": true,
    "ThrottleDelayRange": [0.3, 0.8],
    "ThrottleIgnoreCount": 2,
    "PathSmoothing": 0.4,
    "BlendHeading": true,
    "StepsPerTick": 150,
    "MaxPathLength": 100,
    "MaxOpenNodes": 2000,
    "MaxTotalNodes": 8000,
    "DiagonalMoves": true,
    "BuildOptimisedPath": true,
    "AvoidBlockDamage": true,
    "RelativeSpeed": 1.0,
    "StopDistance": 2
  }
}
```

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

## Tips for NPC Pathfinding

1. **Prefab paths** - Define paths in prefabs for structured patrols
2. **World paths** - Use world paths for dynamic routes
3. **Node delays** - Add delays for realistic patrol behavior
4. **Fallback behavior** - Use "Any" sensor for fallback (wander)
5. **Path shapes** - Use Loop for continuous patrols

---

**Previous:** [Entity Damage Types](73_Entity_Damage_Types.md) | **Next:** Back to [README](../README.md)
