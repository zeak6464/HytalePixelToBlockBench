# Interaction Type: Consume

Consume items (food, potions, consumables).

## Overview

`Consume` handles item consumption mechanics like eating food or drinking potions. Typically combines item removal with effect application.

## Example from Game Files

### Consume Interaction

From `Server/Item/Interactions/Consumables/Consume_Charge_Food_T1.json`:

Consume interactions consume items from inventories. These are used for food, potions, consumables, and item consumption mechanics.

## Basic Structure

```json
{
  "Type": "Consume"
}
```

## Notes

**Note:** `Consume` appears to be a simplified interaction type. In practice, consumption is often handled by combining `ModifyInventory` (remove item) with `ApplyEffect` (apply effects). 

For food/potions, see:
- [ApplyEffect](120_Interaction_Type_ApplyEffect.md) - Apply status effects
- [ModifyInventory](135_Interaction_Type_ModifyInventory.md) - Remove consumed items
- [Entity Effects](96_Entity_Effects.md) - Effect system documentation

## Alternative Pattern

Instead of `Consume`, use:

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "ModifyInventory",
      "Operations": [
        {
          "Type": "Remove",
          "ItemId": "Food_Bread",
          "Quantity": 1
        }
      ]
    },
    {
      "Type": "ApplyEffect",
      "EffectId": "Food_Health_Restore",
      "Duration": 1.0
    }
  ]
}
```

This removes the item and applies the effect.

---

**Previous:** [RefillContainer](136_Interaction_Type_RefillContainer.md) | **Next:** [SendMessage](138_Interaction_Type_SendMessage.md)
