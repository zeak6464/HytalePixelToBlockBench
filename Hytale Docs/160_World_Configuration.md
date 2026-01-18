# World Configuration

Learn how to configure world structures, zones, masks, and world generation in `Server/World/`.

## Overview

World configuration files define how the world is structured, including zones, biome masks, terrain generation, and world-specific settings. Each world type (Default, Flat, Void, etc.) has its own configuration.

## Location

`Server/World/{WorldName}/`

## World Structure

Each world configuration contains:

- **`World.json`** - Main world configuration
- **`Mask.json`** - Biome and terrain masks
- **`Zones.json`** - Zone mapping configuration
- **`Zones/`** - Individual zone definitions
- **`Mask/`** - Mask definitions (climate, temperature, continent)

## World.json

The main world configuration file:

`Server/World/Default/World.json`:

```json
{
  "Masks": ["Mask.json"],
  "PrefabStore": "ASSETS",
  "Height": 1,
  "Width": 1,
  "OffsetX": 0,
  "OffsetY": 0,
  "Randomizer": {
    "Generators": []
  }
}
```

**Properties:**
- **`Masks`** - Array of mask file references
- **`PrefabStore`** - Prefab storage type (`"ASSETS"`)
- **`Height`** - World height in chunks
- **`Width`** - World width in chunks
- **`OffsetX`** - X offset for world generation
- **`OffsetY`** - Y offset for world generation
- **`Randomizer`** - Random seed generators

## Mask.json

Defines biome masks, climate zones, and unique zones:

`Server/World/Default/Mask.json`:

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
      {
        "File": "Mask.Climate.Temperate"
      },
      {
        "File": "Mask.Climate.Cold"
      },
      {
        "File": "Mask.Climate.Hot"
      }
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
        "Continent": {
          "Target": 0.0,
          "Radius": 0.3,
          "Weight": 1.0
        },
        "Temperature": {
          "Target": 0.5,
          "Radius": 0.2,
          "Weight": 1.0
        },
        "Intensity": {
          "Target": 0.1,
          "Radius": 0.3,
          "Weight": 1.0
        },
        "Fade": {
          "Target": 1.0,
          "Radius": 0.5,
          "Weight": 0.5
        }
      }
    }
  ]
}
```

**Properties:**

### Noise

Terrain generation noise settings:

- **`Thresholds`** - Height thresholds for terrain types:
  - `Land` - Land/ocean boundary (0.5)
  - `Island` - Island generation threshold (0.75)
  - `BeachSize` - Beach width (0.02)
  - `ShallowOceanSize` - Shallow ocean depth (0.08)

- **`Continent`** - Continent mask file
- **`Temperature`** - Temperature gradient mask file
- **`Intensity`** - Intensity/difficulty mask file

### Climate

Climate zone configuration:

- **`FadeMode`** - How climates fade (`"CHILDREN"`)
- **`FadeRadius`** - Climate fade radius (50.0)
- **`FadeDistance`** - Climate fade distance (100.0)
- **`Climates`** - Array of climate mask files

### UniqueZones

Special zones with custom rules:

- **`Name`** - Zone identifier
- **`Color`** - Zone color for visualization (#ff0000)
- **`Radius`** - Zone radius in blocks
- **`OriginX`** - Zone origin X coordinate
- **`OriginY`** - Zone origin Y coordinate
- **`Distance`** - Zone distance from origin
- **`MinDistance`** - Minimum distance from parent zone (optional)
- **`Parent`** - Parent zone name (optional)
- **`Rule`** - Zone placement rules:
  - `Continent` - Continent mask rules
  - `Temperature` - Temperature rules
  - `Intensity` - Intensity/difficulty rules
  - `Fade` - Fade rules

**Rule Properties:**
- **`Target`** - Target value (0.0 to 1.0)
- **`Radius`** - Valid range around target
- **`Weight`** - Rule weight (importance)

## Zones.json

Maps colors to zone names for world generation:

`Server/World/Default/Zones.json`:

```json
{
  "GridGenerator": {
    "Scale": 0.0008,
    "Jitter": 0.9,
    "Randomizer": {
      "Generators": [
        {
          "NoiseType": "OLD_SIMPLEX",
          "Octaves": 3,
          "Persistence": 0.5,
          "Scale": 0.008,
          "Amplitude": 100
        }
      ]
    }
  },
  "MaskMapping": {
    "#ff0000": ["Zone1_Spawn"],
    "#ffff00": ["Zone1_Temple"],
    "#78ff27": ["Zone1_Tier1"],
    "#5bbf1d": ["Zone1_Tier2"],
    "#3d8013": ["Zone1_Tier3"],
    "#ffb835": ["Zone2_Tier1"],
    "#cf8805": ["Zone2_Tier2"],
    "#8e5d01": ["Zone2_Tier3"]
  }
}
```

**Properties:**

### GridGenerator

Zone grid generation settings:

- **`Scale`** - Grid cell scale (0.0008)
- **`Jitter`** - Grid jitter amount (0.9)
- **`Randomizer`** - Noise generators for zone placement

### MaskMapping

Maps hex colors to zone names:

- **Key:** Hex color code (e.g., `"#ff0000"`)
- **Value:** Array of zone names for that color

Multiple zones can share the same color for blending.

## Zone Configuration

Individual zone definitions in `Server/World/{WorldName}/Zones/{ZoneName}/`:

### Zone.json

Zone discovery and biome generation:

`Server/World/Default/Zones/Zone2_Tier1/Zone.json`:

```json
{
  "Discovery": {
    "ZoneName": "Howling_Sands",
    "Display": true,
    "SoundEventId": "SFX_Discovery_Z2_Medium",
    "Major": true,
    "Duration": 4.0,
    "FadeInDuration": 1.5,
    "FadeOutDuration": 1.5
  },
  "BiomeGenerator": {
    "GridGenerator": {
      "Seed": "Drylands-Biomes-Distrobution",
      "Scale": 0.0022,
      "Randomizer": {
        "Generators": [
          {
            "Seed": "Drylands-Base-Randomizer-1",
            "NoiseType": "OLD_SIMPLEX",
            "Octaves": 3,
            "Persistence": 0.5,
            "Scale": 0.009,
            "Amplitude": 40
          }
        ]
      }
    }
  }
}
```

**Properties:**

### Discovery

Zone discovery notification:

- **`ZoneName`** - Display name key
- **`Display`** - Show discovery notification (`true`/`false`)
- **`SoundEventId`** - Sound to play on discovery
- **`Major`** - Major zone flag (`true`/`false`)
- **`Duration`** - Notification duration (seconds)
- **`FadeInDuration`** - Fade-in duration (seconds)
- **`FadeOutDuration`** - Fade-out duration (seconds)

### BiomeGenerator

Biome placement generation:

- **`GridGenerator`** - Biome grid settings:
  - **`Seed`** - Random seed string
  - **`Scale`** - Grid cell scale
  - **`Randomizer`** - Noise generators for biome placement

## Tile Configuration

Tile files define what blocks, prefabs, and features appear in specific biome tiles:

`Server/World/Default/Zones/Zone2_Tier1/Tile.Scrub_Bushland.json`:

### Covers

Surface decorations (plants, flowers, rubble):

```json
{
  "Weight": 14,
  "SizeModifier": 1,
  "MapColor": "#c89b6b",
  "Covers": [
    {
      "Type": ["Plant_Flower_Flax_Orange"],
      "Weight": [1],
      "Density": 0.2,
      "NoiseMask": {
        "Seed": "Plant-Bush-Arid-Red",
        "NoiseType": "OLD_SIMPLEX",
        "FractalMode": "OLDSCHOOL",
        "Octaves": 3,
        "Persistence": 0.7,
        "Scale": 0.045,
        "Threshold": [0.7, 1]
      },
      "Parent": ["Soil_Grass_Dry", "Soil_Sand_Red"]
    }
  ]
}
```

**Cover Properties:**
- **`Type`** - Array of block/item IDs to place
- **`Weight`** - Probability weights for each type
- **`Density`** - Placement density (0.0 to 1.0)
- **`NoiseMask`** - Noise mask for placement probability
- **`Parent`** - Required parent blocks (what it can be placed on)
- **`HeightThreshold`** - Height-based placement rules

### Prefabs

Structure placement (trees, monuments, caves):

```json
{
  "Prefabs": {
    "Entries": [
      {
        "Prefab": ["Trees.Gum.*"],
        "Pattern": {
          "File": "Zones.Zone2_Tier1.PrefabPatterns.Scrub_Trees_Gum"
        }
      },
      {
        "Weight": [100],
        "Prefab": ["Plants.Bush.Arid_Red.*"],
        "Pattern": {
          "File": "Zones.Zone2_Tier1.PrefabPatterns.Scrub_Bushes"
        }
      }
    ]
  }
}
```

**Prefab Entry Properties:**
- **`Prefab`** - Array of prefab path patterns (wildcards supported)
- **`Weight`** - Probability weights
- **`Pattern`** - Prefab pattern file reference

### Layers

Block layer configuration (bedrock, static, dynamic):

```json
{
  "Layers": {
    "Default": "Rock_Sandstone",
    "Dynamic": [
      {
        "Entries": [
          {
            "Blocks": ["Soil_Gravel_Sand"],
            "NoiseMask": {
              "Type": "MIN",
              "Noise": [
                {
                  "File": "Zones.Noise.Zone2.Layers.Soil.Layer_Cave_Deep"
                }
              ],
              "Threshold": [0.5, 1]
            }
          }
        ]
      }
    ],
    "Static": [
      {
        "File": "Zones.Layers.Zone2.Static"
      }
    ]
  }
}
```

**Layer Properties:**
- **`Default`** - Default block type for this tile
- **`Dynamic`** - Dynamic block layers (caves, rivers)
- **`Static`** - Static block layers (bedrock, base terrain)

### TerrainHeightThreshold

Height-based terrain variation:

```json
{
  "TerrainHeightThreshold": {
    "Noise": {
      "Seed": "Sav",
      "NoiseType": "OLD_SIMPLEX",
      "FractalMode": "OLDSCHOOL",
      "Octaves": 3,
      "Persistence": 0.2,
      "Scale": 0.01
    },
    "Keys": [0.3, 0.7],
    "Thresholds": [
      {
        "Positions": [115, 117, 119, 123],
        "Values": [1, 0.54, 0.44, 0.37]
      }
    ]
  }
}
```

**Properties:**
- **`Noise`** - Noise generator for height variation
- **`Keys`** - Height key points
- **`Thresholds`** - Position/value pairs for terrain height

### HeightmapNoise

Terrain height generation:

```json
{
  "HeightmapNoise": {
    "Type": "SUM",
    "Noise": [
      {
        "Seed": "Drylands-Savannah-Heightmap",
        "NoiseType": "OLD_SIMPLEX",
        "FractalMode": "OLDSCHOOL",
        "Formula": "INVERTED_RIDGED_SQRT",
        "Octaves": 6,
        "Persistence": 0.46,
        "Scale": 0.0043
      }
    ],
    "Factors": [0.4, 0.6]
  }
}
```

**Properties:**
- **`Type`** - Noise combination type (`"SUM"`, `"MULTIPLY"`, `"MIN"`, `"MAX"`)
- **`Noise`** - Array of noise generators
- **`Factors`** - Weight factors for each noise generator

### Tint

Block tinting/color variation:

```json
{
  "Tint": {
    "Default": {
      "Colors": ["da9137", "d49837"],
      "Weights": [50, 50],
      "Noise": {
        "Seed": "Plant_Grass_Sharp",
        "NoiseType": "OLD_SIMPLEX",
        "FractalMode": "OLDSCHOOL",
        "Octaves": 5,
        "Persistence": 0.6,
        "Scale": 0.015
      }
    },
    "Entries": []
  }
}
```

**Properties:**
- **`Default`** - Default tint configuration
- **`Colors`** - Hex color codes (without #)
- **`Weights`** - Color probability weights
- **`Noise`** - Noise generator for color variation

### Environment

Environment configuration (weather, ambience):

```json
{
  "Environment": {
    "Default": {
      "Names": ["Env_Zone2_Scrub"]
    },
    "Entries": [
      {
        "Names": ["Env_Zone2_Feran"],
        "NoiseMask": {
          "Seed": "Drylands-Pattern-Monuments-Tier-1",
          "Density": [0.8125, 1],
          "NoiseType": "CELL",
          "Formula": "INVERTED_SQRT",
          "CellMode": "DISTANCE",
          "Scale": 0.005,
          "Jitter": 0.75,
          "Threshold": [0.6, 1]
        }
      }
    ]
  }
}
```

**Properties:**
- **`Default`** - Default environment IDs
- **`Entries`** - Conditional environments with noise masks

## World Types

### Default

Full world generation with multiple zones and biomes.

`Server/World/Default/`

### Flat

Flat world for building and testing.

`Server/World/Flat/`

```json
{
  "Masks": ["Mask.json"],
  "PrefabStore": "ASSETS",
  "Height": 1,
  "Width": 1
}
```

### Void

Empty void world.

`Server/World/Void/`

### Instance Worlds

World configurations for specific server instances:

- `Server/World/Instance_Creative_Hub/`
- `Server/World/Instance_Dungeon_Goblin/`
- `Server/World/Instance_Forgotten_Temple/`

## Creating a Custom World

### Step 1: Create World Folder

Create `Server/World/MyCustomWorld/`

### Step 2: Create World.json

```json
{
  "Masks": ["Mask.json"],
  "PrefabStore": "ASSETS",
  "Height": 1,
  "Width": 1,
  "OffsetX": 0,
  "OffsetY": 0
}
```

### Step 3: Create Mask.json

Define biome masks, climates, and unique zones.

### Step 4: Create Zones.json

Map colors to zone names.

### Step 5: Create Zone Definitions

Create `Zones/MyZone/Zone.json` and tile files.

## Tips

1. **Use Existing Zones** - Reference existing zones as templates
2. **Test Incrementally** - Test masks and zones one at a time
3. **Noise Parameters** - Adjust `Scale`, `Persistence`, and `Octaves` for different terrain styles
4. **Zone Colors** - Use distinct colors for zones to visualize in world editor
5. **Mask Files** - Reference mask files in `Mask/` subdirectory
6. **Prefab Patterns** - Use prefab pattern files for structure placement
7. **Layer Organization** - Separate static (bedrock) and dynamic (caves) layers

---

**Previous:** [Debug Files](159_Debug_Files.md) | **Next:** [Projectile Configs](161_Projectile_Configs.md)
