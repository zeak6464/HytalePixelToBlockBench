# Interaction Type: SpawnNPC

Spawn NPCs into the world with position offsets and custom configurations.

## Overview

`SpawnNPC` spawns NPCs at specified locations relative to the interaction source (block, item, or entity). Useful for summoning, spawning systems, and world generation.

## Basic Structure

```json
{
  "Type": "SpawnNPC",
  "EntityId": "Sheep",
  "SpawnOffset": {
    "X": 0,
    "Y": 0.5,
    "Z": 0
  }
}
```

## Properties

### EntityId

```json
{
  "EntityId": "Sheep"
}
```

ID of NPC role to spawn. Must match NPC role definition.

### SpawnOffset

```json
{
  "SpawnOffset": {
    "X": 0,
    "Y": 0.5,
    "Z": 0
  }
}
```

Position offset relative to interaction source:
- **`X`** - Left/right (blocks)
- **`Y`** - Up/down (blocks, typically 0.5 for ground spawn)
- **`Z`** - Forward/back (blocks)

## Complete Examples

### Example 1: Basic Spawn

```json
{
  "Type": "SpawnNPC",
  "EntityId": "Sheep",
  "SpawnOffset": {
    "X": 0,
    "Y": 0.5,
    "Z": 0
  }
}
```

Spawns sheep at interaction source position.

### Example 2: Spawn in Front

```json
{
  "Type": "SpawnNPC",
  "EntityId": "Wolf",
  "SpawnOffset": {
    "X": 0,
    "Y": 0.5,
    "Z": -2
  }
}
```

Spawns wolf 2 blocks in front of source.

### Example 3: Multiple Spawns

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "SpawnNPC",
      "EntityId": "Goblin",
      "SpawnOffset": {
        "X": -1,
        "Y": 0.5,
        "Z": 0
      }
    },
    {
      "Type": "SpawnNPC",
      "EntityId": "Goblin",
      "SpawnOffset": {
        "X": 1,
        "Y": 0.5,
        "Z": 0
      }
    }
  ]
}
```

Spawns two goblins on either side.

## Tips

1. **Y offset** - Use 0.5 for ground spawns (half block height)
2. **Z direction** - Negative Z is forward (toward player view)
3. **Spacing** - Offset multiple spawns to avoid overlap
4. **Entity IDs** - Must match NPC role IDs exactly
5. **Combine with Replace** - Use interaction variables for dynamic spawns

---

**Previous:** [ApplyForce](124_Interaction_Type_ApplyForce.md) | **Next:** [Heal](126_Interaction_Type_Heal.md)
