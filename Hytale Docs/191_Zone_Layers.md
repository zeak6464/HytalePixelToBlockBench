# Zone Layers System

Configure terrain layers, placement masks, noise, and prefab patterns for world generation.

## Overview

The Zone Layers system controls detailed terrain generation within each biome zone. It includes vertical block layers, placement masks, noise configurations, and prefab pattern placement.

## Location
`Server/World/Default/Zones/`

## Directory Structure

```
Server/World/Default/Zones/
├── Layers/           # Vertical terrain layers
├── Masks/            # Placement rules
├── Noise/            # Noise configurations
├── PrefabPatterns_Shared/  # Shared prefab patterns
├── Oceans/           # Ocean configurations
├── Zone1_Tier1/      # Zone-specific configs
│   ├── Layers/
│   ├── Masks/
│   ├── Noise/
│   └── PrefabPatterns/
└── Zones.json        # Zone definitions
```

---

## Layers

Layers define vertical block composition with noise-controlled variation.

### Layer Structure

```json
{
  "Blocks": [
    {
      "Type": "Rock_Stone",
      "Repeat": [30, 36],
      "RepeatNoise": {
        "Seed": "1",
        "NoiseType": "OLD_SIMPLEX",
        "FractalMode": "OLDSCHOOL",
        "Octaves": 2,
        "Persistence": 0.7,
        "Scale": 0.01,
        "Formula": "INVERTED"
      }
    },
    "Rock_Basalt"
  ],
  "Max": 114,
  "Min": 0,
  "MaxNoise": {
    "Seed": "Global-Bedrock",
    "NoiseType": "OLD_SIMPLEX",
    "FractalMode": "OLDSCHOOL",
    "Octaves": 3,
    "Persistence": 0.8,
    "Scale": 1
  }
}
```

### Layer Properties

| Property | Type | Description |
|----------|------|-------------|
| `Blocks` | Array | Block types to place |
| `Type` | String | Block type identifier |
| `Repeat` | Array | `[min, max]` vertical repeat |
| `RepeatNoise` | Object | Noise controlling repeat variation |
| `Max` | Number/Array | Maximum Y height |
| `Min` | Number | Minimum Y height |
| `MaxNoise` | Object | Noise for variable max height |

### Block Definitions

Simple string:
```json
{ "Blocks": ["Rock_Stone"] }
```

With repeat:
```json
{
  "Blocks": [
    {
      "Type": "Rock_Stone",
      "Repeat": [30, 36]
    }
  ]
}
```

---

## Masks

Masks control where blocks and features can be placed.

### Mask Structure

```json
{
  "Default": ["*"],
  "Specific": [
    {
      "Block": ["Plant_Leaves_Amber", "Wood_Amber_Roots"],
      "Rule": ["Empty", "Plant_Grass_Sharp"]
    },
    {
      "Block": ["Deco_SpiderWeb"],
      "Rule": ["Empty"]
    }
  ]
}
```

### Mask Properties

| Property | Description |
|----------|-------------|
| `Default` | Default placement rules (`["*"]` = all) |
| `Specific` | Array of specific block rules |
| `Block` | Block types this rule applies to |
| `Rule` | Allowed placement conditions |

### Rule Values

- `"*"` - All blocks allowed
- `"Empty"` - Only air
- `"!BlockType"` - Exclude this block type
- `"BlockType"` - Specific block required

---

## Noise

Noise configurations control procedural terrain variation.

### Simple Noise

```json
{
  "Seed": "Drylands-Canyon-Noise",
  "NoiseType": "SIMPLEX",
  "Octaves": 1,
  "Persistence": 0.8,
  "Scale": 0.0012
}
```

### Composite Noise (SUM)

```json
{
  "Type": "SUM",
  "Noise": [
    {
      "Seed": "Shore",
      "NoiseType": "OLD_SIMPLEX",
      "FractalMode": "OLDSCHOOL",
      "Octaves": 2,
      "Persistence": 0.6,
      "Scale": 0.09
    }
  ],
  "Factors": [0.5, 0.5],
  "Threshold": [0, 0.32],
  "Normalize": 0.2
}
```

### Cell Noise

```json
{
  "Seed": "Pattern_Monuments",
  "NoiseType": "CELL",
  "CellMode": "DISTANCE",
  "Density": [0, 0.35],
  "Scale": 0.004,
  "Jitter": 0.75
}
```

### Noise Properties

| Property | Description |
|----------|-------------|
| `NoiseType` | `"SIMPLEX"`, `"OLD_SIMPLEX"`, `"CELL"` |
| `Seed` | Random seed string |
| `Octaves` | Number of noise octaves |
| `Persistence` | Octave persistence |
| `Scale` | Noise frequency |
| `Formula` | `"NORMAL"`, `"INVERTED"` |
| `FractalMode` | `"OLDSCHOOL"` for OLD_SIMPLEX |
| `Type` | `"SUM"`, `"MULTIPLY"` for composite |
| `Threshold` | `[min, max]` value range |
| `CellMode` | `"DISTANCE"` for cell noise |

### Noise Organization

```
Noise/
├── Zone1/
│   ├── Covers/      # Grass, flowers placement
│   ├── Heightmaps/  # Terrain height
│   ├── Layers/      # Layer generation
│   └── NoiseMasks/  # Mask generation
└── Shared/          # Shared configurations
```

---

## Prefab Patterns

Control grid-based placement of structures.

### Pattern Structure

```json
{
  "GridGenerator": {
    "Seed": "Pattern_Monuments",
    "Density": [0.40, 0.625],
    "Scale": 0.004,
    "Jitter": 0.75
  },
  "HeightThreshold": {
    "Positions": [113, 114],
    "Values": [0, 1]
  },
  "Mask": {
    "File": "Zones.Masks.Zone1.Monuments_Encounter"
  },
  "NoiseMask": {
    "Type": "MIN",
    "Noise": [
      { "File": "Zones.Noise.Shared.Grids.Monuments_Encounter" }
    ],
    "Threshold": [0, 0.25]
  },
  "MaxSize": 100,
  "ExclusionRadius": 100,
  "Parent": ["Soil_Pebbles_Frozen", "Soil_Dirt"]
}
```

### Pattern Properties

| Property | Description |
|----------|-------------|
| `GridGenerator` | Grid placement config |
| `Density` | Placement density |
| `Scale` | Grid cell scale |
| `Jitter` | Grid randomization |
| `HeightThreshold` | Height-based placement |
| `Mask` | Reference to mask file |
| `NoiseMask` | Optional noise masking |
| `MaxSize` | Maximum prefab size |
| `ExclusionRadius` | Min distance between prefabs |
| `Parent` | Allowed parent block types |

---

## Creating Custom Zone Layer

### 1. Create Layer

`Server/World/Default/Zones/MyZone/Layers/Terrain.json`:

```json
{
  "Blocks": [
    {
      "Type": "Rock_Volcanic",
      "Repeat": [20, 30],
      "RepeatNoise": {
        "Seed": "MyZone-Volcanic",
        "NoiseType": "SIMPLEX",
        "Octaves": 2,
        "Persistence": 0.7,
        "Scale": 0.02
      }
    },
    {
      "Type": "Soil_Ash",
      "Repeat": [3, 5]
    }
  ],
  "Max": 128,
  "Min": 64
}
```

### 2. Create Mask

`Server/World/Default/Zones/MyZone/Masks/Plants.json`:

```json
{
  "Default": ["*"],
  "Specific": [
    {
      "Block": ["Plant_Dead_Tree"],
      "Rule": ["Soil_Ash", "Rock_Volcanic"]
    }
  ]
}
```

### 3. Create Prefab Pattern

`Server/World/Default/Zones/MyZone/PrefabPatterns/Boulders.json`:

```json
{
  "GridGenerator": {
    "Seed": "MyZone-Boulders",
    "Density": 0.3,
    "Scale": 0.01,
    "Jitter": 0.5
  },
  "HeightThreshold": {
    "Positions": [64, 128],
    "Values": [0.5, 1]
  },
  "MaxSize": 50,
  "ExclusionRadius": 30,
  "Parent": ["Soil_Ash"]
}
```

---

## Tips

1. **Unique seeds** - Each noise needs a unique seed
2. **Scale values** - Lower = larger features
3. **Persistence** - Higher = more detail
4. **Exclusion radius** - Prevents prefab overlap
5. **Height thresholds** - Control altitude-based placement
6. **Test incrementally** - Small changes, test often

---

**Related:** [World Generation](38_World_Generation.md) | [World Masks](190_World_Masks.md) | [Prefabs](186_Prefabs.md)
