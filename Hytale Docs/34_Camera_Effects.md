# Camera Effects

Learn how to configure camera shake, view bobbing, and camera effects for weapons, damage, and abilities.

## Overview

Camera effects in Hytale control how the camera responds to player actions like attacking, taking damage, and moving. They're defined in `Server/Camera/` and include camera shake, view bobbing, and camera effects.

## Location
- Camera shake: `Server/Camera/CameraShake/`
- Camera effects: `Server/Camera/CameraEffect/`
- View bobbing: `Server/Camera/ViewBobbing/`

## Example from Game Files

### Sword Swing Camera Shake

From `Server/Camera/CameraShake/Sword/Sword_Swing_Vertical.json`:

```1:56:Server/Camera/CameraShake/Sword/Sword_Swing_Vertical.json
{
  "FirstPerson": {
    "Duration": 0.0,
    "EaseIn": {
      "Time": 0.05,
      "Type": "QuadInOut"
    },
    "EaseOut": {
      "Time": 0.25,
      "Type": "QuadInOut"
    },
    "Offset": {
      "X": [],
      "Y": [],
      "Z": []
    },
    "Rotation": {
      "Pitch": [
        {
          "Frequency": 20.0,
          "Amplitude": 0.2,
          "Type": "Sin"
        }
      ],
      "Yaw": [],
      "Roll": []
    }
  },
  "ThirdPerson": {
    "Duration": 0.0,
    "EaseIn": {
      "Time": 0.05,
      "Type": "QuadInOut"
    },
    "EaseOut": {
      "Time": 0.25,
      "Type": "QuadInOut"
    },
    "Offset": {
      "X": [],
      "Y": [],
      "Z": []
    },
    "Rotation": {
      "Pitch": [
        {
          "Frequency": 20.0,
          "Amplitude": 0.1,
          "Type": "Sin"
        }
      ],
      "Yaw": [],
      "Roll": []
    }
  }
}
```

This shows a camera shake configuration for sword attacks with first-person and third-person settings.

## Camera Shake

Camera shake creates screen shake when attacking, taking damage, or using abilities.

### Camera Shake Properties

| Property | Description |
|----------|-------------|
| `Duration` | Shake duration (0.0 = use EaseIn/EaseOut timing) |
| `EaseIn` | Transition in settings (`Time`, `Type`) |
| `EaseOut` | Transition out settings (`Time`, `Type`) |
| `Offset` | Position offset (`X`, `Y`, `Z` arrays) |
| `Rotation` | Rotation (`Pitch`, `Yaw`, `Roll` arrays) |

### Easing Types

- **`Linear`** - Constant rate transition
- **`QuadInOut`** - Smooth quadratic ease in and out

### Function Types

| Type | Description |
|------|-------------|
| `Sin` | Sine wave oscillation |
| `Cos` | Cosine wave oscillation |
| `Perlin_Hermite` | Smooth noise-based movement (used for sprinting/mounting) |

### Function Properties

```json
{
  "Type": "Sin",
  "Frequency": 20.0,
  "Amplitude": 0.2,
  "Seed": 2,
  "Clamp": {
    "Min": -0.5,
    "Max": 0.5
  }
}
```

- **`Frequency`** - Oscillation speed
- **`Amplitude`** - Movement amount
- **`Seed`** - Random seed for variation (optional)
- **`Clamp`** - Min/max limits (optional)

### Using Camera Shake

Camera shake is referenced in interactions via `CameraShakeId`:

```json
{
  "Effects": {
    "CameraShakeId": "Sword_Swing_Vertical"
  }
}
```

## Camera Effect Wrapper

Camera effects wrap camera shakes with intensity modifiers.

### CameraEffect Structure

From `Server/Camera/CameraEffect/Damage/Damage.json`:

```json
{
  "Type": "CameraShake",
  "CameraShake": "Damage",
  "Intensity": {
    "AccumulationMode": "Sum",
    "Modifier": {
      "Input": [0.0, 1.0],
      "Output": [0.0, 2.5]
    }
  }
}
```

| Property | Description |
|----------|-------------|
| `Type` | Always `"CameraShake"` |
| `CameraShake` | Reference to CameraShake ID |
| `Intensity` | Intensity configuration |
| `Intensity.AccumulationMode` | How intensity accumulates (`Sum`) |
| `Intensity.Modifier` | Input/Output mapping for intensity scaling |

## View Bobbing

View bobbing controls camera movement while walking, running, or performing actions.

### Basic View Bobbing Structure

Create `Server/Camera/ViewBobbing/MyCustom_Walking.json`:

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

## View Bobbing Properties

### Ease In

```json
{
  "EaseIn": {
    "Time": 0.5,
    "Type": "Linear"
  }
}
```

Transition time when bobbing starts:
- **`Time`** - Duration in seconds
- **`Type`** - Easing type (e.g., `"Linear"`)

### Offset (Position)

```json
{
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
  }
}
```

Camera position offsets:
- **`X`, `Y`, `Z`** - Arrays of movement functions
- **`Type`** - Function type (`"Sin"`, `"Cos"`)
- **`Frequency`** - Oscillation frequency
- **`Amplitude`** - Movement amount
- **`Clamp`** - Min/max limits

### Rotation

```json
{
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
```

Camera rotation:
- **`Pitch`** - Vertical rotation (up/down)
- **`Yaw`** - Horizontal rotation (left/right)
- **`Roll`** - Side-to-side tilt

## All View Bobbing Types

| Type | File | Description |
|------|------|-------------|
| `Idle` | `Idle.json` | Subtle breathing motion when standing still |
| `Walking` | `Walking.json` | Standard walking bob |
| `Running` | `Running.json` | Faster bob for running |
| `Sprinting` | `Sprinting.json` | Intense bob with `Perlin_Hermite` noise |
| `Crouching` | `Crouching.json` | Subtle bob for crouch walking |
| `Swimming` | `Swimming.json` | Unique underwater movement |
| `Climbing` | `Climbing.json` | Ladder/rope climbing bob |
| `Mounting` | `Mounting.json` | Riding mount movement |
| `SprintMounting` | `SprintMounting.json` | Sprint on mount with `Perlin_Hermite` |
| `Sliding` | `Sliding.json` | Slide movement bob |
| `Flying` | `Flying.json` | Flying/gliding movement |
| `None` | `None.json` | No bobbing (empty) |

### Walking Example

`Server/Camera/ViewBobbing/Walking.json`:

```json
{
  "FirstPerson": {
    "Offset": {
      "X": [
        { "Type": "Sin", "Frequency": 4, "Amplitude": 0.01 }
      ],
      "Y": [
        { "Type": "Cos", "Frequency": 8, "Amplitude": 0.01, "Clamp": { "Min": -0.5 } }
      ]
    },
    "Rotation": {
      "Pitch": [
        { "Type": "Cos", "Frequency": 8, "Amplitude": 0.005 }
      ],
      "Yaw": [
        { "Type": "Sin", "Frequency": 4, "Amplitude": 0.001 }
      ]
    }
  }
}
```

### Sprinting with Perlin_Hermite

Sprinting uses `Perlin_Hermite` for realistic camera shake:

```json
{
  "FirstPerson": {
    "Offset": {
      "Y": [
        { "Type": "Perlin_Hermite", "Frequency": 16, "Amplitude": 0.015, "Seed": 2 }
      ]
    }
  }
}
```

The `Perlin_Hermite` type creates smooth noise-based movement that feels more natural than pure sine waves.

## Camera Effects

Camera effects are triggered by actions like attacking or taking damage.

### Using Camera Effects

Camera effects are referenced in interactions:

```json
{
  "Effects": {
    "CameraEffectId": "Sword_Swing_Horizontal"
  }
}
```

### Weapon Type Folders

Camera effects are organized by weapon type in `Server/Camera/CameraEffect/`:

| Folder | Examples |
|--------|----------|
| `Sword/` | Swing_Vertical, Stab, Spin, Bash |
| `Battleaxe/` | Swing_Horizontal, Downstrike, Whirlwind, Frontflip_Chop |
| `Daggers/` | Slash, Stab, Sweep, Dash, Jump, Land |
| `Mace/` | Swing_Horizontal, Jump, Explode, Bash |
| `Shield/` | Bash |
| `Handgun/` | Shoot |
| `Crossbow/` | Bash |
| `Shortbow/` | Bash |
| `Pickaxe/` | Mine, Mine_Impact |
| `Shovel/` | Dig |
| `Hatchet/` | Chop |
| `Hoe/` | Swing_Horizontal |
| `Block/` | Swing_Diagonal_Right |
| `Unarmed/` | Swing_Horizontal, Block_Impact |
| `Damage/` | Damage, Damage_Fall |
| `Impact/` | Impact, Impact_Light, Impact_Strong |
| `NPC/` | Hedera_Scream |

## Common Camera Effect IDs

### Sword Effects

- `Sword_Swing_Vertical`
- `Sword_Swing_Diagonal_Right`
- `Sword_Stab`
- `Sword_Spin`
- `Sword_Bash`

### Battleaxe Effects

- `Battleaxe_Swing_Horizontal`
- `Battleaxe_Swing_Vertical`
- `Battleaxe_Downstrike`
- `Battleaxe_Whirlwind`
- `Battleaxe_Frontflip_Chop`

### Damage/Impact Effects

- `Damage`
- `Damage_Fall`
- `Impact`
- `Impact_Light`
- `Impact_Strong`

## Using Camera Effects in Items

### Weapon Camera Effects

```json
{
  "InteractionVars": {
    "Swing_Left_Damage": {
      "Interactions": [
        {
          "Effects": {
            "CameraEffectId": "Sword_Swing_Horizontal",
            "CameraShakeId": "Sword_Swing_Horizontal"
          }
        }
      ]
    }
  }
}
```

### Damage Camera Effects

```json
{
  "DamageEffects": {
    "CameraEffectId": "Damage_Medium",
    "CameraShakeId": "Damage_Medium"
  }
}
```

## Complete Example: Custom View Bobbing

### 1. Custom Walking Bobbing

`Server/Camera/ViewBobbing/MyCustom_Walking.json`:

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
          "Frequency": 5,
          "Amplitude": 0.015
        }
      ],
      "Y": [
        {
          "Type": "Cos",
          "Frequency": 10,
          "Amplitude": 0.015,
          "Clamp": {
            "Min": -0.6,
            "Max": 0.6
          }
        }
      ],
      "Z": []
    },
    "Rotation": {
      "Pitch": [
        {
          "Type": "Cos",
          "Frequency": 10,
          "Amplitude": 0.008
        }
      ],
      "Yaw": [
        {
          "Type": "Sin",
          "Frequency": 5,
          "Amplitude": 0.002
        }
      ],
      "Roll": []
    }
  }
}
```

## Tips for Configuring Camera Effects

1. **Balance frequency** - Higher frequency = faster bobbing
2. **Adjust amplitude** - Control how much the camera moves
3. **Use clamps** - Limit movement to prevent excessive bobbing
4. **Test in-game** - Camera effects feel different than expected
5. **Match to movement** - Bobbing should match walk/run speed
6. **Use appropriate effects** - Match camera effects to weapon type
7. **Consider first-person only** - Most effects are first-person only

## Related Documentation

- [View Bobbing](198_View_Bobbing.md) - Detailed view bobbing configuration guide

---

**Previous:** [Trails](33_Trails.md) | **Next:** [Sound Effects](35_Sound_Effects.md)
