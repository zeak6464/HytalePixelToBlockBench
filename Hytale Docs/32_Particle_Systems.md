# Particle Systems

Learn how to create and configure particle effects for items, abilities, and visual effects.

## Overview

Particle systems in Hytale are defined using `.particlespawner` and `.particlesystem` files. They create visual effects like explosions, magic, fire, and environmental particles.

## Location
- Particle spawners: `Server/Particles/{Category}/Spawners/`
- Particle systems: `Server/Particles/{Category}/`
- Particle textures: `Common/Particles/Textures/`

## Example from Game Files

### Block Break Particles

From `Server/Particles/Block/Wood/Spawners/Block_Break_Wood_Parts.particlespawner`:

```1:63:Server/Particles/Block/Wood/Spawners/Block_Break_Wood_Parts.particlespawner
{
  "RenderMode": "Erosion",
  "EmitOffset": {
    "X": {
      "Min": 0,
      "Max": 0.3
    },
    "Y": {
      "Min": 0,
      "Max": 0.3
    },
    "Z": {
      "Min": 0,
      "Max": 0.3
    }
  },
  "ParticleRotationInfluence": "Billboard",
  "LinearFiltering": false,
  "LightInfluence": 1,
  "ParticleRotateWithSpawner": false,
  "TrailSpawnerPositionMultiplier": 1,
  "TrailSpawnerRotationMultiplier": 1,
  "MaxConcurrentParticles": 20,
  "ParticleLifeSpan": {
    "Min": 0.4,
    "Max": 0.8
  },
  "SpawnRate": {
    "Min": 20,
    "Max": 20
  },
  "TotalParticles": {
    "Min": 20,
    "Max": 20
  },
  "InitialVelocity": {
    "Speed": {
      "Min": 1,
      "Max": 4
    },
    "Pitch": {
      "Min": 0,
      "Max": 180
    },
    "Yaw": {
      "Min": -90,
      "Max": 90
    }
  },
  "Attractors": [
    {
      "LinearAcceleration": {
        "X": 0,
        "Y": -6.5,
        "Z": 0
      },
      "DampingMultiplier": {
        "X": 0.8,
        "Y": 1,
        "Z": 0.8
      }
    }
  ],
```

This shows a particle spawner configuration for block breaking effects with physics, spawn rates, and gravity.

## Basic Particle Spawner

Create `Server/Particles/MyCustom/Spawners/MyCustom_Effect.particlespawner`:

```json
{
  "RenderMode": "Erosion",
  "ParticleRotationInfluence": "Billboard",
  "MaxConcurrentParticles": 30,
  "SpawnRate": {
    "Min": 10,
    "Max": 20
  },
  "ParticleLifeSpan": {
    "Min": 1.0,
    "Max": 2.0
  },
  "Particle": {
    "Texture": "Particles/Textures/Basic/Spark.png",
    "FrameSize": {
      "Width": 32,
      "Height": 32
    },
    "InitialAnimationFrame": {
      "Opacity": 1.0,
      "Color": "#ffffff"
    },
    "Animation": {
      "0": {},
      "100": {
        "Opacity": 0.0
      }
    }
  }
}
```

## Particle Spawner Properties

### Render Mode

```json
{
  "RenderMode": "Erosion"
}
```

- **`"Erosion"`** - Standard particle rendering
- Other modes may be available

### Particle Rotation

```json
{
  "ParticleRotationInfluence": "Billboard"
}
```

- **`"Billboard"`** - Particles face camera
- Controls how particles rotate

### Spawn Rate

```json
{
  "SpawnRate": {
    "Min": 10,
    "Max": 20
  }
}
```

Particles spawned per second (range).

### Max Concurrent Particles

```json
{
  "MaxConcurrentParticles": 30
}
```

Maximum particles active at once.

### Particle Life Span

```json
{
  "ParticleLifeSpan": {
    "Min": 1.0,
    "Max": 2.0
  }
}
```

How long particles live (seconds).

## Particle Properties

### Texture

```json
{
  "Particle": {
    "Texture": "Particles/Textures/Basic/Spark.png",
    "FrameSize": {
      "Width": 32,
      "Height": 32
    }
  }
}
```

- **`Texture`** - Path to particle texture (relative to `Common/`)
- **`FrameSize`** - Size of each frame in spritesheet

### Initial Animation Frame

```json
{
  "InitialAnimationFrame": {
    "Opacity": 1.0,
    "Color": "#ffffff",
    "Scale": {
      "X": { "Min": 0.1, "Max": 0.2 },
      "Y": { "Min": 0.1, "Max": 0.2 }
    }
  }
}
```

Starting properties for particles:
- **`Opacity`** - Transparency (0.0 to 1.0)
- **`Color`** - Hex color
- **`Scale`** - Size multipliers

### Animation Keyframes

```json
{
  "Animation": {
    "0": {
      "Color": "#ffffff",
      "Opacity": 1.0,
      "Scale": {
        "X": { "Min": 0.1, "Max": 0.2 },
        "Y": { "Min": 0.1, "Max": 0.2 }
      }
    },
    "50": {
      "Color": "#ffff00",
      "Opacity": 0.8
    },
    "100": {
      "Color": "#ff0000",
      "Opacity": 0.0,
      "Scale": {
        "X": { "Min": 0.5, "Max": 0.8 },
        "Y": { "Min": 0.5, "Max": 0.8 }
      }
    }
  }
}
```

Keyframes are percentages (0-100) of particle lifetime.

## Particle Velocity

### Initial Velocity

```json
{
  "InitialVelocity": {
    "Speed": {
      "Min": 2.0,
      "Max": 5.0
    },
    "Yaw": {
      "Min": -180,
      "Max": 180
    },
    "Pitch": {
      "Min": -90,
      "Max": 90
    }
  }
}
```

- **`Speed`** - Particle speed
- **`Yaw`** - Horizontal angle (-180 to 180)
- **`Pitch`** - Vertical angle (-90 to 90)

### Emit Offset

```json
{
  "EmitOffset": {
    "X": { "Min": -0.5, "Max": 0.5 },
    "Y": { "Min": 0, "Max": 1.0 },
    "Z": { "Min": -0.5, "Max": 0.5 }
  }
}
```

Spawn area offset from source position.

## Particle Attractors

Attractors affect particle movement:

```json
{
  "Attractors": [
    {
      "LinearAcceleration": {
        "Y": -9.8
      }
    }
  ]
}
```

### Gravity

```json
{
  "LinearAcceleration": {
    "Y": -9.8
  }
}
```

Applies gravity (negative Y = down).

### Radial Attraction

```json
{
  "Attractors": [
    {
      "RadialAxis": {
        "X": 0,
        "Y": 1,
        "Z": 0
      },
      "RadialAcceleration": -3,
      "RadialTangentImpulse": -1.2,
      "Radius": 10
    }
  ]
}
```

Creates spiral/radial particle motion.

## Particle Systems

Particle systems combine multiple spawners:

Create `Server/Particles/MyCustom/MyCustom_Explosion.particlesystem`:

```json
{
  "Spawners": [
    {
      "SpawnerId": "MyCustom_Explosion_Sparks",
      "Delay": 0
    },
    {
      "SpawnerId": "MyCustom_Explosion_Smoke",
      "Delay": 0.1
    }
  ]
}
```

### System Properties

- **`Spawners`** - Array of spawner references
- **`SpawnerId`** - ID of particle spawner
- **`Delay`** - Delay before spawning (seconds)

## Using Particles

### In Items

```json
{
  "Particles": [
    {
      "SystemId": "MyCustom_Weapon_Effect",
      "TargetEntityPart": "PrimaryItem",
      "TargetNodeName": "Crystal1",
      "Scale": 1.0
    }
  ]
}
```

### In Status Effects

```json
{
  "ApplicationEffects": {
    "Particles": [
      {
        "SystemId": "Effect_Fire"
      }
    ]
  }
}
```

### In Interactions

```json
{
  "Effects": {
    "Particles": [
      {
        "SystemId": "Explosion_Medium",
        "TargetNodeName": "Handle"
      }
    ]
  }
}
```

## Common Particle Textures

Particle textures are in `Common/Particles/Textures/`:

- `Basic/Spark.png` - Spark particles
- `Basic/Glow_Square.png` - Glowing square
- `Fire/Ember.png` - Fire embers
- `Magic/Magic_Glow.png` - Magic effects

## Complete Example: Custom Fire Effect

### 1. Particle Spawner

`Server/Particles/MyCustom/Spawners/MyCustom_Fire.particlespawner`:

```json
{
  "RenderMode": "Erosion",
  "ParticleRotationInfluence": "Billboard",
  "MaxConcurrentParticles": 50,
  "SpawnRate": {
    "Min": 20,
    "Max": 30
  },
  "ParticleLifeSpan": {
    "Min": 0.5,
    "Max": 1.5
  },
  "EmitOffset": {
    "X": { "Min": -0.2, "Max": 0.2 },
    "Y": { "Min": 0, "Max": 0.5 },
    "Z": { "Min": -0.2, "Max": 0.2 }
  },
  "InitialVelocity": {
    "Speed": {
      "Min": 0.5,
      "Max": 2.0
    },
    "Pitch": {
      "Min": 70,
      "Max": 90
    }
  },
  "Attractors": [
    {
      "LinearAcceleration": {
        "Y": 2.0
      }
    }
  ],
  "Particle": {
    "Texture": "Particles/Textures/Fire/Ember.png",
    "FrameSize": {
      "Width": 32,
      "Height": 32
    },
    "InitialAnimationFrame": {
      "Opacity": 1.0,
      "Color": "#ffff00",
      "Scale": {
        "X": { "Min": 0.2, "Max": 0.4 },
        "Y": { "Min": 0.2, "Max": 0.4 }
      }
    },
    "Animation": {
      "0": {
        "Color": "#ffff00"
      },
      "50": {
        "Color": "#ff8800"
      },
      "100": {
        "Color": "#ff0000",
        "Opacity": 0.0,
        "Scale": {
          "X": { "Min": 0.1, "Max": 0.2 },
          "Y": { "Min": 0.1, "Max": 0.2 }
        }
      }
    }
  }
}
```

### 2. Particle System

`Server/Particles/MyCustom/MyCustom_Fire_Effect.particlesystem`:

```json
{
  "Spawners": [
    {
      "SpawnerId": "MyCustom_Fire",
      "Delay": 0
    }
  ]
}
```

### 3. Using in Item

```json
{
  "Particles": [
    {
      "SystemId": "MyCustom_Fire_Effect",
      "TargetEntityPart": "PrimaryItem"
    }
  ]
}
```

## Tips for Creating Particles

1. **Use appropriate textures** - Match particle appearance to effect
2. **Balance spawn rates** - Too many particles impact performance
3. **Set life spans** - Particles should fade out naturally
4. **Animate properties** - Use keyframes for smooth transitions
5. **Test in-game** - Visual effects look different in engine
6. **Use systems for complex effects** - Combine multiple spawners
7. **Optimize particle count** - Keep `MaxConcurrentParticles` reasonable

---

**Previous:** [Music](31_Music.md) | **Next:** Back to [README](../README.md)
