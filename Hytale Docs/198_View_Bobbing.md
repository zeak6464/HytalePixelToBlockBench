# View Bobbing

Learn how to configure camera view bobbing for different movement states.

## Overview

View bobbing creates rhythmic camera movement that simulates head motion during player actions. Each movement state (walking, running, swimming, etc.) has its own bobbing configuration with wave functions controlling position and rotation oscillations.

## Location

`Server/Camera/ViewBobbing/`

## Available Configurations

| File | Movement State |
|------|----------------|
| `Idle.json` | Standing still |
| `Walking.json` | Normal walking |
| `Running.json` | Running speed |
| `Sprinting.json` | Sprint speed |
| `Crouching.json` | Crouched movement |
| `Swimming.json` | In water |
| `Climbing.json` | Climbing surfaces |
| `Mounting.json` | Riding a mount |
| `SprintMounting.json` | Sprint riding |
| `Sliding.json` | Sliding on slopes |
| `Flying.json` | Creative flight |
| `None.json` | No bobbing |

## Examples from Game Files

### Idle Bobbing (Minimal)

From `Server/Camera/ViewBobbing/Idle.json`:

```json
{
  "FirstPerson": {
    "EaseIn": {
      "Time": 0.5,
      "Type": "Linear"
    },
    "Offset": {
      "X": [],
      "Y": [
        {
          "Type": "Cos",
          "Frequency": 1.5,
          "Amplitude": 0.0015
        }
      ],
      "Z": []
    },
    "Rotation": {
      "Pitch": [
        {
          "Type": "Cos",
          "Frequency": 1.5,
          "Amplitude": 0.002
        }
      ],
      "Yaw": [
        {
          "Type": "Sin",
          "Frequency": 0.75,
          "Amplitude": 0.001
        }
      ],
      "Roll": []
    }
  }
}
```

Subtle breathing motion - minimal Y offset and tiny rotation.

### Walking Bobbing

From `Server/Camera/ViewBobbing/Walking.json`:

```json
{
  "FirstPerson": {
    "EaseIn": {
      "Time": 0.5,
      "Type": "Linear"
    },
    "Offset": {
      "X": [
        {
          "Type": "Sin",
          "Frequency": 4,
          "Amplitude": 0.01
        }
      ],
      "Y": [
        {
          "Type": "Cos",
          "Frequency": 8,
          "Amplitude": 0.01,
          "Clamp": {
            "Min": -0.5
          }
        }
      ],
      "Z": []
    },
    "Rotation": {
      "Pitch": [
        {
          "Type": "Cos",
          "Frequency": 8,
          "Amplitude": 0.005
        }
      ],
      "Yaw": [
        {
          "Type": "Sin",
          "Frequency": 4,
          "Amplitude": 0.001
        }
      ],
      "Roll": []
    }
  }
}
```

Side-to-side sway (X) and vertical bounce (Y) with doubled frequency for Y to match footsteps.

### Running Bobbing

From `Server/Camera/ViewBobbing/Running.json`:

```json
{
  "FirstPerson": {
    "EaseIn": {
      "Time": 0.5,
      "Type": "Linear"
    },
    "Offset": {
      "X": [
        {
          "Type": "Sin",
          "Frequency": 11.0,
          "Amplitude": 0.02
        }
      ],
      "Y": [
        {
          "Type": "Cos",
          "Frequency": 22.0,
          "Amplitude": 0.024,
          "Clamp": {
            "Min": -0.5
          }
        },
        {
          "Type": "Perlin_Hermite",
          "Frequency": 22.0,
          "Amplitude": 0.005,
          "Clamp": {
            "Min": -0.5
          }
        }
      ],
      "Z": []
    },
    "Rotation": {
      "Pitch": [
        {
          "Type": "Cos",
          "Frequency": 22.0,
          "Amplitude": 0.001
        }
      ],
      "Roll": []
    }
  }
}
```

Higher frequency and amplitude than walking, with Perlin noise for organic feel.

### Sprinting Bobbing (Complex)

From `Server/Camera/ViewBobbing/Sprinting.json`:

```json
{
  "FirstPerson": {
    "EaseIn": {
      "Time": 0.5,
      "Type": "Linear"
    },
    "Offset": {
      "X": [
        {
          "Type": "Sin",
          "Frequency": 9.0,
          "Amplitude": 0.05
        },
        {
          "Type": "Perlin_Hermite",
          "Frequency": 9.0,
          "Amplitude": 0.01
        }
      ],
      "Y": [
        {
          "Type": "Cos",
          "Frequency": 18.0,
          "Amplitude": 0.04,
          "Clamp": {
            "Min": -0.5,
            "Max": 1.0
          }
        },
        {
          "Type": "Perlin_Hermite",
          "Frequency": 18.0,
          "Amplitude": 0.002
        },
        {
          "Type": "Cos",
          "Frequency": 18.0,
          "Amplitude": 0.015
        },
        {
          "Type": "Perlin_Hermite",
          "Clamp": {
            "Max": 1.0,
            "Min": 0.99
          },
          "Frequency": 1.0,
          "Amplitude": 0.15
        }
      ],
      "Z": [
        {
          "Type": "Perlin_Hermite",
          "Clamp": {
            "Min": -1.0,
            "Max": -0.9
          },
          "Frequency": 1.0,
          "Amplitude": 0.1
        }
      ]
    },
    "Rotation": {
      "Pitch": [
        {
          "Type": "Cos",
          "Frequency": 18.0,
          "Amplitude": 0.002
        },
        {
          "Type": "Perlin_Hermite",
          "Frequency": 18.0,
          "Amplitude": 0.001
        }
      ],
      "Yaw": [
        {
          "Type": "Sin",
          "Frequency": 9.0,
          "Amplitude": 0.002
        }
      ],
      "Roll": [
        {
          "Type": "Perlin_Hermite",
          "Frequency": 9.0,
          "Amplitude": 0.001,
          "Clamp": {
            "Min": -1.0,
            "Max": 1.0
          }
        }
      ]
    }
  }
}
```

Most complex bobbing - multiple layered waves with Perlin noise for chaotic sprint feeling.

### Swimming Bobbing

From `Server/Camera/ViewBobbing/Swimming.json`:

```json
{
  "FirstPerson": {
    "EaseIn": {
      "Time": 0.5,
      "Type": "Linear"
    },
    "Offset": {
      "X": [
        {
          "Type": "Sin",
          "Frequency": 1.0,
          "Amplitude": 0.005
        }
      ],
      "Y": [
        {
          "Type": "Cos",
          "Frequency": 3.5,
          "Amplitude": 0.005
        }
      ],
      "Z": []
    },
    "Rotation": {
      "Pitch": [
        {
          "Type": "Cos",
          "Frequency": 3.5,
          "Amplitude": 0.0025
        }
      ],
      "Yaw": [
        {
          "Type": "Sin",
          "Frequency": 1.0,
          "Amplitude": 0.00625
        }
      ],
      "Roll": [
        {
          "Type": "Sin",
          "Frequency": 1.5,
          "Amplitude": -0.0025
        }
      ]
    }
  }
}
```

Slow, gentle wave motion for floating in water.

### Crouching Bobbing

From `Server/Camera/ViewBobbing/Crouching.json`:

```json
{
  "FirstPerson": {
    "EaseIn": {
      "Time": 0.5,
      "Type": "Linear"
    },
    "Offset": {
      "X": [
        {
          "Type": "Sin",
          "Frequency": 4.8,
          "Amplitude": 0.05
        }
      ],
      "Y": [
        {
          "Type": "Cos",
          "Frequency": 9.6,
          "Amplitude": 0.02
        }
      ],
      "Z": []
    },
    "Rotation": {
      "Pitch": [
        {
          "Type": "Cos",
          "Frequency": 9.6,
          "Amplitude": 0.003
        }
      ],
      "Yaw": [
        {
          "Type": "Sin",
          "Frequency": 4.8,
          "Amplitude": 0.002
        }
      ],
      "Roll": [
        {
          "Type": "Sin",
          "Frequency": 4.8,
          "Amplitude": -0.002
        }
      ]
    }
  }
}
```

Exaggerated lateral sway for sneaking motion.

### Climbing Bobbing

From `Server/Camera/ViewBobbing/Climbing.json`:

```json
{
  "FirstPerson": {
    "EaseIn": {
      "Time": 0.5,
      "Type": "Linear"
    },
    "Offset": {
      "X": [
        {
          "Type": "Sin",
          "Frequency": 0.0,
          "Amplitude": 0.0,
          "Clamp": {
            "Min": -0.512,
            "Max": 0.504
          }
        }
      ],
      "Y": [
        {
          "Type": "Cos",
          "Frequency": 0.0,
          "Amplitude": 0.15,
          "Clamp": {
            "Min": -0.9,
            "Max": 0.9,
            "Normalize": false
          }
        }
      ],
      "Z": [
        {
          "Type": "Cos",
          "Frequency": 0.0,
          "Amplitude": 0.003,
          "Clamp": {
            "Min": -0.256,
            "Max": 0.256
          }
        }
      ]
    },
    "Rotation": {
      "Pitch": [
        {
          "Clamp": {
            "Min": -0.3,
            "Max": 0.3
          },
          "Frequency": 0.0,
          "Amplitude": 0.001,
          "Type": "Sin"
        }
      ],
      "Yaw": [
        {
          "Type": "Cos",
          "Frequency": 0.0,
          "Amplitude": 0.0,
          "Clamp": {
            "Min": -0.4,
            "Max": 0.4
          }
        }
      ],
      "Roll": [
        {
          "Type": "Sin",
          "Frequency": 0.0,
          "Amplitude": 0.0,
          "Clamp": {
            "Min": -0.4,
            "Max": 0.4
          }
        }
      ]
    }
  }
}
```

Uses clamping with zero frequency - position follows climbing animation rather than cycling.

### Sliding Bobbing

From `Server/Camera/ViewBobbing/Sliding.json`:

```json
{
  "FirstPerson": {
    "EaseIn": {
      "Time": 0.5,
      "Type": "Linear"
    },
    "Offset": {
      "X": [],
      "Y": [
        {
          "Type": "Perlin_Hermite",
          "Frequency": 20.0,
          "Amplitude": 0.02,
          "Clamp": {
            "Min": -1.0,
            "Max": 1.0
          }
        },
        {
          "Type": "Sin",
          "Frequency": 20.0,
          "Amplitude": 0.005,
          "Clamp": {
            "Min": -1.0,
            "Max": 1.0
          }
        }
      ],
      "Z": []
    },
    "Rotation": {
      "Pitch": [],
      "Yaw": [],
      "Roll": []
    }
  }
}
```

High-frequency Perlin noise for bumpy slide feeling.

### None (Disabled)

From `Server/Camera/ViewBobbing/None.json`:

```json
{
  "FirstPerson": {
    "EaseIn": {
      "Time": 0.5,
      "Type": "Linear"
    },
    "Offset": {
      "X": [],
      "Y": [],
      "Z": []
    },
    "Rotation": {
      "Pitch": [],
      "Yaw": [],
      "Roll": []
    }
  }
}
```

Empty arrays disable all bobbing.

## Properties Reference

### Root Structure

```json
{
  "FirstPerson": {
    "EaseIn": { ... },
    "Offset": { ... },
    "Rotation": { ... }
  }
}
```

Currently only `FirstPerson` is configured. Third-person uses different camera handling.

### EaseIn

Controls transition when entering this movement state:

```json
{
  "EaseIn": {
    "Time": 0.5,
    "Type": "Linear"
  }
}
```

| Property | Description |
|----------|-------------|
| `Time` | Transition duration in seconds |
| `Type` | Easing curve (`"Linear"`) |

### Offset

Camera position oscillation on X, Y, Z axes:

```json
{
  "Offset": {
    "X": [ ... ],
    "Y": [ ... ],
    "Z": [ ... ]
  }
}
```

Each axis contains an array of wave functions that are summed together.

### Rotation

Camera rotation oscillation on Pitch, Yaw, Roll:

```json
{
  "Rotation": {
    "Pitch": [ ... ],
    "Yaw": [ ... ],
    "Roll": [ ... ]
  }
}
```

| Axis | Description |
|------|-------------|
| `Pitch` | Up/down rotation |
| `Yaw` | Left/right rotation |
| `Roll` | Tilt rotation |

### Wave Functions

Each offset/rotation channel contains wave function objects:

```json
{
  "Type": "Sin",
  "Frequency": 8.0,
  "Amplitude": 0.01,
  "Clamp": {
    "Min": -0.5,
    "Max": 1.0,
    "Normalize": false
  }
}
```

| Property | Type | Description |
|----------|------|-------------|
| `Type` | String | Wave type (see below) |
| `Frequency` | Float | Oscillation speed (Hz) |
| `Amplitude` | Float | Wave intensity |
| `Clamp` | Object | Output clamping (optional) |

#### Wave Types

| Type | Description |
|------|-------------|
| `Sin` | Sine wave - smooth oscillation |
| `Cos` | Cosine wave - starts at peak |
| `Perlin_Hermite` | Perlin noise - organic, random |

#### Clamp Options

| Property | Description |
|----------|-------------|
| `Min` | Minimum output value |
| `Max` | Maximum output value |
| `Normalize` | Normalize to clamp range (default true) |

## Design Patterns

### Footstep Matching

Y frequency is typically double X frequency to match two steps per side-sway cycle:

```json
{
  "Offset": {
    "X": [{ "Frequency": 4 }],
    "Y": [{ "Frequency": 8 }]
  }
}
```

### Layered Noise

Add Perlin noise to base sine waves for organic feel:

```json
{
  "Y": [
    { "Type": "Cos", "Frequency": 18.0, "Amplitude": 0.04 },
    { "Type": "Perlin_Hermite", "Frequency": 18.0, "Amplitude": 0.002 }
  ]
}
```

### Negative Amplitude

Use negative amplitude for inverse/opposite motion:

```json
{
  "Roll": [
    {
      "Type": "Sin",
      "Frequency": 4.8,
      "Amplitude": -0.002
    }
  ]
}
```

### Asymmetric Clamping

Clamp only minimum (or maximum) to create directional motion:

```json
{
  "Y": [
    {
      "Type": "Cos",
      "Frequency": 8,
      "Amplitude": 0.01,
      "Clamp": {
        "Min": -0.5
      }
    }
  ]
}
```

This prevents the camera from going below a certain Y offset.

## Creating Custom View Bobbing

### Example: Heavy Armor Walking

```json
{
  "FirstPerson": {
    "EaseIn": {
      "Time": 0.8,
      "Type": "Linear"
    },
    "Offset": {
      "X": [
        {
          "Type": "Sin",
          "Frequency": 3.0,
          "Amplitude": 0.015
        }
      ],
      "Y": [
        {
          "Type": "Cos",
          "Frequency": 6.0,
          "Amplitude": 0.025,
          "Clamp": {
            "Min": -0.3
          }
        },
        {
          "Type": "Perlin_Hermite",
          "Frequency": 6.0,
          "Amplitude": 0.005
        }
      ],
      "Z": []
    },
    "Rotation": {
      "Pitch": [
        {
          "Type": "Cos",
          "Frequency": 6.0,
          "Amplitude": 0.008
        }
      ],
      "Yaw": [],
      "Roll": [
        {
          "Type": "Sin",
          "Frequency": 3.0,
          "Amplitude": 0.003
        }
      ]
    }
  }
}
```

Slower frequency with higher amplitude for heavy, weighty feel.

### Example: Injured Limp

```json
{
  "FirstPerson": {
    "EaseIn": {
      "Time": 0.3,
      "Type": "Linear"
    },
    "Offset": {
      "X": [
        {
          "Type": "Sin",
          "Frequency": 2.5,
          "Amplitude": 0.04
        }
      ],
      "Y": [
        {
          "Type": "Cos",
          "Frequency": 2.5,
          "Amplitude": 0.03,
          "Clamp": {
            "Min": -0.8,
            "Max": 0.2
          }
        }
      ],
      "Z": []
    },
    "Rotation": {
      "Pitch": [],
      "Yaw": [],
      "Roll": [
        {
          "Type": "Sin",
          "Frequency": 2.5,
          "Amplitude": 0.01
        }
      ]
    }
  }
}
```

Asymmetric Y clamp and high roll for limping motion.

## Tips

1. **Match movement speed** - Faster movement = higher frequency
2. **Layer waves** - Combine Sin/Cos with Perlin for realism
3. **Use 2:1 ratio** - Y frequency double X for footstep feel
4. **Clamp minimum Y** - Prevents camera going through ground
5. **Keep amplitudes small** - Values over 0.1 can cause motion sickness
6. **Test transitions** - EaseIn time affects how jarring speed changes feel

## Related Documentation

- [Camera Effects](34_Camera_Effects.md) - Camera shake and effects
- [Entity Stats](28_Entity_Stats.md) - Movement stat configuration

---

**Previous:** [Instance Configuration](197_Instance_Configuration.md) | **Next:** [Getting Started](01_Getting_Started.md)
