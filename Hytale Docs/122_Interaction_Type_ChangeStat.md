# Interaction Type: ChangeStat

Directly modify entity stats with set/add/subtract behaviors.

## Overview

`ChangeStat` directly modifies entity stats like Health, Mana, Stamina, or custom stats. Supports absolute (set), relative (add/subtract), and percentage-based changes.

## Basic Structure

```json
{
  "Type": "ChangeStat",
  "StatModifiers": {
    "Mana": -25
  },
  "Behaviour": "Set",
  "ValueType": "Absolute"
}
```

## Properties

### StatModifiers

```json
{
  "StatModifiers": {
    "Health": 50,
    "Mana": -25,
    "Stamina": 100
  }
}
```

Map of stat names to values to modify.

### Behaviour

```json
{
  "Behaviour": "Set"
}
```

How to modify the stat:
- **`"Set"`** - Set stat to value
- **`"Add"`** - Add value to stat
- **`"Subtract"`** - Subtract value from stat

### ValueType

```json
{
  "ValueType": "Absolute"
}
```

Value type:
- **`"Absolute"`** - Raw value
- **`"Percentage"`** - Percentage of max stat
- **`"PercentageCurrent"`** - Percentage of current stat

## Complete Examples

### Example 1: Consume Mana

```json
{
  "Type": "ChangeStat",
  "StatModifiers": {
    "Mana": -25
  }
}
```

Reduces mana by 25 (defaults to Subtract).

### Example 2: Restore Health

```json
{
  "Type": "ChangeStat",
  "StatModifiers": {
    "Health": 50
  },
  "Behaviour": "Add"
}
```

Adds 50 health.

### Example 3: Set Stamina Regen Delay

```json
{
  "Type": "ChangeStat",
  "StatModifiers": {
    "StaminaRegenDelay": -0.7
  },
  "Behaviour": "Set",
  "ValueType": "Absolute"
}
```

Sets stamina regen delay to -0.7.

### Example 4: Percentage Healing

```json
{
  "Type": "ChangeStat",
  "StatModifiers": {
    "Health": 0.5
  },
  "Behaviour": "Add",
  "ValueType": "Percentage"
}
```

Adds 50% of max health.

## Tips

1. **Negative values** - Use negative for consumption (mana cost, stamina cost)
2. **Set vs Add** - Use "Set" for exact values, "Add" for relative changes
3. **Percentage** - Use "Percentage" for scaling effects (heal 50% max health)
4. **Custom stats** - Works with custom stats defined in entity stats

---

**Previous:** [ClearEntityEffect](121_Interaction_Type_ClearEntityEffect.md) | **Next:** [ChangeStatWithModifier](123_Interaction_Type_ChangeStatWithModifier.md)
