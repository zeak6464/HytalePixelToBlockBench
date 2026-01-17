# Interaction Type: Wielding

Weapon blocking and parrying mechanics with damage reduction.

## Overview

`Wielding` enables blocking/parrying while holding a weapon. Reduces incoming damage and can trigger parry effects. Typically used for shields and defensive weapons.

## Basic Structure

```json
{
  "Type": "Wielding",
  "Effects": {
    "ItemAnimationId": "Guard",
    "ClearAnimationOnFinish": true
  },
  "DamageModifiers": {
    "Physical": 0.7,
    "Projectile": 0.7
  }
}
```

## Properties

### Effects

```json
{
  "Effects": {
    "ItemAnimationId": "Guard",
    "WorldSoundEventId": "SFX_Block",
    "ClearAnimationOnFinish": true
  }
}
```

Effects while wielding (blocking animation, sounds).

### DamageModifiers

```json
{
  "DamageModifiers": {
    "Physical": 0.7,
    "Projectile": 0.7,
    "Fire": 0.5,
    "Ice": 0.8
  }
}
```

Damage reduction multipliers per damage type (0-1). 1 = no reduction, 0 = immune.

## Complete Examples

### Example 1: Shield Block

```json
{
  "Type": "Wielding",
  "Effects": {
    "ItemAnimationId": "Guard",
    "WorldSoundEventId": "SFX_Shield_Block",
    "ClearAnimationOnFinish": true
  },
  "DamageModifiers": {
    "Physical": 0.5,
    "Projectile": 0.3,
    "Fire": 0.6,
    "Ice": 0.8
  }
}
```

Shield reduces physical by 50%, projectiles by 70%.

### Example 2: Weapon Parry

```json
{
  "Type": "Wielding",
  "Effects": {
    "ItemAnimationId": "Parry",
    "ClearAnimationOnFinish": true
  },
  "DamageModifiers": {
    "Physical": 0.7
  }
}
```

Weapon parry reduces physical damage by 30%.

### Example 3: Magic Shield

```json
{
  "Type": "Wielding",
  "Effects": {
    "ItemAnimationId": "Magic_Shield",
    "WorldParticles": [
      {
        "SystemId": "Shield_Barrier"
      }
    ],
    "ClearAnimationOnFinish": true
  },
  "DamageModifiers": {
    "Physical": 0.6,
    "Fire": 0.4,
    "Ice": 0.4,
    "Lightning": 0.4
  }
}
```

Magic shield with particle effects, strong against magic damage.

## Tips

1. **Damage reduction** - Values 0.3-0.7 typical (30-70% reduction)
2. **All damage types** - Specify modifiers for all relevant damage types
3. **Clear animation** - Use `ClearAnimationOnFinish: true` for clean transitions
4. **Parry timing** - Combine with parry detection for perfect blocks
5. **Shield vs weapon** - Shields typically have better reduction than weapons

---

**Previous:** [Charging](115_Interaction_Type_Charging.md) | **Next:** [Replace](117_Interaction_Type_Replace.md)
