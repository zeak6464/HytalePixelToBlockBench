# AmbienceFX

Learn how to configure ambient environmental audio and music that plays based on conditions like time of day, weather, and biome.

## Overview

AmbienceFX (Ambient Effects) define environmental audio that plays automatically based on conditions like:
- Environment/biome tags
- Time of day
- Weather
- Sunlight levels
- Wall proximity (indoor/outdoor)

AmbienceFX can include both **ambient beds** (environmental sounds) and **music tracks**.

## Example from Game Files

### Plains Day AmbienceFX

From `Server/Audio/AmbienceFX/Ambience/Zone1/Environments/Plains/Day/AmbFX_Zone1_Plains_Gen_Day.json`:

```1:42:Server/Audio/AmbienceFX/Ambience/Zone1/Environments/Plains/Day/AmbFX_Zone1_Plains_Gen_Day.json
{
  "Conditions": {
    "DayTime": {
      "Min": 5,
      "Max": 19
    },
    "SunLightLevel": {
      "Min": 10,
      "Max": 15
    },
    "EnvironmentTagPattern": {
      "Op": "And",
      "Patterns": [
        {
          "Op": "Equals",
          "Tag": "Zone1"
        },
        {
          "Op": "Or",
          "Patterns": [
            {
              "Op": "Equals",
              "Tag": "Plains"
            },
            {
              "Op": "Equals",
              "Tag": "Encounters"
            }
          ]
        }
      ]
    },
    "Walls": {
      "Min": 0,
      "Max": 3
    }
  },
  "AmbientBed": {
    "Track": "Sounds/Environments/Zone1/Environments/Plains/Day/Z1_Plains_Gen_Day_Stereo_LOOP.ogg",
    "Volume": 1.0
  }
}
```

This shows an AmbienceFX configuration with conditions for time of day, light level, environment tags, and wall proximity.

## Location
- Ambient effects: `Server/Audio/AmbienceFX/Ambience/`
- Music: `Server/Audio/AmbienceFX/Music/`
- Reverb zones: `Server/Audio/AmbienceFX/ReverbZones/`

## Basic AmbienceFX Structure

Create `Server/Audio/AmbienceFX/Ambience/Zone1/Environments/Plains/AmbFX_Zone1_Plains_Day.json`:

```json
{
  "Conditions": {
    "DayTime": {
      "Min": 5,
      "Max": 19
    },
    "EnvironmentTagPattern": {
      "Op": "Equals",
      "Tag": "Zone1"
    }
  },
  "AmbientBed": {
    "Track": "Sounds/Environments/Zone1/Plains_Day.ogg",
    "Volume": 1.0
  }
}
```

## Conditions

### DayTime

```json
{
  "Conditions": {
    "DayTime": {
      "Min": 5,
      "Max": 19
    }
  }
}
```

- **`Min`** - Earliest hour (0-23)
- **`Max`** - Latest hour (0-23)
- Time range when this ambience plays

### Environment Tag Pattern

```json
{
  "Conditions": {
    "EnvironmentTagPattern": {
      "Op": "And",
      "Patterns": [
        {
          "Op": "Equals",
          "Tag": "Zone1"
        },
        {
          "Op": "Equals",
          "Tag": "Plains"
        }
      ]
    }
  }
}
```

Matches biomes/environments using tag patterns (see Tag Patterns guide).

### Sunlight Level

```json
{
  "Conditions": {
    "SunLightLevel": {
      "Min": 10,
      "Max": 15
    }
  }
}
```

- Light level range (indoor/outdoor detection)
- Lower = more enclosed/indoor
- Higher = more open/outdoor

### Walls

```json
{
  "Conditions": {
    "Walls": {
      "Min": 0,
      "Max": 3
    }
  }
}
```

- Number of walls nearby (indoor detection)
- **0** = fully outdoor
- **Higher** = more enclosed

## Ambient Bed

Environmental background sounds:

```json
{
  "AmbientBed": {
    "Track": "Sounds/Environments/Zone1/Plains_Day.ogg",
    "Volume": 1.0
  }
}
```

**Properties:**
- **`Track`** - Path to `.ogg` audio file (relative to `Common/`)
- **`Volume`** - Volume level (0.0 to 1.0)

## Music

Background music tracks:

```json
{
  "Music": {
    "Tracks": [
      "Music/Zone1/Track_01.ogg",
      "Music/Zone1/Track_02.ogg"
    ],
    "Volume": -10,
    "AudioCategory": "AudioCat_Music"
  }
}
```

**Properties:**
- **`Tracks`** - Array of music files (randomly selected)
- **`Volume`** - Volume in dB (typically negative, e.g., -10 to -20)
- **`AudioCategory`** - Audio category for volume control

## Complete Example: Daytime Plains Ambience

```json
{
  "Conditions": {
    "DayTime": {
      "Min": 5,
      "Max": 19
    },
    "SunLightLevel": {
      "Min": 10,
      "Max": 15
    },
    "EnvironmentTagPattern": {
      "Op": "And",
      "Patterns": [
        {
          "Op": "Equals",
          "Tag": "Zone1"
        },
        {
          "Op": "Or",
          "Patterns": [
            {
              "Op": "Equals",
              "Tag": "Plains"
            },
            {
              "Op": "Equals",
              "Tag": "Encounters"
            }
          ]
        }
      ]
    },
    "Walls": {
      "Min": 0,
      "Max": 3
    }
  },
  "AmbientBed": {
    "Track": "Sounds/Environments/Zone1/Environments/Plains/Day/Z1_Plains_Gen_Day_Stereo_LOOP.ogg",
    "Volume": 1.0
  }
}
```

## Priority

AmbienceFX can have priority to determine which plays when multiple match:

```json
{
  "Conditions": { ... },
  "Priority": 20,
  "AmbientBed": { ... }
}
```

Higher priority = plays first.

## Music with Time Conditions

```json
{
  "Conditions": {
    "DayTime": {
      "Min": 5,
      "Max": 19
    },
    "EnvironmentTagPattern": {
      "Op": "Equals",
      "Tag": "Zone1"
    }
  },
  "Music": {
    "Tracks": [
      "Music/Zone1/Z1AD-BountifulHarvest.ogg",
      "Music/Zone1/Z1AD-Comradery.ogg",
      "Music/Zone1/Z1AD-EmeraldGrove.ogg"
    ],
    "Volume": -14,
    "AudioCategory": "AudioCat_Music"
  },
  "Priority": 20
}
```

## Underwater Ambience

```json
{
  "Conditions": {
    "EnvironmentIds": ["Underwater"]
  },
  "AmbientBed": {
    "Track": "Sounds/Environments/Underwater/Underwater_Loop.ogg",
    "Volume": 0.8
  }
}
```

## Void/Portal Ambience

```json
{
  "Conditions": {
    "EnvironmentIds": ["Env_Void"]
  },
  "AmbientBed": {
    "Track": "Sounds/Environments/Void/Void_Static_Loop_Stereo_01.ogg",
    "Volume": -4.0
  },
  "Music": {
    "Tracks": [
      "Music/_general/00a1_Silence.ogg"
    ],
    "Volume": -10
  }
}
```

## Tips for Creating AmbienceFX

1. **Use conditions wisely** - Combine multiple conditions for precise control
2. **Tag patterns** - Use tag patterns for flexible biome matching
3. **Time-based** - Create separate day/night ambience files
4. **Priority** - Set higher priority for specific areas
5. **Volume balance** - Ambient beds typically 0.8-1.0, music -10 to -20 dB
6. **Looping audio** - Ambient tracks should loop seamlessly
7. **Multiple tracks** - Music arrays provide variety

---

**Previous:** [Collision Events](53_Collision_Events.md) | **Next:** [Reverb](55_Reverb.md)
