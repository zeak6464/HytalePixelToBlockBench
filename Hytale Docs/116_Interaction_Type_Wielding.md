# Interaction Type: Wielding

Weapon blocking and parrying mechanics with damage reduction.

## Overview

`Wielding` enables blocking/parrying while holding a weapon. Reduces incoming damage, supports angled blocking, stamina costs, and blocked effects. Typically used for shields and defensive weapons.

## Example from Game Files

### Shield Guard with Angled Wielding

From `Server/Item/Interactions/Weapons/Shield/Secondary/Guard/Weapon_Shield_Secondary_Guard_Wield.json`:

```json
{
  "Type": "Wielding",
  "Effects": {
    "ItemAnimationId": "Guard",
    "ClearAnimationOnFinish": true,
    "WorldSoundEventId": "SFX_Shield_T2_Raise",
    "LocalSoundEventId": "SFX_Shield_T2_Raise_Local"
  },
  "Forks": {
    "Primary": {
      "Interactions": [
        {
          "Type": "Replace",
          "Var": "Guard_Bash",
          "DefaultOk": true,
          "DefaultValue": {
            "Interactions": ["Weapon_Shield_Secondary_Guard_Bash"]
          }
        }
      ]
    }
  },
  "AngledWielding": {
    "Angle": 0,
    "AngleDistance": 90,
    "DamageModifiers": {
      "Physical": 0,
      "Projectile": 0,
      "Poison": 0
    },
    "KnockbackModifiers": {
      "Physical": 0.25,
      "Projectile": 0.25
    }
  },
  "StaminaCost": {
    "Value": 7,
    "CostType": "Damage"
  },
  "CancelOnOtherClick": false,
  "Failed": {
    "Type": "Replace",
    "Var": "Guard_Break",
    "DefaultOk": true,
    "DefaultValue": {
      "Interactions": [
        {
          "Type": "Simple",
          "Effects": {
            "Particles": [{ "SystemId": "Shield_Shatter" }],
            "WorldSoundEventId": "SFX_Shield_T1_Break",
            "LocalSoundEventId": "SFX_Shield_T1_Break"
          }
        }
      ]
    }
  },
  "BlockedEffects": {
    "WorldSoundEventId": "SFX_Shield_T2_Impact",
    "WorldParticles": [{ "SystemId": "Shield_Block" }]
  },
  "Next": {
    "Type": "ChangeStat",
    "Behaviour": "Set",
    "StatModifiers": { "StaminaRegenDelay": -1 }
  }
}
```

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

## Advanced Properties

### AngledWielding

Controls directional blocking:

```json
{
  "AngledWielding": {
    "Angle": 0,
    "AngleDistance": 90,
    "DamageModifiers": {
      "Physical": 0,
      "Projectile": 0
    },
    "KnockbackModifiers": {
      "Physical": 0.25,
      "Projectile": 0.25
    }
  }
}
```

| Property | Description |
|----------|-------------|
| `Angle` | Center angle of block zone |
| `AngleDistance` | Width of block zone (degrees) |
| `DamageModifiers` | Damage reduction when blocked |
| `KnockbackModifiers` | Knockback reduction when blocked |

### StaminaCost

Stamina consumed when blocking damage:

```json
{
  "StaminaCost": {
    "Value": 7,
    "CostType": "Damage"
  }
}
```

### BlockedEffects

Effects when successfully blocking:

```json
{
  "BlockedEffects": {
    "WorldSoundEventId": "SFX_Shield_T2_Impact",
    "WorldParticles": [{ "SystemId": "Shield_Block" }]
  }
}
```

### Failed (Guard Break)

What happens when guard breaks:

```json
{
  "Failed": {
    "Type": "Simple",
    "Effects": {
      "Particles": [{ "SystemId": "Shield_Shatter" }],
      "WorldSoundEventId": "SFX_Shield_Break"
    }
  }
}
```

### Forks

Allow other actions while blocking:

```json
{
  "Forks": {
    "Primary": {
      "Interactions": [
        {
          "Type": "Replace",
          "Var": "Guard_Bash",
          "DefaultValue": { "Interactions": ["Shield_Bash"] }
        }
      ]
    }
  }
}
```

## Tips

1. **Damage reduction** - Values 0.3-0.7 typical (30-70% reduction)
2. **All damage types** - Specify modifiers for all relevant damage types
3. **Clear animation** - Use `ClearAnimationOnFinish: true` for clean transitions
4. **Angled blocking** - Use `AngledWielding` for directional shields
5. **Shield vs weapon** - Shields typically have better reduction than weapons
6. **Stamina cost** - Add `StaminaCost` for balanced blocking
7. **Failed state** - Always define guard break effects

---

**Previous:** [Charging](115_Interaction_Type_Charging.md) | **Next:** [Replace](117_Interaction_Type_Replace.md)
