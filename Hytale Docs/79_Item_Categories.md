# Item Categories

Learn how item categories organize items in creative libraries and crafting interfaces.

## Overview

Item categories define how items are organized in creative libraries and crafting benches. They help players find items quickly and organize crafting recipes.

## Location
`Server/Item/Category/`

## Basic Category Structure

Create `Server/Item/Category/MyCustom/MyCustom_Items.json`:

```json
{
  "Items": [
    "Item_ID_1",
    "Item_ID_2",
    "Item_ID_3"
  ]
}
```

## Category Properties

### Items Array

```json
{
  "Items": [
    "Weapon_Sword_Iron",
    "Weapon_Sword_Steel",
    "Weapon_Axe_Iron"
  ]
}
```

Array of item IDs that belong to this category.

## Common Category Types

### Creative Library Categories

- **`CreativeLibrary/Blocks`** - All placeable blocks
- **`CreativeLibrary/Furniture`** - Furniture items
- **`CreativeLibrary/Items`** - General items
- **`CreativeLibrary/Tool`** - Tools and equipment

### Fieldcraft Categories

- **`Fieldcraft/Tools`** - Gathering and crafting tools

## Category Examples

### Blocks Category

`Server/Item/Category/CreativeLibrary/Blocks.json`:

Organizes all blocks for creative library.

### Furniture Category

`Server/Item/Category/CreativeLibrary/Furniture.json`:

Organizes furniture items.

## Using Categories

Categories are referenced by:
- Creative library organization
- Crafting bench categories
- Item filtering systems

## Tips for Item Categories

1. **Logical organization** - Group items by function or type
2. **Creative libraries** - Use for organizing creative mode items
3. **Crafting benches** - Categories match bench category IDs
4. **Consistency** - Use consistent category names across mods

---

**Previous:** [Item Groups](78_Item_Groups.md) | **Next:** Back to [README](../README.md)
