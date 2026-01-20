# Weather Systems

Learn how to create and configure weather systems for environments and zones.

## Overview

Weather in Hytale is configured through environment files that define weather forecasts for different times of day. Each weather type can have custom fog, particles, sky colors, and screen effects.

## Location
- Weather files: `Server/Weathers/`
- Weather configuration: `Server/Environments/{Zone}/`

## Example from Game Files

### Rain Weather

From `Server/Weathers/Zone1/Zone1_Rain.json`:

```1:295:Server/Weathers/Zone1/Zone1_Rain.json
{
  "Stars": "Sky/Stars.png",
  "Tags": {
    "Zone1": [
    ],
    "Rain": [
      "Moderate"
    ]
  },
  "Moons": [
    {
      "Day": 0,
      "Texture": "Sky/MoonCycle/Moon_Full.png"
    },
    {
      "Day": 1,
      "Texture": "Sky/MoonCycle/Moon_Gibbous.png"
    },
    {
      "Day": 2,
      "Texture": "Sky/MoonCycle/Moon_Half.png"
    },
    {
      "Day": 3,
      "Texture": "Sky/MoonCycle/Moon_Crescent.png"
    },
    {
      "Day": 4,
      "Texture": "Sky/MoonCycle/Moon_New.png"
    }
  ],
  "Clouds": [
    {
      "Texture": "Sky/Clouds/Rain_Light_Highlights.png",
      "Colors": [
        {
          "Hour": 3,
          "Color": "rgba(#304444, 0.293)"
        },
        {
          "Hour": 5,
          "Color": "rgba(#a09385, 0.333)"
        },
        {
          "Hour": 7,
          "Color": "rgba(#bad8e1, 0.401)"
        },
        {
          "Hour": 17,
          "Color": "rgba(#bad8e1, 0.65)"
        },
        {
          "Hour": 19,
          "Color": "rgba(#a79d9d, 0.444)"
        },
        {
          "Hour": 21,
          "Color": "rgba(#304444, 0.315)"
        }
      ],
      "Speeds": [
        {
          "Hour": 0,
          "Value": 0.7
        }
      ]
    }
  ],
  "SkyTopColors": [
    {
      "Hour": 3,
      "Color": "rgba(#000000, 1)"
    },
    {
      "Hour": 5,
      "Color": "rgba(#2b313b, 1)"
    },
    {
      "Hour": 7,
      "Color": "rgba(#92aabf, 1)"
    },
    {
      "Hour": 17,
      "Color": "rgba(#92aabf, 1)"
    },
    {
      "Hour": 19,
      "Color": "rgba(#2b313b, 1)"
    },
    {
      "Hour": 21,
      "Color": "#2a2a2bdb"
    }
  ],
  "FogColors": [
    {
      "Hour": 3,
      "Color": "#16191b"
    },
    {
      "Hour": 5,
      "Color": "#b1b1b1"
    },
    {
      "Hour": 7,
      "Color": "#a4abbf"
    },
    {
      "Hour": 17,
      "Color": "#c9c9c9"
    },
    {
      "Hour": 19,
      "Color": "#b1b1b1"
    },
    {
      "Hour": 21,
      "Color": "#16191b"
    }
  ],
  "FogDistance": [
    -96,
    1024
  ],
  "Particle": {
    "SystemId": "Rain",
    "OvergroundOnly": true
  },
  "FogDensities": [
    {
      "Hour": 0,
      "Value": 0.2
    }
  ]
}
```

This shows a complete weather configuration with fog, particles, sky colors, cloud colors, and moon cycles.

## Creating Weather Definitions

Weather definitions are referenced by Weather IDs in environment configurations. They define visual and atmospheric effects.

### Basic Weather Structure

Create `Server/Weathers/MyCustom/MyCustom_Weather.json`:

```json
{
  "FogDistance": [0.0, 1024.0],
  "Particle": {
    "SystemId": "Dust_Sparkles_Fine",
    "Color": "#ea0000",
    "Scale": 1.0
  },
  "SkyTopColors": [
    {
      "Hour": 0,
      "Color": "#1a4d8cff"
    },
    {
      "Hour": 12,
      "Color": "#87ceebff"
    },
    {
      "Hour": 23,
      "Color": "#000000ff"
    }
  ],
  "FogColors": [
    {
      "Hour": 0,
      "Color": "#020205"
    },
    {
      "Hour": 12,
      "Color": "#e0e0e0ff"
    }
  ],
  "FogHeightFalloffs": [
    {
      "Hour": 0,
      "Value": 4.0
    },
    {
      "Hour": 12,
      "Value": 8.0
    }
  ],
  "FogDensities": [
    {
      "Hour": 0,
      "Value": 1.2
    },
    {
      "Hour": 12,
      "Value": 0.5
    }
  ],
  "ScreenEffect": "ScreenEffects/Rain.png",
  "ScreenEffectColors": [
    {
      "Hour": 0,
      "Color": "#ffffff80"
    }
  ]
}
```

## Weather Properties

### Fog Configuration

#### Fog Distance

```json
"FogDistance": [0.0, 1024.0]
```

- `[0]` - Minimum fog distance (near clipping)
- `[1]` - Maximum fog distance (far clipping)

#### Fog Colors (Time-Based)

```json
"FogColors": [
  {
    "Hour": 0,      // Time of day (0-23)
    "Color": "#020205"  // Hex color with alpha (RRGGBBAA)
  },
  {
    "Hour": 12,
    "Color": "#e0e0e0ff"
  }
]
```

Colors interpolate between defined hours.

#### Fog Density (Time-Based)

```json
"FogDensities": [
  {
    "Hour": 0,
    "Value": 1.2  // Higher = denser fog
  },
  {
    "Hour": 12,
    "Value": 0.5
  }
]
```

#### Fog Height Falloff (Time-Based)

```json
"FogHeightFalloffs": [
  {
    "Hour": 0,
    "Value": 4.0  // How quickly fog decreases with height
  }
]
```

### Sky Colors (Time-Based)

```json
"SkyTopColors": [
  {
    "Hour": 0,
    "Color": "#000000ff"  // Night sky
  },
  {
    "Hour": 6,
    "Color": "#ff6b35ff"  // Sunrise
  },
  {
    "Hour": 12,
    "Color": "#87ceebff"  // Day sky
  },
  {
    "Hour": 18,
    "Color": "#ff6b35ff"  // Sunset
  },
  {
    "Hour": 23,
    "Color": "#000000ff"  // Night sky
  }
]
```

Sky colors interpolate between defined hours for smooth transitions.

### Particle Effects

```json
"Particle": {
  "SystemId": "Dust_Sparkles_Fine",
  "Color": "#ea0000",
  "Scale": 1.0
}
```

- **`SystemId`** - Particle system ID (e.g., rain, snow, dust)
- **`Color`** - Particle color tint (hex color)
- **`Scale`** - Particle size multiplier

### Screen Effects

```json
"ScreenEffect": "ScreenEffects/Rain.png",
"ScreenEffectColors": [
  {
    "Hour": 0,
    "Color": "#ffffff80"  // Semi-transparent white
  }
]
```

- **`ScreenEffect`** - Texture path for screen overlay (rain, snow, etc.)
- **`ScreenEffectColors`** - Color tint for screen effect (time-based)

## Configuring Weather in Environments

Weather is configured in environment files using `WeatherForecasts`:

### Basic Weather Forecast

Create or modify `Server/Environments/Zone1/Env_Zone1.json`:

```json
{
  "WeatherForecasts": {
    "0": [
      {
        "WeatherId": "Zone1_Sunny",
        "Weight": 52
      },
      {
        "WeatherId": "Zone1_Rain",
        "Weight": 1
      },
      {
        "WeatherId": "Zone1_Storm",
        "Weight": 1
      },
      {
        "WeatherId": "Zone1_Cloudy_Medium",
        "Weight": 10
      }
    ],
    "1": [
      {
        "WeatherId": "Zone1_Sunny",
        "Weight": 52
      },
      {
        "WeatherId": "Zone1_Rain",
        "Weight": 1
      }
    ]
  }
}
```

### Weather Forecast Keys

Keys in `WeatherForecasts` are **hour numbers** (0-23):
- `"0"` - Midnight (00:00)
- `"6"` - 6 AM
- `"12"` - Noon (12:00)
- `"18"` - 6 PM
- `"23"` - 11 PM

### Weather Weight System

Weights determine the probability of each weather type:

```json
{
  "WeatherId": "Zone1_Sunny",
  "Weight": 52  // Higher = more likely
}
```

**Weight Calculation:**
- Total all weights in the hour's array
- Each weather's probability = `Weight / TotalWeights`

**Example:**
- Sunny: Weight 52
- Rain: Weight 1
- Storm: Weight 1
- Cloudy: Weight 10
- **Total:** 64
- **Probabilities:** Sunny (81%), Rain (1.5%), Storm (1.5%), Cloudy (15.6%)

### Complete Environment Example

```json
{
  "WaterTint": "#1983d9",
  "SpawnDensity": 0.5,
  "Tags": {
    "Zone1": []
  },
  "WeatherForecasts": {
    "0": [
      {
        "WeatherId": "Zone1_Foggy_Light",
        "Weight": 20
      },
      {
        "WeatherId": "Zone1_Sunny",
        "Weight": 32
      },
      {
        "WeatherId": "Zone1_Cloudy_Medium",
        "Weight": 10
      },
      {
        "WeatherId": "Zone1_Rain_Light",
        "Weight": 2
      },
      {
        "WeatherId": "Zone1_Storm",
        "Weight": 1
      }
    ],
    "6": [
      {
        "WeatherId": "Zone1_Sunny_Fireflies",
        "Weight": 0
      },
      {
        "WeatherId": "Zone1_Sunny",
        "Weight": 52
      },
      {
        "WeatherId": "Zone1_Cloudy_Medium",
        "Weight": 10
      },
      {
        "WeatherId": "Zone1_Rain",
        "Weight": 1
      }
    ],
    "12": [
      {
        "WeatherId": "Zone1_Sunny",
        "Weight": 52
      },
      {
        "WeatherId": "Zone1_Cloudy_Medium",
        "Weight": 10
      },
      {
        "WeatherId": "Zone1_Rain",
        "Weight": 1
      },
      {
        "WeatherId": "Zone1_Storm",
        "Weight": 1
      }
    ]
  }
}
```

## Common Weather Types

### Sunny Weather

```json
{
  "FogDistance": [512.0, 2048.0],
  "SkyTopColors": [
    { "Hour": 12, "Color": "#87ceebff" }
  ],
  "FogDensities": [
    { "Hour": 12, "Value": 0.1 }
  ]
}
```

### Rainy Weather

```json
{
  "FogDistance": [128.0, 512.0],
  "Particle": {
    "SystemId": "Rain_Drops",
    "Color": "#ffffff",
    "Scale": 1.0
  },
  "ScreenEffect": "ScreenEffects/Rain.png",
  "FogColors": [
    { "Hour": 12, "Color": "#808080ff" }
  ],
  "FogDensities": [
    { "Hour": 12, "Value": 0.8 }
  ]
}
```

### Stormy Weather

```json
{
  "FogDistance": [64.0, 256.0],
  "Particle": {
    "SystemId": "Rain_Drops_Heavy",
    "Color": "#404040",
    "Scale": 1.5
  },
  "ScreenEffect": "ScreenEffects/Rainstorm.png",
  "SkyTopColors": [
    { "Hour": 12, "Color": "#2d2d2dff" }
  ],
  "FogColors": [
    { "Hour": 12, "Color": "#1a1a1aff" }
  ],
  "FogDensities": [
    { "Hour": 12, "Value": 1.5 }
  ]
}
```

### Foggy Weather

```json
{
  "FogDistance": [32.0, 128.0],
  "FogColors": [
    { "Hour": 12, "Color": "#e0e0e0ff" }
  ],
  "FogDensities": [
    { "Hour": 12, "Value": 2.0 }
  ],
  "FogHeightFalloffs": [
    { "Hour": 12, "Value": 2.0 }
  ]
}
```

## Weather ID Naming Convention

Weather IDs typically follow zone-based naming:

- `Zone1_Sunny` - Sunny weather for Zone 1
- `Zone1_Rain` - Rain for Zone 1
- `Zone1_Storm` - Storm for Zone 1
- `Zone1_Cloudy_Medium` - Medium clouds for Zone 1
- `Zone1_Foggy_Light` - Light fog for Zone 1
- `Zone2_Blazing_Light` - Hot weather for Zone 2 (desert)
- `Zone3_Cloudy_Light` - Light clouds for Zone 3 (tundra)

## Hour-Based Variation

Weather can vary throughout the day:

```json
"0": [  // Midnight - more fog, darker
  { "WeatherId": "Zone1_Foggy_Light", "Weight": 30 },
  { "WeatherId": "Zone1_Sunny", "Weight": 20 }
],
"6": [  // Dawn - clear skies more likely
  { "WeatherId": "Zone1_Sunny_Fireflies", "Weight": 10 },
  { "WeatherId": "Zone1_Sunny", "Weight": 50 }
],
"12": [  // Noon - bright and clear
  { "WeatherId": "Zone1_Sunny", "Weight": 60 },
  { "WeatherId": "Zone1_Cloudy_Medium", "Weight": 10 }
],
"18": [  // Dusk - storms more common
  { "WeatherId": "Zone1_Sunny", "Weight": 40 },
  { "WeatherId": "Zone1_Storm", "Weight": 5 },
  { "WeatherId": "Zone1_Rain", "Weight": 2 }
]
```

## Tips for Creating Weather

1. **Define all 24 hours** - Even if using same weather, define each hour for control
2. **Use weights carefully** - Higher weights = more common weather
3. **Vary by time of day** - Morning/evening can have different weather patterns
4. **Match particle systems** - Use appropriate particle IDs for weather type
5. **Set fog appropriately** - Fog density affects visibility
6. **Use screen effects** - Rain/snow overlays add atmosphere
7. **Test in-game** - Weather effects can be subtle, test visually

---

**Previous:** [Dungeons & Structures](22_Dungeons_and_Structures.md) | **Next:** [Biomes & Environments](24_Biomes_and_Environments.md)
