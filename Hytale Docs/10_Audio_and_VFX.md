# Sound Effects & Particle Effects

Learn how to add sound effects and visual effects to your mods.

## Sound Effects & Audio

### Overview

Sound events play audio for various game actions. You reference them in items, interactions, and effects.

### Location
`Server/Audio/SoundEvents/`

### Basic Sound Event

Create `Server/Audio/SoundEvents/SFX/Custom/SFX_MyCustom_Sound.json`:

```json
{
  "Sounds": [
    {
      "SoundPath": "Sounds/Custom/MySound.ogg",
      "Volume": 1.0,
      "Pitch": 1.0
    }
  ]
}
```

### Multiple Sound Variants

```json
{
  "Sounds": [
    {
      "SoundPath": "Sounds/Weapons/Sword/Impact_01.ogg",
      "Volume": 1.0
    },
    {
      "SoundPath": "Sounds/Weapons/Sword/Impact_02.ogg",
      "Volume": 1.0
    },
    {
      "SoundPath": "Sounds/Weapons/Sword/Impact_03.ogg",
      "Volume": 1.0
    }
  ]
}
```

### Sound Event Types

| Type | Location | Use Case |
|------|----------|----------|
| `WorldSoundEventId` | Heard by everyone nearby | Block breaking, explosions |
| `LocalSoundEventId` | Heard only by player | UI interactions, personal effects |
| `PlayerSoundEventId` | Player-specific sounds | Taking damage, eating |

### Using Sounds in Items

```json
{
  "ItemSoundSetId": "ISS_Weapons_Blade_Large",
  "InteractionVars": {
    "Swing_Left_Damage": {
      "Interactions": [
        {
          "DamageEffects": {
            "WorldSoundEventId": "SFX_Sword_Impact",
            "LocalSoundEventId": "SFX_Sword_Impact"
          }
        }
      ]
    }
  }
}
```

---

## Particle Effects & VFX

### Overview

Particle effects add visual flair to items, abilities, and status effects. They're referenced by `SystemId`.

### Using Particles

**In Items:**
```json
{
  "Particles": [
    {
      "SystemId": "Weapon_Frost_Mist",
      "TargetEntityPart": "PrimaryItem",
      "TargetNodeName": "Crystal1",
      "PositionOffset": {
        "Y": 0
      },
      "Scale": 1
    }
  ]
}
```

**In Status Effects:**
```json
{
  "ApplicationEffects": {
    "Particles": [
      {
        "SystemId": "Effect_Fire"
      },
      {
        "SystemId": "Effect_Poison"
      }
    ]
  }
}
```

**In Interactions:**
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

### Particle Properties

| Property | Description |
|----------|-------------|
| `SystemId` | Particle system ID (defined in game) |
| `TargetEntityPart` | Where to attach (`PrimaryItem`, `Entity`) |
| `TargetNodeName` | Specific node/bone to attach to |
| `PositionOffset` | Offset from target position |
| `RotationOffset` | Rotation offset |
| `Scale` | Size multiplier |

### Common Particle Systems

- `Effect_Fire` - Fire effects
- `Effect_Poison` - Poison cloud
- `Explosion_Medium` - Explosion
- `MagicPortal` - Portal effects
- `Weapon_Frost_Mist` - Frost weapon effect
- `Impact_Feathers_Black` - Feather particles

### Model VFX (Visual Overrides)

```json
{
  "ApplicationEffects": {
    "ModelOverride": {
      "Model": "VFX/Spells/Roots/Model.blockymodel",
      "Texture": "VFX/Spells/Roots/Model.png",
      "AnimationSets": {
        "Spawn": {
          "Animations": [
            {
              "Animation": "VFX/Spells/Roots/Spawn.blockyanim",
              "Looping": false
            }
          ]
        }
      }
    }
  }
}
```

---

**Previous:** [Creating UI](09_UI.md) | **Next:** [Creating Accessories](11_Creating_Accessories.md)
