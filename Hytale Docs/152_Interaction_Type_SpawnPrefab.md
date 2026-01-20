# Interaction Type: SpawnPrefab

Spawn prefab structures into the world.

## Overview

`SpawnPrefab` spawns prefab structures (buildings, dungeons, structures) at specified locations. Supports position offsets, rotation, and forced placement.

## Example from Game Files

### Spawn Prefab Interaction

Spawn prefab interactions spawn prefab structures. These are used for structure spawning, building placement, and prefab-based mechanics.

## Basic Structure

```json
{
  "Type": "SpawnPrefab",
  "PrefabPath": "Example_Portal1.prefab.json",
  "Offset": {
    "X": 6,
    "Y": 0,
    "Z": 3
  },
  "RotationYaw": "OneEighty",
  "OriginSource": "Block",
  "Force": true
}
```

## Properties

### PrefabPath

```json
{
  "PrefabPath": "Example_Portal1.prefab.json"
}
```

Path to prefab file (relative to prefab directory).

### Offset

```json
{
  "Offset": {
    "X": 6,
    "Y": 0,
    "Z": 3
  }
}
```

Position offset relative to spawn source (blocks).

### RotationYaw

```json
{
  "RotationYaw": "OneEighty"
}
```

Rotation at spawn:
- **`"Zero"`** - 0 degrees
- **`"Ninety"`** - 90 degrees
- **`"OneEighty"`** - 180 degrees
- **`"TwoSeventy"`** - 270 degrees

### OriginSource

```json
{
  "OriginSource": "Block"
}
```

Source of spawn:
- **`"Block"`** - Spawning from a block
- **`"Entity"`** - Spawning from an entity

### Force

```json
{
  "Force": true
}
```

If `true`, forces placement even if space is occupied. If `false`, may fail if space is occupied.

## Complete Examples

### Example 1: Portal Prefab

```json
{
  "Type": "SpawnPrefab",
  "PrefabPath": "Example_Portal1.prefab.json",
  "Offset": {
    "X": 6,
    "Y": 0,
    "Z": 3
  },
  "RotationYaw": "OneEighty",
  "OriginSource": "Block",
  "Force": true
}
```

Spawns portal prefab 6 blocks right, 3 blocks forward, rotated 180 degrees.

### Example 2: Building Spawn

```json
{
  "Type": "SpawnPrefab",
  "PrefabPath": "Building_House.prefab.json",
  "Offset": {
    "X": 0,
    "Y": 0,
    "Z": 0
  },
  "RotationYaw": "Zero",
  "Force": false
}
```

Spawns building at source location if space is clear.

## Tips

1. **Prefab paths** - Use relative paths to prefab files
2. **Offset** - Position prefabs relative to spawn source
3. **Rotation** - Use RotationYaw for structure orientation
4. **Force** - Use `true` for guaranteed placement, `false` for validation
5. **Clear space** - Ensure sufficient space before spawning large prefabs

---

**Previous:** [Explode](151_Interaction_Type_Explode.md) | **Next:** [SpawnDrops](153_Interaction_Type_SpawnDrops.md)
