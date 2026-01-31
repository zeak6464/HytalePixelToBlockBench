# Damage Calculator

Learn how to configure damage calculations for weapons, projectiles, status effects, and NPCs using the DamageCalculator system.

## Overview

DamageCalculator defines how damage is calculated and applied. It supports multiple damage types, random variance, different calculation modes (Absolute vs DPS), and is used in weapons, projectiles, status effects, and NPC attacks.

## Location
- Weapon interactions: `Server/Item/Interactions/Weapons/`
- Projectile configs: `Server/ProjectileConfigs/`
- Status effects: `Server/Entity/Effects/`
- NPC attacks: `Server/NPC/Roles/`

## Example from Game Files

### Damage Calculator in Weapon

From `Server/Item/Items/Weapon/Sword/Weapon_Sword_Cobalt.json`:

```40:54:Server/Item/Items/Weapon/Sword/Weapon_Sword_Cobalt.json
    "Swing_Left_Damage": {
      "Interactions": [
        {
          "Parent": "Weapon_Sword_Primary_Swing_Left_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 11
            }
          },
          "DamageEffects": {
            "WorldSoundEventId": "SFX_Sword_T2_Impact",
            "LocalSoundEventId": "SFX_Sword_T2_Impact"
          }
        }
      ]
    },
```

This shows a damage calculator with base physical damage of 11 and associated damage effects.

### Cobalt Sword Damage

From `Server/Item/Items/Weapon/Sword/Weapon_Sword_Cobalt.json`:

```39:54:Server/Item/Items/Weapon/Sword/Weapon_Sword_Cobalt.json
  "InteractionVars": {
    "Swing_Left_Damage": {
      "Interactions": [
        {
          "Parent": "Weapon_Sword_Primary_Swing_Left_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 11
            }
          },
          "DamageEffects": {
            "WorldSoundEventId": "SFX_Sword_T2_Impact",
            "LocalSoundEventId": "SFX_Sword_T2_Impact"
          }
        }
      ]
    },
```

This shows a weapon using `DamageCalculator` to define base physical damage for a sword attack.

## Basic DamageCalculator Structure

```json
{
  "DamageCalculator": {
    "Type": "Absolute",
    "BaseDamage": {
      "Physical": 10
    }
  }
}
```

## DamageCalculator Properties

### Type

Determines how damage is calculated:

```json
{
  "Type": "Absolute"
}
```

**Available Types:**
- **`"Absolute"`** - Fixed damage amount (most common, used in NPC files)
- **`"Dps"`** - Damage per second (for status effects over time)

### Class

Weapon files use `Class` instead of `Type` to categorize damage for combat mechanics:

```json
{
  "DamageCalculator": {
    "Class": "Light",
    "BaseDamage": { "Physical": 10 }
  }
}
```

**Available Classes (from weapon files):**
- **`"Light"`** - Fast light attacks (e.g., sword swing)
- **`"Charged"`** - Charged/heavy attacks (e.g., thrust, power attack)
- **`"Signature"`** - Special signature moves

**Example from `Weapon_Sword_Primary_Swing_Right_Damage.json`:**
```json
{
  "DamageCalculator": {
    "Class": "Light"
  }
}
```

**Note:** `Type` is used in NPC files (`"Absolute"`, `"Dps"`), while `Class` is used in weapon interaction files (`"Light"`, `"Charged"`, `"Signature"`).

### BaseDamage

Base damage values for different damage types:

```json
{
  "BaseDamage": {
    "Physical": 10,
    "Fire": 5
  }
}
```

You can specify multiple damage types to apply all at once.

### RandomPercentageModifier

Adds random variance to damage:

```json
{
  "RandomPercentageModifier": 0.1
}
```

- **`0.1`** = ±10% variance (damage varies between 90%-110% of base)
- **`0.15`** = ±15% variance (damage varies between 85%-115% of base)
- **`0.2`** = ±20% variance (damage varies between 80%-120% of base)

**Example:**
With `BaseDamage: { "Physical": 10 }` and `RandomPercentageModifier: 0.15`:
- Minimum damage: 8.5 (85% of 10)
- Maximum damage: 11.5 (115% of 10)

## Using DamageCalculator in Weapons

### Basic Weapon Damage

`Server/Item/Interactions/Weapons/Weapon_Damage.json`:

```json
{
  "Type": "DamageEntity",
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 5
    }
  },
  "DamageEffects": {
    "Knockback": {
      "Force": 0.5
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

### Weapon Damage with Random Variance

```json
{
  "Parent": "NPC_Attack_Melee_Damage",
  "DamageCalculator": {
    "Type": "Absolute",
    "BaseDamage": {
      "Physical": 10
    },
    "RandomPercentageModifier": 0.1
  }
}
```

### Multiple Damage Types

```json
{
  "DamageCalculator": {
    "Type": "Absolute",
    "BaseDamage": {
      "Physical": 10,
      "Fire": 5
    }
  }
}
```

This deals both physical (10) and fire (5) damage for a total of 15 damage.

### Using Parent Interactions

You can inherit from parent damage interactions and override the calculator:

```json
{
  "Parent": "DamageEntityParent",
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 6
    }
  },
  "DamageEffects": {
    "Knockback": {
      "Force": 5.5
    }
  }
}
```

## Using DamageCalculator in Projectiles

### Projectile Config with Damage

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

### Projectile Damage Types

```json
{
  "DamageCalculator": {
    "Type": "Absolute",
    "BaseDamage": {
      "Projectile": 10,
      "Fire": 5
    }
  }
}
```

## Using DamageCalculator in Status Effects

### Damage Over Time (DPS)

`Server/Entity/Effects/MyCustom_Burn.json`:

```json
{
  "Duration": 5,
  "Debuff": true,
  "DamageCalculator": {
    "Type": "Dps",
    "BaseDamage": {
      "Fire": 10
    }
  },
  "DamageCalculatorCooldown": 1
}
```

**Status Effect Properties:**
- **`Type: "Dps"`** - Damage per second (applied over time)
- **`DamageCalculatorCooldown`** - Interval between damage ticks (seconds)
- With `DamageCalculatorCooldown: 1`, damage is applied every 1 second

### Poison Effect

```json
{
  "Duration": 10,
  "Debuff": true,
  "DamageCalculator": {
    "Type": "Dps",
    "BaseDamage": {
      "Poison": 5
    }
  },
  "DamageCalculatorCooldown": 2
}
```

Applies 5 poison damage every 2 seconds for 10 seconds (total 5 ticks = 25 damage).

### Burn Effect with Variance

```json
{
  "Duration": 5,
  "Debuff": true,
  "DamageCalculator": {
    "Type": "Dps",
    "BaseDamage": {
      "Fire": 10
    },
    "RandomPercentageModifier": 0.2
  },
  "DamageCalculatorCooldown": 1
}
```

Each damage tick has ±20% variance.

## Using DamageCalculator in NPCs

### NPC Attack Damage

In NPC role files:

```json
{
  "InteractionVars": {
    "Attack_Damage": {
      "Interactions": [
        {
          "Parent": "NPC_Attack_Melee_Damage",
          "DamageCalculator": {
            "Type": "Absolute",
            "BaseDamage": {
              "Physical": 15
            },
            "RandomPercentageModifier": 0.1
          }
        }
      ]
    }
  }
}
```

## Damage Types Reference

Common damage types for `BaseDamage`:

| Damage Type | Use Case |
|-------------|----------|
| **`Physical`** | Melee weapons, unarmed attacks |
| **`Projectile`** | Arrows, bolts, bullets |
| **`Fire`** | Fire spells, burn effects |
| **`Ice`** | Ice spells, freeze effects |
| **`Poison`** | Poison effects, DoT |
| **`Bludgeoning`** | Hammers, maces (subtype of Physical) |
| **`Slashing`** | Swords, axes (subtype of Physical) |
| **`Elemental`** | Generic magical damage |

See [Damage Types](25_Damage_Types.md) for complete list.

## Complete Examples

### Example 1: Weapon with Physical Damage

```json
{
  "Type": "DamageEntity",
  "DamageCalculator": {
    "Type": "Absolute",
    "BaseDamage": {
      "Physical": 15
    },
    "RandomPercentageModifier": 0.1
  },
  "DamageEffects": {
    "Knockback": {
      "Force": 5.5
    },
    "WorldSoundEventId": "SFX_Sword_Hit",
    "WorldParticles": [
      {
        "SystemId": "Impact_Blade_01"
      }
    ]
  }
}
```

### Example 2: Fire Spell Damage

```json
{
  "Parent": "DamageEntityParent",
  "DamageCalculator": {
    "Type": "Absolute",
    "BaseDamage": {
      "Fire": 30
    }
  },
  "DamageEffects": {
    "WorldParticles": [
      {
        "SystemId": "Explosion_Fire"
      }
    ]
  }
}
```

### Example 3: Hybrid Damage Weapon

```json
{
  "DamageCalculator": {
    "Type": "Absolute",
    "BaseDamage": {
      "Physical": 10,
      "Fire": 5,
      "Poison": 2
    },
    "RandomPercentageModifier": 0.15
  }
}
```

Deals 10 physical + 5 fire + 2 poison = 17 total damage with ±15% variance.

### Example 4: Damage Over Time Effect

```json
{
  "ApplicationEffects": {
    "EntityTopTint": "#FF0000",
    "ScreenEffect": "ScreenEffects/Burn.png"
  },
  "Duration": 8,
  "Debuff": true,
  "DamageCalculator": {
    "Type": "Dps",
    "BaseDamage": {
      "Fire": 12
    },
    "RandomPercentageModifier": 0.1
  },
  "DamageCalculatorCooldown": 1
}
```

Applies 12 fire damage every 1 second for 8 seconds (8 ticks = 96 total damage).

### Example 5: Projectile with Damage

`Server/ProjectileConfigs/Weapons/Bows/Projectile_Config_Bow_Combat.json`:

```json
{
  "Interactions": {
    "ProjectileHit": {
      "Interactions": [
        {
          "Parent": "Bow_Combat_Projectile_Damage",
          "DamageCalculator": {
            "Type": "Absolute",
            "BaseDamage": {
              "Projectile": 6
            }
          },
          "DamageEffects": {
            "Knockback": {
              "Force": 3
            }
          }
        }
      ]
    }
  }
}
```

## DamageCalculator vs Direct Damage

### Direct Damage (Simple Projectiles)

```json
{
  "Appearance": "Arrow_MyCustom",
  "Damage": 6
}
```

Simple, no modifiers.

### DamageCalculator (Advanced)

```json
{
  "DamageCalculator": {
    "Type": "Absolute",
    "BaseDamage": {
      "Projectile": 6
    },
    "RandomPercentageModifier": 0.1
  }
}
```

Supports modifiers, multiple damage types, and DPS.

**Use DamageCalculator when you need:**
- Random variance
- Multiple damage types
- Damage over time (DPS)
- Inheritance from parent interactions

## Line of Sight Checks

**Update 1 Note:** Weapon attacks now check line of sight before applying damage. This is configured through the `Selector` used in weapon interactions:

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Horizontal",
    "Direction": "ToLeft",
    "TestLineOfSight": true,  // Requires clear line of sight to hit
    "EndDistance": 2.5
  },
  "HitEntity": {
    "Interactions": [
      {
        "Type": "DamageEntity",
        "DamageCalculator": {
          "BaseDamage": {
            "Physical": 10
          }
        }
      }
    ]
  }
}
```

**Selector Properties:**
- **`TestLineOfSight: true`** - Requires unobstructed line of sight between attacker and target
- Without line of sight, damage is not applied even if the attack would otherwise hit

## Damage Immunity and Resistance

### NPC Immunities

**Update 1 Note:** Certain NPCs have damage immunities:

- **Fire-themed NPCs** - Immune to fire damage (still catch fire visually)
- **Kweebecs** - Immune to environmental damage from cactus/brambles
- **Skeletons** - No longer take drowning damage

### Knockback Resistance

**Update 1 Note:** NPCs now have knockback resistance. This affects how knockback force is applied:

```json
{
  "DamageEffects": {
    "Knockback": {
      "Force": 5.5,
      "VelocityY": 5
    }
  }
}
```

NPCs with knockback resistance will be pushed less by the same knockback force.

### Environmental Damage Types

**Update 1 Note:** Cactus and brambles now deal **Environmental** damage type instead of other types.

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Environmental": 5  // Cactus/brambles now use this
    }
  }
}
```

### Burn Effect Changes

**Update 1 Note:** The regular Burn status effect now deals less damage overall, but Burn from lava remains unchanged.

When creating burn effects:
- **Regular Burn** - Reduced damage (balanced for gameplay)
- **Lava Burn** - Full damage (unchanged)

## Tips for DamageCalculator

1. **Use Absolute for instant damage** - Most weapons and projectiles
2. **Use Dps for status effects** - Damage over time effects
3. **Random variance** - 0.1-0.15 typical for weapon variance
4. **Multiple damage types** - Combine Physical + Elemental for hybrid damage
5. **Inherit from parents** - Use `Parent` to inherit damage interactions and override calculator
6. **DamageCalculatorCooldown** - For DPS effects, set to 1-2 seconds typically
7. **Balance damage** - Consider total damage (sum of all types) for balance
8. **Test variance** - Verify random modifiers feel good in gameplay
9. **Line of sight** - Weapon attacks require clear line of sight (configured in Selector)
10. **Damage immunities** - Consider NPC immunities when designing damage types
11. **Environmental damage** - Cactus/brambles now use Environmental damage type

---

**Previous:** [Creating Totems](103_Totems.md) | **Next:** [Drop Tables](105_Drop_Tables.md)
