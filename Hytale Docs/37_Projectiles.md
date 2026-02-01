# Projectiles

Learn how to create and configure projectiles for weapons, spells, and abilities.

## Overview

Projectiles in Hytale are entities that travel through the world. They're used for arrows, bolts, magic spells, and other ranged attacks. Projectiles are defined in `Server/Projectiles/` and `Server/Models/Projectiles/`.

## Location
- Projectile definitions: `Server/Projectiles/`
- Projectile models: `Server/Models/Projectiles/`
- Projectile configs: `Server/ProjectileConfigs/`

## Example from Game Files

### Arrow Projectile

From `Server/Projectiles/Arrow_NoCharge.json`:

```1:21:Server/Projectiles/Arrow_NoCharge.json
{
  "Appearance": "Arrow_Crude",
  "SticksVertically": true,
  "MuzzleVelocity": 10,
  "TerminalVelocity": 50,
  "Gravity": 20,
  "Bounciness": 0,
  "ImpactSlowdown": 0,
  "TimeToLive": 20,
  "Damage": 4,
  "DeadTime": 0.1,
  "HorizontalCenterShot": 0.1,
  "DepthShot": 1,
  "PitchAdjustShot": true,
  "VerticalCenterShot": 0.1,
  "HitSoundEventId": "SFX_Arrow_NoCharge_Hit",
  "MissSoundEventId": "SFX_Arrow_NoCharge_Miss",
  "HitParticles": {
    "SystemId": "Impact_Blade_01"
  }
}
```

This shows a complete arrow projectile with physics, damage, sound effects, and particles.

## Basic Projectile Structure

Create `Server/Projectiles/Arrow/Arrow_MyCustom.json`:

```json
{
  "Appearance": "Arrow_MyCustom",
  "SticksVertically": true,
  "MuzzleVelocity": 10,
  "TerminalVelocity": 50,
  "Gravity": 20,
  "Bounciness": 0,
  "ImpactSlowdown": 0,
  "TimeToLive": 20,
  "Damage": 4,
  "DeadTime": 0.1,
  "HorizontalCenterShot": 0.1,
  "DepthShot": 1,
  "PitchAdjustShot": true,
  "VerticalCenterShot": 0.1,
  "HitSoundEventId": "SFX_Arrow_NoCharge_Hit",
  "MissSoundEventId": "SFX_Arrow_NoCharge_Miss",
  "HitParticles": {
    "SystemId": "Impact_Blade_01"
  }
}
```

## Projectile Properties

### Appearance

```json
{
  "Appearance": "Arrow_MyCustom"
}
```

References the projectile model ID in `Server/Models/Projectiles/`.

### Velocity

```json
{
  "MuzzleVelocity": 10,
  "TerminalVelocity": 50
}
```

- **`MuzzleVelocity`** - Initial speed
- **`TerminalVelocity`** - Maximum speed

### Physics

```json
{
  "Gravity": 20,
  "Bounciness": 0,
  "ImpactSlowdown": 0
}
```

- **`Gravity`** - Gravity strength (0 = no gravity)
- **`Bounciness`** - Bounce on impact (0-1)
- **`ImpactSlowdown`** - Speed reduction on impact

### Behavior

```json
{
  "SticksVertically": true,
  "TimeToLive": 20,
  "DeadTime": 0.1
}
```

- **`SticksVertically`** - Arrow sticks vertically when hitting ground
- **`TimeToLive`** - Lifetime in seconds
- **`DeadTime`** - Delay before despawning

### Accuracy

```json
{
  "HorizontalCenterShot": 0.1,
  "DepthShot": 1,
  "PitchAdjustShot": true,
  "VerticalCenterShot": 0.1
}
```

- **`HorizontalCenterShot`** - Horizontal accuracy offset
- **`DepthShot`** - Depth accuracy
- **`PitchAdjustShot`** - Adjust pitch for accuracy
- **`VerticalCenterShot`** - Vertical accuracy offset

### Damage

```json
{
  "Damage": 4
}
```

Direct damage value. For modifiers, use `DamageCalculator` in projectile config's `ProjectileHit` interaction.

### Effects

```json
{
  "HitSoundEventId": "SFX_Arrow_NoCharge_Hit",
  "MissSoundEventId": "SFX_Arrow_NoCharge_Miss",
  "HitParticles": {
    "SystemId": "Impact_Blade_01"
  }
}
```

- **`HitSoundEventId`** - Sound when hitting target
- **`MissSoundEventId`** - Sound when missing
- **`HitParticles`** - Particle effect on hit

## Projectile Model

Create `Server/Models/Projectiles/Weapons/Arrow/Arrow_MyCustom.json`:

```json
{
  "HitBox": {
    "Max": {
      "X": 0.075,
      "Y": 0.075,
      "Z": 0.075
    },
    "Min": {
      "X": -0.075,
      "Y": -0.075,
      "Z": -0.075
    }
  },
  "MinScale": 1,
  "MaxScale": 1,
  "Trails": [
    {
      "PositionOffset": {
        "X": 0.5,
        "Y": 0,
        "Z": 0
      },
      "TargetNodeName": "Handle",
      "TrailId": "Arrow",
      "RotationOffset": {
        "Yaw": 90,
        "Pitch": 0,
        "Roll": 45
      }
    }
  ],
  "DefaultAttachments": [
    {
      "Model": "Items/Weapons/Arrow/Arrow.blockymodel",
      "Texture": "Items/Weapons/Arrow/Arrow_MyCustom.png"
    }
  ],
  "Model": "Items/Projectiles/Projectile.blockymodel",
  "Texture": "Items/Projectiles/Projectile_default.png",
  "AnimationSets": {
    "Idle": {
      "Animations": [
        {
          "Animation": "Items/Animations/Fly/Fly_Vertical.blockyanim"
        }
      ]
    }
  }
}
```

## Projectile Model Properties

### Hit Box

```json
{
  "HitBox": {
    "Max": { "X": 0.075, "Y": 0.075, "Z": 0.075 },
    "Min": { "X": -0.075, "Y": -0.075, "Z": -0.075 }
  }
}
```

Collision detection box.

### Scale

```json
{
  "MinScale": 1,
  "MaxScale": 1
}
```

Size variation (same value = fixed size).

### Trails

```json
{
  "Trails": [
    {
      "PositionOffset": { "X": 0.5, "Y": 0, "Z": 0 },
      "TargetNodeName": "Handle",
      "TrailId": "Arrow",
      "RotationOffset": {
        "Yaw": 90,
        "Pitch": 0,
        "Roll": 45
      }
    }
  ]
}
```

Visual trails following the projectile. See [Trails](33_Trails.md) for details.

### Attachments

```json
{
  "DefaultAttachments": [
    {
      "Model": "Items/Weapons/Arrow/Arrow.blockymodel",
      "Texture": "Items/Weapons/Arrow/Arrow_MyCustom.png"
    }
  ]
}
```

3D model attachments for the projectile.

### Animations

```json
{
  "AnimationSets": {
    "Idle": {
      "Animations": [
        {
          "Animation": "Items/Animations/Fly/Fly_Vertical.blockyanim"
        }
      ]
    }
  }
}
```

Animations while in flight.

## Using Projectiles in Weapons

### Crossbow Projectile

```json
{
  "InteractionVars": {
    "Shoot": {
      "Interactions": [
        {
          "Type": "LaunchProjectile",
          "ProjectileId": "Arrow_MyCustom"
        }
      ]
    }
  }
}
```

### Spell Projectile

```json
{
  "InteractionVars": {
    "Cast": {
      "Interactions": [
        {
          "Type": "LaunchProjectile",
          "ProjectileId": "Fireball_MyCustom"
        }
      ]
    }
  }
}
```

## Projectile Types

### Arrows

Standard arrows for bows/crossbows:
- **`Arrow_Iron`** - Iron arrow
- **`Arrow_Steel`** - Steel arrow
- **`Arrow_Bone`** - Bone arrow

### Bolts

Crossbow bolts (similar to arrows):
- **`Bolt_Iron`** - Iron bolt
- **`Bolt_Steel`** - Steel bolt

### Magic Projectiles

Spell projectiles:
- **`Fireball`** - Fire spell
- **`Icebolt`** - Ice spell
- **`Lightning`** - Lightning spell

### Thrown Weapons

Thrown weapons:
- **`Spear`** - Thrown spear
- **`Knife`** - Thrown knife

## Complete Example: Custom Arrow

### 1. Projectile Definition

`Server/Projectiles/Arrow/Arrow_MyCustom.json`:

```json
{
  "Appearance": "Arrow_MyCustom",
  "SticksVertically": true,
  "MuzzleVelocity": 12,
  "TerminalVelocity": 60,
  "Gravity": 20,
  "Bounciness": 0,
  "ImpactSlowdown": 0,
  "TimeToLive": 25,
  "Damage": 6,
  "DeadTime": 0.1,
  "HorizontalCenterShot": 0.08,
  "DepthShot": 1,
  "PitchAdjustShot": true,
  "VerticalCenterShot": 0.08,
  "HitSoundEventId": "SFX_Arrow_NoCharge_Hit",
  "MissSoundEventId": "SFX_Arrow_NoCharge_Miss",
  "HitParticles": {
    "SystemId": "Impact_Blade_01"
  }
}
```

### 2. Projectile Model

`Server/Models/Projectiles/Weapons/Arrow/Arrow_MyCustom.json`:

```json
{
  "HitBox": {
    "Max": { "X": 0.075, "Y": 0.075, "Z": 0.075 },
    "Min": { "X": -0.075, "Y": -0.075, "Z": -0.075 }
  },
  "Trails": [
    {
      "PositionOffset": { "X": 0.5, "Y": 0, "Z": 0 },
      "TargetNodeName": "Handle",
      "TrailId": "Arrow",
      "RotationOffset": {
        "Yaw": 90,
        "Pitch": 0,
        "Roll": 45
      }
    }
  ],
  "DefaultAttachments": [
    {
      "Model": "Items/Weapons/Arrow/Arrow.blockymodel",
      "Texture": "Items/Weapons/Arrow/Arrow_MyCustom.png"
    }
  ],
  "Model": "Items/Projectiles/Projectile.blockymodel",
  "Texture": "Items/Projectiles/Projectile_default.png",
  "AnimationSets": {
    "Idle": {
      "Animations": [
        {
          "Animation": "Items/Animations/Fly/Fly_Vertical.blockyanim"
        }
      ]
    }
  }
}
```

### 3. Using in Weapon

```json
{
  "InteractionVars": {
    "Shoot": {
      "Interactions": [
        {
          "Type": "LaunchProjectile",
          "ProjectileId": "Arrow_MyCustom"
        }
      ]
    }
  }
}
```

## Damage Modifiers in Projectiles

### Direct Damage

Simple projectiles use direct damage:
```json
{
  "Damage": 4
}
```

### Damage with Modifiers

To add modifiers (random variance, multiple damage types), use `DamageCalculator` in projectile config's `ProjectileHit` interaction:

`Server/ProjectileConfigs/Weapons/Bows/Projectile_Config_MyCustom.json`:
```json
{
  "Interactions": {
    "ProjectileHit": {
      "Interactions": [
        {
          "Parent": "DamageEntityParent",
          "DamageCalculator": {
            "Type": "Absolute",
            "BaseDamage": {
              "Projectile": 6
            },
            "RandomPercentageModifier": 0.1
          }
        }
      ]
    }
  }
}
```

**DamageCalculator Properties:**
- **`Type`**: `"Absolute"` (fixed damage) or `"Dps"` (damage per second)
- **`BaseDamage`**: Base damage values (should match `Damage` from projectile if converting)
- **`RandomPercentageModifier`**: Random variance (0.1 = ±10% damage variation)

**Example:**
```json
{
  "DamageCalculator": {
    "Type": "Absolute",
    "BaseDamage": {
      "Projectile": 6
    },
    "RandomPercentageModifier": 0.15
  }
}
```

With `RandomPercentageModifier: 0.15`, damage of 6 can vary between 5.1 and 6.9 (85% to 115%).

---

## ProjectileConfigs (Advanced)

ProjectileConfigs define weapon-specific projectile behavior, interactions, and physics.

### Location
`Server/ProjectileConfigs/`

### Full ProjectileConfig Structure

```json
{
  "Parent": "Projectile_Config_Arrow_Base",
  "Model": "Arrow_Iron",
  "SpawnOffset": { "X": 0, "Y": 0.5, "Z": 0 },
  "SpawnRotationOffset": { "Pitch": 0, "Yaw": 0, "Roll": 0 },
  "Physics": {
    "Type": "Standard",
    "Gravity": 20,
    "TerminalVelocityAir": 50,
    "TerminalVelocityWater": 10,
    "RotationMode": "VelocityDamped",
    "Bounciness": 0.3,
    "BounceLimit": 5,
    "BounceCount": 3,
    "AllowRolling": true,
    "RollingFrictionFactor": 0.5,
    "SticksVertically": true
  },
  "LaunchForce": 35,
  "LaunchLocalSoundEventId": "SFX_Bow_Release",
  "LaunchWorldSoundEventId": "SFX_Bow_Release_World",
  "Interactions": {
    "ProjectileSpawn": { ... },
    "ProjectileHit": { ... },
    "ProjectileMiss": { ... },
    "ProjectileBounce": { ... }
  }
}
```

### Physics Properties

| Property | Type | Description |
|----------|------|-------------|
| `Type` | String | Usually `"Standard"` |
| `Gravity` | Float | Gravity force |
| `TerminalVelocityAir` | Float | Max velocity in air |
| `TerminalVelocityWater` | Float | Max velocity in water |
| `RotationMode` | String | `"VelocityDamped"`, `"VelocityRoll"` |
| `Bounciness` | Float | Bounce coefficient (0-1) |
| `BounceLimit` | Float | Velocity threshold for bounce |
| `BounceCount` | Int | Maximum bounces |
| `AllowRolling` | Bool | Can roll on ground |
| `RollingFrictionFactor` | Float | Friction when rolling |
| `SticksVertically` | Bool | Stick to surfaces |

### Projectile Interactions

#### ProjectileHit (Entity Hit)

```json
{
  "Interactions": {
    "ProjectileHit": {
      "Cooldown": { "Cooldown": 0.1 },
      "Interactions": [
        {
          "Parent": "DamageEntityParent",
          "DamageCalculator": {
            "Class": "Charged",
            "BaseDamage": { "Physical": 8, "Ice": 4 }
          },
          "DamageEffects": {
            "Knockback": {
              "Type": "Point",
              "Force": 5,
              "VelocityType": "Add"
            },
            "WorldParticles": [
              { "SystemId": "Impact_Frost" }
            ],
            "WorldSoundEventId": "SFX_Arrow_Hit"
          }
        }
      ]
    }
  }
}
```

#### ProjectileMiss (Block/Ground Hit)

```json
{
  "Interactions": {
    "ProjectileMiss": {
      "Interactions": [
        {
          "Type": "Simple",
          "RunTime": 0,
          "Effects": {
            "WorldSoundEventId": "SFX_Arrow_Miss",
            "WorldParticles": [
              { "SystemId": "Impact_Dirt" }
            ]
          }
        }
      ]
    }
  }
}
```

#### ProjectileBounce

```json
{
  "Interactions": {
    "ProjectileBounce": {
      "Interactions": [
        {
          "Type": "Simple",
          "Effects": {
            "WorldSoundEventId": "SFX_Bounce"
          }
        }
      ]
    }
  }
}
```

### Knockback Configuration

```json
{
  "Knockback": {
    "Type": "Point",
    "Force": 8,
    "Direction": { "X": 0, "Y": 0.3, "Z": 1 },
    "VelocityConfig": {
      "AirResistance": 0.1,
      "GroundResistance": 0.5,
      "Threshold": 0.1,
      "Style": "Exp"
    },
    "VelocityType": "Set"
  }
}
```

| Property | Description |
|----------|-------------|
| `Type` | `"Point"` (radial) or `"Force"` (directional) |
| `Force` | Knockback strength |
| `Direction` | Direction vector (for Force type) |
| `VelocityType` | `"Set"` (replace) or `"Add"` (additive) |

---

## NPC Projectiles

NPCs use specialized projectiles in `Server/Projectiles/NPCs/`.

### Organization

```
Server/Projectiles/NPCs/
├── Beast/
│   ├── Scarak_Seeker/Scarak_Seeker_Spitball.json
│   └── ...
├── Intelligent/
│   ├── Goblin_Duke/Goblin_Duke_Fire.json
│   ├── Goblin_Lobber/Goblin_Lobber_Bomb.json
│   └── ...
└── Undead/
    ├── Skeleton_Archer/Skeleton_Archer_Arrow.json
    └── ...
```

### NPC Projectile Example

`Server/Projectiles/NPCs/Intelligent/Goblin_Duke/Goblin_Duke_Fire.json`:

```json
{
  "Parent": "Projectile_Base",
  "Appearance": "Goblin_Fire",
  "MuzzleVelocity": 15,
  "TerminalVelocity": 30,
  "Gravity": 5,
  "TimeToLive": 10,
  "Damage": 12,
  "HitParticles": { "SystemId": "Explosion_Fire" },
  "DeathEffectsOnHit": true
}
```

### Explosion Projectiles

For bombs and explosive projectiles:

```json
{
  "ExplosionConfig": {
    "DamageEntities": true,
    "DamageBlocks": true,
    "BlockDamageRadius": 3,
    "EntityDamageRadius": 5,
    "EntityDamageFalloff": 0.5,
    "Knockback": {
      "Type": "Point",
      "Force": 10,
      "VelocityType": "Add"
    }
  }
}
```

---

## Spell Projectiles

Magic projectiles in `Server/Projectiles/Spells/`:

```json
{
  "Appearance": "Fireball",
  "MuzzleVelocity": 20,
  "TerminalVelocity": 20,
  "Gravity": 0,
  "Bounciness": 0,
  "TimeToLive": 5,
  "Damage": 15,
  "HitParticles": { "SystemId": "Explosion_Fire_Medium" },
  "MissParticles": { "SystemId": "Explosion_Fire_Small" },
  "DeathEffectsOnHit": true,
  "ExplosionConfig": {
    "DamageEntities": true,
    "EntityDamageRadius": 3,
    "EntityDamageFalloff": 0.3
  }
}
```

---

## Charged Projectiles

Bows can have charge levels affecting damage:

```json
{
  "DamageCalculator": {
    "Class": "Charged",
    "BaseDamage": { "Physical": 4 }
  }
}
```

The `Charged` class scales damage based on bow charge time.

---

## Tips for Creating Projectiles

1. **Set appropriate velocity** - Balance speed with accuracy
2. **Configure gravity** - Realistic gravity for arrows, none for magic
3. **Add trails** - Visual feedback for projectile path
4. **Set hit box size** - Match to projectile model
5. **Configure accuracy** - Balance between precision and difficulty
6. **Add effects** - Sounds and particles enhance feel
7. **Test flight path** - Ensure projectiles behave correctly
8. **Use ProjectileConfigs** - For complex interactions and damage
9. **Knockback** - Use Point for radial, Force for directional
10. **Explosions** - Configure radius and falloff for AoE

---

**Previous:** [Resource Types](36_Resource_Types.md) | **Next:** [World Generation](38_World_Generation.md)
