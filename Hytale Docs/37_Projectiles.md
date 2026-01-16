# Projectiles

Learn how to create and configure projectiles for weapons, spells, and abilities.

## Overview

Projectiles in Hytale are entities that travel through the world. They're used for arrows, bolts, magic spells, and other ranged attacks. Projectiles are defined in `Server/Projectiles/` and `Server/Models/Projectiles/`.

## Location
- Projectile definitions: `Server/Projectiles/`
- Projectile models: `Server/Models/Projectiles/`
- Projectile configs: `Server/ProjectileConfigs/`

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

## Tips for Creating Projectiles

1. **Set appropriate velocity** - Balance speed with accuracy
2. **Configure gravity** - Realistic gravity for arrows, none for magic
3. **Add trails** - Visual feedback for projectile path
4. **Set hit box size** - Match to projectile model
5. **Configure accuracy** - Balance between precision and difficulty
6. **Add effects** - Sounds and particles enhance feel
7. **Test flight path** - Ensure projectiles behave correctly

---

**Previous:** [Resource Types](36_Resource_Types.md) | **Next:** [World Generation](38_World_Generation.md)
