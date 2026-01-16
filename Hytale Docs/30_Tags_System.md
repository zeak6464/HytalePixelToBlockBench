# Tags System

Learn how tags work in Hytale and how to use them for categorization, filtering, and game logic.

## Overview

Tags are string arrays used to categorize and identify items, blocks, NPCs, and other assets. They're used for filtering, crafting requirements, objectives, and game logic.

## Location
Tags are defined in JSON files:
- Items: `Server/Item/Items/`
- Blocks: `Server/Item/Items/` (block items)
- NPCs: `Server/NPC/Roles/`
- Environments: `Server/Environments/`

## Basic Tag Structure

Tags are defined as objects with string arrays:

```json
{
  "Tags": {
    "Type": ["Sword", "Weapon"],
    "Family": ["Iron", "Custom"],
    "Category": ["Melee"]
  }
}
```

## Item Tags

### Basic Item Tags

```json
{
  "Tags": {
    "Type": ["Weapon"],
    "Family": ["MyCustom"]
  }
}
```

### Common Item Tag Categories

#### Type Tags

Categories items by function:

```json
{
  "Tags": {
    "Type": ["Sword", "Weapon"]
  }
}
```

Common types:
- `"Weapon"` - Combat weapons
- `"Armor"` - Protective gear
- `"Tool"` - Gathering tools
- `"Ingredient"` - Crafting materials
- `"Food"` - Consumable food
- `"Bench"` - Crafting benches
- `"Block"` - Placeable blocks
- `"SpawnNPC"` - Items that spawn NPCs (e.g., fish)

#### Family Tags

Groups related items:

```json
{
  "Tags": {
    "Family": ["Iron", "MyCustom", "Ancient"]
  }
}
```

Used to:
- Identify item sets
- Filter items in crafting
- Organize related items

#### Category Tags

Additional categorization:

```json
{
  "Tags": {
    "Category": ["Melee", "TwoHanded"]
  }
}
```

## Block Tags

Blocks use tags to identify material types:

```json
{
  "BlockType": {
    "Material": "Solid"
  },
  "Tags": {
    "Type": ["Rock"]
  }
}
```

### Common Block Tags

- `"Rock"` - Stone/rock blocks
- `"Wood"` - Wooden blocks
- `"Dirt"` - Dirt/soil blocks
- `"Metal"` - Metal blocks

## NPC Tags

NPCs can use tags for categorization:

```json
{
  "Tags": {
    "Type": ["Intelligent", "Hostile"],
    "Family": ["Goblin"]
  }
}
```

## Environment Tags

Environments use tags for biome categorization:

```json
{
  "Tags": {
    "Zone1": ["Plains", "Forests"],
    "Biome": ["Temperate", "Overground"]
  }
}
```

See [Biomes & Environments](24_Biomes_and_Environments.md) for details.

## Using Tags in Game Logic

### Crafting Recipes

Recipes can filter by tags using `ResourceTypeId` or item properties:

```json
{
  "Recipe": {
    "Input": [
      {
        "ResourceTypeId": "Wood_All"  // Matches items with Wood resource type
      },
      {
        "Tags": {
          "Type": ["Ingredient"],
          "Family": ["Iron"]
        }
      }
    ]
  }
}
```

### Objectives

Objectives can track items/blocks by tags:

```json
{
  "Tasks": [
    {
      "Type": "Gather",
      "BlockTagOrItemId": {
        "ItemId": "Ore_Iron"  // Specific item
      },
      "Count": 5
    },
    {
      "Type": "Gather",
      "BlockTagOrItemId": {
        "Tags": {
          "Type": ["Ore"]  // Any ore
        }
      },
      "Count": 10
    }
  ]
}
```

### NPC Spawning

NPC spawns can filter by environment tags:

```json
{
  "Environments": [
    {
      "Tags": {
        "Zone1": ["Plains"]
      },
      "Weight": 100
    }
  ]
}
```

## Tag Combinations

Items can have multiple tag categories:

```json
{
  "Tags": {
    "Type": ["Weapon", "Sword"],
    "Family": ["Iron", "Ancient"],
    "Category": ["Melee", "TwoHanded"]
  }
}
```

## Complete Examples

### Example 1: Weapon with Tags

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_MyCustom_Sword.name"
  },
  "Tags": {
    "Type": ["Weapon", "Sword"],
    "Family": ["MyCustom", "Iron"],
    "Category": ["Melee"]
  }
}
```

### Example 2: Tool with Tags

```json
{
  "TranslationProperties": {
    "Name": "server.items.Tool_MyCustom_Pickaxe.name"
  },
  "Tags": {
    "Type": ["Tool"],
    "Family": ["MyCustom"]
  }
}
```

### Example 3: Ingredient with Tags

```json
{
  "TranslationProperties": {
    "Name": "server.items.Ingredient_MyCustom_Essence.name"
  },
  "Tags": {
    "Type": ["Ingredient"],
    "Family": ["MyCustom", "Magic"]
  }
}
```

### Example 4: Bench with Tags

```json
{
  "TranslationProperties": {
    "Name": "server.items.Bench_MyCustom_Crafting.name"
  },
  "Tags": {
    "Type": ["Bench"]
  }
}
```

### Example 5: Fish Item with Tags

```json
{
  "Parent": "Template_Fish_Item",
  "Tags": {
    "Type": ["SpawnNPC"],
    "Family": ["Fish"]
  }
}
```

## Tag Best Practices

### Consistent Naming

Use consistent tag names across your mod:

```json
// Good: Consistent family tag
"MyCustom"
"MyCustom_Weapon"
"MyCustom_Armor"

// Bad: Inconsistent naming
"MyCustom"
"CustomStuff"
"My_Stuff"
```

### Appropriate Categories

Use meaningful tag categories:

- **Type** - Primary function (Weapon, Tool, Food)
- **Family** - Item set/group (Iron, Ancient, Custom)
- **Category** - Secondary classification (Melee, Ranged)

### Avoid Tag Conflicts

Don't reuse tags for different purposes:

```json
// Bad: Using "Type" for both function and material
"Tags": {
  "Type": ["Weapon", "Iron"]  // Mixing function and material
}

// Good: Separate categories
"Tags": {
  "Type": ["Weapon"],
  "Family": ["Iron"]
}
```

## Tips for Using Tags

1. **Use consistent naming** - Follow a naming convention for your mod
2. **Group related items** - Use Family tags to group item sets
3. **Keep it simple** - Don't over-tag items unnecessarily
4. **Use existing tags** - Reuse common tags when possible
5. **Document your tags** - Keep a reference of tag meanings
6. **Test tag filtering** - Ensure tags work in recipes/objectives

---

**Previous:** [Localization](29_Localization.md) | **Next:** [Music](31_Music.md)
