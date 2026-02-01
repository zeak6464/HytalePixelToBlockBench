# Ore Generation

Configure ore vein placement and distribution in the world.

## Overview

Ore generation controls how mineral deposits spawn underground. Each zone has its own ore configurations with different densities, depths, and vein sizes.

## Location
- Ore definitions: `Server/World/Default/Ores/{Zone}/`
- Placement configs: `Server/World/Default/Ores/{Zone}/{Ore}Placement.json`

## Example from Game Files

### Copper Ore Definition

From `Server/World/Default/Ores/Zone1/Copper.json`:

```json
{
  "Name": "Entry",
  "Children": [
    {
      "Weights": [99, 1, 0.1],
      "Node": [
        {
          "Type": "EMPTY_LINE",
          "Children": [
            {
              "Weights": [100],
              "Node": [
                {
                  "Name": "Vein",
                  "Children": [],
                  "Length": [2.5, 3.5],
                  "Radius": [1.5, 1.5],
                  "Filling": "Rock_Stone"
                }
              ]
            },
            {
              "Weights": [100],
              "Node": [
                {
                  "Name": "Offset",
                  "Type": "EMPTY_LINE",
                  "Length": [0, 0],
                  "Children": [
                    {
                      "Weights": [100],
                      "Node": [
                        {
                          "Name": "Vein",
                          "Children": [],
                          "Length": [1.5, 2.5],
                          "Radius": [0.5, 0.5],
                          "Filling": "Ore_Copper_Stone"
                        }
                      ]
                    }
                  ]
                }
              ]
            }
          ]
        }
      ],
      "PitchSet": [0, 180],
      "YawSet": [0, 360]
    }
  ],
  "Type": "EMPTY_LINE",
  "Length": [55, 118]
}
```

### Copper Placement

From `Server/World/Default/Ores/Zone1/CopperPlacement.json`:

```json
{
  "Name": "Copper",
  "BlockMask": {
    "Default": [
      "Rock_Stone",
      "Rock_Basalt",
      "Rock_Marble",
      { "Replace": false, "Fluid": "Water_Source" }
    ]
  },
  "Entry": {
    "File": "Ores.Zone1.Copper"
  },
  "EntryPoints": {
    "Density": [0, 1],
    "Seed": "Caves-Ore-Copper",
    "Scale": 0.4,
    "Jitter": 0.5
  },
  "FixedEntryHeight": 0,
  "HeightRadiusFactor": {
    "Positions": [0, 255],
    "Values": [1, 1]
  },
  "Pitch": 0
}
```

---

## Ore Vein Structure

### Vein Types by Weight

```json
{
  "Weights": [99, 1, 0.1],
  "Node": [
    { "Name": "Vein" },        // 99% - Small vein
    { "Name": "VeinLarge" },   // 1% - Large vein
    { "Name": "Motherlode" }   // 0.1% - Massive deposit
  ]
}
```

### Vein Properties

| Property | Description |
|----------|-------------|
| `Length` | Vein height/length `[min, max]` |
| `Radius` | Vein width `[min, max]` |
| `Filling` | Block type to place |
| `PitchSet` | Vertical angle range |
| `YawSet` | Horizontal angle range |

### Vein Size Examples

```json
// Small Vein
{ "Length": [2.5, 3.5], "Radius": [0.5, 0.5] }

// Large Vein
{ "Length": [4, 6], "Radius": [1, 1.25] }

// Motherlode
{ "Length": [15, 15], "Radius": [0.75, 1] }
```

---

## Placement Configuration

### BlockMask

Which blocks can be replaced:

```json
{
  "BlockMask": {
    "Default": [
      "Rock_Stone",
      "Rock_Basalt",
      "Rock_Marble",
      { "Replace": false, "Fluid": "Water_Source" }
    ]
  }
}
```

- String = can replace this block
- `{ "Replace": false, "Fluid": "..." }` = cannot replace this

### EntryPoints (Density)

```json
{
  "EntryPoints": {
    "Density": [0, 1],
    "Seed": "Caves-Ore-Copper",
    "Scale": 0.4,
    "Jitter": 0.5
  }
}
```

| Property | Description |
|----------|-------------|
| `Density` | Range `[min, max]` |
| `Seed` | Unique seed for placement |
| `Scale` | Horizontal density (lower = more spread) |
| `Jitter` | Randomness in placement |

### HeightRadiusFactor

Controls spawn rate by height:

```json
{
  "HeightRadiusFactor": {
    "Positions": [0, 64, 128, 255],
    "Values": [1, 1, 0.5, 0]
  }
}
```

- Position 0-64: 100% spawn rate
- Position 128: 50% spawn rate
- Position 255: 0% spawn rate

---

## Zone-Specific Ores

### Zone 1 (Temperate)
- Copper, Iron, Silver, Gold, Treasure

### Zone 2 (Desert)
- Copper, Iron, Silver, Gold, Treasure
- Additional rare ores

### Zone 3 (Cold)
- Copper, Iron, Silver, Gold, Cobalt, Treasure

### Zone 4 (Volcanic)
- Iron, Silver, Adamantite

---

## Creating Custom Ore

### 1. Create Ore Definition

`Server/World/Default/Ores/Zone1/MyOre.json`:

```json
{
  "Name": "Entry",
  "Children": [
    {
      "Weights": [90, 10],
      "Node": [
        {
          "Type": "EMPTY_LINE",
          "Children": [
            {
              "Weights": [100],
              "Node": [
                {
                  "Name": "Vein",
                  "Length": [3, 5],
                  "Radius": [1, 1.5],
                  "Filling": "Rock_Stone"
                }
              ]
            },
            {
              "Weights": [100],
              "Node": [
                {
                  "Name": "Offset",
                  "Type": "EMPTY_LINE",
                  "Length": [0, 0],
                  "Children": [
                    {
                      "Weights": [100],
                      "Node": [
                        {
                          "Name": "Vein",
                          "Length": [2, 4],
                          "Radius": [0.5, 1],
                          "Filling": "Ore_MyCustom_Stone"
                        }
                      ]
                    }
                  ]
                }
              ]
            }
          ]
        },
        {
          "Type": "EMPTY_LINE",
          "Children": [
            {
              "Weights": [100],
              "Node": [
                {
                  "Name": "VeinLarge",
                  "Length": [8, 12],
                  "Radius": [1.5, 2],
                  "Filling": "Rock_Stone"
                }
              ]
            },
            {
              "Weights": [100],
              "Node": [
                {
                  "Name": "Offset",
                  "Type": "EMPTY_LINE",
                  "Length": [0, 0],
                  "Children": [
                    {
                      "Weights": [100],
                      "Node": [
                        {
                          "Name": "VeinLarge",
                          "Length": [6, 10],
                          "Radius": [1, 1.5],
                          "Filling": "Ore_MyCustom_Stone"
                        }
                      ]
                    }
                  ]
                }
              ]
            }
          ]
        }
      ],
      "PitchSet": [0, 180],
      "YawSet": [0, 360]
    }
  ],
  "Type": "EMPTY_LINE",
  "Length": [40, 100]
}
```

### 2. Create Placement Config

`Server/World/Default/Ores/Zone1/MyOrePlacement.json`:

```json
{
  "Name": "MyOre",
  "BlockMask": {
    "Default": [
      "Rock_Stone",
      "Rock_Basalt"
    ]
  },
  "Entry": {
    "File": "Ores.Zone1.MyOre"
  },
  "EntryPoints": {
    "Density": [0, 1],
    "Seed": "Caves-Ore-MyOre",
    "Scale": 0.3,
    "Jitter": 0.5
  },
  "FixedEntryHeight": 0,
  "HeightRadiusFactor": {
    "Positions": [0, 50, 100, 255],
    "Values": [0, 1, 1, 0.5]
  },
  "Pitch": 0
}
```

---

## Tips

1. **Unique seeds** - Each ore needs a unique seed
2. **Depth-based rarity** - Use `HeightRadiusFactor` for rare deep ores
3. **Vein weights** - Motherlodes should be very rare (0.1% or less)
4. **Scale value** - Lower = more spread out
5. **Stone surround** - Wrap ore in stone for realistic veins
6. **Test in creative** - Verify spawn rates

---

**Related:** [World Generation](38_World_Generation.md) | [Blocks](06_Blocks_and_Portals.md)
