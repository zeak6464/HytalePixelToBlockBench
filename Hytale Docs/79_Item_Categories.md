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

## Complete List of Item Categories

All item categories available in the game (from `server.lang`):

### Building & Construction
- **`benches`** - Crafting benches and workstations
- **`containers`** - Storage containers and chests
- **`furniture`** - Decorative furniture items
- **`doors`** - Doors and gates
- **`lighting`** - Light sources (torches, lamps, etc.)
- **`beds`** - Beds and sleeping items
- **`shelves`** - Shelf blocks for storage/display
- **`signs`** - Signs and markers
- **`structural`** - Structural building blocks

### Natural Blocks
- **`rocks`** - Stone and rock blocks
- **`soils`** - Dirt, grass, sand blocks
- **`ores`** - Ore blocks
- **`plants`** - Plant blocks and foliage
- **`fluids`** - Water and other fluid blocks
- **`deco`** - Decorative natural items

### Equipment & Combat
- **`tools`** - Pickaxes, axes, hoes, shovels
- **`weapons`** - Swords, bows, maces, daggers
- **`armors`** - Armor pieces (head, chest, hands, legs)

### Consumables
- **`foods`** - Food items
- **`potions`** - Potions and elixirs
- **`recipes`** - Recipe books and scrolls

### Special Items
- **`portals`** - Portal devices and items
- **`ingredients`** - Crafting ingredients and materials

### Builder/Creative Mode
- **`buildertool`** - Builder tools for creative mode
- **`scriptedbrushes`** - Scripted brushes for advanced building
- **`buildertoolsecondpage`** - Additional builder tools
- **`brushfilters`** - Brush filters for building
- **`machinima`** - Machinima and cinematic tools
- **`block`** - General block category

## Crafting Bench Categories

Categories used in crafting benches (from `benchCategories`):

### Workbench Categories
- **`workbench.crafting`** - General crafting recipes
- **`workbench.survival`** - Survival items
- **`workbench.housing`** - Housing items
- **`workbench.tools`** - Tool crafting
- **`workbench.tinkering`** - Advanced tinkering recipes

### Furniture Bench Categories
- **`furniture.seasonal`** - Seasonal furniture
- **`furniture.storage`** - Storage furniture
- **`furniture.lighting`** - Lighting furniture
- **`furniture.beds`** - Beds
- **`furniture.pottery`** - Pottery items
- **`furniture.textiles`** - Textile items
- **`furniture.village_walls`** - Wooden walls
- **`furniture.misc`** - Miscellaneous furniture

### Farming Bench Categories
- **`farmingbench.farming`** - Farming items
- **`farmingbench.decorative`** - Decorative farming items
- **`farmingbench.seeds`** - Seeds
- **`farmingbench.planters`** - Planter boxes
- **`farmingbench.saplings`** - Tree saplings
- **`farmingbench.essence`** - Essence of Life items

### Armor Bench Categories
- **`head`** - Helmets and head armor
- **`chest`** - Chest armor and cuirasses
- **`hands`** - Gauntlets and gloves
- **`legs`** - Leg armor and greaves
- **`shield`** - Shields

### Alchemy Bench Categories
- **`combatPotions`** - Combat potions
- **`miscPotions`** - Miscellaneous potions
- **`bombs`** - Bombs and explosives

### Food Preparation Categories
- **`prepared`** - Prepared foods
- **`baked`** - Baked goods
- **`ingredients`** - Food ingredients

### Weapon Smithing Categories
- **`sword`** - Swords
- **`mace`** - Maces
- **`battleaxe`** - Battleaxes
- **`daggers`** - Daggers
- **`bow`** - Bows

### General Categories
- **`portals`** - Portal items
- **`misc`** - Miscellaneous items
- **`all`** - All items

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
5. **Reference bench categories** - Use the exact category IDs from the bench categories list above
6. **Test in-game** - Verify items appear in correct creative menu tabs

## Related Guides

- [Creating Items](02_Items.md) - Complete bench categories reference
- [Asset Types Reference](173_Asset_Types_Reference.md) - All asset types
- [Builder Tools Guide](172_Builder_Tools_Guide.md) - Creative mode tools

---

**Previous:** [Item Groups](78_Item_Groups.md) | **Next:** [NPC State Transitions](80_NPC_State_Transitions.md)
