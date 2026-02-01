# Interaction Type: SpawnDrops

Spawn item drops at target location using drop tables.

## Overview

`SpawnDrops` spawns items from a drop table at a specified location. This is primarily used internally by the engine for block breaking and NPC death drops.

## Note on Usage

This interaction type is not commonly used directly in item interactions. Item drops are typically handled through:

1. **Block Gathering** - Drops configured in block definitions
2. **NPC Death** - Drops configured in NPC definitions
3. **Harvest Interactions** - Using `BreakBlock` with harvest mode

## Alternative: Using Drop Tables with BreakBlock

For harvest drops:

```json
{
  "Type": "BreakBlock",
  "GatherType": "Harvest",
  "DropList": "Drops_Plant_Crop_Wheat"
}
```

## Alternative: Direct Item Spawning

For direct item drops, use `ModifyInventory` to add to player inventory:

```json
{
  "Type": "ModifyInventory",
  "ItemsToAdd": [
    { "Id": "Gold_Coin", "Quantity": 5 }
  ]
}
```

## Drop Table Configuration

Drop tables are configured in `Server/Drops/`:

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Plant_Crop_Wheat_Item",
          "QuantityMin": 1,
          "QuantityMax": 3
        }
      },
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Plant_Seeds_Wheat",
          "QuantityMin": 0,
          "QuantityMax": 2,
          "Chance": 0.5
        }
      }
    ]
  }
}
```

See [Drop Tables](105_Drop_Tables.md) for complete documentation.

## Tips

1. **Use BreakBlock** - For most drop scenarios, use block breaking
2. **Use ModifyInventory** - For direct player rewards
3. **Configure Drop Tables** - Create drop tables in `Server/Drops/`
4. **NPC Drops** - Configure in NPC role definitions

---

**Previous:** [SpawnPrefab](152_Interaction_Type_SpawnPrefab.md) | **Next:** [UseEntity](154_Interaction_Type_UseEntity.md)
