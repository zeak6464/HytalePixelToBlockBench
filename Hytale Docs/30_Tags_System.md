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
- `"Weapon"` - Combat weapons (swords, axes, guns, spellbooks)
- `"Armor"` - Protective gear (chestplates, helmets, boots, legs)
- `"Tool"` - Gathering tools (pickaxes, axes, shovels)
- `"Ingredient"` - Crafting materials (bars, essences, components)
- `"Food"` - Consumable food items
- `"Bench"` - Crafting benches (workbenches, forges, arcanebenches)
- `"Block"` - Placeable blocks
- `"SpawnNPC"` - Items that spawn NPCs (e.g., fish, spawn eggs)
- `"Sword"` - Sword weapons
- `"Axe"` - Axe weapons/tools
- `"Bow"` - Bow weapons
- `"Gun"` - Gun weapons
- `"Spellbook"` - Spellbook weapons
- `"Ore"` - Ore blocks/items
- `"Plant"` - Plant blocks
- `"Crop"` - Crop blocks
- `"Seed"` - Seed items
- `"Bush"` - Bush blocks
- `"Rock"` - Rock/stone blocks
- `"Wood"` - Wood blocks
- `"Dirt"` - Dirt/soil blocks
- `"Metal"` - Metal blocks
- `"Sand"` - Sand blocks
- `"Grass"` - Grass blocks
- `"Soil"` - Soil blocks
- `"Portal"` - Portal blocks/items
- `"Deployable"` - Deployable items (totems, turrets)
- `"Furniture"` - Furniture items
- `"Torch"` - Torch blocks
- `"Vine"` - Vine blocks
- `"CactusGrowth"` - Cactus growth blocks

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

**Material Type Tags:**
- `"Rock"` - Stone/rock blocks
- `"Wood"` - Wooden blocks
- `"Dirt"` - Dirt/soil blocks
- `"Metal"` - Metal blocks
- `"Sand"` - Sand blocks
- `"Grass"` - Grass blocks
- `"Soil"` - Soil blocks
- `"Stone"` - Stone blocks
- `"Gravel"` - Gravel blocks
- `"Clay"` - Clay blocks
- `"Glass"` - Glass blocks
- `"Ice"` - Ice blocks
- `"Snow"` - Snow blocks

**Functional Type Tags:**
- `"Plant"` - Plant blocks
- `"Crop"` - Crop blocks
- `"Bush"` - Bush blocks
- `"Vine"` - Vine blocks
- `"Ore"` - Ore blocks
- `"Portal"` - Portal blocks
- `"Torch"` - Torch blocks
- `"Door"` - Door blocks
- `"Fence"` - Fence blocks
- `"Gate"` - Gate blocks
- `"Stairs"` - Stair blocks
- `"Roof"` - Roof blocks
- `"Planks"` - Plank blocks
- `"Beam"` - Beam blocks
- `"Decorative"` - Decorative blocks
- `"Ornate"` - Ornate blocks

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

## Item Categories vs Tags

### Categories Property

Items can have a `Categories` property (separate from `Tags`) that organizes items in crafting benches and creative libraries:

```json
{
  "Categories": [
    "Items.Weapons",
    "Items.Utility",
    "Items.Debug"
  ]
}
```

**Common Category Values:**
- `"Items.Weapons"` - Weapons category
- `"Items.Utility"` - Utility items (totems, deployables, utility items)
- `"Items.Armors"` - Armor category
- `"Items.Tools"` - Tools category
- `"Items.Debug"` - Debug items (for testing)
- `"Items.CombatMilestone2"` - Combat milestone items

### Utility Category and Tag

**As a Category** (`Categories` property):
```json
{
  "Categories": [
    "Items.Weapons",
    "Items.Utility"
  ]
}
```
Used for totems, deployables, and utility items in crafting/creative interfaces.

**As a Type Tag** (`Tags.Type`):
```json
{
  "Tags": {
    "Type": ["Utility"]
  }
}
```
Used for utility items like backpacks, seed bags, and helipacks.

**Example: Healing Totem**
```json
{
  "Categories": [
    "Items.Weapons",
    "Items.Utility"
  ],
  "Tags": {
    "Type": ["Weapon"]
  }
}
```

The totem is categorized as `Items.Utility` but tagged as `Type: Weapon` because it's still a weapon item.

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

## Complete Tag Reference

### Type Tags (Item Function)

**Weapons:**
- `"Weapon"` - General weapons
- `"Sword"` - Sword weapons
- `"Axe"` - Axe weapons
- `"Bow"` - Bow weapons
- `"Gun"` - Gun weapons
- `"Spellbook"` - Spellbook weapons
- `"Stick"` - Stick weapons

**Tools:**
- `"Tool"` - General tools
- `"Pickaxe"` - Pickaxe tools
- `"Axe"` - Axe tools
- `"Shovel"` - Shovel tools
- `"Scythe"` - Scythe tools

**Armor:**
- `"Armor"` - General armor
- `"Helmet"` - Helmet armor
- `"Chestplate"` - Chestplate armor
- `"Leggings"` - Leggings armor
- `"Boots"` - Boots armor

**Items:**
- `"Ingredient"` - Crafting ingredients
- `"Food"` - Food items
- `"Potion"` - Potion items
- `"Bench"` - Crafting benches
- `"Deployable"` - Deployable items
- `"Furniture"` - Furniture items
- `"Block"` - Placeable blocks
- `"SpawnNPC"` - Items that spawn NPCs
- `"Utility"` - Utility items (bags, backpacks, helipacks, etc.)

**Utility Items:**
- `"Utility"` - Utility items (backpacks, bags, helipacks)

**Block Types:**
- `"Rock"` / `"Stone"` - Rock/stone blocks
- `"Wood"` - Wood blocks
- `"Dirt"` / `"Soil"` - Dirt/soil blocks
- `"Sand"` - Sand blocks
- `"Grass"` - Grass blocks
- `"Metal"` - Metal blocks
- `"Gravel"` - Gravel blocks
- `"Clay"` - Clay blocks
- `"Glass"` - Glass blocks
- `"Ice"` - Ice blocks
- `"Snow"` - Snow blocks
- `"Plant"` - Plant blocks
- `"Crop"` - Crop blocks
- `"Bush"` - Bush blocks
- `"Vine"` - Vine blocks
- `"Ore"` - Ore blocks
- `"Portal"` - Portal blocks
- `"Torch"` - Torch blocks
- `"Door"` - Door blocks
- `"Fence"` - Fence blocks
- `"Gate"` - Gate blocks
- `"Stairs"` - Stair blocks
- `"Roof"` - Roof blocks
- `"Planks"` - Plank blocks
- `"Beam"` - Beam blocks
- `"Decorative"` - Decorative blocks
- `"Ornate"` - Ornate blocks

### Family Tags (Item Sets)

Common families:
- `"Iron"` - Iron item set
- `"Steel"` - Steel item set
- `"Gold"` - Gold item set
- `"Diamond"` - Diamond item set
- `"Ancient"` - Ancient item set
- `"Custom"` - Custom item set
- `"MyCustom"` - Custom mod items
- `"Fish"` - Fish items
- `"Magic"` - Magic items
- `"Fire"` - Fire-themed items
- `"Frost"` / `"Ice"` - Ice-themed items
- `"Lightning"` - Lightning-themed items
- `"Stone"` - Stone-themed items
- `"Wood"` - Wood-themed items
- `"Metal"` - Metal-themed items
- `"Demon"` - Demon-themed items
- `"Spellbook"` - Spellbook family
- `"Grimoire"` - Grimoire family

**Wood Type Families:**
- `"Tropicalwood"` - Tropical wood
- `"Softwood"` - Soft wood
- `"Redwood"` - Redwood
- `"Stormbark"` - Stormbark wood
- `"Sallow"` - Sallow wood
- `"Spiral"` - Spiral wood
- `"Wisteria"` - Wisteria wood
- `"Windwillow"` - Windwillow wood
- `"Village"` - Village wood

### Category Tags (Secondary Classification)

- `"Melee"` - Melee weapons
- `"Ranged"` - Ranged weapons
- `"TwoHanded"` - Two-handed items
- `"OneHanded"` - One-handed items
- `"Light"` - Light items
- `"Heavy"` - Heavy items

### Environment Tags

**Zone Tags:**
- `"Zone1"` - Zone 1 environments
- `"Zone2"` - Zone 2 environments
- `"Zone3"` - Zone 3 environments
- `"Zone4"` - Zone 4 environments

**Zone1 Values:**
- `"Plains"` - Plains biome
- `"Forests"` - Forests biome
- `"Mountains"` - Mountains biome
- `"Swamps"` - Swamps biome

**Zone3 Values:**
- `"Northern_Lights"` - Northern lights weather

**Zone4 Values:**
- `"Swamp"` - Swamp biome
- `"Storm"` - Storm weather
- `"Ash_Wastes"` - Ash wastes biome

**Weather Tags:**
- `"Rain"` - Rain weather
  - `"Moderate"` - Moderate rain
  - `"Heavy"` - Heavy rain
- `"Snow"` - Snow weather
  - `"Moderate"` - Moderate snow
  - `"Heavy"` - Heavy snow
  - `"Storm"` - Snow storm

**Biome Tags:**
- `"Temperate"` - Temperate biome
- `"Overground"` - Overground biome

### NPC Tags

**Type Tags:**
- `"Intelligent"` - Intelligent NPCs
- `"Hostile"` - Hostile NPCs
- `"Neutral"` - Neutral NPCs
- `"Passive"` - Passive NPCs
- `"Creature"` - Creature NPCs
- `"Undead"` - Undead NPCs
- `"Elemental"` - Elemental NPCs

**Family Tags:**
- `"Goblin"` - Goblin family
- `"Orc"` - Orc family
- `"Skeleton"` - Skeleton family
- `"Zombie"` - Zombie family
- `"Dragon"` - Dragon family
- `"Beast"` - Beast family

### Character Creator Tags

**Hair Tags:**
- `"HairLength.Short"` - Short hair
- `"HairLength.Medium"` - Medium hair
- `"HairLength.Long"` - Long hair

**Style Tags:**
- `"Style.Fantasy"` - Fantasy style
- `"Style.Modern"` - Modern style
- `"Style.Costume"` - Costume style
- `"Style.Special"` - Special style

## Tips for Using Tags

1. **Use consistent naming** - Follow a naming convention for your mod
2. **Group related items** - Use Family tags to group item sets
3. **Keep it simple** - Don't over-tag items unnecessarily
4. **Use existing tags** - Reuse common tags when possible
5. **Document your tags** - Keep a reference of tag meanings
6. **Test tag filtering** - Ensure tags work in recipes/objectives
7. **Type for function** - Use Type tags for what an item does (Weapon, Tool, Food)
8. **Family for groups** - Use Family tags for item sets (Iron, Ancient, Custom)
9. **Category for classification** - Use Category tags for secondary classification (Melee, Ranged, TwoHanded)

---

**Previous:** [Localization](29_Localization.md) | **Next:** [Music](31_Music.md)
