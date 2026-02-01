# Damage Types

Learn how damage types work, their properties, and how to use them in items and effects.

## Overview

Damage types define how damage is applied and what effects it has (durability loss, stamina loss, visual effects, etc.). They're used in weapons, spells, status effects, and entity damage.

## Location
`Server/Entity/Damage/`

## Example from Game Files

### Physical Damage Type

From `Server/Entity/Damage/Physical.json`:

```1:5:Server/Entity/Damage/Physical.json
{
  "$Comment": "This damage type exists to facilitate sub types",
  "DurabilityLoss": true,
  "StaminaLoss": true
}
```

This shows a damage type that causes durability and stamina loss when applied.

## Basic Damage Type Structure

Create `Server/Entity/Damage/MyCustom.json`:

```json
{
  "Parent": "Physical",
  "DamageTextColor": "#ff00ff",
  "DurabilityLoss": true,
  "StaminaLoss": true
}
```

## Common Damage Type Properties

### Durability Loss

```json
"DurabilityLoss": true
```

If `true`, this damage type causes armor/items to lose durability.

### Stamina Loss

```json
"StaminaLoss": true
```

If `true`, this damage type drains player stamina.

### Damage Text Color

```json
"DamageTextColor": "#ff00ff"
```

Hex color for damage numbers displayed to players.

### Parent Inheritance

```json
{
  "Parent": "Elemental",
  "Inherits": "Elemental"
}
```

Damage types can inherit from parent types.

## Base Damage Types

### Physical

`Server/Entity/Damage/Physical.json`:

```json
{
  "$Comment": "This damage type exists to facilitate sub types",
  "DurabilityLoss": true,
  "StaminaLoss": true
}
```

Base type for all physical damage (melee weapons, punches, etc.).

### Sub-Types of Physical

#### Bludgeoning

```json
{
  "Parent": "Physical",
  "Inherits": "Physical"
}
```

Blunt weapon damage (hammers, maces).

#### Slashing

```json
{
  "Parent": "Physical",
  "Inherits": "Physical"
}
```

Sharp weapon damage (swords, axes, daggers).

### Projectile

`Server/Entity/Damage/Projectile.json`:

```json
{
  "$Comment": "This damage type exists to facilitate sub types",
  "DurabilityLoss": true,
  "StaminaLoss": false
}
```

Base type for ranged weapon damage (arrows, crossbows).

### Elemental

`Server/Entity/Damage/Elemental.json`:

```json
{
  "$Comment": "This damage type exists to facilitate sub types"
}
```

Base type for magical/elemental damage. It has no properties itself - it exists as a parent category for sub-types.

### Sub-Types of Elemental

**Note:** Sub-types like Fire and Ice inherit from Elemental through the game's type system, but individual files may only contain comments. The damage type hierarchy is:
- Elemental (base)
  - Fire (inherits behavior)
  - Ice (inherits behavior)
  - Poison (inherits behavior)

Actual behavior inheritance is handled by the game engine, not explicit JSON properties.

Ice/cold damage (frost spells, freeze effects).

### Poison

`Server/Entity/Damage/Poison.json`:

```json
{
  "DamageTextColor": "#00FF00"
}
```

Poison damage (damage over time effects).

### Environment

`Server/Entity/Damage/Environment.json`:

```json
{
  "DurabilityLoss": true,
  "StaminaLoss": false,
  "BypassResistances": true
}
```

Environmental damage (falling, drowning, etc.). Bypasses damage resistances.

**Update 1 Note:** Cactus and brambles now deal **Environmental** damage type instead of other damage types. NPCs that are immune to environmental damage (like Kweebecs) will not take damage from these sources.

### Sub-Types of Environment

#### Fall

```json
{
  "Parent": "Environment",
  "Inherits": "Environment"
}
```

Fall damage.

#### Drowning

```json
{
  "Parent": "Environment",
  "Inherits": "Environment"
}
```

Drowning damage.

#### Suffocation

```json
{
  "Parent": "Environment",
  "Inherits": "Environment"
}
```

Suffocation damage.

### Command

`Server/Entity/Damage/Command.json`:

```json
{
  "DurabilityLoss": false,
  "StaminaLoss": false,
  "BypassResistances": true
}
```

Command-based damage (admin/debug damage). Bypasses resistances and doesn't cause durability or stamina loss.

### OutOfWorld

`Server/Entity/Damage/OutOfWorld.json`:

```json
{
  "Parent": "Environment",
  "Inherits": "Environment"
}
```

Damage from falling out of world bounds. Inherits from Environment (bypasses resistances).

## Using Damage Types in Items

### Weapon Damage

```json
{
  "InteractionVars": {
    "Swing_Left_Damage": {
      "Interactions": [
        {
          "Parent": "Weapon_Sword_Primary_Swing_Left_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 15
            }
          }
        }
      ]
    }
  }
}
```

### Multiple Damage Types

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 10,
      "Fire": 5
    }
  }
}
```

This deals both physical and fire damage.

### Elemental Damage (Spells)

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Fire": 30
    }
  }
}
```

Fire spell damage.

## Using Damage Types in Status Effects

### Burn Effect

```json
{
  "Duration": 5,
  "Debuff": true,
  "DamageCalculator": {
    "BaseDamage": {
      "Fire": 10
    }
  },
  "DamageCalculatorCooldown": 1
}
```

**Update 1 Note:** The regular Burn status effect now deals less damage overall, but Burn from lava remains unchanged. Fire-themed NPCs are immune to fire damage but will still catch fire visually.

### Poison Effect

```json
{
  "Duration": 10,
  "Debuff": true,
  "DamageCalculator": {
    "BaseDamage": {
      "Poison": 5
    }
  },
  "DamageCalculatorCooldown": 2
}
```

## Damage Resistance (Armor)

Armor can resist specific damage types:

```json
{
  "Armor": {
    "DamageResistance": {
      "Physical": [
        {
          "Amount": 0.09,
          "CalculationType": "Multiplicative"
        }
      ],
      "Fire": [
        {
          "Amount": 0.15,
          "CalculationType": "Multiplicative"
        }
      ],
      "Poison": [
        {
          "Amount": 0.20,
          "CalculationType": "Multiplicative"
        }
      ]
    }
  }
}
```

## Damage Type Reference

| Damage Type | Parent | Durability Loss | Stamina Loss | Use Case |
|-------------|--------|-----------------|--------------|----------|
| **Physical** | - | Yes | Yes | Base physical damage |
| **Bludgeoning** | Physical | Yes | Yes | Hammers, maces |
| **Slashing** | Physical | Yes | Yes | Swords, axes |
| **Projectile** | - | Yes | No | Arrows, crossbows |
| **Elemental** | - | - | - | Base magical damage |
| **Fire** | Elemental | - | - | Fire spells, burn |
| **Ice** | Elemental | - | - | Frost spells, freeze |
| **Poison** | - | - | - | Poison effects, DoT |
| **Environment** | - | - | - | Environmental hazards |
| **Fall** | Environment | - | - | Fall damage |
| **Drowning** | Environment | - | - | Underwater damage |
| **Suffocation** | Environment | - | - | No air damage |

## Complete Example: Custom Damage Type

Create `Server/Entity/Damage/Custom_Electric.json`:

```json
{
  "Parent": "Elemental",
  "Inherits": "Elemental",
  "DamageTextColor": "#ffff00",
  "DurabilityLoss": false,
  "StaminaLoss": true
}
```

Use in a spell:

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Custom_Electric": 25
    }
  },
  "DamageEffects": {
    "WorldParticles": [
      {
        "SystemId": "Electric_Spark"
      }
    ]
  }
}
```

Use in armor resistance:

```json
{
  "Armor": {
    "DamageResistance": {
      "Custom_Electric": [
        {
          "Amount": 0.50,
          "CalculationType": "Multiplicative"
        }
      ]
    }
  }
}
```

## Tips for Using Damage Types

1. **Inherit from base types** - Use `Parent` to inherit properties
2. **Set appropriate colors** - `DamageTextColor` helps players identify damage types
3. **Consider durability** - Physical/projectile damage should cause durability loss
4. **Balance resistances** - Armor can resist specific damage types
5. **Use in status effects** - Damage over time effects use damage types
6. **Combine damage types** - Weapons can deal multiple damage types
7. **Match visuals** - Use particle effects that match the damage type

---

**Previous:** [Biomes & Environments](24_Biomes_and_Environments.md) | **Next:** [Fishing](26_Fishing.md)
