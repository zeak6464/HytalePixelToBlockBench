# Resource Types

Learn how resource types work and how to use them in recipes and crafting systems.

## Overview

Resource types in Hytale allow items to be categorized and used generically in recipes. Instead of requiring specific items, recipes can accept any item with a matching resource type.

## Location
- Resource type definitions: `Server/Item/ResourceTypes/`
- Usage: `Server/Item/Items/` (in item definitions)

## Example from Game Files

### Wood All Resource Type

From `Server/Item/ResourceTypes/Wood_All.json`:

```1:4:Server/Item/ResourceTypes/Wood_All.json
{
  "Name": "Any Wood",
  "Icon": "Icons/ResourceTypes/Wood_Planks.png"
}
```

This shows a resource type that accepts any wood material in recipes.

## Basic Resource Type Structure

Create `Server/Item/ResourceTypes/MyCustom_Material.json`:

```json
{
  "Name": "Any type of MyCustom Material",
  "Icon": "Icons/ResourceTypes/MyCustom_Material.png"
}
```

## Resource Type Properties

### Name

```json
{
  "Name": "Any type of Raw Meat"
}
```

Display name shown in recipes when using resource types.

### Icon

```json
{
  "Icon": "Icons/ResourceTypes/Any_Meat.png"
}
```

Icon displayed in crafting UI for resource type requirements.

## Using Resource Types in Items

### Assigning Resource Types

In item definitions, assign resource types:

```json
{
  "ResourceTypes": [
    {
      "Id": "Metal_Bars",
      "Quantity": 1
    }
  ]
}
```

- **`Id`** - Resource type ID (matches filename in `ResourceTypes/`)
- **`Quantity`** - Quantity of this resource type (optional, defaults to 1)

### Multiple Resource Types

Items can have multiple resource types:

```json
{
  "ResourceTypes": [
    {
      "Id": "Metal_Bars"
    },
    {
      "Id": "Iron"  // Can be both generic and specific
    }
  ]
}
```

## Using Resource Types in Recipes

### Resource Type Requirements

Instead of specific item IDs, recipes can use resource types:

```json
{
  "Recipe": {
    "Input": [
      {
        "ResourceTypeId": "Metal_Bars",
        "Quantity": 5
      }
    ]
  }
}
```

Any item with `"Id": "Metal_Bars"` resource type can be used.

### Mixed Requirements

Recipes can mix item IDs and resource types:

```json
{
  "Recipe": {
    "Input": [
      {
        "ItemId": "Ingredient_Stick",
        "Quantity": 2
      },
      {
        "ResourceTypeId": "Metal_Bars",
        "Quantity": 3
      },
      {
        "ResourceTypeId": "Fabrics",
        "Quantity": 1
      }
    ]
  }
}
```

## Common Resource Types

### Metals

- **`Metal_Bars`** - All metal bars/ingots
- **`Copper_Iron_Bar`** - Copper and iron bars

### Materials

- **`Wood_All`** - All wood types
- **`Rock`** - Stone/rock materials
- **`Fabrics`** - Fabric materials
- **`Leathers`** - Leather materials

### Food

- **`Foods`** - General food items
- **`Meats`** - Raw or cooked meat
- **`Fruits`** - Fruit items
- **`Vegetables`** - Vegetable items

### Fish

- **`Fish`** - All fish types
- **`Fish_Common`** - Common fish
- **`Fish_Rare`** - Rare fish
- **`Fish_Epic`** - Epic fish
- **`Fish_Legendary`** - Legendary fish

### Resources

- **`Fuel`** - Fuel for furnaces/cooking
- **`Charcoal`** - Charcoal for fuel
- **`Bone`** - Bone materials
- **`Ice`** - Ice materials

## Complete Examples

### Example 1: Item with Resource Type

`Server/Item/Items/Armor/MyCustom/Armor_MyCustom_Chest.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Armor_MyCustom_Chest.name"
  },
  "ResourceTypes": [
    {
      "Id": "Metal_Bars",
      "Quantity": 1
    }
  ],
  "Icon": "Icons/ItemsGenerated/Armor_MyCustom_Chest.png"
}
```

### Example 2: Recipe Using Resource Type

```json
{
  "Recipe": {
    "Input": [
      {
        "ResourceTypeId": "Metal_Bars",
        "Quantity": 6
      },
      {
        "ResourceTypeId": "Leathers",
        "Quantity": 3
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Id": "Armor_Bench",
        "Categories": ["Armor_Chest"]
      }
    ],
    "TimeSeconds": 5
  }
}
```

This recipe accepts any metal bar and any leather.

### Example 3: Resource Type Definition

`Server/Item/ResourceTypes/MyCustom_Material.json`:

```json
{
  "Name": "Any type of MyCustom Material",
  "Icon": "Icons/ResourceTypes/MyCustom_Material.png"
}
```

### Example 4: Food with Resource Types

```json
{
  "TranslationProperties": {
    "Name": "server.items.Food_Beef_Raw.name"
  },
  "ResourceTypes": [
    {
      "Id": "Meats"
    },
    {
      "Id": "Foods"
    }
  ]
}
```

This item can be used in recipes requiring either `Meats` or `Foods`.

## Resource Type Tiers

Some resource types have quality tiers:

### Fish Tiers

- **`Fish_Common`** - Common fish
- **`Fish_Uncommon`** - Uncommon fish
- **`Fish_Rare`** - Rare fish
- **`Fish_Epic`** - Epic fish
- **`Fish_Legendary`** - Legendary fish

All also have base `"Fish"` resource type.

## Tips for Using Resource Types

1. **Create generic categories** - Group similar materials together
2. **Use in recipes** - Allows flexible ingredient choices
3. **Assign to items** - Give items appropriate resource types
4. **Use icons** - Provide icons for better UI display
5. **Combine types** - Items can have multiple resource types
6. **Document types** - Keep track of which items use which types
7. **Test recipes** - Ensure resource type matching works correctly

---

**Previous:** [Sound Effects](35_Sound_Effects.md) | **Next:** [Projectiles](37_Projectiles.md)
