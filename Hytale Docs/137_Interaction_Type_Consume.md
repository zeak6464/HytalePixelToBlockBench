# Interaction Type: Consume

Consume items (food, potions, consumables).

## Overview

> **Important:** The `"Type": "Consume"` interaction does NOT exist in game files. Consumption is handled through other interaction types.

In actual game files, consumption is handled by:
1. **`ModifyInventory`** with `AdjustHeldItemQuantity: -1` to remove the item
2. **`ApplyEffect`** to apply any effects (health restore, buffs, etc.)
3. **`ChangeStat`** for direct stat changes

## How Consumption Actually Works 

For food/potions, see:
- [ApplyEffect](120_Interaction_Type_ApplyEffect.md) - Apply status effects
- [ModifyInventory](135_Interaction_Type_ModifyInventory.md) - Remove consumed items
- [Entity Effects](96_Entity_Effects.md) - Effect system documentation

## Correct Pattern

Instead of `Consume`, use:

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "ModifyInventory",
      "AdjustHeldItemQuantity": -1
    },
    {
      "Type": "ApplyEffect",
      "EffectId": "Food_Health_Restore"
    }
  ]
}
```

This consumes one of the held item and applies the effect.

---

**Previous:** [RefillContainer](136_Interaction_Type_RefillContainer.md) | **Next:** [SendMessage](138_Interaction_Type_SendMessage.md)
