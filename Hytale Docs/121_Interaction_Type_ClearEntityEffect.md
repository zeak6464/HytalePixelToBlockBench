# Interaction Type: ClearEntityEffect

Remove status effects from entities.

## Overview

`ClearEntityEffect` removes specific status effects from entities. Useful for cleansing debuffs, removing old buffs before applying new ones, or curing conditions.

## Example from Game Files

### Clear Entity Effect Interaction

Clear entity effect interactions remove status effects from entities. These are commonly used to remove buffs, debuffs, or other temporary status modifications.

## Basic Structure

```json
{
  "Type": "ClearEntityEffect",
  "EntityEffectId": "Burn",
  "Entity": "Target"
}
```

## Properties

### EntityEffectId

```json
{
  "EntityEffectId": "Burn"
}
```

ID of the effect to remove. Must match the effect definition ID.

### Entity

```json
{
  "Entity": "Target"
}
```

Target entity:
- **`"Target"`** - Selected entity (from Selector)
- **`"Self"`** - Entity performing interaction
- Omit for self

## Complete Examples

### Example 1: Clear Debuff

```json
{
  "Type": "ClearEntityEffect",
  "EntityEffectId": "Poison"
}
```

Removes Poison effect from self.

### Example 2: Clear Multiple Effects

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "ClearEntityEffect",
      "EntityEffectId": "Meat_Buff_T1"
    },
    {
      "Type": "ClearEntityEffect",
      "EntityEffectId": "Meat_Buff_T2"
    },
    {
      "Type": "ApplyEffect",
      "EffectId": "Meat_Buff_T3"
    }
  ]
}
```

Clears tier 1 and 2 buffs, then applies tier 3.

### Example 3: Target Entity

```json
{
  "Type": "ClearEntityEffect",
  "EntityEffectId": "Freeze",
  "Entity": "Target"
}
```

Removes Freeze from targeted entity.

### Example 4: Cure Condition

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "ClearEntityEffect",
      "EntityEffectId": "Potion_Health_Lesser_Regen"
    },
    {
      "Type": "ClearEntityEffect",
      "EntityEffectId": "Potion_Health_Greater_Regen"
    },
    {
      "Type": "ClearEntityEffect",
      "EntityEffectId": "Potion_Stamina_Regen"
    },
    {
      "Type": "ApplyEffect",
      "EffectId": "Potion_Health_Regen_Cure",
      "Duration": 30.0
    }
  ]
}
```

Clears old health/stamina regen, applies cure effect.

## Tips

1. **Effect IDs** - Must match exactly (case-sensitive)
2. **Multiple effects** - Use Serial to clear multiple effects
3. **Replacement pattern** - Clear old effects before applying new ones
4. **Target entity** - Use "Target" to remove effects from selected entities

---

**Previous:** [ApplyEffect](120_Interaction_Type_ApplyEffect.md) | **Next:** [ChangeStat](122_Interaction_Type_ChangeStat.md)
