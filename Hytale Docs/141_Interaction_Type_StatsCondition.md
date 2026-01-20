# Interaction Type: StatsCondition

Check and consume entity stats before executing interactions.

## Overview

`StatsCondition` checks if entity has required stats (Health, Mana, Stamina) and optionally consumes them. Executes `Next` if condition passes, `Failed` if it fails.

## Example from Game Files

### Stats Condition Interaction

Stats condition interactions check entity statistics before proceeding. These are used for conditional interactions based on health, mana, stamina, or other stats.

## Basic Structure

```json
{
  "Type": "StatsCondition",
  "Costs": {
    "Mana": 50,
    "Stamina": 10
  },
  "ValueType": "Absolute",
  "LessThan": false,
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

## Properties

### Costs

```json
{
  "Costs": {
    "Health": 100,
    "Mana": 50,
    "Stamina": 10
  }
}
```

Map of stat names to required values. All stats must pass check.

### ValueType

```json
{
  "ValueType": "Absolute"
}
```

Value type:
- **`"Absolute"`** - Raw value
- **`"Percent"`** - Percentage of max stat

### LessThan

```json
{
  "LessThan": true
}
```

If `true`, checks if stat is less than value (for reverse checks). If `false` or omitted, checks if stat is greater than or equal to value.

### Next

```json
{
  "Next": {
    "Type": "LaunchProjectile"
  }
}
```

Interaction to execute if condition passes.

### Failed

```json
{
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.spells.insufficient.mana"
  }
}
```

Interaction to execute if condition fails.

## Complete Examples

### Example 1: Mana Check

```json
{
  "Type": "StatsCondition",
  "Costs": {
    "Mana": 50
  },
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

Checks if mana >= 50, casts fireball if true.

### Example 2: Percentage Check

```json
{
  "Type": "StatsCondition",
  "Costs": {
    "Health": 100
  },
  "ValueType": "Percent",
  "LessThan": true,
  "Next": "Consume_Charge"
}
```

Checks if health < 100% (less than full).

### Example 3: Multiple Stats

```json
{
  "Type": "StatsCondition",
  "Costs": {
    "Mana": 50,
    "Stamina": 10
  },
  "Next": {
    "Type": "Serial",
    "Interactions": [
      {
        "Type": "ChangeStat",
        "StatModifiers": {
          "Mana": -50,
          "Stamina": -10
        }
      },
      {
        "Type": "LaunchProjectile",
        "ProjectileId": "Fireball"
      }
    ]
  }
}
```

Checks both stats, then consumes them and casts.

## Tips

1. **Costs** - All stats in Costs must pass for condition to succeed
2. **Consumption** - This only checks; use ChangeStat to actually consume
3. **LessThan** - Use for reverse checks (health < threshold)
4. **Failed branches** - Always provide Failed for better UX
5. **Percentage** - Use "Percent" for scaling effects based on max stat

---

**Previous:** [FirstClick](140_Interaction_Type_FirstClick.md) | **Next:** [StatsConditionWithModifier](142_Interaction_Type_StatsConditionWithModifier.md)
