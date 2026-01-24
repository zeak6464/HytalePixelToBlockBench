# Procedural World Generation - Advanced Guide

Master Hytale's world generation: create custom biomes, place structures, control terrain, and build complete zones.

## Overview

This guide shows how to integrate HytaleGenerator systems to create complete custom biomes:
- **Density Maps** - Define terrain shape
- **Assignments** - Place structures/prefabs
- **Block Masks** - Control placement rules
- **Biomes** - Combine everything together
- **World Structures** - Zone-level configuration

All systems working together for procedural generation.

**Update 2 (Jan 2026):** **World Gen V2** has been prepared for public documentation. **Default_Flat** and **Default_Void** use World Gen V2. Ore placement was refactored (more ores, especially Devastated Lands); existing worlds need **new chunks** for ore changes. See [Patch Notes Update 2](Patch_Notes_Update_2.md).

---

## World Generation Pipeline

```
1. Density Maps (terrain shape)
   ↓
2. Biomes (surface blocks, grass, dirt)
   ↓
3. Assignments (trees, rocks, structures)
   ↓
4. Block Masks (placement rules)
   ↓
5. World Structure (final integration)
```

---

## Complete Example: Custom Forest Biome

Let's create a complete custom forest biome step-by-step.

---

### Step 1: Create Density Map

File: `Server/HytaleGenerator/Density/MyForest_Terrain.json`

```json
{
  "$NodeId": "Cache.Density",
  "Type": "Cache",
  "ExportAs": "MyForest_Terrain",
  "Skip": false,
  "Capacity": 3,
  "Inputs": [{
    "$NodeId": "Sum.Density",
    "Type": "Sum",
    "Skip": false,
    "Inputs": [
      {
        "$NodeId": "SimplexNoise2D.Base",
        "Type": "SimplexNoise2D",
        "Skip": false,
        "Lacunarity": 2,
        "Persistence": 0.5,
        "Octaves": 3,
        "Scale": 200,
        "Seed": "MyForest-Base"
      },
      {
        "$NodeId": "CurveMapper.Height",
        "Type": "CurveMapper",
        "Skip": false,
        "Curve": {
          "Type": "Manual",
          "Points": [
            {
              "In": -80,
              "Out": -1
            },
            {
              "In": 60,
              "Out": 1
            }
          ]
        },
        "Inputs": [{
          "Type": "BaseHeight",
          "Skip": false,
          "BaseHeightName": "Base",
          "Distance": true
        }]
      }
    ]
  }]
}
```

**What this does:**
- Creates terrain density using Simplex Noise
- Maps height to density curve
- Caches result for performance

---

### Step 2: Create Tree Assignment

File: `Server/HytaleGenerator/Assignments/MyForest/MyForest_Oak_Trees.json`

```json
{
  "$Title": "[ROOT] Weighted Assignments",
  "Type": "FieldFunction",
  "ExportAs": "MyForest_Oak_Trees",
  "FieldFunction": {
    "Type": "SimplexNoise2D",
    "Skip": false,
    "Lacunarity": 2,
    "Persistence": 0.5,
    "Octaves": 1,
    "Scale": 60,
    "Seed": "Trees"
  },
  "Delimiters": [
    {
      "Min": 0.6,
      "Max": 1.0,
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
                "Skip": false,
                "WeightedPrefabPaths": [
                  {
                    "Path": "Trees/Oak/Stage_4",
                    "Weight": 60
                  },
                  {
                    "Path": "Trees/Oak/Stage_5",
                    "Weight": 40
                  }
                ],
                "LegacyPath": false,
                "LoadEntities": true,
                "Directionality": {
                  "Type": "Random",
                  "Seed": "A",
                  "Pattern": {
                    "Type": "Floor",
                    "Skip": false,
                    "Origin": {
                      "Type": "BlockSet",
                      "Skip": false,
                      "BlockSet": {
                        "Inclusive": true,
                        "Materials": [
                          {
                            "Solid": "Empty"
                          }
                        ]
                      }
                    },
                    "Floor": {
                      "Type": "BlockSet",
                      "Skip": false,
                      "BlockSet": {
                        "Inclusive": true,
                        "Materials": [
                          {
                            "Solid": "Soil_Grass"
                          },
                          {
                            "Solid": "Soil_Dirt"
                          }
                        ]
                      }
                    }
                  }
                },
                "Scanner": {
                  "Type": "ColumnLinear",
                  "Skip": false,
                  "MaxY": 60,
                  "MinY": 0,
                  "RelativeToPosition": false,
                  "BaseHeightName": "Water",
                  "TopDownOrder": true,
                  "ResultCap": 1
                }
              }
            }
          },
          {
            "Weight": 30,
            "Assignments": {
              "Type": "Constant",
              "Prop": {
                "Type": "Column",
                "Skip": false,
                "ColumnBlocks": [
                  {
                    "Y": 0,
                    "Material": {
                      "Solid": "Plant_Bush_Green"
                    }
                  }
                ],
                "Scanner": {
                  "Type": "ColumnLinear",
                  "Skip": false,
                  "MaxY": 60,
                  "MinY": 0,
                  "RelativeToPosition": false,
                  "BaseHeightName": "Water",
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

**What this does:**
- Uses noise (Scale: 60) to determine tree placement
- When noise value is 0.6-1.0:
  - 70% chance: Place Oak tree (Stage 4 or 5)
  - 30% chance: Place bush
- Only places on grass/dirt
- Scans from water level up to find suitable spot

---

### Step 3: Create Block Mask

File: `Server/HytaleGenerator/BlockMasks/MyForest_Trees.json`

```json
{
  "$Title": "[ROOT] BlockMask",
  "ExportAs": "MyForest_Trees",
  "DontPlace": {
    "Inclusive": true,
    "Materials": [
      {
        "Solid": "Wood_Oak_Roots"
      },
      {
        "Solid": "Wood_Oak_Log"
      }
    ]
  },
  "DontReplace": {
    "Inclusive": false,
    "Materials": [
      {
        "Solid": "Empty"
      },
      {
        "Solid": "Soil_Grass"
      },
      {
        "Solid": "Soil_Dirt"
      },
      {
        "Solid": "Soil_Leaves"
      }
    ]
  }
}
```

**What this does:**
- **DontPlace** - Cannot place trees on existing tree blocks
- **DontReplace** - Only replace air, grass, dirt, leaves (preserves stone, water, etc.)

---

### Step 4: Create Biome

File: `Server/HytaleGenerator/Biomes/MyForest/MyForest_Main.json`

```json
{
  "$Title": "MyForest Main Biome",
  "ExportAs": "MyForest_Main",
  "SurfaceBlocks": [
    {
      "Y": 0,
      "Material": {
        "Solid": "Soil_Grass"
      }
    },
    {
      "Y": -1,
      "Material": {
        "Solid": "Soil_Dirt"
      }
    },
    {
      "Y": -2,
      "Material": {
        "Solid": "Soil_Dirt"
      }
    },
    {
      "Y": -3,
      "Material": {
        "Solid": "Rock_Stone"
      }
    }
  ],
  "CaveBlocks": [
    {
      "Material": {
        "Solid": "Rock_Stone"
      }
    }
  ],
  "WaterBlock": {
    "Solid": "Water"
  },
  "Assignments": [
    "MyForest_Oak_Trees",
    "MyForest_Rocks",
    "MyForest_Flowers"
  ],
  "Environment": "Zone1"
}
```

**What this does:**
- Top layer: Grass
- Next 2 layers: Dirt
- Below: Stone
- Water blocks: Water
- Applies assignments (trees, rocks, flowers)
- Uses Zone1 environment (weather, lighting)

---

### Step 5: Create World Structure

File: `Server/HytaleGenerator/WorldStructures/MyCustomWorld.json`

```json
{
  "$Title": "My Custom World",
  "GroundLevel": 64,
  "WaterLevel": 63,
  "BaseHeights": [
    {
      "Name": "Base",
      "Type": "SimplexNoise2D",
      "Seed": "BaseHeight",
      "Scale": 300,
      "Octaves": 4,
      "Persistence": 0.5,
      "Lacunarity": 2.0,
      "HeightScaling": 30,
      "HeightOffset": 64
    },
    {
      "Name": "Water",
      "Type": "Constant",
      "Value": 63
    }
  ],
  "Stacks": [
    {
      "Biome": "MyForest_Main",
      "Density": "MyForest_Terrain",
      "HeightRange": {
        "Min": 0,
        "Max": 255
      }
    }
  ]
}
```

**What this does:**
- Sets ground level (Y=64) and water level (Y=63)
- Defines base terrain height using noise
- Links biome + density map together
- Generates terrain from Y=0 to Y=255

---

## Assignment Types

### Prefab Assignment

Place structures:

```json
{
  "Type": "Prefab",
  "WeightedPrefabPaths": [
    {
      "Path": "Trees/Oak/Stage_4",
      "Weight": 60
    },
    {
      "Path": "Trees/Beech/Stage_0",
      "Weight": 20
    }
  ],
  "LoadEntities": true
}
```

**60% Oak, 20% Beech**

---

### Column Assignment

Place single blocks or columns:

```json
{
  "Type": "Column",
  "ColumnBlocks": [
    {
      "Y": 0,
      "Material": {
        "Solid": "Plant_Bush_Green"
      }
    }
  ]
}
```

**Places bush at ground level**

---

### Union Assignment

Combine multiple placements:

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

**Places prefab AND clusters**

---

### Cluster Assignment

Group placements:

```json
{
  "Type": "Cluster",
  "Range": 10,
  "Seed": "A",
  "DistanceCurve": {
    "Type": "Manual",
    "Points": [
      {
        "In": 0,
        "Out": 0.8
      },
      {
        "In": 3,
        "Out": 0
      }
    ]
  },
  "WeightedProps": [...]
}
```

**Creates clusters** with density falloff over 10 blocks

---

## Pattern Matching

### Floor Pattern

Place on specific surface blocks:

```json
{
  "Pattern": {
    "Type": "Floor",
    "Origin": {
      "Type": "BlockSet",
      "BlockSet": {
        "Inclusive": true,
        "Materials": [
          {
            "Solid": "Empty"
          }
        ]
      }
    },
    "Floor": {
      "Type": "BlockSet",
      "BlockSet": {
        "Inclusive": true,
        "Materials": [
          {
            "Solid": "Soil_Grass"
          },
          {
            "Solid": "Soil_Dirt"
          }
        ]
      }
    }
  }
}
```

**Requires:**
- **Origin** - Air above
- **Floor** - Grass or dirt below

**Use:** Trees, rocks, flowers on ground

---

### Scanner

Find placement locations:

```json
{
  "Scanner": {
    "Type": "ColumnLinear",
    "Skip": false,
    "MaxY": 60,
    "MinY": 0,
    "RelativeToPosition": false,
    "BaseHeightName": "Water",
    "TopDownOrder": true,
    "ResultCap": 1
  }
}
```

**Properties:**
- **MaxY/MinY** - Search range
- **BaseHeightName** - Reference height (Water level)
- **TopDownOrder** - Scan top-down
- **ResultCap** - Max results (1 = first found)

---

## Weighted Distribution

### Weighted Assignments

```json
{
  "Type": "Weighted",
  "SkipChance": 0.2,
  "WeightedAssignments": [
    {
      "Weight": 70,
      "Assignments": {
        "Type": "Constant",
        "Prop": {
          "Type": "Prefab",
          "WeightedPrefabPaths": [
            {
              "Path": "Trees/Oak/Stage_4",
              "Weight": 60
            },
            {
              "Path": "Trees/Oak/Stage_3",
              "Weight": 40
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
          "Type": "Column",
          "ColumnBlocks": [
            {
              "Y": 0,
              "Material": {
                "Solid": "Plant_Bush_Green"
              }
            }
          ]
        }
      }
    }
  ]
}
```

**Probabilities:**
- 20% chance: Nothing (SkipChance)
- 56% chance: Oak tree (70% × 80%)
  - 60% Stage 4
  - 40% Stage 3
- 24% chance: Bush (30% × 80%)

---

## Delimiters (Noise-Based Placement)

```json
{
  "Delimiters": [
    {
      "Min": -0.5,
      "Max": -0.2,
      "Assignments": {
        "Type": "Constant",
        "Prop": {
          "Type": "Prefab",
          "WeightedPrefabPaths": [
            {
              "Path": "Trees/Oak/Stage_0",
              "Weight": 60
            }
          ]
        }
      }
    },
    {
      "Min": -0.2,
      "Max": 1.0,
      "Assignments": {
        "Type": "Weighted",
        "WeightedAssignments": [
          {
            "Weight": 40,
            "Assignments": {
              "Type": "Constant",
              "Prop": {
                "Type": "Prefab",
                "WeightedPrefabPaths": [
                  {
                    "Path": "Trees/Oak/Stage_4",
                    "Weight": 60
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

**What this does:**
- Noise value -0.5 to -0.2: Small trees (Stage 0)
- Noise value -0.2 to 1.0: Large trees (Stage 4)

**Result:** Natural-looking forest density variation

---

## Directionality

### Random Rotation

```json
{
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
            {
              "Solid": "Empty"
            }
          ]
        }
      },
      "Floor": {
        "Type": "BlockSet",
        "BlockSet": {
          "Inclusive": true,
          "Materials": [
            {
              "Solid": "Soil_Grass"
            }
          ]
        }
      }
    }
  }
}
```

**Effect:** Trees face random directions (natural forest)

---

### Static Rotation

```json
{
  "Directionality": {
    "Type": "Static",
    "Rotation": 45
  }
}
```

**Effect:** All structures face same direction (aligned buildings)

---

## Complete Integration Example

### Custom Desert Oasis

**Goal:** Create desert with rare oases (palm trees + water)

---

#### 1. Density Map

```json
{
  "Type": "Cache",
  "ExportAs": "MyDesert_Terrain",
  "Inputs": [{
    "Type": "SimplexNoise2D",
    "Scale": 400,
    "Seed": "Desert",
    "Octaves": 2
  }]
}
```

---

#### 2. Oasis Detection Assignment

```json
{
  "Type": "FieldFunction",
  "ExportAs": "MyDesert_Oasis",
  "FieldFunction": {
    "Type": "SimplexNoise2D",
    "Scale": 200,
    "Seed": "Oasis"
  },
  "Delimiters": [
    {
      "Min": 0.8,
      "Max": 1.0,
      "Assignments": {
        "Type": "Weighted",
        "WeightedAssignments": [
          {
            "Weight": 100,
            "Assignments": {
              "Type": "Constant",
              "Prop": {
                "Type": "Union",
                "Props": [
                  {
                    "Type": "Prefab",
                    "WeightedPrefabPaths": [
                      {
                        "Path": "Trees/Palm/Stage_3",
                        "Weight": 100
                      }
                    ]
                  },
                  {
                    "Type": "Column",
                    "ColumnBlocks": [
                      {
                        "Y": -1,
                        "Material": { "Solid": "Water" }
                      },
                      {
                        "Y": -2,
                        "Material": { "Solid": "Water" }
                      }
                    ]
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

**Result:**
- Only when noise > 0.8 (rare, ~10% of map)
- Places palm tree
- AND water pool beneath it

---

#### 3. Biome Configuration

```json
{
  "ExportAs": "MyDesert_Main",
  "SurfaceBlocks": [
    {
      "Y": 0,
      "Material": { "Solid": "Sand_Desert" }
    },
    {
      "Y": -1,
      "Material": { "Solid": "Sand_Desert" }
    },
    {
      "Y": -2,
      "Material": { "Solid": "Sandstone" }
    }
  ],
  "Assignments": [
    "MyDesert_Oasis",
    "MyDesert_Cacti",
    "MyDesert_Ruins"
  ],
  "Environment": "Zone2"
}
```

---

#### 4. World Structure

```json
{
  "$Title": "My Desert World",
  "GroundLevel": 64,
  "WaterLevel": 60,
  "BaseHeights": [
    {
      "Name": "Base",
      "Type": "SimplexNoise2D",
      "Seed": "Base",
      "Scale": 500,
      "Octaves": 3,
      "HeightScaling": 20,
      "HeightOffset": 64
    },
    {
      "Name": "Water",
      "Type": "Constant",
      "Value": 60
    }
  ],
  "Stacks": [
    {
      "Biome": "MyDesert_Main",
      "Density": "MyDesert_Terrain",
      "HeightRange": {
        "Min": 0,
        "Max": 255
      }
    }
  ]
}
```

---

## Advanced Techniques

### Multi-Biome Blending

```json
{
  "Stacks": [
    {
      "Biome": "Plains_Main",
      "Density": "Plains_Terrain",
      "HeightRange": {
        "Min": 0,
        "Max": 80
      }
    },
    {
      "Biome": "Mountains_Main",
      "Density": "Mountains_Terrain",
      "HeightRange": {
        "Min": 80,
        "Max": 255
      }
    }
  ]
}
```

**Result:** Plains at low altitude, mountains at high altitude

---

### Clustered Placement

```json
{
  "Type": "Cluster",
  "Range": 15,
  "Seed": "Rocks",
  "DistanceCurve": {
    "Type": "Manual",
    "Points": [
      {
        "In": 0,
        "Out": 1.0
      },
      {
        "In": 5,
        "Out": 0.5
      },
      {
        "In": 15,
        "Out": 0.0
      }
    ]
  },
  "WeightedProps": [
    {
      "Weight": 1,
      "ColumnProp": {
        "Type": "Prefab",
        "WeightedPrefabPaths": [
          {
            "Path": "Rocks/Boulder_Large",
            "Weight": 100
          }
        ]
      }
    }
  ]
}
```

**Effect:**
- Center: 100% density
- 5 blocks away: 50% density
- 15 blocks away: 0% density

**Result:** Boulders spawn in tight clusters with gradual falloff

---

### Conditional Placement

```json
{
  "Pattern": {
    "Type": "Floor",
    "Origin": {
      "Type": "BlockSet",
      "BlockSet": {
        "Inclusive": true,
        "Materials": [
          { "Solid": "Empty" }
        ]
      }
    },
    "Floor": {
      "Type": "BlockSet",
      "BlockSet": {
        "Inclusive": true,
        "Materials": [
          { "Solid": "Sand_Desert" }
        ]
      }
    }
  }
}
```

**Only places on desert sand** (not grass, not stone)

---

## Best Practices

### ✅ Do

1. **Use appropriate scales** - Small Scale (30-80) = dense, Large Scale (200-500) = sparse
2. **Cache density maps** - Improves performance
3. **Set CullDistance** - Don't generate too far away
4. **Use BlockMasks** - Prevent overwriting important blocks
5. **Test in-game** - Generation looks different than code
6. **Balance weights** - Test distribution percentages
7. **Use SkipChance** - Prevent too-dense placement
8. **Scan from water level** - Accounts for varying terrain
9. **Use Union** for complex features - Trees + undergrowth
10. **Document with $Title** - Makes debugging easier

---

### ❌ Don't

1. **Don't use Scale < 20** - Too dense, performance issues
2. **Don't forget SkipChance** - Forests become too thick
3. **Don't skip BlockMasks** - Trees overwrite each other
4. **Don't use too many Octaves** - Expensive (max 3-4)
5. **Don't forget ResultCap** - Scans entire height otherwise
6. **Don't hardcode heights** - Use BaseHeightName
7. **Don't overlap assignments** - Careful with noise ranges

---

## Performance Optimization

### 1. Cache Expensive Operations

```json
{
  "Type": "Cache",
  "Capacity": 3,
  "Inputs": [...]
}
```

**Caches** results to avoid recalculation

---

### 2. Limit Scanner Range

```json
{
  "Scanner": {
    "MaxY": 60,
    "MinY": 0,
    "ResultCap": 1
  }
}
```

**Don't scan 0-255** if features only spawn at ground level

---

### 3. Use Appropriate Noise Scale

| Feature | Scale | Why |
|---------|-------|-----|
| Flowers | 20-40 | Small patches |
| Bushes | 40-80 | Medium groups |
| Trees | 80-200 | Distributed |
| Biomes | 300-800 | Large regions |

---

### 4. Optimize Weights

```json
{
  "SkipChance": 0.5
}
```

**50% of potential placements skipped** - Reduces density

---

## Troubleshooting

### Trees Not Spawning

**Check:**
- Delimiter range (-0.2 to 1.0 covers enough noise values?)
- Pattern matches (air above, correct blocks below?)
- Scanner range (0-60 reaches ground level?)
- BlockMask allows placement?
- Weight > 0?

### Trees Overlap

**Fix:**
- Increase noise Scale (more spread out)
- Add SkipChance (0.3-0.5)
- Use Cluster with distance curve

### Wrong Surface Blocks

**Check:**
- Biome SurfaceBlocks configuration
- HeightRange in Stacks
- Density map values

### Features Spawn Underwater

**Fix:**
- Use Scanner with BaseHeightName: "Water"
- Set MinY above water level
- Check Pattern Floor blocks

---

## Related Guides

- **[HytaleGenerator Assignments](163_HytaleGenerator_Assignments.md)** - Assignment system reference
- **[HytaleGenerator Density](164_HytaleGenerator_Density.md)** - Density maps reference
- **[HytaleGenerator BlockMasks](166_HytaleGenerator_BlockMasks.md)** - Block mask reference
- **[Biomes & Environments](24_Biomes_and_Environments.md)** - Biome configuration
- **[World Configuration](160_World_Configuration.md)** - World structure setup

---

**Previous:** [Advanced AI & NPC Behavior](180_Advanced_AI_NPC_Behavior.md) | **Next:** [Advanced Combat Mechanics](182_Advanced_Combat_Mechanics.md)
