# Adding Prefabs to World Generation

Step-by-step guide for adding custom prefabs to procedural world generation.

## Overview

Prefabs are added to world generation through a three-step pipeline:
1. **Prefab files** - The structure itself (`Server/Prefabs/`)
2. **Assignment files** - Define where/how prefabs spawn (`Server/HytaleGenerator/Assignments/`)
3. **Biome files** - Import assignments into biomes (`Server/HytaleGenerator/Biomes/`)

---

## The Complete Flow

```
Prefab File → Assignment → Biome → World Structure → Generated World
```

---

## Step 1: Create Your Prefab

Place your prefab in `Server/Prefabs/`:

```
Server/Prefabs/MyPrefabs/
├── Small/
│   ├── MyStructure_Small_001.prefab.json
│   └── MyStructure_Small_002.prefab.json
└── Large/
    └── MyStructure_Large_001.prefab.json
```

---

## Step 2: Create an Assignment

Assignments define placement rules. Create a file in `Server/HytaleGenerator/Assignments/`.

### Basic Tree Assignment

From `Server/HytaleGenerator/Assignments/Plains1/Plains1_Oak_Trees.json`:

```json
{
  "$Title": "[ROOT] Weighted Assignments",
  "Type": "FieldFunction",
  "ExportAs": "Plains1_Oak_Trees",
  "FieldFunction": {
    "Type": "SimplexNoise2D",
    "Seed": "1235",
    "Scale": 80,
    "Octaves": 1,
    "Persistence": 0.5,
    "Lacunarity": 2
  },
  "Delimiters": [
    {
      "Min": 0.7,
      "Max": 0.85,
      "Assignments": {
        "Type": "Weighted",
        "SkipChance": 0,
        "Seed": "A",
        "WeightedAssignments": [
          {
            "Weight": 70,
            "Assignments": {
              "Type": "Constant",
              "Prop": {
                "Type": "Prefab",
                "WeightedPrefabPaths": [
                  { "Path": "Trees/Oak/Stage_0", "Weight": 60 },
                  { "Path": "Trees/Beech/Stage_0", "Weight": 20 }
                ],
                "LegacyPath": false,
                "LoadEntities": true,
                "Directionality": {
                  "Type": "Random",
                  "Seed": "A",
                  "Pattern": {
                    "Type": "Floor",
                    "Origin": {
                      "Type": "BlockSet",
                      "BlockSet": {
                        "Inclusive": true,
                        "Materials": [{ "Solid": "Empty" }]
                      }
                    },
                    "Floor": {
                      "Type": "BlockSet",
                      "BlockSet": {
                        "Inclusive": true,
                        "Materials": [
                          { "Solid": "Soil_Grass" },
                          { "Solid": "Soil_Grass_Sunny" }
                        ]
                      }
                    }
                  }
                },
                "Scanner": {
                  "Type": "ColumnLinear",
                  "MaxY": 60,
                  "MinY": 0,
                  "RelativeToPosition": false,
                  "BaseHeightName": "Base",
                  "TopDownOrder": true,
                  "ResultCap": 1
                }
              }
            }
          }
        ]
      }
    }
  ]
}
```

---

## Step 3: Import Assignment into Biome

Biomes import assignments using the `ExportAs` name.

From `Server/HytaleGenerator/Biomes/Plains1/Plains1_Oak.json`:

```json
{
  "Assignments": {
    "Type": "Imported",
    "Name": "Plains1_Oak_Trees"
  }
}
```

---

## Prefab Node Properties

### WeightedPrefabPaths

Define which prefabs spawn and their probability:

```json
{
  "WeightedPrefabPaths": [
    { "Path": "Trees/Oak/Stage_4", "Weight": 70 },
    { "Path": "Trees/Oak/Stage_5", "Weight": 30 },
    { "Path": "Trees/Beech/Stage_4", "Weight": 15 }
  ]
}
```

| Property | Description |
|----------|-------------|
| `Path` | Relative path from `Server/Prefabs/` |
| `Weight` | Probability weight (higher = more common) |

**Path formats:**
- Folder: `"Trees/Oak/Stage_0"` - Picks random file from folder
- File: `"Trees/Oak/Stage_0/Oak_001.prefab.json"` - Specific file

### LoadEntities

```json
{
  "LoadEntities": true
}
```

If `true`, spawns NPCs/entities defined in the prefab.

### Directionality

Controls prefab rotation:

**Random rotation:**
```json
{
  "Directionality": {
    "Type": "Random",
    "Seed": "A",
    "Pattern": { ... }
  }
}
```

**Fixed rotation:**
```json
{
  "Directionality": {
    "Type": "Static",
    "Rotation": 0
  }
}
```

**Face direction:**
```json
{
  "Directionality": {
    "Type": "Facing",
    "Direction": "North"
  }
}
```

### Pattern (Placement Rules)

Floor pattern - requires specific ground:

```json
{
  "Pattern": {
    "Type": "Floor",
    "Origin": {
      "Type": "BlockSet",
      "BlockSet": {
        "Inclusive": true,
        "Materials": [{ "Solid": "Empty" }]
      }
    },
    "Floor": {
      "Type": "BlockSet",
      "BlockSet": {
        "Inclusive": true,
        "Materials": [
          { "Solid": "Soil_Grass" },
          { "Solid": "Soil_Dirt" }
        ]
      }
    }
  }
}
```

| Property | Description |
|----------|-------------|
| `Origin` | Block type at spawn point (usually `Empty`/air) |
| `Floor` | Required ground blocks |

### Scanner

Defines search area for valid locations:

```json
{
  "Scanner": {
    "Type": "ColumnLinear",
    "MaxY": 60,
    "MinY": 0,
    "RelativeToPosition": false,
    "BaseHeightName": "Base",
    "TopDownOrder": true,
    "ResultCap": 1
  }
}
```

| Property | Description |
|----------|-------------|
| `MaxY` / `MinY` | Height range to search |
| `TopDownOrder` | Search from top down |
| `ResultCap` | Max placements per column |
| `BaseHeightName` | Height reference |

---

## Assignment FieldFunction

Controls spawn density using noise:

```json
{
  "Type": "FieldFunction",
  "FieldFunction": {
    "Type": "SimplexNoise2D",
    "Seed": "MyPrefab",
    "Scale": 80,
    "Octaves": 1,
    "Persistence": 0.5
  },
  "Delimiters": [
    {
      "Min": 0.7,
      "Max": 1.0,
      "Assignments": { ... }
    }
  ]
}
```

- `Scale`: Higher = larger clusters
- `Min`/`Max`: Noise threshold for spawning (0.7-1.0 = 30% of area)

---

## Complete Example: Custom Monument

### 1. Prefab

`Server/Prefabs/MyMod/Monuments/Shrine_001.prefab.json`

### 2. Assignment

`Server/HytaleGenerator/Assignments/MyMod/MyMod_Shrines.json`:

```json
{
  "$Title": "[ROOT] My Mod Shrines",
  "Type": "FieldFunction",
  "ExportAs": "MyMod_Shrines",
  "FieldFunction": {
    "Type": "SimplexNoise2D",
    "Seed": "MyMod-Shrines",
    "Scale": 200,
    "Octaves": 1,
    "Persistence": 0.5,
    "Lacunarity": 2
  },
  "Delimiters": [
    {
      "Min": 0.9,
      "Max": 1.0,
      "Assignments": {
        "Type": "Weighted",
        "SkipChance": 0.5,
        "Seed": "Shrine",
        "WeightedAssignments": [
          {
            "Weight": 100,
            "Assignments": {
              "Type": "Constant",
              "Prop": {
                "Type": "Prefab",
                "WeightedPrefabPaths": [
                  { "Path": "MyMod/Monuments/Shrine_001.prefab.json", "Weight": 1 }
                ],
                "LegacyPath": false,
                "LoadEntities": true,
                "Directionality": {
                  "Type": "Random",
                  "Seed": "A",
                  "Pattern": {
                    "Type": "Floor",
                    "Origin": {
                      "Type": "BlockType",
                      "Material": { "Solid": "Empty" }
                    },
                    "Floor": {
                      "Type": "BlockSet",
                      "BlockSet": {
                        "Inclusive": true,
                        "Materials": [
                          { "Solid": "Soil_Grass" },
                          { "Solid": "Soil_Dirt" },
                          { "Solid": "Rock_Stone" }
                        ]
                      }
                    }
                  }
                },
                "Scanner": {
                  "Type": "ColumnLinear",
                  "MaxY": 50,
                  "MinY": -20,
                  "RelativeToPosition": false,
                  "BaseHeightName": "Base",
                  "TopDownOrder": true,
                  "ResultCap": 1
                },
                "MoldingDirection": "DOWN",
                "MoldingChildren": false
              }
            }
          }
        ]
      }
    }
  ]
}
```

### 3. Add to Biome

In your biome file, add the assignment:

```json
{
  "Assignments": {
    "Type": "Union",
    "Assignments": [
      {
        "Type": "Imported",
        "Name": "Plains1_Oak_Trees"
      },
      {
        "Type": "Imported",
        "Name": "MyMod_Shrines"
      }
    ]
  }
}
```

---

## Tips

1. **Unique seeds** - Each assignment needs unique seeds
2. **Scale for rarity** - Higher `Scale` + higher `Min` = rarer spawns
3. **SkipChance** - Add randomness (0.5 = 50% skip)
4. **Test in creative** - Fly around to verify spawn rates
5. **Floor materials** - Match biome ground types
6. **Scanner height** - Match terrain elevation range
7. **LoadEntities** - Set `false` for pure decoration

---

**Related:** [Prefabs](186_Prefabs.md) | [Prefab Catalog](195_Prefab_Catalog.md) | [World Generation](38_World_Generation.md) | [Zone Layers](191_Zone_Layers.md)
