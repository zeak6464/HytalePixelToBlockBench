# World Masks & Climate

Configure world generation masks for climate, temperature, and continent distribution.

## Overview

World Masks control large-scale world generation patterns including climate zones, temperature gradients, continent shapes, and unique zone placement. They determine where different biomes spawn.

## Location
- Main config: `Server/World/Default/Mask.json`
- Climate masks: `Server/World/Default/Mask/Climate/`
- Temperature masks: `Server/World/Default/Mask/Temperature/`
- Continent masks: `Server/World/Default/Mask/Continent/`

## Example from Game Files

### Main Mask Config

From `Server/World/Default/Mask.json`:

```json
{
  "Randomizer": {
    "Generators": [
      {
        "Seed": "RANDOMIZER",
        "NoiseType": "SIMPLEX",
        "Scale": 0.01,
        "Amplitude": 16.0
      }
    ]
  },
  "Noise": {
    "Thresholds": {
      "Land": 0.5,
      "Island": 0.75,
      "BeachSize": 0.02,
      "ShallowOceanSize": 0.08
    },
    "Continent": {
      "File": "Mask.Continent"
    },
    "Temperature": {
      "File": "Mask.Temperature"
    },
    "Intensity": {
      "File": "Mask.Intensity"
    }
  },
  "Climate": {
    "FadeMode": "CHILDREN",
    "FadeRadius": 50.0,
    "FadeDistance": 100.0,
    "Climates": [
      { "File": "Mask.Climate.Temperate" },
      { "File": "Mask.Climate.Cold" },
      { "File": "Mask.Climate.Hot" }
    ]
  },
  "UniqueZones": [
    {
      "Name": "Zone1_Spawn",
      "Color": "#ff0000",
      "Radius": 35,
      "OriginX": 0,
      "OriginY": 0,
      "Distance": 3000,
      "Rule": {
        "Continent": { "Target": 0.0, "Radius": 0.3, "Weight": 1.0 },
        "Temperature": { "Target": 0.5, "Radius": 0.2, "Weight": 1.0 },
        "Intensity": { "Target": 0.1, "Radius": 0.3, "Weight": 1.0 }
      }
    }
  ]
}
```

---

## Mask Sections

### Randomizer

Controls noise generation:

```json
{
  "Randomizer": {
    "Generators": [
      {
        "Seed": "RANDOMIZER",
        "NoiseType": "SIMPLEX",
        "Scale": 0.01,
        "Amplitude": 16.0
      }
    ]
  }
}
```

| Property | Description |
|----------|-------------|
| `Seed` | Seed string for reproducibility |
| `NoiseType` | `"SIMPLEX"` for smooth noise |
| `Scale` | Noise frequency (smaller = larger features) |
| `Amplitude` | Noise intensity |

### Noise Thresholds

Land/water distribution:

```json
{
  "Noise": {
    "Thresholds": {
      "Land": 0.5,
      "Island": 0.75,
      "BeachSize": 0.02,
      "ShallowOceanSize": 0.08
    }
  }
}
```

| Threshold | Description |
|-----------|-------------|
| `Land` | Noise value for land (0.5 = 50% land) |
| `Island` | Noise value for islands |
| `BeachSize` | Width of beach transition |
| `ShallowOceanSize` | Width of shallow ocean |

### Climate Configuration

```json
{
  "Climate": {
    "FadeMode": "CHILDREN",
    "FadeRadius": 50.0,
    "FadeDistance": 100.0,
    "Climates": [
      { "File": "Mask.Climate.Temperate" },
      { "File": "Mask.Climate.Cold" },
      { "File": "Mask.Climate.Hot" }
    ]
  }
}
```

| Property | Description |
|----------|-------------|
| `FadeMode` | How climates blend (`"CHILDREN"`) |
| `FadeRadius` | Blend start distance |
| `FadeDistance` | Full blend distance |
| `Climates` | List of climate files |

---

## Climate Masks

### Cold Climate (Zone 3)

From `Server/World/Default/Mask/Climate/Cold.json`:

```json
{
  "Name": "Zone 3",
  "Color": "#0000ff",
  "Points": [
    {
      "Temperature": 0.1,
      "Intensity": 0.5
    }
  ],
  "Children": [
    {
      "Name": "Tier 1",
      "Color": "#8ecdff",
      "Shore": "#8bafab",
      "Ocean": "#008479",
      "ShallowOcean": "#63ccc3",
      "Island": {
        "File": "Mask.Climate.Island.Tier1"
      },
      "Points": [
        {
          "Temperature": 0.310546875,
          "Intensity": 0.455078125,
          "Modifier": 0.9
        }
      ]
    },
    {
      "Name": "Tier 2",
      "Color": "#38a8ff",
      "Shore": "#9cc7c2",
      "Ocean": "#008479",
      "ShallowOcean": "#00c2b2",
      "Island": {
        "File": "Mask.Climate.Island.Tier2"
      },
      "Points": [
        {
          "Temperature": 0.185546875,
          "Intensity": 0.478515625,
          "Modifier": 0.83
        }
      ]
    }
  ]
}
```

### Climate Properties

| Property | Description |
|----------|-------------|
| `Name` | Climate/zone name |
| `Color` | Debug map color |
| `Shore` | Shore water color |
| `Ocean` | Deep ocean color |
| `ShallowOcean` | Shallow ocean color |
| `Points` | Temperature/intensity coordinates |
| `Modifier` | Generation intensity modifier |
| `Children` | Sub-tiers within climate |

---

## Unique Zones

Special predefined locations:

```json
{
  "UniqueZones": [
    {
      "Name": "Zone1_Spawn",
      "Color": "#ff0000",
      "Radius": 35,
      "OriginX": 0,
      "OriginY": 0,
      "Distance": 3000,
      "Rule": {
        "Continent": { "Target": 0.0, "Radius": 0.3, "Weight": 1.0 },
        "Temperature": { "Target": 0.5, "Radius": 0.2, "Weight": 1.0 },
        "Intensity": { "Target": 0.1, "Radius": 0.3, "Weight": 1.0 },
        "Fade": { "Target": 1.0, "Radius": 0.5, "Weight": 0.5 }
      }
    },
    {
      "Name": "Zone1_Temple",
      "Parent": "Zone1_Spawn",
      "Radius": 20,
      "Distance": 400,
      "MinDistance": 250
    }
  ]
}
```

### Unique Zone Properties

| Property | Description |
|----------|-------------|
| `Name` | Zone identifier |
| `Parent` | Parent zone (for relative placement) |
| `Radius` | Zone size |
| `OriginX/Y` | Starting search point |
| `Distance` | Max search distance |
| `MinDistance` | Min distance from parent |
| `Rule` | Placement criteria |

### Rule Targets

```json
{
  "Rule": {
    "Continent": { "Target": 0.0, "Radius": 0.3, "Weight": 1.0 },
    "Temperature": { "Target": 0.5, "Radius": 0.2, "Weight": 1.0 },
    "Intensity": { "Target": 0.1, "Radius": 0.3, "Weight": 1.0 }
  }
}
```

- `Target`: Desired value (0-1)
- `Radius`: Acceptable range around target
- `Weight`: Importance of this criterion

---

## Temperature/Intensity System

The world uses a 2D coordinate system:
- **Temperature**: 0 = cold, 1 = hot
- **Intensity**: 0 = mild, 1 = extreme

```
Temperature →
     0.0    0.5    1.0
     ┌──────┬──────┐
0.0  │ Cold │ Temp │  Intensity
     │ Mild │erate │     ↓
0.5  ├──────┼──────┤
     │ Cold │ Hot  │
     │Extreme│Extreme│
1.0  └──────┴──────┘
```

---

## Creating Custom Climate

`Server/World/Default/Mask/Climate/Tropical.json`:

```json
{
  "Name": "Tropical",
  "Color": "#00ff00",
  "Points": [
    {
      "Temperature": 0.8,
      "Intensity": 0.3
    }
  ],
  "Children": [
    {
      "Name": "Tier 1",
      "Color": "#80ff80",
      "Shore": "#ffdd88",
      "Ocean": "#0088aa",
      "ShallowOcean": "#00cccc",
      "Points": [
        {
          "Temperature": 0.75,
          "Intensity": 0.25,
          "Modifier": 1.0
        }
      ]
    }
  ]
}
```

Add to Mask.json:
```json
{
  "Climate": {
    "Climates": [
      { "File": "Mask.Climate.Temperate" },
      { "File": "Mask.Climate.Cold" },
      { "File": "Mask.Climate.Hot" },
      { "File": "Mask.Climate.Tropical" }
    ]
  }
}
```

---

## Tips

1. **Temperature/Intensity** - Plan your climate in 2D space
2. **Unique zones** - Use for spawn points and dungeons
3. **Tier children** - Create variation within climates
4. **Colors** - Use for debugging on maps
5. **Fade settings** - Control biome transition smoothness

---

**Related:** [World Generation](38_World_Generation.md) | [Procedural World Generation](181_Procedural_World_Generation.md)
