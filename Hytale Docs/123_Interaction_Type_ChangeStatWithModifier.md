# Interaction Type: ChangeStatWithModifier

Modify entity stats with interaction modifiers applied.

## Overview

`ChangeStatWithModifier` modifies entity stats like `ChangeStat`, but applies interaction modifiers (from `InteractionModifierId`) to the values. Useful for abilities that scale with modifiers.

## Basic Structure

```json
{
  "Type": "ChangeStatWithModifier",
  "StatModifiers": {
    "Stamina": -2
  },
  "ValueType": "Absolute",
  "InteractionModifierId": "Dodge"
}
```

## Properties

### StatModifiers

```json
{
  "StatModifiers": {
    "Stamina": -2,
    "Mana": -10
  }
}
```

Map of stat names to base values (before modifiers).

### ValueType

```json
{
  "ValueType": "Absolute"
}
```

Value type:
- **`"Absolute"`** - Raw value
- **`"Percentage"`** - Percentage of max stat

### InteractionModifierId

```json
{
  "InteractionModifierId": "Dodge"
}
```

ID of interaction modifier to apply. Modifiers adjust the stat change based on item/enchantment bonuses.

## Complete Examples

### Example 1: Stamina Cost

```json
{
  "Type": "ChangeStatWithModifier",
  "StatModifiers": {
    "Stamina": -2
  },
  "ValueType": "Absolute",
  "InteractionModifierId": "Dodge"
}
```

Reduces stamina by 2, modified by Dodge interaction modifier.

### Example 2: With Serial

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "ChangeStatWithModifier",
      "StatModifiers": {
        "Stamina": -2
      },
      "ValueType": "Absolute",
      "InteractionModifierId": "Dodge"
    },
    {
      "Type": "ChangeStat",
      "StatModifiers": {
        "StaminaRegenDelay": -0.7
      },
      "Behaviour": "Set",
      "ValueType": "Absolute"
    }
  ]
}
```

Reduces stamina with modifier, then sets regen delay.

## Tips

1. **Interaction modifiers** - Use when stat cost should scale with item/enchantment bonuses
2. **Base values** - Specify base values, modifiers adjust them
3. **Combine with ChangeStat** - Use both for complex stat changes
4. **Modifier IDs** - Match `InteractionModifierId` defined in interaction modifiers

---

**Previous:** [ChangeStat](122_Interaction_Type_ChangeStat.md) | **Next:** [ApplyForce](124_Interaction_Type_ApplyForce.md)
