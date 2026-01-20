# Interaction Type: Charging

Hold-to-charge mechanics with time-based power levels and optional indefinite holding.

## Overview

`Charging` allows players to hold an interaction to charge it up. Power increases over time, with different outcomes based on charge level. Useful for spells, charged attacks, and special abilities.

## Example from Game Files

### Charging Interaction

Charging interactions allow weapons and items to build up power over time while the player holds the interaction. This is commonly used for charged attacks, spell casting, and ranged weapon draws.

## Basic Structure

```json
{
  "Type": "Charging",
  "Effects": {
    "ItemAnimationId": "CastLeftCharging"
  },
  "AllowIndefiniteHold": true,
  "HorizontalSpeedMultiplier": 0.75,
  "Next": {
    "0": {
      "Type": "Chaining",
      "Next": [
        "Sword_Swing_Left_Fast",
        "Sword_Swing_Right_Fast"
      ]
    },
    "0.35": "Wand_Cast_Left_Charged"
  }
}
```

## Properties

### Effects

```json
{
  "Effects": {
    "ItemAnimationId": "CastLeftCharging",
    "WorldSoundEventId": "SFX_Charging"
  }
}
```

Effects to play while charging (animations, sounds, particles).

### AllowIndefiniteHold

```json
{
  "AllowIndefiniteHold": true
}
```

If `true`, player can hold indefinitely. If `false`, automatically releases at max charge.

### HorizontalSpeedMultiplier

```json
{
  "HorizontalSpeedMultiplier": 0.75
}
```

Movement speed multiplier while charging (0-1). 1 = normal speed, 0 = cannot move.

### Next

```json
{
  "Next": {
    "0": {
      "Type": "Simple",
      "Effects": { "ItemAnimationId": "Quick_Attack" }
    },
    "0.35": {
      "Type": "Simple",
      "Effects": { "ItemAnimationId": "Charged_Attack" }
    },
    "0.7": "Spell_Charged_Max"
  }
}
```

Map of charge times (in seconds) to interactions. Key is time as string, value is interaction or ID.

## Complete Examples

### Example 1: Basic Charge Levels

```json
{
  "Type": "Charging",
  "Effects": {
    "ItemAnimationId": "CastLeftCharging",
    "WorldParticles": [
      {
        "SystemId": "Magic_Charging"
      }
    ]
  },
  "HorizontalSpeedMultiplier": 0.5,
  "Next": {
    "0": {
      "Type": "LaunchProjectile",
      "ProjectileId": "Fireball_Small"
    },
    "0.5": {
      "Type": "LaunchProjectile",
      "ProjectileId": "Fireball_Medium"
    },
    "1.0": {
      "Type": "LaunchProjectile",
      "ProjectileId": "Fireball_Large"
    }
  }
}
```

Three charge levels: quick (0s), medium (0.5s), max (1.0s).

### Example 2: Indefinite Hold

```json
{
  "Type": "Charging",
  "Effects": {
    "ItemAnimationId": "CastLeftCharging"
  },
  "AllowIndefiniteHold": true,
  "HorizontalSpeedMultiplier": 0.25,
  "Next": {
    "0": "Spell_Quick",
    "0.35": "Spell_Charged",
    "1.0": "Spell_Max",
    "2.0": "Spell_Overcharge"
  }
}
```

Can hold indefinitely, with overcharge at 2 seconds.

### Example 3: Movement While Charging

```json
{
  "Type": "Charging",
  "Effects": {
    "ItemAnimationId": "Charge_Attack",
    "WorldSoundEventId": "SFX_Charging"
  },
  "AllowIndefiniteHold": false,
  "HorizontalSpeedMultiplier": 0.75,
  "Next": {
    "0": {
      "Type": "Simple",
      "Effects": { "ItemAnimationId": "Quick_Stab" }
    },
    "0.5": {
      "Type": "Simple",
      "Effects": { "ItemAnimationId": "Charged_Stab" }
    }
  }
}
```

Can move at 75% speed while charging, auto-releases at max.

### Example 4: Combo After Charge

```json
{
  "Type": "Charging",
  "Effects": {
    "ItemAnimationId": "CastLeftCharging"
  },
  "HorizontalSpeedMultiplier": 0.75,
  "Next": {
    "0": {
      "Type": "Chaining",
      "ChainId": "Quick_Combo",
      "Next": [
        "Sword_Swing_Left_Fast",
        "Sword_Swing_Right_Fast"
      ]
    },
    "0.35": "Wand_Cast_Left_Charged"
  }
}
```

Quick release = combo, charged = spell.

## Tips

1. **Charge times** - Use 0.35-1.0s for responsive feel, 2.0s+ for slow powerful charges
2. **Movement** - Lower `HorizontalSpeedMultiplier` (0.25-0.5) for more powerful charges
3. **Indefinite hold** - Use `AllowIndefiniteHold: true` for strategic holding
4. **String references** - Reference other interactions by ID for cleaner code
5. **Charge levels** - Start with "0" for instant release, add more levels for power scaling

---

**Previous:** [Chaining](114_Interaction_Type_Chaining.md) | **Next:** [Wielding](116_Interaction_Type_Wielding.md)
