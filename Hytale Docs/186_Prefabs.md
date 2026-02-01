# Prefabs

Learn how prefabs work, their file structure, organization, and how to create custom prefabs for world generation.

## Overview

Prefabs are pre-built block structures used for world generation. They include trees, rocks, dungeons, monuments, caves, and NPC buildings. Prefabs are JSON files containing block positions and types.

## Location

`Server/Prefabs/`

## Prefab File Structure

### Basic Structure

From `Server/Prefabs/Trees/Oak/Stage_0/Oak_Stage0_001.prefab.json`:

```json
{
  "version": 8,
  "blockIdVersion": 3,
  "anchorX": 0,
  "anchorY": 0,
  "anchorZ": 0,
  "blocks": [
    {
      "x": 0,
      "y": 0,
      "z": 0,
      "name": "Wood_Oak_Branch_Long"
    },
    {
      "x": 0,
      "y": 1,
      "z": 0,
      "name": "Wood_Oak_Branch_Long"
    },
    {
      "x": -1,
      "y": 3,
      "z": 0,
      "name": "Plant_Leaves_Oak"
    }
  ]
}
```

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `version` | Integer | Prefab format version (current: 8) |
| `blockIdVersion` | Integer | Block ID mapping version (current: 3) |
| `anchorX` | Integer | X anchor point for placement |
| `anchorY` | Integer | Y anchor point for placement |
| `anchorZ` | Integer | Z anchor point for placement |
| `blocks` | Array | Array of block definitions |

### Block Definition

Each block in the `blocks` array has:

| Property | Type | Description |
|----------|------|-------------|
| `x` | Integer | X position relative to anchor |
| `y` | Integer | Y position relative to anchor |
| `z` | Integer | Z position relative to anchor |
| `name` | String | Block item ID |
| `state` | String | (Optional) Block state |
| `rotation` | Integer | (Optional) Block rotation (0-3) |

## Directory Organization

Prefabs are organized by category:

```
Server/Prefabs/
├── Trees/           # 1,027 files - All tree types
│   ├── Oak/         # Oak trees by growth stage
│   │   ├── Stage_0/ # Saplings
│   │   ├── Stage_1/ # Small trees
│   │   ├── Stage_2/ # Medium trees
│   │   └── ...
│   ├── Birch/
│   ├── Fir/
│   └── ...
├── Rock_Formations/ # 1,676 files - Boulders, crystals, stalactites
├── Mineshaft/       # 1,160 files - Mineshaft segments
├── Npc/             # 856 files - NPC structures (villages, camps)
│   ├── Kweebec/
│   ├── Trork/
│   ├── Feran/
│   └── ...
├── Monuments/       # 800 files - Large structures
├── Dungeon/         # 858 files - Dungeon rooms and segments
├── Plants/          # 555 files - Vegetation prefabs
├── Cave/            # 492 files - Cave formations
├── Testing/         # 154 files - Test prefabs
├── Mineshaft_Drift/ # 117 files - Drift mine segments
├── Spawn/           # 20 files - Spawn-related prefabs
└── Unique/          # Special one-off prefabs
```

## Major Prefab Categories

### Trees

Trees are organized by species and growth stage:

```
Trees/{Species}/Stage_{N}/{Species}_Stage{N}_{Variant}.prefab.json
```

**Growth Stages:**
- `Stage_0`, `Stage_00` - Saplings (smallest)
- `Stage_1` through `Stage_3` - Small to medium
- `Stage_4` through `Stage_7` - Large trees
- `Stumps/` - Cut tree stumps

**Tree Species:** Oak, Birch, Fir, Cedar, Redwood, Willow, Palm, Bamboo, Maple, Aspen, Beech, and many more.

### Rock Formations

```
Rock_Formations/{Type}/{Type}_{Variant}.prefab.json
```

Includes: Boulders, crystals, stalactites, stalagmites, rubble, coral.

### Dungeons

```
Dungeon/{DungeonType}/{RoomType}/{Room}_{Variant}.prefab.json
```

Dungeon types include various themed areas with rooms, corridors, and special chambers.

### NPC Structures

```
Npc/{Race}/{StructureType}/{Structure}_{Variant}.prefab.json
```

**Races:** Kweebec, Trork, Feran, Outlander, Void, etc.

**Structure Types:** Huts, camps, villages, workshops, guard posts.

## Using Prefabs

### In World Generation

Prefabs are referenced in world generation assignments via PrefabLists:

```json
{
  "Type": "Prefab",
  "WeightedPrefabPaths": [
    { "Path": "Trees/Oak/Stage_0", "Weight": 60 },
    { "Path": "Trees/Beech/Stage_0", "Weight": 20 }
  ]
}
```

See [World Generation](38_World_Generation.md) and [Prefab Lists](49_Prefab_Lists.md).

### In Interactions

Spawn prefabs via interactions:

```json
{
  "Type": "SpawnPrefab",
  "PrefabPath": "Trees/Oak/Stage_0/Oak_Stage0_001"
}
```

See [Interaction Type: SpawnPrefab](152_Interaction_Type_SpawnPrefab.md).

## Creating Custom Prefabs

### Method 1: Prefab Editor

Use the in-game Prefab Editor to create and save prefabs:

1. Enter Creative Mode
2. Build your structure
3. Use the selection tool to select blocks
4. Save as prefab

See [Prefab Editor Settings](162_Prefab_Editor_Settings.md).

### Method 2: Manual JSON

Create a JSON file manually:

```json
{
  "version": 8,
  "blockIdVersion": 3,
  "anchorX": 0,
  "anchorY": 0,
  "anchorZ": 0,
  "blocks": [
    { "x": 0, "y": 0, "z": 0, "name": "Rock_Stone" },
    { "x": 1, "y": 0, "z": 0, "name": "Rock_Stone" },
    { "x": 0, "y": 1, "z": 0, "name": "Rock_Stone" },
    { "x": 1, "y": 1, "z": 0, "name": "Rock_Stone" }
  ]
}
```

### Naming Convention

Follow the existing naming pattern:

```
{Category}_{Type}_{Variant}.prefab.json
```

Examples:
- `Oak_Stage0_001.prefab.json`
- `Boulder_Large_A.prefab.json`
- `Kweebec_Hut_Small_001.prefab.json`

## Anchor Points

The anchor (`anchorX`, `anchorY`, `anchorZ`) determines the placement origin:

- **Trees:** Anchor at base of trunk (Y=0)
- **Boulders:** Anchor at bottom center
- **Buildings:** Anchor at entrance or center

## Block States and Rotation

Some blocks support states and rotations:

```json
{
  "x": 5,
  "y": 0,
  "z": 3,
  "name": "Furniture_Chair",
  "state": "Occupied",
  "rotation": 2
}
```

- **`rotation`** - 0-3 (90° increments)
- **`state`** - Block-specific state (e.g., "On", "Off", "Occupied")

## Tips for Prefabs

1. **Use consistent anchors** - Place anchors at logical points (base, center)
2. **Follow naming conventions** - Match existing patterns for organization
3. **Test in-game** - Verify placement and appearance
4. **Use growth stages** - For trees, create multiple sizes
5. **Create variants** - Multiple versions add variety to world gen
6. **Consider rotation** - Prefabs may be rotated during placement

## Related Documentation

- [Prefab Lists](49_Prefab_Lists.md) - Grouping prefabs for world generation
- [Prefab Editor Settings](162_Prefab_Editor_Settings.md) - Editor configuration
- [World Generation](38_World_Generation.md) - Using prefabs in world gen
- [Interaction Type: SpawnPrefab](152_Interaction_Type_SpawnPrefab.md) - Spawning prefabs

---

**Previous:** [NPC Spawn Beacons](185_NPC_Spawn_Beacons.md) | **Next:** [Scripted Brushes](45_Scripted_Brushes.md)
