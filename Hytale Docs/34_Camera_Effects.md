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

### Basic Camera Shake Structure

Camera shake is typically configured per weapon type. Examples are in `Server/Camera/CameraShake/{WeaponType}/`.

### Using Camera Shake

Camera shake is referenced in interactions via `CameraShakeId`:

```json
{
  "Effects": {
    "CameraShakeId": "Sword_Swing_Vertical"
  }
}
```

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

## Common View Bobbing Types

### Walking

`Server/Camera/ViewBobbing/Walking.json`:

```json
{
  "FirstPerson": {
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
      ]
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
      ]
    }
  }
}
```

### Running

`Server/Camera/ViewBobbing/Running.json`:

Similar to walking but with higher frequency and amplitude for faster movement.

### Sprinting

`Server/Camera/ViewBobbing/Sprinting.json`:

Even more pronounced bobbing for sprinting.

### Crouching

`Server/Camera/ViewBobbing/Crouching.json`:

Subtle bobbing for crouch walking.

### Swimming

`Server/Camera/ViewBobbing/Swimming.json`:

Unique bobbing pattern for swimming.

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

### Camera Effect Types

Common camera effect categories:
- **Weapon swings** - `Sword_Swing_Vertical`, `Battleaxe_Swing_Horizontal`
- **Damage** - `Damage_Small`, `Damage_Large`
- **Impacts** - `Impact_Medium`, `Impact_Large`
- **NPC actions** - `NPC_Attack`

## Common Camera Effect IDs

### Sword Effects

- `Sword_Swing_Vertical`
- `Sword_Swing_Horizontal`
- `Sword_Stab`
- `Sword_Spin`

### Battleaxe Effects

- `Battleaxe_Swing_Horizontal`
- `Battleaxe_Downstrike`
- `Battleaxe_Whirlwind`

### Damage Effects

- `Damage_Small`
- `Damage_Medium`
- `Damage_Large`

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

---

**Previous:** [Trails](33_Trails.md) | **Next:** [Sound Effects](35_Sound_Effects.md)
