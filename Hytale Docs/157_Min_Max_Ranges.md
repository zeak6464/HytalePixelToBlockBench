# Min/Max Ranges

Learn how to configure min/max ranges for damage, quantities, and other numeric values.

## Overview

Hytale uses min/max ranges for damage variance, quantity ranges (item drops), and other numeric ranges. This guide covers all methods for creating ranges, calculating values, and applying them to different systems.

## Location
Used throughout the codebase:
- Damage: `DamageCalculator` in interactions
- Quantities: Drop tables, item definitions
- Other: Farming modifiers, particle systems, etc.

## Damage Ranges

### RandomPercentageModifier

The primary method for damage ranges uses `RandomPercentageModifier` in `DamageCalculator`:

```json
{
  "DamageCalculator": {
    "Type": "Absolute",
    "BaseDamage": {
      "Physical": 10
    },
    "RandomPercentageModifier": 0.1
  }
}
```

**How it works:**
- `RandomPercentageModifier: 0.1` = ±10% variance
- Damage varies between 90%-110% of base damage
- Calculated as: `BaseDamage × (1 ± RandomPercentageModifier)`

**Calculation Formula:**
```
Minimum Damage = BaseDamage × (1 - RandomPercentageModifier)
Maximum Damage = BaseDamage × (1 + RandomPercentageModifier)
```

### Damage Range Examples

#### Example 1: Small Variance (±10%)

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 10
    },
    "RandomPercentageModifier": 0.1
  }
}
```

**Result:**
- Minimum: 10 × 0.9 = **9 damage**
- Maximum: 10 × 1.1 = **11 damage**
- Range: 9-11 damage

#### Example 2: Medium Variance (±15%)

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 20
    },
    "RandomPercentageModifier": 0.15
  }
}
```

**Result:**
- Minimum: 20 × 0.85 = **17 damage**
- Maximum: 20 × 1.15 = **23 damage**
- Range: 17-23 damage

#### Example 3: Large Variance (±20%)

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 50
    },
    "RandomPercentageModifier": 0.2
  }
}
```

**Result:**
- Minimum: 50 × 0.8 = **40 damage**
- Maximum: 50 × 1.2 = **60 damage**
- Range: 40-60 damage

#### Example 4: Critical Hit Variance

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 30
    },
    "RandomPercentageModifier": 0.25
  }
}
```

**Result:**
- Minimum: 30 × 0.75 = **22.5 damage** (rounded)
- Maximum: 30 × 1.25 = **37.5 damage** (rounded)
- Range: 23-38 damage (wide variance for critical hits)

#### Example 5: Multiple Damage Types

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 15,
      "Fire": 5
    },
    "RandomPercentageModifier": 0.1
  }
}
```

**Result:**
- Physical: 13.5-16.5 (15 ± 10%)
- Fire: 4.5-5.5 (5 ± 10%)
- Total: 18-22 damage (varies independently)

### Common Damage Variance Values

| Modifier | Variance | Use Case |
|----------|----------|----------|
| `0.05` | ±5% | Very consistent (boss weapons) |
| `0.1` | ±10% | Standard variance |
| `0.15` | ±15% | Moderate variance |
| `0.2` | ±20% | High variance |
| `0.25` | ±25% | Critical/unstable (wide range) |

## Quantity Ranges (QuantityMin/QuantityMax)

For item quantities in drop tables and item definitions:

### Basic Quantity Range

```json
{
  "Item": {
    "ItemId": "Ingredient_Bone_Fragment",
    "QuantityMin": 1,
    "QuantityMax": 3
  }
}
```

**Result:**
- Drops 1-3 items (random between min and max, inclusive)

### Quantity Range Examples

#### Example 1: Fixed Quantity

```json
{
  "Item": {
    "ItemId": "Food_Bread",
    "QuantityMin": 1,
    "QuantityMax": 1
  }
}
```

**Result:** Always drops exactly 1 item.

#### Example 2: Small Range

```json
{
  "Item": {
    "ItemId": "Ingredient_Fibre",
    "QuantityMin": 1,
    "QuantityMax": 2
  }
}
```

**Result:** Drops 1-2 items (50% chance for each).

#### Example 3: Medium Range

```json
{
  "Item": {
    "ItemId": "Ingredient_Hide_Medium",
    "QuantityMin": 2,
    "QuantityMax": 3
  }
}
```

**Result:** Drops 2-3 items (random between min and max).

#### Example 4: Large Range

```json
{
  "Item": {
    "ItemId": "Ingredient_Gold_Coin",
    "QuantityMin": 5,
    "QuantityMax": 10
  }
}
```

**Result:** Drops 5-10 coins (random between min and max).

#### Example 5: Chance with 0-1 Range

```json
{
  "Item": {
    "ItemId": "Ingredient_Crystal_Cyan",
    "QuantityMin": 0,
    "QuantityMax": 1
  }
}
```

**Result:** 50% chance to drop (0 or 1).

## Generic Min/Max Patterns

For other systems that use `Min` and `Max` properties:

### Farming Modifiers

```json
{
  "Modifiers": {
    "LightLevel": {
      "Min": 5,
      "Max": 127
    },
    "Temperature": {
      "Min": 0.0,
      "Max": 5.0
    }
  }
}
```

**Result:**
- `Min`: Minimum value
- `Max`: Maximum value
- Value ranges between min and max (inclusive)

### Particle Systems

```json
{
  "Velocity": {
    "Min": 8.0,
    "Max": 8.0
  },
  "Lifetime": {
    "Min": 4.0,
    "Max": 5.0
  },
  "Rotation": {
    "Min": -180.0,
    "Max": 180.0
  }
}
```

**Result:**
- Values randomly selected between min and max
- If min = max, value is fixed

## Range Calculation Guide

### Converting Percentage to Min/Max

If you want specific min/max values from a base:

**Formula:**
```
RandomPercentageModifier = (DesiredMax - DesiredMin) / (2 × Base)
```

**Example:**
- Base: 10
- Want: 8-12 range
- Calculation: (12 - 8) / (2 × 10) = 4 / 20 = 0.2
- Result: `RandomPercentageModifier: 0.2`

### Converting Min/Max to Percentage

If you have min/max and want percentage:

**Formula:**
```
RandomPercentageModifier = (Max - Min) / (2 × (Min + Max) / 2)
```

**Example:**
- Min: 8, Max: 12
- Average: (8 + 12) / 2 = 10
- Calculation: (12 - 8) / (2 × 10) = 4 / 20 = 0.2
- Result: `RandomPercentageModifier: 0.2`

### Finding Base from Range

If you want specific min/max and need base:

**Formula:**
```
Base = (Min + Max) / 2
RandomPercentageModifier = (Max - Min) / (2 × Base)
```

**Example:**
- Want: 15-25 range
- Base: (15 + 25) / 2 = 20
- Modifier: (25 - 15) / (2 × 20) = 10 / 40 = 0.25
- Result:
  ```json
  {
    "BaseDamage": { "Physical": 20 },
    "RandomPercentageModifier": 0.25
  }
  ```

## Common Range Patterns

### Consistent Damage (Low Variance)

```json
{
  "DamageCalculator": {
    "BaseDamage": { "Physical": 50 },
    "RandomPercentageModifier": 0.05
  }
}
```

**Range:** 47.5-52.5 (very consistent, ±5%)

### Standard Damage (Medium Variance)

```json
{
  "DamageCalculator": {
    "BaseDamage": { "Physical": 50 },
    "RandomPercentageModifier": 0.1
  }
}
```

**Range:** 45-55 (standard, ±10%)

### Variable Damage (High Variance)

```json
{
  "DamageCalculator": {
    "BaseDamage": { "Physical": 50 },
    "RandomPercentageModifier": 0.2
  }
}
```

**Range:** 40-60 (variable, ±20%)

### Guaranteed Drop

```json
{
  "Item": {
    "ItemId": "Food_Bread",
    "QuantityMin": 1,
    "QuantityMax": 1
  }
}
```

**Result:** Always 1 item.

### Variable Drop

```json
{
  "Item": {
    "ItemId": "Ingredient_Coins",
    "QuantityMin": 5,
    "QuantityMax": 10
  }
}
```

**Result:** 5-10 items (random).

### Chance Drop

```json
{
  "Item": {
    "ItemId": "Rare_Crystal",
    "QuantityMin": 0,
    "QuantityMax": 1
  }
}
```

**Result:** 50% chance to drop.

## Complete Examples

### Example 1: Weapon with Damage Range

```json
{
  "InteractionVars": {
    "Swing_Left_Damage": {
      "Interactions": [
        {
          "Type": "DamageEntity",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 15
            },
            "RandomPercentageModifier": 0.15
          },
          "DamageEffects": {
            "WorldSoundEventId": "SFX_Sword_Impact",
            "WorldParticles": [
              {
                "SystemId": "Impact_Blade_01"
              }
            ]
          }
        }
      ]
    }
  }
}
```

**Damage Range:** 12.75-17.25 (15 ± 15%)

### Example 2: Drop Table with Ranges

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Ingredient_Bone_Fragment",
          "QuantityMin": 1,
          "QuantityMax": 3
        }
      },
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Ingredient_Gold_Coin",
          "QuantityMin": 5,
          "QuantityMax": 10
        }
      }
    ]
  }
}
```

**Result:**
- Always drops 1-3 bone fragments
- Always drops 5-10 gold coins

### Example 3: Spell with Fire Damage Range

```json
{
  "Type": "LaunchProjectile",
  "ProjectileId": "Fireball",
  "Next": {
    "DamageCalculator": {
      "BaseDamage": {
        "Fire": 30
      },
      "RandomPercentageModifier": 0.2
    }
  }
}
```

**Damage Range:** 24-36 fire damage (30 ± 20%)

### Example 4: Status Effect with DoT Range

```json
{
  "Duration": 10.0,
  "DamageCalculator": {
    "Type": "Dps",
    "BaseDamage": {
      "Poison": 2
    },
    "RandomPercentageModifier": 0.25
  },
  "DamageCalculatorCooldown": 2
}
```

**Damage per tick:** 1.5-2.5 (2 ± 25%)
**Total over 10s:** 15-25 damage (5 ticks × 1.5-2.5)

## Tips for Min/Max Ranges

1. **Damage variance** - Use `RandomPercentageModifier` for damage ranges (0.1-0.2 typical)
2. **Quantity ranges** - Use `QuantityMin`/`QuantityMax` for item quantities
3. **Consistent damage** - Lower variance (0.05-0.1) for reliable weapons
4. **Variable damage** - Higher variance (0.2-0.25) for chaotic/unstable attacks
5. **Guaranteed drops** - Set `QuantityMin = QuantityMax` for fixed quantities
6. **Chance drops** - Use `QuantityMin: 0, QuantityMax: 1` for 50% chance
7. **Calculating ranges** - Use formulas above to convert between formats
8. **Multiple damage types** - Each type varies independently with `RandomPercentageModifier`

## Range Quick Reference

### Damage Ranges

| Base | Modifier | Min | Max | Range |
|------|----------|-----|-----|-------|
| 10 | 0.1 | 9 | 11 | ±10% |
| 20 | 0.15 | 17 | 23 | ±15% |
| 50 | 0.2 | 40 | 60 | ±20% |
| 100 | 0.25 | 75 | 125 | ±25% |

### Quantity Ranges

| Min | Max | Result |
|-----|-----|--------|
| 1 | 1 | Always 1 |
| 0 | 1 | 50% chance |
| 1 | 2 | 1-2 (random) |
| 5 | 10 | 5-10 (random) |

---

**Previous:** [ResetCooldown](156_Interaction_Type_ResetCooldown.md) | **Next:** Back to [README](../README.md)
