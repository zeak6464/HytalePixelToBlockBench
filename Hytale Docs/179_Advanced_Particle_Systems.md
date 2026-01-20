# Advanced Particle Systems Guide

Master particle systems in Hytale: complex effects, multi-spawner systems, advanced techniques, and optimization.

## Overview

This guide covers advanced particle system techniques used in Hytale's most impressive effects:
- Multi-spawner particle systems (explosions, sword trails, magic)
- Advanced render modes and blending
- Complex animation keyframes
- Physics and attractors
- Performance optimization
- Complete real-world examples

All examples are from actual game files.

---

## Particle System Architecture

### Two-File System

Hytale particle effects use two file types:

1. **`.particlespawner`** - Defines HOW particles spawn, move, and animate
2. **`.particlesystem`** - Combines multiple spawners into complete effects

**Example:**

```
Explosion_Medium.particlesystem           ← Main system file
├── Explosion_Medium_Flash.particlespawner
├── Explosion_Medium_Sparks_Out.particlespawner
├── Explosion_Medium_Smoke_Main.particlespawner
└── Explosion_Medium_Fires.particlespawner
```

---

## Complete Explosion Example

From `Explosion_Medium.particlesystem`:

### Particle System File

```json
{
  "Spawners": [
    {
      "SpawnerId": "Explosion_Medium_Flash",
      "PositionOffset": {
        "Y": 0.5
      },
      "FixedRotation": false,
      "LifeSpan": {
        "Min": 0.0,
        "Max": 0.0
      }
    },
    {
      "SpawnerId": "Explosio_Medium_Flash_Glow",
      "PositionOffset": {
        "Y": 0.5
      }
    },
    {
      "SpawnerId": "Explosion_Medium_Shockwave",
      "PositionOffset": {
        "Y": 0.5
      },
      "FixedRotation": false
    },
    {
      "SpawnerId": "Explosion_Medium_Sparks_Out",
      "PositionOffset": {
        "Y": 0.5
      },
      "FixedRotation": false
    },
    {
      "SpawnerId": "Explosion_Medium_Smoke_Main",
      "PositionOffset": {
        "Y": 0.5
      },
      "StartDelay": 0.0
    },
    {
      "SpawnerId": "Explosion_Medium_Fires",
      "PositionOffset": {
        "Y": 0.5
      }
    },
    {
      "SpawnerId": "Explosion_Medium_Glow",
      "PositionOffset": {
        "Y": 0.5
      }
    },
    {
      "SpawnerId": "Explosion_Medium_Smoke_Side",
      "PositionOffset": {
        "Y": 0.5,
        "Z": -0.2
      },
      "StartDelay": 0.0
    },
    {
      "PositionOffset": {
        "Y": 0.5,
        "Z": 0.2
      },
      "RotationOffset": {
        "Yaw": 180.0
      },
      "StartDelay": 0.0,
      "SpawnerId": "Explosion_Medium_Smoke_Side"
    }
  ],
  "BoundingRadius": 100.0,
  "CullDistance": 100.0,
  "IsImportant": false
}
```

**Layer breakdown:**
1. **Flash** - Initial bright flash (instant)
2. **Flash Glow** - Glowing core
3. **Shockwave** - Expanding ring
4. **Sparks Out** - Flying particles
5. **Smoke Main** - Rising smoke cloud
6. **Fires** - Lingering fire particles
7. **Glow** - Ambient glow
8. **Smoke Side** (×2) - Side smoke puffs

---

### Advanced Spawner: Flash Effect

From `Explosion_Medium_Flash.particlespawner`:

```json
{
  "RenderMode": "BlendAdd",
  "ParticleRotationInfluence": "Billboard",
  "LinearFiltering": true,
  "LightInfluence": 0,
  "MaxConcurrentParticles": 1,
  "ParticleLifeSpan": {
    "Min": 0.1,
    "Max": 0.12
  },
  "TotalParticles": {
    "Min": 1,
    "Max": 1
  },
  "InitialVelocity": {
    "Speed": {
      "Min": 0,
      "Max": 0
    }
  },
  "Particle": {
    "Texture": "Particles/Textures/Basic/Star.png",
    "ScaleRatioConstraint": "OneToOne",
    "Animation": {
      "0": {
        "Scale": {
          "X": {
            "Min": 0.0,
            "Max": 0.0
          }
        },
        "Opacity": 0.0
      },
      "5": {
        "Opacity": 1.0
      },
      "13": {
        "Scale": {
          "X": {
            "Min": 1.0,
            "Max": 1.0
          }
        }
      },
      "50": {
        "Opacity": 1.0
      },
      "100": {
        "Opacity": 0,
        "Scale": {
          "X": {
            "Min": 2.0,
            "Max": 2.0
          }
        },
        "Color": "#550000"
      }
    },
    "InitialAnimationFrame": {
      "Opacity": 1.0,
      "Color": "#ffe435",
      "Scale": {
        "X": {
          "Min": 0.45,
          "Max": 0.55
        }
      },
      "Rotation": {
        "Z": {
          "Min": -360.0,
          "Max": 360.0
        }
      }
    },
    "SoftParticles": "Enable"
  },
  "SpawnBurst": true,
  "CameraOffset": -0.48
}
```

**Effect timeline:**
- **0%**: Invisible, scale 0
- **5%**: Fade in (bright yellow)
- **13%**: Scale to full size
- **50%**: Hold brightness
- **100%**: Fade out, expand 2×, darken to red

---

### Advanced Spawner: Flying Sparks

From `Explosion_Medium_Sparks_Out.particlespawner`:

```json
{
  "RenderMode": "Erosion",
  "ParticleRotationInfluence": "BillboardVelocity",
  "LinearFiltering": false,
  "LightInfluence": 0,
  "ParticleRotateWithSpawner": true,
  "MaxConcurrentParticles": 50,
  "ParticleLifeSpan": {
    "Min": 0.2,
    "Max": 0.3
  },
  "SpawnRate": {
    "Min": 120.0,
    "Max": 120.0
  },
  "InitialVelocity": {
    "Speed": {
      "Max": 15.0,
      "Min": 5.0
    },
    "Yaw": {
      "Min": -180,
      "Max": 180
    },
    "Pitch": {
      "Min": -180.0,
      "Max": 180.0
    }
  },
  "Attractors": [
    {
      "DampingMultiplier": {
        "Y": 0.85,
        "Z": 0.85,
        "X": 0.85
      }
    }
  ],
  "Particle": {
    "Texture": "Particles/Textures/Basic/Glow_Direction.png",
    "ScaleRatioConstraint": "None",
    "Animation": {
      "0": {
        "Scale": {
          "X": {
            "Min": 0.5,
            "Max": 0.5
          },
          "Y": {
            "Min": 1.0,
            "Max": 1.0
          }
        },
        "Color": "#fffce9",
        "Opacity": 1.0
      },
      "10": {
        "Color": "#ffeeb6"
      },
      "20": {
        "Scale": {
          "X": {
            "Min": 0.3,
            "Max": 0.3
          },
          "Y": {
            "Min": 3.0,
            "Max": 4.0
          }
        }
      },
      "50": {
        "Color": "#fed243"
      },
      "100": {
        "Opacity": 0.0,
        "Scale": {
          "Y": {
            "Min": 1.0,
            "Max": 2.0
          }
        },
        "Color": "#fe9c43"
      }
    },
    "UVOption": "RandomFlipU",
    "InitialAnimationFrame": {
      "Scale": {
        "X": {
          "Min": 0.15,
          "Max": 0.25
        },
        "Y": {
          "Min": 0.2,
          "Max": 0.25
        }
      },
      "Opacity": 1,
      "Color": "#ffffff"
    }
  },
  "SpawnBurst": true,
  "TotalParticles": {
    "Min": 10,
    "Max": 15
  },
  "Shape": "Sphere"
}
```

**Advanced techniques used:**
- **BillboardVelocity** - Particles stretch in direction of movement (motion blur)
- **Damping** - 0.85 multiplier slows particles over time (air resistance)
- **Asymmetric Scale** - X: 0.5, Y: 3-4 (creates streak effect)
- **Color gradient** - Yellow → Orange over lifetime
- **RandomFlipU** - Random horizontal flip for variety

---

## Render Modes

### BlendAdd

```json
{
  "RenderMode": "BlendAdd"
}
```

**Effect:** Additive blending - particles lighten what's behind them  
**Use for:** Fire, explosions, glowing effects, magic, energy

**Visual:**
```
Particle + Background = Brighter
```

---

### Erosion

```json
{
  "RenderMode": "Erosion"
}
```

**Effect:** Standard alpha blending  
**Use for:** Smoke, dust, clouds, general particles

**Visual:**
```
Particle replaces background based on opacity
```

---

### Comparison

| Render Mode | Blending | Best For | Example |
|-------------|----------|----------|---------|
| **BlendAdd** | Additive (brightens) | Fire, magic, energy, explosions | Flash effects, glowing orbs |
| **Erosion** | Alpha (standard) | Smoke, dust, debris, water | Smoke clouds, flying sparks |

---

## Particle Rotation Modes

### Billboard

```json
{
  "ParticleRotationInfluence": "Billboard"
}
```

**Effect:** Particles always face camera (2D sprites in 3D space)  
**Use for:** Most effects, explosions, magic, UI-like particles

---

### BillboardVelocity

```json
{
  "ParticleRotationInfluence": "BillboardVelocity"
}
```

**Effect:** Particles align with movement direction (motion blur/streaks)  
**Use for:** Fast-moving particles, sparks, trails, projectiles

**Example:** Explosion sparks stretch in direction they're flying

---

### Fixed Rotation

```json
{
  "ParticleRotateWithSpawner": true,
  "FixedRotation": false
}
```

**Effect:** Particles follow spawner rotation  
**Use for:** Attached effects, weapon trails, entity-based particles

---

## Advanced Animation Techniques

### Multi-Stage Fade

```json
{
  "Animation": {
    "0": {
      "Opacity": 0.0
    },
    "10": {
      "Opacity": 1.0
    },
    "90": {
      "Opacity": 1.0
    },
    "100": {
      "Opacity": 0.0
    }
  }
}
```

**Effect:**
- 0-10%: Fade in
- 10-90%: Hold full opacity
- 90-100%: Fade out

**Use for:** Particles that need to be visible for most of their life

---

### Scale + Fade Combined

```json
{
  "Animation": {
    "0": {
      "Opacity": 1.0,
      "Scale": {
        "X": { "Min": 0.1, "Max": 0.1 }
      }
    },
    "50": {
      "Scale": {
        "X": { "Min": 1.0, "Max": 1.0 }
      }
    },
    "100": {
      "Opacity": 0.0,
      "Scale": {
        "X": { "Min": 2.0, "Max": 2.0 }
      }
    }
  }
}
```

**Effect:** Small → Large → Huge while fading out  
**Use for:** Explosions, expanding rings, growing effects

---

### Color Transitions

```json
{
  "Animation": {
    "0": {
      "Color": "#ffff00"
    },
    "33": {
      "Color": "#ff8800"
    },
    "66": {
      "Color": "#ff0000"
    },
    "100": {
      "Color": "#330000"
    }
  }
}
```

**Effect:** Yellow → Orange → Red → Dark red  
**Use for:** Fire, heat effects, energy dissipation

---

### Asymmetric Scaling (Streaks)

```json
{
  "Animation": {
    "0": {
      "Scale": {
        "X": { "Min": 0.2, "Max": 0.2 },
        "Y": { "Min": 0.5, "Max": 0.5 }
      }
    },
    "50": {
      "Scale": {
        "X": { "Min": 0.1, "Max": 0.1 },
        "Y": { "Min": 3.0, "Max": 4.0 }
      }
    },
    "100": {
      "Scale": {
        "X": { "Min": 0.05, "Max": 0.05 },
        "Y": { "Min": 1.0, "Max": 1.0 }
      }
    }
  }
}
```

**Effect:** Creates motion streaks (thin → long → short)  
**Use for:** Speed lines, sparks, energy trails

---

## Advanced Physics: Attractors

### Gravity

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

**Effect:** Standard gravity (9.8 m/s² down)  
**Use for:** Falling debris, rain, realistic physics

---

### Damping (Air Resistance)

```json
{
  "Attractors": [
    {
      "DampingMultiplier": {
        "X": 0.85,
        "Y": 0.85,
        "Z": 0.85
      }
    }
  ]
}
```

**Effect:** Particles slow down over time (0.85 = 85% of velocity each frame)  
**Use for:** Explosion sparks, smoke settling, natural deceleration

**Values:**
- **1.0** = No damping
- **0.85** = Gentle slow-down
- **0.5** = Heavy damping
- **0.0** = Instant stop

---

### Gravity + Damping Combined

```json
{
  "Attractors": [
    {
      "LinearAcceleration": {
        "Y": -6.5
      },
      "DampingMultiplier": {
        "X": 0.8,
        "Y": 1.0,
        "Z": 0.8
      }
    }
  ]
}
```

**Effect:**
- Particles fall (gravity)
- Horizontal movement slows (X/Z damping 0.8)
- Vertical movement unaffected (Y damping 1.0)

**Use for:** Realistic debris, bouncing particles, natural physics

---

### Radial Spiral

```json
{
  "Attractors": [
    {
      "RadialAxis": {
        "Y": 1
      },
      "RadialAcceleration": -3,
      "RadialTangentAcceleration": -1.2,
      "Radius": 10
    }
  ]
}
```

**Effect:** Particles spiral inward around Y-axis  
**Use for:** Vortex, whirlpool, magic absorption, energy gathering

**Properties:**
- **RadialAxis** - Axis of rotation (Y = vertical)
- **RadialAcceleration** - Pull toward center (negative = inward)
- **RadialTangentAcceleration** - Spin speed
- **Radius** - Effect range

---

## Particle System Properties

### PositionOffset

```json
{
  "PositionOffset": {
    "X": 0.0,
    "Y": 0.5,
    "Z": 0.0
  }
}
```

**Effect:** Spawn particles offset from system origin  
**Use for:** Raising effect above ground, multi-layer effects

---

### RotationOffset

```json
{
  "RotationOffset": {
    "Yaw": 180.0
  }
}
```

**Effect:** Rotate spawner direction  
**Use for:** Mirrored effects, directional particles

**Example:** Explosion smoke puffs left and right:

```json
{
  "Spawners": [
    {
      "SpawnerId": "Explosion_Medium_Smoke_Side",
      "PositionOffset": {
        "Z": -0.2
      }
    },
    {
      "SpawnerId": "Explosion_Medium_Smoke_Side",
      "PositionOffset": {
        "Z": 0.2
      },
      "RotationOffset": {
        "Yaw": 180.0
      }
    }
  ]
}
```

**Result:** Two smoke puffs shoot in opposite directions

---

### StartDelay

```json
{
  "StartDelay": 0.3
}
```

**Effect:** Delay spawner activation (seconds)  
**Use for:** Sequenced effects, staggered particles

**Example:** Explosion sequence:
1. Flash (0.0s)
2. Sparks (0.0s)
3. Smoke (0.1s) ← Slightly delayed
4. Fires (0.2s) ← More delayed

---

### LifeSpan

```json
{
  "LifeSpan": {
    "Min": 1.0,
    "Max": 2.0
  }
}
```

**Effect:** How long spawner emits particles  
**Use for:** Burst effects vs continuous effects

**Examples:**
- **0.0** - Single burst (instant)
- **1.0** - Emit for 1 second
- **Infinite** - Continuous (looping effects)

---

### FixedRotation

```json
{
  "FixedRotation": false
}
```

**Effect:**
- **false** - Spawner rotates with parent (weapon swings, entity turns)
- **true** - Spawner stays fixed orientation

**Use for:**
- **false**: Weapon trails, entity effects
- **true**: Ground effects, fixed-direction particles

---

## Spawn Patterns

### Burst

```json
{
  "SpawnBurst": true,
  "TotalParticles": {
    "Min": 10,
    "Max": 15
  }
}
```

**Effect:** Spawn all particles instantly  
**Use for:** Explosions, impacts, one-time effects

---

### Continuous

```json
{
  "SpawnRate": {
    "Min": 20,
    "Max": 30
  },
  "MaxConcurrentParticles": 100
}
```

**Effect:** Steady stream of particles (20-30 per second)  
**Use for:** Fire, smoke, continuous effects

---

### Shape-Based Spawning

```json
{
  "Shape": "Sphere"
}
```

**Shapes:**
- **Sphere** - Particles spawn in spherical volume
- **Point** - Single point (default)
- Other shapes available

---

## Advanced Textures

### UV Options

```json
{
  "UVOption": "RandomFlipU"
}
```

**Options:**
- **None** - Standard texture
- **RandomFlipU** - Random horizontal flip
- **RandomFlipV** - Random vertical flip
- **RandomFlipUV** - Random both

**Use for:** Adding variety without multiple textures

---

### Frame Animation

```json
{
  "Particle": {
    "Texture": "Particles/Textures/Fire/Flames_Sheet.png",
    "FrameSize": {
      "Width": 64,
      "Height": 64
    },
    "Animation": {
      "0": {
        "FrameIndex": {
          "Min": 0,
          "Max": 0
        }
      },
      "50": {
        "FrameIndex": {
          "Min": 4,
          "Max": 4
        }
      },
      "100": {
        "FrameIndex": {
          "Min": 7,
          "Max": 7
        }
      }
    }
  }
}
```

**Effect:** Animate through sprite sheet frames  
**Use for:** Animated fire, explosions, complex effects

---

### Soft Particles

```json
{
  "SoftParticles": "Enable"
}
```

**Effect:** Smooth blending where particles intersect geometry  
**Use for:** All effects (prevents hard edges)

**Always enable** unless you have a specific reason not to.

---

## Performance Optimization

### 1. Particle Count Limits

```json
{
  "MaxConcurrentParticles": 50
}
```

**Guidelines:**
- **Subtle effects**: 10-30 particles
- **Medium effects**: 30-100 particles
- **Large effects**: 100-200 particles
- **Explosions**: 200-500 particles (short-lived)

**Warning:** Effects with 1000+ particles cause lag

---

### 2. Lifespan Balance

```json
{
  "ParticleLifeSpan": {
    "Min": 0.5,
    "Max": 1.5
  }
}
```

**Balance:**
- Short lifespan = More particles needed for visibility
- Long lifespan = Fewer particles, less overhead

**Sweet spot:** 0.5-2.0 seconds for most effects

---

### 3. Cull Distance

```json
{
  "CullDistance": 100.0,
  "BoundingRadius": 100.0
}
```

**Effect:** Don't render particles beyond distance  
**Use for:** All effects (prevents rendering off-screen particles)

**Recommended:**
- **Small effects** (sparks): 50-75
- **Medium effects** (explosions): 75-150
- **Large effects** (boss abilities): 150-250

---

### 4. IsImportant Flag

```json
{
  "IsImportant": false
}
```

**Effect:**
- **true** - Always render (high priority)
- **false** - Can be culled when many effects active

**Use:**
- **true**: Player abilities, boss attacks
- **false**: Environmental effects, ambient particles

---

### 5. Reduce Spawn Rate

```json
{
  "SpawnRate": {
    "Min": 20,
    "Max": 20
  }
}
```

**vs**

```json
{
  "SpawnRate": {
    "Min": 100,
    "Max": 100
  }
}
```

**Lower spawn rate** = Better performance  
**Compensate** with longer lifespan or larger particles

---

## Complete Custom Effect: Magic Orb

### 1. Core Glow Spawner

`Server/Particles/Magic/Spawners/Magic_Orb_Core.particlespawner`:

```json
{
  "RenderMode": "BlendAdd",
  "ParticleRotationInfluence": "Billboard",
  "LinearFiltering": true,
  "LightInfluence": 0,
  "MaxConcurrentParticles": 10,
  "SpawnRate": {
    "Min": 15,
    "Max": 20
  },
  "ParticleLifeSpan": {
    "Min": 0.8,
    "Max": 1.2
  },
  "EmitOffset": {
    "X": { "Min": -0.1, "Max": 0.1 },
    "Y": { "Min": -0.1, "Max": 0.1 },
    "Z": { "Min": -0.1, "Max": 0.1 }
  },
  "InitialVelocity": {
    "Speed": { "Min": 0, "Max": 0 }
  },
  "Particle": {
    "Texture": "Particles/Textures/Magic/Glow.png",
    "InitialAnimationFrame": {
      "Color": "#4488ff",
      "Opacity": 1.0,
      "Scale": {
        "X": { "Min": 0.3, "Max": 0.5 }
      },
      "Rotation": {
        "Z": { "Min": 0, "Max": 360 }
      }
    },
    "Animation": {
      "0": {
        "Opacity": 0.0,
        "Scale": {
          "X": { "Min": 0.1, "Max": 0.1 }
        }
      },
      "20": {
        "Opacity": 1.0
      },
      "80": {
        "Opacity": 1.0
      },
      "100": {
        "Opacity": 0.0,
        "Scale": {
          "X": { "Min": 0.8, "Max": 1.0 }
        },
        "Color": "#2244aa"
      }
    },
    "SoftParticles": "Enable"
  }
}
```

---

### 2. Orbiting Particles Spawner

`Server/Particles/Magic/Spawners/Magic_Orb_Orbits.particlespawner`:

```json
{
  "RenderMode": "BlendAdd",
  "ParticleRotationInfluence": "Billboard",
  "MaxConcurrentParticles": 20,
  "SpawnRate": {
    "Min": 10,
    "Max": 15
  },
  "ParticleLifeSpan": {
    "Min": 1.5,
    "Max": 2.0
  },
  "InitialVelocity": {
    "Speed": { "Min": 1, "Max": 2 },
    "Yaw": { "Min": -180, "Max": 180 },
    "Pitch": { "Min": -90, "Max": 90 }
  },
  "Attractors": [
    {
      "RadialAxis": {
        "Y": 1
      },
      "RadialAcceleration": -1.5,
      "RadialTangentAcceleration": 2.0,
      "Radius": 2.0
    }
  ],
  "Particle": {
    "Texture": "Particles/Textures/Basic/Spark.png",
    "InitialAnimationFrame": {
      "Color": "#88ccff",
      "Opacity": 1.0,
      "Scale": {
        "X": { "Min": 0.1, "Max": 0.15 }
      }
    },
    "Animation": {
      "0": {
        "Opacity": 0.0
      },
      "10": {
        "Opacity": 1.0
      },
      "90": {
        "Opacity": 1.0
      },
      "100": {
        "Opacity": 0.0,
        "Color": "#2266cc"
      }
    },
    "SoftParticles": "Enable"
  }
}
```

---

### 3. Particle System

`Server/Particles/Magic/Magic_Orb.particlesystem`:

```json
{
  "Spawners": [
    {
      "SpawnerId": "Magic_Orb_Core",
      "PositionOffset": {
        "Y": 0.0
      }
    },
    {
      "SpawnerId": "Magic_Orb_Orbits",
      "PositionOffset": {
        "Y": 0.0
      }
    }
  ],
  "BoundingRadius": 50.0,
  "CullDistance": 75.0,
  "IsImportant": false
}
```

---

### 4. Using in Item

```json
{
  "Particles": [
    {
      "SystemId": "Magic_Orb",
      "TargetEntityPart": "PrimaryItem",
      "TargetNodeName": "Crystal",
      "Scale": 1.0
    }
  ]
}
```

**Result:** Glowing blue orb with orbiting sparks attached to weapon's crystal node

---

## Best Practices Summary

### ✅ Do
1. **Use BlendAdd** for bright/glowing effects
2. **Use Erosion** for smoke/dust/clouds
3. **Enable SoftParticles** for smooth blending
4. **Set CullDistance** to save performance
5. **Use BillboardVelocity** for motion blur
6. **Combine multiple spawners** for complex effects
7. **Animate color + opacity + scale** together
8. **Add damping** for natural deceleration
9. **Test in-game** before finalizing

### ❌ Don't
1. **Spawn 1000+ particles** unless very short-lived
2. **Forget MaxConcurrentParticles** limit
3. **Make all effects IsImportant: true**
4. **Use identical particles** (add RandomFlipU for variety)
5. **Ignore performance** on low-end hardware
6. **Skip animation keyframes** (makes effects flat)
7. **Use only one spawner** for complex effects

---

## Common Effect Patterns

### Pattern 1: Explosion
- Flash (instant)
- Shockwave (expanding ring)
- Sparks (flying debris with damping)
- Smoke (delayed, rising with upward acceleration)
- Fire (lingering embers)

### Pattern 2: Magic Cast
- Core glow (pulsing)
- Orbiting particles (radial attractor)
- Rising sparkles (upward velocity)
- Energy trails (BillboardVelocity)

### Pattern 3: Impact
- Flash (instant)
- Sparks (burst, all directions)
- Debris (gravity + damping)
- Dust cloud (expanding, fading)

### Pattern 4: Trail
- Continuous emission
- BillboardVelocity for streaks
- Fade out at end of life
- Color shift over time

---

## Related Guides

- **[Particle Systems](32_Particle_Systems.md)** - Basic particle creation
- **[Trails](33_Trails.md)** - Visual trails for projectiles
- **[Audio & VFX](10_Audio_and_VFX.md)** - Sound and visual effects
- **[Advanced Techniques](176_Advanced_Techniques.md)** - Complex modding patterns

---

**Previous:** [Block Flags Reference](178_Block_Flags_Reference.md) | **Next:** [Advanced AI & NPC Behavior](180_Advanced_AI_NPC_Behavior.md)
