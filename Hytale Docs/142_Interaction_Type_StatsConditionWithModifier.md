# Interaction Type: StatsConditionWithModifier

Check and consume stats with interaction modifiers applied.

## Overview

`StatsConditionWithModifier` checks stats like `StatsCondition`, but applies interaction modifiers (from `InteractionModifierId`) to the costs. Useful for abilities that scale with modifiers.

## Basic Structure

```json
{
  "Type": "StatsConditionWithModifier",
  "Costs": {
    "Stamina": 2
  },
  "InteractionModifierId": "Dodge",
  "Next": {
    "Type": "Serial",
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "Dodge_Invulnerability"
      }
    ]
  },
  "Failed": "Stamina_Bar_Flash"
}
```

## Properties

### Costs

```json
{
  "Costs": {
    "Stamina": 2,
    "Mana": 10
  }
}
```

Map of stat names to base costs (before modifiers).

### InteractionModifierId

```json
{
  "InteractionModifierId": "Dodge"
}
```

ID of interaction modifier to apply. Modifiers adjust the stat cost based on item/enchantment bonuses.

### Next

```json
{
  "Next": {
    "Type": "Serial"
  }
}
```

Interaction to execute if condition passes.

### Failed

```json
{
  "Failed": "Stamina_Bar_Flash"
}
```

Interaction to execute if condition fails.

## Complete Examples

### Example 1: Dodge Cost

```json
{
  "Type": "StatsConditionWithModifier",
  "Costs": {
    "Stamina": 2
  },
  "InteractionModifierId": "Dodge",
  "Next": {
    "Type": "Serial",
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "Dodge_Invulnerability"
      },
      {
        "Type": "ApplyEffect",
        "EffectId": "Dodge_Right"
      },
      {
        "Type": "ApplyForce",
        "Direction": { "X": 1, "Y": 0, "Z": 0 },
        "Force": 13
      }
    ]
  },
  "Failed": "Stamina_Bar_Flash"
}
```

Checks stamina cost (modified by Dodge modifier), executes dodge if passed.

### Example 2: Spell Cost

```json
{
  "Type": "StatsConditionWithModifier",
  "Costs": {
    "Mana": 50
  },
  "InteractionModifierId": "Spell_Cost",
  "Next": {
    "Type": "LaunchProjectile",
    "ProjectileId": "Fireball"
  },
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.spells.insufficient.mana"
  }
}
```

Checks mana cost (modified by Spell_Cost modifier).

## Tips

1. **Interaction modifiers** - Use when stat cost should scale with item/enchantment bonuses
2. **Base costs** - Specify base costs, modifiers adjust them
3. **Modifier IDs** - Match `InteractionModifierId` defined in interaction modifiers
4. **Failed feedback** - Provide visual/audio feedback on failure

---

**Previous:** [StatsCondition](141_Interaction_Type_StatsCondition.md) | **Next:** [EffectCondition](143_Interaction_Type_EffectCondition.md)
