# World Generation

Learn how to configure world generation, zones, biomes, and terrain generation in Hytale.

## Overview

World generation in Hytale uses a **complex node-based graph system**. Configuration files contain interconnected nodes that define terrain shape, material placement, and structure spawning. These files are typically edited in visual graph editors, not hand-written.

> **Important:** The world generation system uses deeply nested node graphs. The examples in this guide show actual file structures - they are complex by design.

### Update 2 (Jan 2026) — World Gen V2 & ore

- **World Gen V2** has been prepared for public documentation; expect new worldgen docs.
- **Default_Flat** and **Default_Void** templates now use World Gen V2.
- **Creative Hub Portal** spawns with World Gen V2 flat worldgen (clear weather, limited fog) in **new creative worlds only**.
- **Ore placement** was refactored in all zones: **more ores**, especially in Devastated Lands underground. **Existing worlds:** explore **new chunks** to see changes.

See [Patch Notes Update 2](Patch_Notes_Update_2.md) and [World Configuration](160_World_Configuration.md).

## Location
- World settings: `Server/HytaleGenerator/Settings/`
- World structures: `Server/HytaleGenerator/WorldStructures/`
- Biomes: `Server/HytaleGenerator/Biomes/`
- Assignments: `Server/HytaleGenerator/Assignments/`
- Density: `Server/HytaleGenerator/Density/`

## World Structures

World structures define the top-level configuration for terrain generation.

### Default World Structure

From `Server/HytaleGenerator/WorldStructures/Default.json`:

```1:11:Server/HytaleGenerator/WorldStructures/Default.json
{
  "Type": "NoiseRange",
  "DefaultBiome": "Basic",
  "MaxBiomeEdgeDistance" : 32,
  "DefaultTransitionDistance": 32,
  "Density": {
    "Type": "Imported",
    "Name": "Biome-Map"
  },
  "ContentFields": []
}
```

This is one of the simpler world structure files. Most world structures reference imported density maps.

## Biome Configuration (Node-Based)

Biomes use a **node-based graph system** for terrain generation. They are NOT simple key-value configs.

### Actual Biome Structure

From `Server/HytaleGenerator/Biomes/Basic.json` (simplified excerpt):

```json
{
  "$Title": "[ROOT] Biome",
  "Name": "Basic",
  "Terrain": {
    "Type": "DAOTerrain",
    "Density": {
      "Type": "Sum",
      "Inputs": [
        {
          "Type": "SimplexNoise2D",
          "Lacunarity": 10,
          "Persistence": 0.05,
          "Octaves": 1,
          "Scale": 150,
          "Seed": "A"
        },
        {
          "Type": "CurveMapper",
          "Curve": {
            "Type": "Manual",
            "Points": [
              { "In": 0, "Out": 1 },
              { "In": 50, "Out": -1 }
            ]
          },
          "Inputs": [
            {
              "Type": "BaseHeight",
              "BaseHeightName": "Base",
              "Distance": true
            }
          ]
        }
      ]
    }
  },
  "MaterialProvider": {
    "Type": "Solidity",
    "Solid": {
      "Type": "Queue",
      "Queue": [
        {
          "Type": "Constant",
          "Material": {
            "Solid": "Rock_Stone"
          }
        }
      ]
    },
    "Empty": {
      "Type": "Constant",
      "Material": {
        "Solid": "Empty"
      }
    }
  }
}
```

### Biome Node Types

| Node Type | Purpose |
|-----------|---------|
| `DAOTerrain` | Root terrain definition |
| `Sum` | Combines multiple density inputs |
| `SimplexNoise2D` | Generates noise patterns |
| `CurveMapper` | Maps values through curves |
| `BaseHeight` | References base height fields |
| `Solidity` | Material provider for solid/empty |
| `Queue` | Ordered material queue |
| `Constant` | Constant material value |

### Key Properties

- **`$Title`** - Display name for the node
- **`$Position`** - Visual editor position (X, Y)
- **`Type`** - Node type identifier
- **`Inputs`** - Array of child nodes
- **`Skip`** - Whether to skip this node

## Assignments (Node-Based)

Assignments define how structures spawn. They use **FieldFunction nodes** with weighted delimiters.

### Actual Assignment Structure

From `Server/HytaleGenerator/Assignments/Plains1/Plains1_Oak_Trees.json` (simplified):

```json
{
  "$Title": "[ROOT] Weighted Assignments",
  "Type": "FieldFunction",
  "ExportAs": "Plains1_Oak_Trees",
  "FieldFunction": {
    "Type": "SimplexNoise2D",
    "Lacunarity": 2,
    "Persistence": 0.5,
    "Octaves": 1,
    "Scale": 80,
    "Seed": "1235"
  },
  "Delimiters": [
    {
      "Max": 0.85,
      "Min": 0.7,
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
                "Type": "Union",
                "Props": [
                  {
                    "Type": "Prefab",
                    "WeightedPrefabPaths": [
                      { "Path": "Trees/Oak/Stage_0", "Weight": 60 },
                      { "Path": "Trees/Beech/Stage_0", "Weight": 20 }
                    ],
                    "Directionality": {
                      "Type": "Random",
                      "Seed": "A"
                    },
                    "Scanner": {
                      "Type": "ColumnLinear",
                      "MaxY": 60,
                      "MinY": 0,
                      "BaseHeightName": "Base"
                    }
                  }
                ]
              }
            }
          }
        ]
      }
    }
  ]
}
```

### Assignment Node Types

| Node Type | Purpose |
|-----------|---------|
| `FieldFunction` | Root assignment with noise field |
| `Weighted` | Weighted random selection |
| `Constant` | Constant assignment |
| `Union` | Combines multiple props |
| `Prefab` | Places prefab structures |
| `Cluster` | Clustered placement |
| `Column` | Column-based block placement |

### Delimiter Properties

- **`Min`/`Max`** - Noise range for this delimiter (0.0-1.0)
- **`Assignments`** - What to place in this range
- **`SkipChance`** - Probability of skipping placement

### Prefab Properties

- **`WeightedPrefabPaths`** - Array of weighted prefab paths
- **`Path`** - Prefab folder path
- **`Weight`** - Selection weight
- **`Scanner`** - How to scan for valid positions
- **`Directionality`** - Rotation/direction settings

## Density Configuration (Node-Based)

Density maps control biome distribution using complex node chains.

### Actual Density Structure

From `Server/HytaleGenerator/Density/Map_Default.json` (simplified excerpt):

```json
{
  "Type": "Exported",
  "ExportAs": "Biome-Map",
  "SingleInstance": true,
  "Inputs": [
    {
      "Type": "YOverride",
      "Value": 0,
      "Inputs": [
        {
          "Type": "Cache",
          "Capacity": 1,
          "Inputs": [
            {
              "Type": "Scale",
              "ScaleX": 1,
              "ScaleY": 1,
              "ScaleZ": 1,
              "Inputs": [
                {
                  "Type": "Mix",
                  "Inputs": [
                    {
                      "Type": "FastGradientWarp",
                      "WarpScale": 100,
                      "WarpFactor": 100,
                      "Seed": "A",
                      "Inputs": [...]
                    }
                  ]
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

### Density Node Types

| Node Type | Purpose |
|-----------|---------|
| `Exported` | Exports density for use elsewhere |
| `YOverride` | Overrides Y coordinate |
| `Cache` | Caches computed values |
| `Scale` | Scales density values |
| `Mix` | Mixes multiple inputs |
| `FastGradientWarp` | Warps using gradient noise |
| `Normalizer` | Normalizes value ranges |
| `PositionsCellNoise` | Cell-based noise |

## World Settings

### Basic Settings

`Server/HytaleGenerator/Settings/Settings.json`:

```json
{
  "StatsCheckpoints": [1, 100, 500, 1000],
  "CustomConcurrency": -1,
  "BufferCapacityFactor": 0.1,
  "TargetViewDistance": 512,
  "TargetPlayerCount": 3
}
```

These settings control generation performance, not terrain shape.

## Tips for World Generation

1. **Use visual editors** - These files are designed for node-based visual editing, not hand-writing
2. **Start with existing files** - Copy and modify existing biomes/assignments
3. **Understand the node graph** - Each file is a tree of interconnected nodes
4. **Test incrementally** - Small changes can have large effects
5. **Reference documentation** - See [Advanced Procedural World Generation](181_Procedural_World_Generation.md) for more details

## Related Documentation

- [World Configuration](160_World_Configuration.md) - Instance configuration
- [Biomes & Environments](24_Biomes_and_Environments.md) - Environment settings
- [Procedural World Generation](181_Procedural_World_Generation.md) - Advanced techniques

---

**Previous:** [Projectiles](37_Projectiles.md) | **Next:** Back to [README](../README.md)
