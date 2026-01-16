# Biomes & Environments

Learn how to create and configure biomes, environments, and zone-based world generation.

## Overview

Environments define biome-specific settings including weather, water colors, spawn density, and tags. They're used for world generation and NPC spawning.

## Location
`Server/Environments/{Zone}/`

## Basic Environment Structure

Create `Server/Environments/Zone1/Env_MyCustom_Biome.json`:

```json
{
  "Parent": "Env_Zone1",
  "WaterTint": "#1983d9",
  "SpawnDensity": 0.5,
  "Tags": {
    "Zone1": ["MyCustom", "Plains"]
  },
  "WeatherForecasts": {
    "0": [
      {
        "WeatherId": "Zone1_Sunny",
        "Weight": 60
      },
      {
        "WeatherId": "Zone1_Cloudy_Medium",
        "Weight": 10
      },
      {
        "WeatherId": "Zone1_Rain",
        "Weight": 2
      }
    ]
  }
}
```

## Environment Properties

### Parent Inheritance

Environments can inherit from parent environments:

```json
{
  "Parent": "Env_Zone1",
  "Tags": {
    "Zone1": ["Plains"]
  }
}
```

This inherits all properties from `Env_Zone1` and overrides only specified fields.

### Water Tint

```json
"WaterTint": "#1983d9"
```

Hex color for water in this biome:
- `#1983d9` - Blue water (Zone 1)
- `#2076b5` - Darker blue (Zone 0)
- `#1386c2` - Light blue (Zone 2)

### Spawn Density

```json
"SpawnDensity": 0.5
```

Controls NPC and entity spawn rates:
- `0.0` - No natural spawns
- `0.3` - Low spawns (Zone 2, Zone 3)
- `0.5` - Medium spawns (Zone 1)
- `1.0` - High spawns

### Tags

Tags categorize biomes for NPC spawning and world generation:

```json
"Tags": {
  "Zone1": ["Plains", "Forests"],
  "Biome": ["Temperate", "Overground"]
}
```

Common tag categories:
- **Zone tags** - `Zone1`, `Zone2`, `Zone3`, `Zone4`
- **Biome tags** - `Plains`, `Forests`, `Deserts`, `Swamps`, `Mountains`
- **Type tags** - `Overground`, `Underground`, `Temperate`, `Cold`, `Hot`

### Weather Forecasts

See [Weather Systems](23_Weather.md) for detailed weather configuration.

```json
"WeatherForecasts": {
  "0": [
    { "WeatherId": "Zone1_Sunny", "Weight": 52 },
    { "WeatherId": "Zone1_Rain", "Weight": 2 }
  ]
}
```

## Zone-Based Environments

Environments are organized by zones:

### Zone 1 (Temperate/Plains)

```json
{
  "WaterTint": "#1983d9",
  "SpawnDensity": 0.5,
  "Tags": {
    "Zone1": ["Plains"]
  },
  "WeatherForecasts": {
    "12": [
      { "WeatherId": "Zone1_Sunny", "Weight": 52 },
      { "WeatherId": "Zone1_Cloudy_Medium", "Weight": 10 },
      { "WeatherId": "Zone1_Rain", "Weight": 1 }
    ]
  }
}
```

Common Zone 1 biomes:
- `Env_Zone1_Plains` - Plains biome
- `Env_Zone1_Forests` - Forest biome
- `Env_Zone1_Mountains` - Mountain biome
- `Env_Zone1_Swamps` - Swamp biome

### Zone 2 (Desert/Hot)

```json
{
  "WaterTint": "#1386c2",
  "SpawnDensity": 0.3,
  "Tags": {
    "Zone2": ["Deserts"]
  },
  "WeatherForecasts": {
    "12": [
      { "WeatherId": "Zone2_Blazing_Light", "Weight": 30 },
      { "WeatherId": "Zone2_Sunny", "Weight": 40 },
      { "WeatherId": "Zone2_Thunder_Storm", "Weight": 5 }
    ]
  }
}
```

Common Zone 2 biomes:
- `Env_Zone2_Deserts` - Desert biome
- `Env_Zone2_Savanna` - Savanna biome
- `Env_Zone2_Oasis` - Oasis biome

### Zone 3 (Tundra/Cold)

```json
{
  "WaterTint": "#2076b5",
  "SpawnDensity": 0.3,
  "Tags": {
    "Zone3": ["Tundra"]
  },
  "WeatherForecasts": {
    "12": [
      { "WeatherId": "Zone3_Cloudy_Medium", "Weight": 25 },
      { "WeatherId": "Zone3_Rain", "Weight": 25 },
      { "WeatherId": "Zone3_Cloudy_Light", "Weight": 25 }
    ]
  }
}
```

Common Zone 3 biomes:
- `Env_Zone3_Tundra` - Tundra biome
- `Env_Zone3_Forests` - Cold forest biome

### Zone 4 (Jungle/Tropical)

```json
{
  "WaterTint": "#2076b5",
  "SpawnDensity": 0.4,
  "Tags": {
    "Zone4": ["Jungles"]
  }
}
```

## Environment Hierarchy

Environments can inherit from base zone environments:

### Base Zone Environment

`Server/Environments/Zone1/Env_Zone1.json`:

```json
{
  "WaterTint": "#1983d9",
  "SpawnDensity": 0.5,
  "Tags": {
    "Zone1": []
  },
  "WeatherForecasts": {
    "0": [
      { "WeatherId": "Zone1_Sunny", "Weight": 52 },
      { "WeatherId": "Zone1_Cloudy_Medium", "Weight": 10 },
      { "WeatherId": "Zone1_Rain", "Weight": 1 }
    ]
  }
}
```

### Biome-Specific Environment

`Server/Environments/Zone1/Env_Zone1_Plains.json`:

```json
{
  "Parent": "Env_Zone1",
  "Tags": {
    "Zone1": ["Plains"]
  },
  "WeatherForecasts": {
    "12": [
      { "WeatherId": "Zone1_Sunny", "Weight": 60 },
      { "WeatherId": "Zone1_Cloudy_Medium", "Weight": 8 }
    ]
  }
}
```

This inherits all Zone 1 settings and overrides:
- Tags to include "Plains"
- Weather forecasts for noon hour

## Unique Environments

Some environments are for special areas:

### Creative Hub

```json
{
  "WaterTint": "#2076b5",
  "SpawnDensity": 0.0,
  "Tags": {},
  "WeatherForecasts": {
    "0": [
      { "WeatherId": "Zone1_Sunny", "Weight": 100 }
    ]
  }
}
```

### Void

```json
{
  "WaterTint": "#2076b5",
  "SpawnDensity": 0.0,
  "Tags": {}
}
```

### Dungeon Environments

```json
{
  "Parent": "Env_Forgotten_Temple_Base",
  "WaterTint": "#000000",
  "SpawnDensity": 0.8,
  "Tags": {
    "Dungeon": ["Interior"]
  }
}
```

## Using Environments in World Generation

Environments are referenced in world generation:

### Flat World Layers

```json
{
  "WorldGen": {
    "Type": "Flat",
    "Layers": [
      {
        "BlockType": "Rock_Stone",
        "Environment": "Env_Zone1_Plains",
        "From": 0,
        "To": 75
      },
      {
        "BlockType": "Soil_Dirt",
        "Environment": "Env_Zone1_Plains",
        "From": 75,
        "To": 79
      },
      {
        "BlockType": "Soil_Grass",
        "Environment": "Env_Zone1_Plains",
        "From": 79,
        "To": 80
      }
    ]
  }
}
```

### Hytale Generator

```json
{
  "WorldGen": {
    "Type": "HytaleGenerator",
    "Name": "Instance_MyCustomWorld",
    "Environment": "Env_Zone1_Plains"
  }
}
```

## Environment Tags for NPC Spawning

NPC spawns reference environment tags:

```json
{
  "Environments": [
    "Env_Zone1_Plains",
    "Env_Zone1_Forests"
  ],
  "NPCs": [
    {
      "Weight": 10,
      "SpawnBlockSet": "Soil",
      "Id": "Goblin_Scrapper"
    }
  ]
}
```

See [Creating NPCs](03_NPCs.md) for more on NPC spawning.

## Complete Example: Custom Biome

```json
{
  "Parent": "Env_Zone1",
  "WaterTint": "#2a8f2aff",
  "SpawnDensity": 0.6,
  "Tags": {
    "Zone1": ["MyCustom", "Plains", "Magical"]
  },
  "WeatherForecasts": {
    "0": [
      { "WeatherId": "Zone1_Sunny", "Weight": 40 },
      { "WeatherId": "Zone1_Foggy_Light", "Weight": 30 },
      { "WeatherId": "Zone1_Sunny_Fireflies", "Weight": 10 }
    ],
    "12": [
      { "WeatherId": "Zone1_Sunny", "Weight": 50 },
      { "WeatherId": "Zone1_Cloudy_Medium", "Weight": 8 },
      { "WeatherId": "Zone1_Rain_Light", "Weight": 2 }
    ]
  }
}
```

## Tips for Creating Environments

1. **Use parent inheritance** - Inherit from base zone environment and override only needed fields
2. **Set appropriate spawn density** - Match to biome type (deserts lower, forests higher)
3. **Define tags clearly** - Tags enable NPC spawning and world generation
4. **Configure weather** - Each biome should have appropriate weather patterns
5. **Match water tint** - Use colors that match the biome theme
6. **Test NPC spawning** - Verify NPCs spawn correctly with your environment tags
7. **Organize by zone** - Keep environments organized in zone folders

---

**Previous:** [Weather Systems](23_Weather.md) | **Next:** [Damage Types](25_Damage_Types.md)
