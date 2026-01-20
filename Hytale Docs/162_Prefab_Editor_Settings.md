# Prefab Editor Settings

Learn how to configure the prefab editor creation settings for world building and prefab placement.

## Overview

Prefab editor settings configure how prefabs are placed and processed in the prefab editor. These settings control prefab alignment, stacking, processing behavior, and world generation parameters.

## Example from Game Files

Prefab editor settings configure how prefabs are placed and processed in the prefab editor. These settings control prefab alignment, stacking, processing behavior, and world generation parameters.

## Location

`Server/PrefabEditorCreationSettings/`

## Basic Prefab Editor Settings Structure

`Server/PrefabEditorCreationSettings/Prefab Editor Demo.json`:

```json
{
  "RootDirectory": "Asset",
  "UnprocessedPrefabPaths": [
    "Monuments/Encounter/Zone1/Tier1/Outpost/"
  ],
  "PasteYLevelGoal": 55,
  "BlocksBetweenEachPrefab": 15,
  "WorldGenType": "Flat",
  "BlocksAboveSurface": 0,
  "PrefabStackingAxis": "X",
  "PrefabAlignment": "Anchor",
  "RecursiveSearch": true,
  "LoadChildren": false,
  "LoadEntities": true
}
```

## Prefab Editor Settings Properties

### RootDirectory

Root directory for prefab paths:

```json
{
  "RootDirectory": "Asset"
}
```

**Values:**
- **`"Asset"`** - Assets directory (prefabs in `Server/Prefabs/`)

### UnprocessedPrefabPaths

Array of prefab paths to skip during processing:

```json
{
  "UnprocessedPrefabPaths": [
    "Monuments/Encounter/Zone1/Tier1/Outpost/",
    "Structures/Custom/Building/"
  ]
}
```

Prefabs in these paths will not be processed when using the prefab editor with these settings.

**Path Format:**
- Relative to root directory
- Use forward slashes `/`
- Trailing slash recommended

### PasteYLevelGoal

Target Y level for prefab placement:

```json
{
  "PasteYLevelGoal": 55
}
```

Y coordinate where prefabs should be placed. Used for aligning prefabs to specific height levels.

### BlocksBetweenEachPrefab

Spacing between prefabs:

```json
{
  "BlocksBetweenEachPrefab": 15
}
```

Number of blocks between each prefab when stacking. Used to prevent prefabs from overlapping.

### WorldGenType

World generation type for prefab testing:

```json
{
  "WorldGenType": "Flat"
}
```

**Values:**
- **`"Flat"`** - Flat world for testing
- **`"HytaleGenerator"`** - Full world generation
- **`"Void"`** - Empty void world

### BlocksAboveSurface

Vertical offset above surface:

```json
{
  "BlocksAboveSurface": 0
}
```

Blocks above the surface to place prefabs. Used for floating structures or surface alignment.

### PrefabStackingAxis

Axis for prefab stacking:

```json
{
  "PrefabStackingAxis": "X"
}
```

**Values:**
- **`"X"`** - Stack along X-axis (east-west)
- **`"Z"`** - Stack along Z-axis (north-south)
- **`"Y"`** - Stack vertically

### PrefabAlignment

How prefabs align when placed:

```json
{
  "PrefabAlignment": "Anchor"
}
```

**Values:**
- **`"Anchor"`** - Align to prefab anchor point
- **`"Corner"`** - Align to corner
- **`"Center"`** - Align to center

### RecursiveSearch

Whether to search subdirectories:

```json
{
  "RecursiveSearch": true
}
```

If `true`, includes prefabs in subdirectories. If `false`, only includes prefabs in the specified directory.

### LoadChildren

Whether to load child prefabs:

```json
{
  "LoadChildren": false
}
```

If `true`, loads nested prefab references. If `false`, only loads the main prefab.

### LoadEntities

Whether to load entities (NPCs, items):

```json
{
  "LoadEntities": true
}
```

If `true`, spawns entities defined in the prefab. If `false`, only places blocks.

## Creating Custom Prefab Editor Settings

### Step 1: Create Settings File

Create `Server/PrefabEditorCreationSettings/MyPrefabSettings.json`:

```json
{
  "RootDirectory": "Asset",
  "PasteYLevelGoal": 60,
  "BlocksBetweenEachPrefab": 20,
  "WorldGenType": "Flat",
  "BlocksAboveSurface": 2,
  "PrefabStackingAxis": "Z",
  "PrefabAlignment": "Anchor",
  "RecursiveSearch": true,
  "LoadChildren": false,
  "LoadEntities": true
}
```

### Step 2: Configure for Your Use Case

**For Building Testing:**
```json
{
  "WorldGenType": "Flat",
  "PasteYLevelGoal": 100,
  "BlocksBetweenEachPrefab": 50,
  "LoadEntities": false
}
```

**For Dungeon Placement:**
```json
{
  "WorldGenType": "HytaleGenerator",
  "PasteYLevelGoal": 0,
  "BlocksBetweenEachPrefab": 10,
  "PrefabAlignment": "Corner",
  "LoadEntities": true
}
```

**For Structure Testing:**
```json
{
  "WorldGenType": "Flat",
  "BlocksAboveSurface": 5,
  "PrefabStackingAxis": "X",
  "RecursiveSearch": true,
  "LoadChildren": true
}
```

## Common Settings Configurations

### Flat World Building

For building and testing prefabs:

```json
{
  "RootDirectory": "Asset",
  "PasteYLevelGoal": 100,
  "BlocksBetweenEachPrefab": 50,
  "WorldGenType": "Flat",
  "BlocksAboveSurface": 0,
  "PrefabStackingAxis": "X",
  "PrefabAlignment": "Anchor",
  "RecursiveSearch": true,
  "LoadChildren": false,
  "LoadEntities": false
}
```

### World Generation Placement

For placing prefabs in generated worlds:

```json
{
  "RootDirectory": "Asset",
  "PasteYLevelGoal": 0,
  "BlocksBetweenEachPrefab": 30,
  "WorldGenType": "HytaleGenerator",
  "BlocksAboveSurface": 0,
  "PrefabStackingAxis": "X",
  "PrefabAlignment": "Anchor",
  "RecursiveSearch": true,
  "LoadChildren": true,
  "LoadEntities": true
}
```

### Encounter Testing

For testing encounter prefabs:

```json
{
  "RootDirectory": "Asset",
  "UnprocessedPrefabPaths": [],
  "PasteYLevelGoal": 55,
  "BlocksBetweenEachPrefab": 100,
  "WorldGenType": "HytaleGenerator",
  "BlocksAboveSurface": 0,
  "PrefabStackingAxis": "X",
  "PrefabAlignment": "Corner",
  "RecursiveSearch": false,
  "LoadChildren": true,
  "LoadEntities": true
}
```

## Tips

1. **PasteYLevelGoal** - Set to surface level for your target biome/zone
2. **BlocksBetweenEachPrefab** - Increase spacing for large structures
3. **LoadEntities** - Set to `false` when testing block placement only
4. **RecursiveSearch** - Use `true` to include all prefabs in subdirectories
5. **UnprocessedPrefabPaths** - Exclude prefabs you don't want to process
6. **WorldGenType** - Use `"Flat"` for faster testing, `"HytaleGenerator"` for realistic placement
7. **PrefabAlignment** - Use `"Anchor"` for consistent placement, `"Corner"` for grid alignment

---

**Previous:** [Projectile Configs](161_Projectile_Configs.md) | **Next:** [HytaleGenerator Assignments](163_HytaleGenerator_Assignments.md)
