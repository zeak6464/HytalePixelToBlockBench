# Interaction Type: DamageEntity

Deal damage to entities with damage types, modifiers, and knockback.

## Overview

`DamageEntity` deals damage to targeted entities. Supports multiple damage types, damage modifiers, knockback, visual/audio effects, and camera shake on hit.

## Example from Game Files

### Damage Entity Interaction

From `Server/Item/Interactions/Weapons/Weapon_Damage.json`:

```1:22:Server/Item/Interactions/Weapons/Weapon_Damage.json
{
  "Type": "DamageEntity",
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 5
    }
  },
  "DamageEffects": {
    "Knockback": {
      "Force": 0.5,
      "RelativeX": -5,
      "RelativeZ": -5,
      "VelocityY": 5
    },
    "WorldSoundEventId": "SFX_Unarmed_Impact",
    "WorldParticles": [
      {
        "SystemId": "Impact_Blade_01"
      }
    ]
  }
}
```

This shows a damage entity interaction with base damage calculation and damage effects including knockback, sounds, and particles.

## Basic Structure

```json
{
  "Type": "DamageEntity",
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 10
    }
  },
  "DamageEffects": {
    "Knockback": {
      "Force": 0.5,
      "RelativeX": -5,
      "RelativeZ": -5,
      "VelocityY": 5
    },
    "WorldSoundEventId": "SFX_Impact",
    "WorldParticles": [
      {
        "SystemId": "Impact_Blade_01"
      }
    ]
  },
  "Effects": {
    "CameraEffect": "Impact_Light"
  }
}
```

## Properties

### Parent (Inheritance)

```json
{
  "Parent": "Weapon_Sword_Primary_Swing_Left",
  "DamageCalculator": {
    "BaseDamage": { "Physical": 26 }
  }
}
```

Inherit from a parent interaction template. Override only the properties you want to change.

### DamageCalculator

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 10,
      "Fire": 5
    },
    "RandomPercentageModifier": 0.1
  }
}
```

Damage calculation configuration. See [Damage Calculator](104_Damage_Calculator.md) for details.

### EntityStatsOnHit

```json
{
  "EntityStatsOnHit": [
    {
      "EntityStatId": "SignatureEnergy",
      "Amount": 3
    }
  ]
}
```

Array of stats to grant the attacker when they successfully hit an entity:
- **`EntityStatId`** - ID of the stat to modify
- **`Amount`** - Amount to add/restore

**Common Stats:** `"SignatureEnergy"`, `"Health"`, `"Mana"`, `"Stamina"`

### DamageEffects

```json
{
  "DamageEffects": {
    "Knockback": {
      "Force": 0.5,
      "RelativeX": -5,
      "RelativeZ": -5,
      "VelocityY": 5
    },
    "WorldSoundEventId": "SFX_Impact",
    "LocalSoundEventId": "SFX_Impact_Local",
    "WorldParticles": [
      {
        "SystemId": "Impact_Blade_01"
      }
    ]
  }
}
```

Effects that trigger when damage is dealt:
- **`Knockback`** - Physics knockback applied to target
  - **`Force`** - Knockback force magnitude
  - **`RelativeX`** - X direction relative to attacker
  - **`RelativeZ`** - Z direction relative to attacker
  - **`VelocityY`** - Vertical velocity component
- **`WorldSoundEventId`** - Sound played at hit location (audible to all nearby players)
- **`LocalSoundEventId`** - Sound played locally to the attacker only
- **`WorldParticles`** - Particle systems spawned on hit

### Effects

```json
{
  "Effects": {
    "CameraEffect": "Impact_Light"
  }
}
```

Camera/shake effects when damage is dealt.

## Complete Examples

### Example 1: Basic Damage

```json
{
  "Type": "DamageEntity",
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 10
    }
  }
}
```

Simple 10 physical damage.

### Example 2: Multiple Damage Types

```json
{
  "Type": "DamageEntity",
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 15,
      "Fire": 5
    }
  }
}
```

15 physical + 5 fire damage.

### Example 3: With Knockback

```json
{
  "Type": "DamageEntity",
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 20
    }
  },
  "DamageEffects": {
    "Knockback": {
      "Force": 0.5,
      "RelativeX": -5,
      "RelativeZ": -5,
      "VelocityY": 5
    },
    "WorldSoundEventId": "SFX_Unarmed_Impact",
    "WorldParticles": [
      {
        "SystemId": "Impact_Blade_01"
      }
    ]
  },
  "Effects": {
    "CameraEffect": "Impact_Light"
  }
}
```

Damage with knockback, sound, particles, and camera shake.

### Example 4: Critical Hit

```json
{
  "Type": "DamageEntity",
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 30
    },
    "RandomPercentageModifier": 0.2
  },
  "DamageEffects": {
    "WorldSoundEventId": "SFX_Critical_Impact",
    "WorldParticles": [
      {
        "SystemId": "Critical_Hit"
      }
    ]
  }
}
```

Damage with 20% random variation for critical hits.

## Tips

1. **Damage types** - Use multiple damage types for complex damage calculations
2. **Knockback** - Adjust Force and direction for satisfying impacts
3. **Visual feedback** - Always include particles and sounds for feedback
4. **Camera shake** - Use camera effects for impact feel
5. **DamageCalculator** - See [Damage Calculator](104_Damage_Calculator.md) for advanced damage calculations

---

**Previous:** [Selector](118_Interaction_Type_Selector.md) | **Next:** [ApplyEffect](120_Interaction_Type_ApplyEffect.md)
