# Inventory Operations

Learn how to manipulate entity inventories using interactions and NPC actions.

## Overview

Inventory operations allow interactions and NPC behaviors to add items, remove items, equip gear, and modify entity inventories dynamically.

## Location
- Item interactions: `Inventory` interaction type
- NPC actions: `Type: "Inventory"` in action lists

## Example from Game Files

### Modify Inventory

From `Server/Item/Interactions/Tests/ModifyInventory.json`:

```1:4:Server/Item/Interactions/Tests/ModifyInventory.json
{
  "Type": "ModifyInventory",
  "AdjustHeldItemQuantity": -1
}
```

This shows a modify inventory interaction that reduces the quantity of the held item by 1 (consuming it).

## Basic Inventory Interaction

```json
{
  "Type": "Inventory",
  "Operation": "AddItem",
  "ItemId": "Ore_Iron",
  "Quantity": 5
}
```

Adds items to entity inventory.

## Inventory Operations

### AddItem

```json
{
  "Type": "Inventory",
  "Operation": "AddItem",
  "ItemId": "Ore_Iron",
  "Quantity": 5
}
```

Adds items to inventory.

### RemoveItem

```json
{
  "Type": "Inventory",
  "Operation": "RemoveItem",
  "ItemId": "Ore_Iron",
  "Quantity": 2
}
```

Removes items from inventory.

### EquipHotbar

```json
{
  "Type": "Inventory",
  "Operation": "EquipHotbar",
  "Slot": 0
}
```

Equips item from hotbar slot.

### EquipOffHand

```json
{
  "Type": "Inventory",
  "Operation": "EquipOffHand",
  "Slot": { "Compute": "DefaultOffHandSlot" }
}
```

Equips item from off-hand slot.

## NPC Inventory Actions

NPCs can use inventory operations in action lists:

```json
{
  "Type": "Inventory",
  "Operation": "EquipHotbar",
  "Slot": { "Compute": "DefaultHotbarSlot" }
}
```

NPC equips weapon from hotbar.

## State Transition Inventory

Common pattern in NPC state transitions:

```json
{
  "StateTransitions": [
    {
      "States": [
        {
          "From": ["Idle"],
          "To": ["Combat"]
        }
      ],
      "Actions": [
        {
          "Type": "Inventory",
          "Operation": "EquipHotbar",
          "Slot": { "Compute": "DefaultHotbarSlot" }
        }
      ]
    }
  ]
}
```

NPC equips weapon when entering combat.

## Tips for Inventory Operations

1. **NPC behavior** - Use in state transitions to equip/unequip items
2. **Item rewards** - Use `AddItem` for quest rewards or loot
3. **Item costs** - Use `RemoveItem` for item-consuming interactions
4. **Compute** - Use `{ "Compute": "ParameterName" }` for dynamic slot selection

---

**Previous:** [Entity Effects](96_Entity_Effects.md) | **Next:** [Model Attachments](98_Model_Attachments.md)
