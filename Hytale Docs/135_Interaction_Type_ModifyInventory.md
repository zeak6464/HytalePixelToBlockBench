# Interaction Type: ModifyInventory

Add, remove, or equip items in entity inventories.

## Overview

`ModifyInventory` modifies entity inventories by adding, removing, or equipping items. Useful for item consumption, rewards, trading, and inventory management.

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

## Basic Structure

```json
{
  "Type": "ModifyInventory",
  "Operations": [
    {
      "Type": "Remove",
      "ItemId": "Potion_Health_Lesser",
      "Quantity": 1
    },
    {
      "Type": "Add",
      "ItemId": "Empty_Bottle",
      "Quantity": 1
    }
  ]
}
```

## Properties

### Operations

```json
{
  "Operations": [
    {
      "Type": "Remove",
      "ItemId": "Potion_Health_Lesser",
      "Quantity": 1
    },
    {
      "Type": "Add",
      "ItemId": "Empty_Bottle",
      "Quantity": 1
    },
    {
      "Type": "Equip",
      "ItemId": "Weapon_Sword_Iron",
      "Slot": "Primary"
    }
  ]
}
```

Array of inventory operations:
- **`"Remove"`** - Remove items from inventory
- **`"Add"`** - Add items to inventory
- **`"Equip"`** - Equip items to slots

### Operation Properties

**Remove/Add:**
- **`ItemId`** - Item to remove/add
- **`Quantity`** - Number of items

**Equip:**
- **`ItemId`** - Item to equip
- **`Slot`** - Equipment slot ("Primary", "Secondary", "Head", etc.)

## Complete Examples

### Example 1: Consume Item

```json
{
  "Type": "ModifyInventory",
  "Operations": [
    {
      "Type": "Remove",
      "ItemId": "Food_Bread",
      "Quantity": 1
    }
  ]
}
```

Removes bread from inventory (consumes item).

### Example 2: Potion with Return

```json
{
  "Type": "ModifyInventory",
  "Operations": [
    {
      "Type": "Remove",
      "ItemId": "Potion_Health_Lesser",
      "Quantity": 1
    },
    {
      "Type": "Add",
      "ItemId": "Empty_Bottle",
      "Quantity": 1
    }
  ]
}
```

Consumes potion, returns empty bottle.

### Example 3: Reward

```json
{
  "Type": "ModifyInventory",
  "Operations": [
    {
      "Type": "Add",
      "ItemId": "Gold_Coin",
      "Quantity": 10
    },
    {
      "Type": "Add",
      "ItemId": "Weapon_Sword_Iron",
      "Quantity": 1
    }
  ]
}
```

Adds 10 gold coins and a sword to inventory.

### Example 4: Equip Item

```json
{
  "Type": "ModifyInventory",
  "Operations": [
    {
      "Type": "Equip",
      "ItemId": "Weapon_Sword_Iron",
      "Slot": "Primary"
    }
  ]
}
```

Equips sword to primary hand.

## Tips

1. **Multiple operations** - Can chain multiple operations in one interaction
2. **Quantity** - Use appropriate quantities (1 for consumables, multiple for rewards)
3. **Item IDs** - Must match item definition IDs exactly
4. **Equip slots** - Use correct slot names ("Primary", "Secondary", "Head", etc.)

---

**Previous:** [Projectile](134_Interaction_Type_Projectile.md) | **Next:** [RefillContainer](136_Interaction_Type_RefillContainer.md)
