# Interaction Type: ModifyInventory

Add, remove, or equip items in entity inventories.

## Overview

`ModifyInventory` modifies entity inventories. Useful for item consumption and rewards.

> **Note:** The `Operations` array structure shown in some documentation is **fabricated**. Actual game files use simpler properties.

## Example from Game Files

### Modify Inventory Interaction

From `Server/Item/Interactions/Tests/ModifyInventory.json`:

```1:4:Server/Item/Interactions/Tests/ModifyInventory.json
{
  "Type": "ModifyInventory",
  "AdjustHeldItemQuantity": -1
}
```

This shows a modify inventory interaction that reduces the quantity of the held item by 1 (consuming it).

## Actual Properties (From Game Files)

### AdjustHeldItemQuantity

```json
{
  "Type": "ModifyInventory",
  "AdjustHeldItemQuantity": -1
}
```

Reduces the held item stack by the specified amount (-1 = consume one item).

### ItemToRemove

```json
{
  "Type": "ModifyInventory",
  "ItemToRemove": {
    "Id": "Potion_Health",
    "Quantity": 1
  }
}
```

Removes a specific item from inventory.

### AdjustHeldItemDurability

```json
{
  "Type": "ModifyInventory",
  "AdjustHeldItemDurability": -0.5
}
```

Reduces the held item's durability.

## Complete Examples

### Example 1: Consume Held Item

```json
{
  "Type": "ModifyInventory",
  "AdjustHeldItemQuantity": -1
}
```

Reduces held item stack by 1 (standard consumption pattern).

### Example 2: Remove Specific Item

```json
{
  "Type": "ModifyInventory",
  "ItemToRemove": {
    "Id": "Potion_Health_Lesser",
    "Quantity": 1
  }
}
```

Removes a specific potion from inventory.

### Example 3: Reduce Durability

```json
{
  "Type": "ModifyInventory",
  "AdjustHeldItemDurability": -10
}
```

Reduces held item durability by 10 points.

### Example 4: Consume with Effect (Serial)

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "ModifyInventory",
      "AdjustHeldItemQuantity": -1
    },
    {
      "Type": "ChangeStat",
      "EntityStatId": "Health",
      "Amount": 50
    }
  ]
}
```

Consumes item and restores health (typical food/potion pattern).

## Tips

1. **Use AdjustHeldItemQuantity** - Standard way to consume held items
2. **Use ItemToRemove** - For removing specific items from inventory
3. **Combine with Serial** - Chain with other interactions for effects
4. **Item IDs** - Must match item definition IDs exactly

---

**Previous:** [Projectile](134_Interaction_Type_Projectile.md) | **Next:** [RefillContainer](136_Interaction_Type_RefillContainer.md)
