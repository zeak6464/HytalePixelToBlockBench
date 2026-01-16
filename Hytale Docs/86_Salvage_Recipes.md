# Salvage Recipes

Learn how to create salvage recipes that recycle items back into materials.

## Overview

Salvage recipes allow players to break down items (weapons, armor, tools) back into crafting materials. This provides a way to recover resources from unwanted items.

## Location
`Server/Item/Recipes/Salvage/`

## Basic Salvage Recipe Structure

Create `Server/Item/Recipes/Salvage/Salvage_Weapon_Sword_MyCustom.json`:

```json
{
  "Recipe": {
    "TimeSeconds": 3,
    "Input": [
      {
        "ItemId": "Weapon_Sword_MyCustom",
        "Quantity": 1
      }
    ],
    "Output": [
      {
        "ItemId": "Ingredient_Bar_Iron",
        "Quantity": 3
      },
      {
        "ItemId": "Ingredient_Stick",
        "Quantity": 1
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Id": "Salvage",
        "Categories": []
      }
    ]
  }
}
```

## Salvage Recipe Properties

### Input

```json
{
  "Input": [
    {
      "ItemId": "Weapon_Sword_Iron",
      "Quantity": 1
    }
  ]
}
```

The item to be salvaged (typically quantity 1).

### Output

```json
{
  "Output": [
    {
      "ItemId": "Ingredient_Bar_Iron",
      "Quantity": 3
    },
    {
      "ItemId": "Ingredient_Fabric_Scrap_Linen",
      "Quantity": 2
    }
  ]
}
```

Materials returned from salvage. Can return multiple items.

### Bench Requirement

```json
{
  "BenchRequirement": [
    {
      "Type": "Crafting",
      "Id": "Salvage",
      "Categories": []
    }
  ]
}
```

Salvage recipes typically require a salvage bench.

## Complete Example: Iron Sword Salvage

`Server/Item/Recipes/Salvage/Salvage_Weapon_Sword_Iron.json`:

Salvages iron sword back into iron bars and materials.

## Salvage Balance

- **Partial recovery** - Salvage typically returns less than original crafting cost
- **Multiple outputs** - Can return multiple material types
- **Bench requirement** - Requires specific salvage crafting bench

## Tips for Salvage Recipes

1. **Recovery rate** - Return 50-80% of original materials for balance
2. **Multiple materials** - Return all material types used in crafting
3. **Bench requirement** - Use Salvage bench for salvage-specific crafting
4. **Tier matching** - Create salvage recipes for each item tier

---

**Previous:** [NPC Appearance](85_NPC_Appearance.md) | **Next:** [Respawn Controllers](87_Respawn_Controllers.md)
