# Materials & Resources

Learn how to create ores, ingots, bars, and other crafting materials.

## Overview

Materials are foundational items used in crafting recipes. They include ores, ingots, bars, fabrics, leathers, and other processed materials.

## Locations
- Ores: `Server/Item/Items/Ore/{OreType}/`
- Ingredient Bars: `Server/Item/Items/Ingredient/Bar/`
- Fabrics: `Server/Item/Items/Ingredient/`
- Leathers: `Server/Item/Items/Ingredient/`

## Creating Ores

Ores are raw materials that can be processed into bars or ingots.

### Basic Ore Structure

Create `Server/Item/Items/Ore/Iron/Ore_Iron_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Ore_Iron_MyCustom.name",
    "Description": "server.items.Ore_Iron_MyCustom.description"
  },
  "Categories": ["Blocks.Ores"],
  "Icon": "Icons/ItemsGenerated/Ore_Iron_MyCustom.png",
  "Model": "Resources/Ores/Ore_Large.blockymodel",
  "Texture": "Resources/Ores/Ore_Textures/Iron.png",
  "Quality": "Common",
  "ItemLevel": 10,
  "PlayerAnimationsId": "Block",
  "IconProperties": {
    "Scale": 0.57323,
    "Translation": [-3.6, -16.2],
    "Rotation": [22.5, 45, 22.5]
  },
  "Tags": {
    "Type": ["Ore"]
  },
  "ItemEntity": {
    "ParticleSystemId": null
  },
  "MaxStack": 25,
  "ItemSoundSetId": "ISS_Blocks_Stone",
  "DropOnDeath": true
}
```

## Creating Ingredient Bars/Ingots

Bars are processed materials made from ores at a Furnace.

### Basic Bar Structure

Create `Server/Item/Items/Ingredient/Bar/Ingredient_Bar_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Ingredient_Bar_MyCustom.name"
  },
  "Categories": ["Items"],
  "Recipe": {
    "Input": [
      {
        "ItemId": "Ore_MyCustom",
        "Quantity": 1
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Processing",
        "Id": "Furnace"
      }
    ],
    "OutputQuantity": 1,
    "TimeSeconds": 14
  },
  "Model": "Resources/Materials/Ingot.blockymodel",
  "Texture": "Resources/Materials/Ingot_Textures/MyCustom.png",
  "Icon": "Icons/ItemsGenerated/Ingredient_Bar_MyCustom.png",
  "ResourceTypes": [
    {
      "Id": "Metal_Bars"
    }
  ],
  "PlayerAnimationsId": "Item",
  "IconProperties": {
    "Scale": 1,
    "Translation": [0, -3],
    "Rotation": [22.5, 45, 22.5]
  },
  "Tags": {
    "Type": ["Ingredient"]
  },
  "ItemEntity": {
    "ParticleSystemId": null
  },
  "ItemSoundSetId": "ISS_Items_Ingots",
  "DropOnDeath": true
}
```

### Key Bar Properties

- **`ResourceTypes`** - Categorizes the material (e.g., `"Metal_Bars"`)
- **`Model`** - Uses the standard ingot model
- **`Texture`** - Unique texture for this material

## Processing Benches

### Furnace (Metal Processing)

```json
"BenchRequirement": [
  {
    "Type": "Processing",
    "Id": "Furnace"
  }
]
```

Used for:
- Ores → Bars/Ingots
- Raw materials → Processed materials

### Salvagebench (Salvaging)

```json
"BenchRequirement": [
  {
    "Type": "Processing",
    "Id": "Salvagebench"
  }
]
```

Used for breaking down items into materials.

## Resource Types

Resource types allow items to be used as generic materials in recipes:

### Metal Bars

```json
"ResourceTypes": [
  {
    "Id": "Metal_Bars"
  }
]
```

### Fabrics

```json
"ResourceTypes": [
  {
    "Id": "Fabrics"
  }
]
```

### Leathers

```json
"ResourceTypes": [
  {
    "Id": "Leathers"
  }
]
```

## Using Resource Types in Recipes

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

This allows any item with `"Id": "Metal_Bars"` resource type to be used.

## Material Tiers

| Tier | Examples | Quality | ItemLevel |
|------|----------|---------|-----------|
| **Basic** | Crude, Scrap, Bone | Common | 1-5 |
| **Low** | Copper, Bronze | Common | 5-10 |
| **Mid** | Iron, Steel | Uncommon | 10-20 |
| **High** | Cobalt, Mithril | Rare | 20-40 |
| **Top** | Thorium, Adamantite | Epic | 40-50 |
| **Legendary** | Onyxium | Legendary | 50+ |

## Common Material Categories

### Metals

- `Ingredient_Bar_Copper`
- `Ingredient_Bar_Bronze`
- `Ingredient_Bar_Iron`
- `Ingredient_Bar_Steel`
- `Ingredient_Bar_Cobalt`
- `Ingredient_Bar_Mithril`
- `Ingredient_Bar_Thorium`
- `Ingredient_Bar_Adamantite`
- `Ingredient_Bar_Onyxium`

### Leathers

- `Ingredient_Leather_Light`
- `Ingredient_Leather_Medium`
- `Ingredient_Leather_Heavy`

### Fabrics

- `Ingredient_Fabric_Scrap_Linen`
- `Ingredient_Fabric_Wool`
- `Ingredient_Fabric_Cotton`
- `Ingredient_Fabric_Silk`

### Other Ingredients

- `Ingredient_Fibre` - Plant fibre
- `Ingredient_Stick` - Wooden stick
- `Ingredient_Life_Essence` - Essence for crafting

## Complete Examples

### Iron Ore

```json
{
  "TranslationProperties": {
    "Name": "server.items.Ore_Iron.name"
  },
  "Categories": ["Blocks.Ores"],
  "Icon": "Icons/ItemsGenerated/Ore_Iron.png",
  "Model": "Resources/Ores/Ore_Large.blockymodel",
  "Texture": "Resources/Ores/Ore_Textures/Iron.png",
  "Quality": "Common",
  "ItemLevel": 10,
  "MaxStack": 25,
  "Tags": {
    "Type": ["Ore"]
  },
  "DropOnDeath": true
}
```

### Iron Bar (Ingot)

```json
{
  "TranslationProperties": {
    "Name": "server.items.Ingredient_Bar_Iron.name"
  },
  "Categories": ["Items"],
  "Recipe": {
    "Input": [
      { "ItemId": "Ore_Iron", "Quantity": 1 }
    ],
    "BenchRequirement": [
      { "Type": "Processing", "Id": "Furnace" }
    ],
    "OutputQuantity": 1,
    "TimeSeconds": 14
  },
  "Model": "Resources/Materials/Ingot.blockymodel",
  "Texture": "Resources/Materials/Ingot_Textures/Iron.png",
  "ResourceTypes": [
    { "Id": "Metal_Bars" }
  ],
  "Tags": {
    "Type": ["Ingredient"]
  },
  "DropOnDeath": true
}
```

## Tips for Creating Materials

1. **Follow tier progression** - Match material tiers to game progression
2. **Use resource types** - Allow flexibility in recipes
3. **Set appropriate ItemLevel** - Higher tiers = higher ItemLevel
4. **Define processing recipes** - Ores → Bars at Furnace
5. **Use standard models** - Reuse existing ingot/bar models
6. **Set DropOnDeath** - Materials should drop when player dies
7. **Organize by type** - Group related materials in subfolders

---

**Previous:** [Containers](18_Containers.md) | **Next:** [Mounts & Vehicles](20_Mounts_and_Vehicles.md)
