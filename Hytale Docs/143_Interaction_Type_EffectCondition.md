# Interaction Type: EffectCondition

Check if entity has specific status effects before executing interactions.

## Overview

`EffectCondition` checks if entity has specific status effects and branches based on the result. Supports "Any", "None", or "All" matching modes.

## Basic Structure

```json
{
  "Type": "EffectCondition",
  "EntityEffectIds": [
    "Meat_Buff_T3"
  ],
  "Match": "None",
  "Next": {
    "Type": "Serial",
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "Meat_Buff_T2"
      }
    ]
  },
  "Failed": {
    "Type": "Simple",
    "RunTime": 0
  }
}
```

## Properties

### EntityEffectIds

```json
{
  "EntityEffectIds": [
    "Meat_Buff_T3",
    "Poison",
    "Burn"
  ]
}
```

Array of effect IDs to check for.

### Match

```json
{
  "Match": "None"
}
```

Matching mode:
- **`"Any"`** - At least one effect must be present
- **`"None"`** - No effects should be present
- **`"All"`** - All effects must be present

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
  "Failed": {
    "Type": "Simple"
  }
}
```

Interaction to execute if condition fails.

## Complete Examples

### Example 1: Check for None

```json
{
  "Type": "EffectCondition",
  "EntityEffectIds": [
    "Meat_Buff_T3"
  ],
  "Match": "None",
  "Next": {
    "Type": "Serial",
    "Interactions": [
      {
        "Type": "ClearEntityEffect",
        "EntityEffectId": "Meat_Buff_T1"
      },
      {
        "Type": "ApplyEffect",
        "EffectId": "Meat_Buff_T2"
      }
    ]
  },
  "Failed": {
    "Type": "Simple",
    "RunTime": 0
  }
}
```

Applies tier 2 buff only if tier 3 not present.

### Example 2: Check for Any

```json
{
  "Type": "EffectCondition",
  "EntityEffectIds": [
    "Poison",
    "Burn"
  ],
  "Match": "Any",
  "Next": {
    "Type": "ApplyEffect",
    "EffectId": "Cleanse",
    "Duration": 1.0
  }
}
```

Applies cleanse if poisoned OR burned.

### Example 3: Check for All

```json
{
  "Type": "EffectCondition",
  "EntityEffectIds": [
    "Poison",
    "Slow"
  ],
  "Match": "All",
  "Next": {
    "Type": "ApplyEffect",
    "EffectId": "Combo_Debuff",
    "Duration": 5.0
  }
}
```

Applies combo debuff only if poisoned AND slowed.

## Tips

1. **Match modes** - Use "None" for tiered buffs, "Any" for cleansing, "All" for combo effects
2. **Effect IDs** - Must match effect definition IDs exactly
3. **Failed branches** - Provide Failed for better UX
4. **Tiered systems** - Use "None" to prevent overlapping buff tiers

---

**Previous:** [StatsConditionWithModifier](142_Interaction_Type_StatsConditionWithModifier.md) | **Next:** [PlacementCountCondition](144_Interaction_Type_PlacementCountCondition.md)
