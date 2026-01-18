# HytaleGenerator Assignments

Learn how to configure assignments for placing prefabs, structures, and features in biomes using `Server/HytaleGenerator/Assignments/`.

## Overview

Assignments define how structures, prefabs, and features are placed in biomes during world generation. They use a node-based system with field functions, delimiters, weighted assignments, and property configurations to control placement probability and location.

## Location

`Server/HytaleGenerator/Assignments/{Zone}/`

Examples:
- `Server/HytaleGenerator/Assignments/Plains1/`
- `Server/HytaleGenerator/Assignments/Desert1/`
- `Server/HytaleGenerator/Assignments/Taiga1/`
- `Server/HytaleGenerator/Assignments/Volcanic1/`

## Basic Assignment Structure

Assignments are complex node-based systems. Basic structure:

`Server/HytaleGenerator/Assignments/Plains1/Plains1_Oak_Trees.json`:

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
        "WeightedAssignments": [...]
      }
    }
  ]
}
```

## Assignment Components

### FieldFunction

Base noise function for placement probability:

```json
{
  "FieldFunction": {
    "Type": "SimplexNoise2D",
    "Seed": "1235",
    "Scale": 80,
    "Octaves": 1,
    "Persistence": 0.5,
    "Lacunarity": 2
  }
}
```

**Properties:**
- **`Type`** - Noise type (`"SimplexNoise2D"`)
- **`Seed`** - Random seed string
- **`Scale`** - Noise scale (80 = larger patterns)
- **`Octaves`** - Detail levels (1 = simple)
- **`Persistence`** - Detail persistence (0.5)
- **`Lacunarity`** - Frequency multiplier (2)

### Delimiters

Value ranges that determine when assignments apply:

```json
{
  "Delimiters": [
    {
      "Min": 0.7,
      "Max": 0.85,
      "Assignments": {
        "Type": "Constant",
        "Prop": {...}
      }
    },
    {
      "Min": 0.85,
      "Max": 1.0,
      "Assignments": {
        "Type": "Constant",
        "Prop": {...}
      }
    }
  ]
}
```

**Properties:**
- **`Min`** - Minimum field function value (0.0 to 1.0)
- **`Max`** - Maximum field function value (0.0 to 1.0)
- **`Assignments`** - What to place in this range

### Weighted Assignments

Multiple assignment options with probability weights:

```json
{
  "Type": "Weighted",
  "Seed": "A",
  "SkipChance": 0,
  "WeightedAssignments": [
    {
      "Weight": 70,
      "Assignments": {
        "Type": "Constant",
        "Prop": {
          "Type": "Prefab",
          "WeightedPrefabPaths": [
            {
              "Path": "Trees/Oak/Stage_0",
              "Weight": 60
            },
            {
              "Path": "Trees/Beech/Stage_0",
              "Weight": 20
            }
          ]
        }
      }
    },
    {
      "Weight": 30,
      "Assignments": {
        "Type": "Constant",
        "Prop": {
          "Type": "Cluster",
          "Range": 10,
          "WeightedProps": [...]
        }
      }
    }
  ]
}
```

**Properties:**
- **`Seed`** - Random seed for weight selection
- **`SkipChance`** - Probability of skipping (0.0 to 1.0)
- **`WeightedAssignments`** - Array of weighted options
  - **`Weight`** - Probability weight (higher = more likely)
  - **`Assignments`** - What to place when selected

### Property Types

#### Prefab Property

Places prefab structures:

```json
{
  "Type": "Prefab",
  "WeightedPrefabPaths": [
    {
      "Path": "Trees/Oak/Stage_0",
      "Weight": 60
    }
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
          "Materials": [
            {"Solid": "Empty"}
          ]
        }
      },
      "Floor": {
        "Type": "BlockSet",
        "BlockSet": {
          "Inclusive": true,
          "Materials": [
            {"Solid": "Soil_Grass"},
            {"Solid": "Soil_Grass_Sunny"}
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
  },
  "MoldingDirection": "None",
  "MoldingChildren": false
}
```

**Properties:**
- **`WeightedPrefabPaths`** - Array of prefab paths with weights
- **`Path`** - Prefab path relative to `Server/Prefabs/`
- **`Weight`** - Probability weight for each prefab
- **`LegacyPath`** - Whether to use legacy path format
- **`LoadEntities`** - Whether to spawn entities in prefab
- **`Directionality`** - How prefab is oriented:
  - **`Type`** - `"Random"`, `"Static"`, `"Facing"`
  - **`Pattern`** - Placement pattern (Floor, Origin blocks)
- **`Scanner`** - Where to search for placement:
  - **`Type`** - `"ColumnLinear"`, `"Column"`, `"Volume"`
  - **`MaxY`** / **`MinY`** - Height range
  - **`BaseHeightName`** - Which base height to use
  - **`TopDownOrder`** - Search order
  - **`ResultCap`** - Max placements per position

#### Cluster Property

Places features in clusters:

```json
{
  "Type": "Cluster",
  "Range": 10,
  "Seed": "A",
  "DistanceCurve": {
    "Type": "Manual",
    "Points": [
      {"In": 9, "Out": 0.005},
      {"In": 10, "Out": 0}
    ]
  },
  "WeightedProps": [
    {
      "Weight": 1,
      "ColumnProp": {
        "Type": "Column",
        "ColumnBlocks": [
          {
            "Y": 0,
            "Material": {
              "Solid": "Wood_Sticks"
            }
          }
        ]
      }
    }
  ],
  "Pattern": {
    "Type": "Imported",
    "Name": "Plains1_OakPattern_Floor"
  },
  "Scanner": {
    "Type": "ColumnLinear",
    "MaxY": 5,
    "MinY": -5,
    "RelativeToPosition": true,
    "BaseHeightName": "Base",
    "TopDownOrder": true,
    "ResultCap": 1
  }
}
```

**Properties:**
- **`Range`** - Cluster radius in blocks
- **`Seed`** - Random seed for cluster placement
- **`DistanceCurve`** - Probability curve based on distance from center
  - **`Type`** - `"Manual"` (curve points) or curve reference
  - **`Points`** - Array of `{In, Out}` pairs
- **`WeightedProps`** - What to place in cluster
- **`Pattern`** - Placement pattern restriction

#### Column Property

Places vertical columns of blocks:

```json
{
  "Type": "Column",
  "ColumnBlocks": [
    {
      "Y": 0,
      "Material": {
        "Solid": "Wood_Sticks"
      }
    },
    {
      "Y": 1,
      "Material": {
        "Solid": "Plant_Grass_Tall"
      }
    }
  ],
  "Directionality": {
    "Type": "Static",
    "Rotation": 0,
    "Pattern": {
      "Type": "Imported",
      "Name": "Plains1_OakPattern_Floor_Trees"
    }
  },
  "Scanner": {
    "Type": "ColumnLinear",
    "MaxY": 5,
    "MinY": -5,
    "RelativeToPosition": true,
    "BaseHeightName": "Base",
    "TopDownOrder": true,
    "ResultCap": 1
  }
}
```

**Properties:**
- **`ColumnBlocks`** - Array of blocks at specific Y offsets
  - **`Y`** - Vertical offset (0 = base, positive = up)
  - **`Material`** - Block type to place
- **`Directionality`** - Rotation and pattern
- **`Scanner`** - Placement search area

#### Union Property

Combines multiple properties:

```json
{
  "Type": "Union",
  "Props": [
    {
      "Type": "Prefab",
      "WeightedPrefabPaths": [...]
    },
    {
      "Type": "Cluster",
      "Range": 10,
      "WeightedProps": [...]
    }
  ]
}
```

Places all properties in the union together.

## Common Assignment Types

### Tree Assignments

`Server/HytaleGenerator/Assignments/Plains1/Plains1_Oak_Trees.json`:

Places trees with different stages based on noise values:

```json
{
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
        "WeightedAssignments": [
          {
            "Weight": 70,
            "Assignments": {
              "Type": "Constant",
              "Prop": {
                "Type": "Prefab",
                "WeightedPrefabPaths": [
                  {"Path": "Trees/Oak/Stage_0", "Weight": 60},
                  {"Path": "Trees/Beech/Stage_0", "Weight": 20}
                ]
              }
            }
          },
          {
            "Weight": 30,
            "Assignments": {
              "Type": "Constant",
              "Prop": {
                "Type": "Cluster",
                "Range": 10,
                "WeightedProps": [
                  {
                    "Weight": 1,
                    "ColumnProp": {
                      "Type": "Column",
                      "ColumnBlocks": [
                        {"Y": 0, "Material": {"Solid": "Wood_Sticks"}}
                      ]
                    }
                  }
                ]
              }
            }
          }
        ]
      }
    },
    {
      "Min": 0.85,
      "Max": 1.0,
      "Assignments": {
        "Type": "Constant",
        "Prop": {
          "Type": "Prefab",
          "WeightedPrefabPaths": [
            {"Path": "Trees/Oak/Stage_4", "Weight": 70},
            {"Path": "Trees/Oak/Stage_5", "Weight": 30}
          ]
        }
      }
    }
  ]
}
```

### Grass/Plant Assignments

Uses clusters to place plants:

```json
{
  "Type": "FieldFunction",
  "FieldFunction": {
    "Type": "SimplexNoise2D",
    "Seed": "Grass_Plains",
    "Scale": 50,
    "Octaves": 2,
    "Persistence": 0.6
  },
  "Delimiters": [
    {
      "Min": 0.5,
      "Max": 0.9,
      "Assignments": {
        "Type": "Constant",
        "Prop": {
          "Type": "Cluster",
          "Range": 5,
          "DistanceCurve": {
            "Type": "Manual",
            "Points": [
              {"In": 0, "Out": 1.0},
              {"In": 5, "Out": 0}
            ]
          },
          "WeightedProps": [
            {
              "Weight": 1,
              "ColumnProp": {
                "Type": "Column",
                "ColumnBlocks": [
                  {"Y": 0, "Material": {"Solid": "Plant_Grass_Arid"}}
                ]
              }
            }
          ]
        }
      }
    }
  ]
}
```

## Export and Import

### Exporting Assignments

Assignments can export their field functions for use elsewhere:

```json
{
  "ExportAs": "Plains1_Oak_Trees",
  "FieldFunction": {...}
}
```

### Importing Patterns

Assignments can import patterns from other assignments:

```json
{
  "Pattern": {
    "Type": "Imported",
    "Name": "Plains1_OakPattern_Floor_Trees"
  }
}
```

## Scanner Types

### ColumnLinear

Searches vertically along a column:

```json
{
  "Type": "ColumnLinear",
  "MaxY": 60,
  "MinY": 0,
  "RelativeToPosition": false,
  "BaseHeightName": "Base",
  "TopDownOrder": true,
  "ResultCap": 1
}
```

**Properties:**
- **`MaxY`** - Maximum Y to search
- **`MinY`** - Minimum Y to search
- **`RelativeToPosition`** - If `true`, offsets from placement position; if `false`, uses world coordinates
- **`BaseHeightName`** - Which base height to use (`"Base"`, `"Bedrock"`)
- **`TopDownOrder`** - Search from top to bottom (`true`) or bottom to top (`false`)
- **`ResultCap`** - Maximum number of placements per position

## Directionality

### Random Rotation

```json
{
  "Type": "Random",
  "Seed": "A",
  "Pattern": {
    "Type": "Floor",
    "Origin": {...},
    "Floor": {...}
  }
}
```

### Static Rotation

```json
{
  "Type": "Static",
  "Rotation": 0,
  "Pattern": {
    "Type": "Imported",
    "Name": "Plains1_OakPattern_Floor_Trees"
  }
}
```

## Creating Custom Assignments

### Step 1: Create Assignment File

Create `Server/HytaleGenerator/Assignments/MyZone/MyZone_Trees.json`:

```json
{
  "$Title": "[ROOT] Weighted Assignments",
  "Type": "FieldFunction",
  "ExportAs": "MyZone_Trees",
  "FieldFunction": {
    "Type": "SimplexNoise2D",
    "Seed": "MyZone_Trees",
    "Scale": 100,
    "Octaves": 1,
    "Persistence": 0.5,
    "Lacunarity": 2
  },
  "Delimiters": [
    {
      "Min": 0.6,
      "Max": 1.0,
      "Assignments": {
        "Type": "Constant",
        "Prop": {
          "Type": "Prefab",
          "WeightedPrefabPaths": [
            {
              "Path": "Trees/MyCustom/Tree_1",
              "Weight": 100
            }
          ],
          "LoadEntities": true,
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
```

### Step 2: Reference in Biome

Reference in biome configuration (typically in biome graph nodes).

## Tips

1. **Start Simple** - Begin with basic field functions and constant assignments
2. **Test Delimiters** - Adjust `Min`/`Max` values to control placement frequency
3. **Weight Balance** - Use weights to balance different prefab/feature types
4. **Scanner Settings** - Adjust `MaxY`/`MinY` to control placement height
5. **Pattern Import** - Reuse patterns across assignments
6. **Export Functions** - Export field functions for reuse
7. **Cluster Range** - Use appropriate cluster ranges (5-15 blocks for plants, 10-20 for trees)
8. **Distance Curves** - Adjust distance curves to control cluster density

---

**Previous:** [Prefab Editor Settings](162_Prefab_Editor_Settings.md) | **Next:** [HytaleGenerator Density](164_HytaleGenerator_Density.md)
