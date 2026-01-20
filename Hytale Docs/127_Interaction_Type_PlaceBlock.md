# Interaction Type: PlaceBlock

Place blocks in the world from items.

## Overview

`PlaceBlock` places blocks in the world when used from an item. Handles item removal, placement validation, and block state initialization.

## Example from Game Files

### Place Block Interaction

Place block interactions place blocks in the world. These are used for building, crafting placements, and block-based mechanics.

## Basic Structure

```json
{
  "Type": "PlaceBlock",
  "BlockTypeToPlace": "Rock_Stone",
  "RemoveItemInHand": false,
  "RunTime": 0.5
}
```

## Properties

### BlockTypeToPlace

```json
{
  "BlockTypeToPlace": "Rock_Stone"
}
```

ID of block type to place. Must match block item definition ID.

### RemoveItemInHand

```json
{
  "RemoveItemInHand": false
}
```

If `true`, removes item from inventory after placement. If `false`, keeps item.

### RunTime

```json
{
  "RunTime": 0.5
}
```

Delay in seconds before placement completes.

## Complete Examples

### Example 1: Basic Placement

```json
{
  "Type": "PlaceBlock",
  "BlockTypeToPlace": "Rock_Stone",
  "RemoveItemInHand": false,
  "RunTime": 0.5
}
```

Places stone block, keeps item.

### Example 2: Seed Placement

```json
{
  "Type": "PlaceBlock",
  "BlockTypeToPlace": "Plant_Crop_Carrot_Seed",
  "RemoveItemInHand": true
}
```

Places seed, removes item from inventory.

### Example 3: With Animation

```json
{
  "Type": "Simple",
  "Effects": {
    "ItemAnimationId": "Place",
    "WorldSoundEventId": "SFX_Place_Block"
  },
  "RunTime": 0.3,
  "Next": {
    "Type": "PlaceBlock",
    "BlockTypeToPlace": "Furniture_Chair",
    "RemoveItemInHand": true,
    "RunTime": 0.2
  }
}
```

Plays placement animation, then places block.

## Tips

1. **RemoveItemInHand** - Set to `true` for consumable blocks (seeds), `false` for tools
2. **RunTime** - Use for placement delays (0.3-0.5s typical)
3. **Block IDs** - Must match block item definition IDs exactly
4. **Combine with Simple** - Add placement animations/sounds before placing

---

**Previous:** [Heal](126_Interaction_Type_Heal.md) | **Next:** [ChangeBlock](128_Interaction_Type_ChangeBlock.md)
