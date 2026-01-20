# Item Categories

Learn how item categories organize items in creative libraries and crafting interfaces.

## Overview

Item categories define how items are organized in creative libraries and crafting benches. They help players find items quickly and organize crafting recipes.

## Location
`Server/Item/Category/`

## Example from Game Files

### Creative Library Blocks Category

From `Server/Item/Category/CreativeLibrary/Blocks.json`:

```1:46:Server/Item/Category/CreativeLibrary/Blocks.json
{
  "Icon": "Icons/ItemCategories/Natural.png",
  "Order": 0,
  "Children": [
    {
      "Id": "Rocks",
      "Name": "server.ui.itemcategory.rocks",
      "Icon": "Icons/ItemCategories/Blocks.png"
    },
    {
      "Id": "Structural",
      "Name": "server.ui.itemcategory.structural",
      "Icon": "Icons/ItemCategories/Build-Roofs.png"
    },
    {
      "Id": "Soils",
      "Name": "server.ui.itemcategory.soils",
      "Icon": "Icons/ItemCategories/Soil.png"
    },
    {
      "Id": "Ores",
      "Name": "server.ui.itemcategory.ores",
      "Icon": "Icons/ItemCategories/Natural-Ore.png"
    },
    {
      "Id": "Plants",
      "Name": "server.ui.itemcategory.plants",
      "Icon": "Icons/ItemCategories/Natural-Vegetal.png"
    },
    {
      "Id": "Fluids",
      "Name": "server.ui.itemcategory.fluids",
      "Icon": "Icons/ItemCategories/Natural-Fluid.png"
    },
    {
      "Id": "Portals",
      "Name": "server.ui.itemcategory.portals",
      "Icon": "Icons/ItemCategories/Portal.png"
    },
    {
      "Id": "Deco",
      "Name": "server.ui.itemcategory.deco",
      "Icon": "Icons/ItemCategories/Natural-Fire.png"
    }
  ]
}
```

This shows an item category configuration for the creative library with subcategories like Rocks, Structural, Soils, Ores, Plants, Fluids, Portals, and Deco.

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
