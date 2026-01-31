# Item System Overview

A comprehensive guide to understanding how the entire item system works in Hytale.

## Overview

Hytale's item system is a complex network of interconnected components. This guide explains how **all 3,330+ items** work together, how they're created, stored, used, and how they interact with every other game system.

## The Complete Item Lifecycle

### 1. Item Definition (Assets)
Items start as JSON configuration files in `Server/Item/Items/`

### 2. Item Spawning (World Generation)
Items enter the world through:
- Crafting at benches
- Loot drops from NPCs/blocks
- World generation (chests, spawners)
- Commands (`/give`)
- Shops/trading

### 3. Item Existence (Runtime)
Items exist in various states:
- In player inventory
- In containers (chests)
- On the ground (item entities)
- Equipped on player/NPC
- In crafting grids

### 4. Item Interaction (Usage)
Items are used through:
- Primary/secondary attacks
- Block interactions
- Crafting ingredients
- Consumption (food, potions)
- Equipment changes

### 5. Item Removal (Destruction)
Items leave the world via:
- Consumption
- Durability reaching 0
- Despawning (timer expires)
- Being replaced in crafting
- Death penalties

## Item Type Hierarchy

### Core Item Types

```
Item (Base)
├── Weapon
│   ├── Sword (one-handed)
│   ├── Longsword (two-handed)
│   ├── Axe
│   ├── Battleaxe
│   ├── Mace
│   ├── Dagger
│   ├── Bow
│   ├── Crossbow
│   └── Staff
├── Tool
│   ├── Pickaxe
│   ├── Axe (harvesting)
│   ├── Shovel
│   ├── Hoe
│   ├── Sickle
│   └── Fishing Rod
├── Armor
│   ├── Head
│   ├── Chest
│   ├── Hands
│   ├── Legs
│   └── Shield (offhand)
├── Consumable
│   ├── Food
│   ├── Potion
│   └── Throwable (bombs)
├── Material
│   ├── Ingredient
│   ├── Resource
│   └── Currency
├── Block
│   ├── Placeable
│   ├── Furniture
│   ├── Container (chest, barrel)
│   └── Bench (crafting station)
├── Special
│   ├── Recipe (learnable)
│   ├── Quest Item
│   ├── Key
│   └── Portal
└── Builder Tool
    ├── Brush
    ├── Selection Tool
    └── Scripted Brush
```

## Universal Item Properties

All items share these core properties:

### Identity Properties

```json
{
  "Id": "Item_Unique_ID",
  "Parent": "Template_Item_Type",
  "TranslationProperties": {
    "Name": "server.items.Item_ID.name",
    "Description": "server.items.Item_ID.description"
  }
}
```

### Visual Properties

```json
{
  "Icon": "Icons/ItemsGenerated/Item_Icon.png",
  "Model": "Items/Category/Item_Model.blockymodel",
  "Texture": "Items/Category/Item_Texture.png",
  "DroppedItemAnimation": "Items/Animations/Dropped/Animation.blockyanim",
  "IconProperties": {
    "Scale": 1.0,
    "Rotation": { "X": 0, "Y": 0, "Z": 0 },
    "Translation": [0, 0]
  }
}
```

### Categorization Properties

```json
{
  "Categories": ["Items.Weapons", "Items.Melee"],
  "Quality": "Common",        // Common, Uncommon, Rare, Epic, Legendary
  "ItemLevel": 10,
  "MaxStack": 64              // Default varies by item type
}
```

**Note:** `IsDroppable` and `CanBeStored` are not simple boolean properties. Item storage/drop behavior is determined by item type and interactions.

### Sound Properties

```json
{
  "ItemSoundSetId": "ISS_Weapons_Sword_Metal"
}
```

**Note:** All item sounds (equip, unequip, use, etc.) are defined in the `ItemSoundSetId` reference, not as individual properties.

### Durability Properties

```json
{
  "MaxDurability": 100,
  "DurabilityLossOnHit": 1.0
}
```

**Note:** Block-breaking durability loss is configured through `Tool.DurabilityLossBlockTypes`. Repair is handled through repair kit items, not item-level `CanBeRepaired` or `RepairIngredient` properties.

### Animation Properties

```json
{
  "PlayerAnimationsId": "Sword",
  "Reticle": "DefaultMelee"
}
```

## Item System Integration

### Integration with Crafting System

Items connect to crafting through:

**1. Recipe Definitions:**
```json
{
  "Recipe": {
    "Input": [
      { "ItemId": "Ingredient_Bar_Iron", "Quantity": 6 }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Categories": ["Weapon_Sword"],
        "Id": "Weapon_Bench"
      }
    ],
    "KnowledgeRequired": true,
    "TimeSeconds": 3
  }
}
```

**2. As Crafting Ingredients:**
Items can be consumed in recipes via:
- `ItemId` (specific item)
- `ResourceTypeId` (any item of type)
- `TagPattern` (items matching tags)

**3. Recipe Learning:**
```json
{
  "Interactions": {
    "Type": "LearnRecipe",
    "Recipes": ["Weapon_Sword_Steel"]
  }
}
```

### Integration with Combat System

Items interact with combat through:

**1. Damage Calculation:**
```json
{
  "InteractionVars": {
    "Primary_Attack_Damage": {
      "Interactions": [{
        "DamageCalculator": {
          "BaseDamage": { "Physical": 15 },
          "DamageScaling": { "Strength": 0.5 }
        }
      }]
    }
  }
}
```

**2. Defense Values (within Armor object):**
```json
{
  "Armor": {
    "DamageResistance": {
      "Physical": [
        { "Amount": 0.09, "CalculationType": "Multiplicative" }
      ]
    }
  }
}
```

**Note:** Combat stats like `CriticalChance` and `AttackSpeed` are not simple item properties. Stats are configured through `Armor.StatModifiers`, interactions with `EntityStatsOnHit`, and entity effects.

### Integration with Inventory System

Items exist in inventories with:

**1. Stack Management:**
- `MaxStack` - How many fit in one slot (note: property is `MaxStack`, not `MaxStackSize`)
- Stackable items combine automatically
- Unique items (tools, armor) don't stack

**2. Slot Restrictions:**
```json
{
  "Armor": {
    "ArmorSlot": "Head"      // Head, Chest, Hands, Legs
  }
}
```

**Note:** Use `Armor.ArmorSlot` for equipment slots. `CanBeHotbarred` and `CanBeEquipped` are not simple boolean properties.

**3. Storage Rules:**
- Player inventory (main + hotbar)
- Backpack (additional storage)
- Equipment slots
- Container storage (chests)
- Bank/vault storage

### Integration with Block System

Block items bridge items and world blocks:

**1. Placeable Blocks:**
```json
{
  "BlockType": {
    "Type": "Normal",
    "PlacedBlock": "Rock_Stone",
    "PlacementSound": "SFX_Block_Place_Stone"
  }
}
```

**2. Block Drops:**
```json
{
  "DropList": {
    "Id": "Drops_Stone",
    "DefaultDrops": [
      { "ItemId": "Rock_Stone", "Quantity": 1 }
    ]
  }
}
```

**3. Container Blocks:**
```json
{
  "BlockType": {
    "Type": "Container",
    "ContainerSize": 27,
    "OpenInteraction": "Container_Open"
  }
}
```

### Integration with NPC System

Items interact with NPCs through:

**1. NPC Equipment:**
```json
{
  "Equipment": {
    "Weapon": "Weapon_Sword_Iron",
    "Armor_Head": "Armor_Iron_Head"
  }
}
```

**2. NPC Drops:**
```json
{
  "DropList": {
    "Drops": [
      { "ItemId": "Ingredient_Leather_Light", "Chance": 0.5 }
    ]
  }
}
```

**3. Trading:**
```json
{
  "Shop": {
    "Trades": [
      {
        "Cost": [{ "ItemId": "Currency_Coin", "Quantity": 50 }],
        "Result": { "ItemId": "Weapon_Sword_Iron", "Quantity": 1 }
      }
    ]
  }
}
```

### Integration with Effect System

Items apply effects through:

**1. Consumable Effects:**
```json
{
  "Interactions": {
    "Type": "Consume",
    "Effects": [
      {
        "EffectId": "Effect_Health_Regen",
        "Duration": 10.0,
        "Strength": 1.0
      }
    ]
  }
}
```

**2. Equipment Stats:**
Equipment bonuses are configured through `Armor.StatModifiers`, not `PassiveEffects`.

**3. Food Effects:**
Food items use `Interactions` with `ChangeStat` and `ApplyEffect` types, not a `FoodStats` object. See [Food Items](13_Food_Items.md).

### Integration with World Generation

Items spawn in the world via:

**1. Chest Loot:**
```json
{
  "LootTable": "Loot_Dungeon_Chest",
  "Items": [
    { "ItemId": "Weapon_Sword_Iron", "Weight": 10 }
  ]
}
```

**2. Block Spawners:**
```json
{
  "SpawnerTable": {
    "SpawnItems": [
      { "ItemId": "Plant_Crop_Wheat", "Chance": 0.3 }
    ]
  }
}
```

## Item Interaction System

### Interaction Hints

All item interactions show hints to players:

| Hint Type | Message | Use Case |
|-----------|---------|----------|
| `generic` | Press [{key}] to interact | Generic interactions |
| `open` | Press [{key}] to open {name} | Containers, doors |
| `gather` | Press [{key}] to pick up {name} | Harvestable items |
| `edit` | Press [{key}] to edit | Builder mode |
| `turnon` / `turnoff` | Press [{key}] to turn on/off | Switches, lights |
| `sit` | Press [{key}] to sit | Chairs, benches |
| `harvest` | Press [{key}] to harvest {name} | Crops, plants |
| `trade` | Press [{key}] to Trade | NPC merchants |
| `mount` | Press [{key}] to Mount | Horses, vehicles |
| `burst` | Press [{key}] to burst {name} | Breakable objects |
| `pickup` | Press [{key}] to pick up {name} | Ground items |

### Interaction Types

Items use these interaction types:

**Primary Interactions:**
- Attack (weapons)
- Block breaking (tools)
- Block placing (blocks)
- Consume (food, potions)

**Secondary Interactions:**
- Special attacks (weapon abilities)
- Tool abilities (grappling hook)
- Block interactions (right-click use)

**Tertiary Interactions:**
- Hold/charge attacks
- Context-sensitive actions
- Multi-stage interactions

## Item Property Reference

### Complete Property List (Verified)

| Category | Properties | Description |
|----------|------------|-------------|
| **Identity** | Id, Parent, Type | Item identification |
| **Translation** | TranslationProperties.Name, .Description | Localized text |
| **Visual** | Icon, Model, Texture, IconProperties, DroppedItemAnimation | Appearance |
| **Quality** | Quality, ItemLevel | Tier and rarity |
| **Stack** | MaxStack | Inventory stacking |
| **Durability** | MaxDurability, DurabilityLossOnHit | Item wear |
| **Armor** | Armor.ArmorSlot, Armor.DamageResistance, Armor.StatModifiers | Wearable items |
| **Combat** | InteractionVars with DamageCalculator | Fighting stats |
| **Tool** | Tool.Specs, Tool.DurabilityLossBlockTypes | Tool configuration |
| **Crafting** | Recipe.Input, Recipe.BenchRequirement, Recipe.OutputQuantity | Creation |
| **Block** | BlockType.DrawType, BlockType.Flags, BlockType.HitboxType | Placeable items |
| **Interaction** | Interactions, InteractionVars | Usage behavior |
| **Sound** | ItemSoundSetId | Audio |
| **Animation** | PlayerAnimationsId, Reticle | Player anims |
| **Categories** | Categories array | Organization |
| **Tags** | Tags object | Categorization tags |

### Property Inheritance

Items inherit properties through templates:

```
Template_Weapon (base weapon properties)
  └── Template_Weapon_Sword (sword-specific)
        └── Weapon_Sword_Iron (concrete item)
```

**Inheritance Rules:**
1. Child inherits ALL parent properties
2. Child can override any property
3. Arrays are replaced, not merged
4. Objects are merged recursively
5. null removes inherited property

## Item Data Flow

### From Asset to Runtime

```
1. JSON Asset File
   ↓ (Loaded at startup)
2. Asset Registry
   ↓ (Referenced by ID)
3. Item Definition
   ↓ (Instantiated)
4. Item Instance
   ↓ (Stored)
5. Inventory/World/Container
   ↓ (Used)
6. Interaction System
   ↓ (Modified)
7. Updated Item Instance
   ↓ (Saved)
8. Player Data / World Data
```

### Item State Changes

Items track state through:

**1. Durability State:**
- New (100%)
- Used (99-26%)
- Damaged (25-1%)
- Broken (0%)

**2. Modification State:**
- Base item
- Enchanted item
- Modified item (stat changes)
- Socketed item (gems)

**3. Ownership State:**
- Owned by player
- Owned by NPC
- In container
- On ground (dropped)
- In world generation

## Performance Considerations

### Item Entity Optimization

**Item Entity Lifetime:**
```json
{
  "ItemEntity": {
    "Lifetime": 600.0  // 10 minutes before despawn
  }
}
```

**Grouping:**
- Similar items merge into stacks
- Reduces entity count
- Improves performance

**Culling:**
- Items outside render distance are hidden
- Still tracked for pickup
- Unloaded with chunks

### Inventory Optimization

**Stack Merging:**
- Automatic combining of identical items
- Reduces inventory bloat
- Faster searches

**Sort Algorithms:**
- Items sorted by type, quality, level
- Custom sort orders supported
- Quick stack to containers

## Common Item Patterns

### Pattern 1: Tiered Items

Progression through material tiers (from actual game files). **Update 2 (Jan 2026):** Mining and pickaxe progression were adjusted. Lower-tier pickaxes deal much less damage to higher-tier ores; upgrading significantly reduces hits required. Adamantite now requires Thorium or Cobalt pickaxe.

```json
// Crude Pickaxe (Tier 1)
{
  "Parent": "Template_Tool_Pickaxe",
  "ItemLevel": 1,
  "MaxDurability": 60
}

// Copper Pickaxe (Tier 2) — Update 2: was Tier 4
{
  "Parent": "Tool_Pickaxe_Crude",
  "ItemLevel": 10,
  "MaxDurability": 120
}

// Iron Pickaxe (Tier 4) — Update 2: was Tier 8
{
  "Parent": "Tool_Pickaxe_Crude",
  "ItemLevel": 20,
  "MaxDurability": 250
}

// Thorium Pickaxe (Tier 6) — Update 2: added at Tier 6
{
  "Parent": "Tool_Pickaxe_Crude",
  "ItemLevel": 40,
  "MaxDurability": 500
}
```

**Tier order:** Crude → Copper (T2) → … → Iron (T4) → … → Thorium (T6) → Cobalt. **Adamantite:** requires Thorium or Cobalt pickaxe.

### Pattern 2: Multi-Specification Tools

From `Tool_Pickaxe_Iron.json` - Tools with different gathering power for different block types:

```json
{
  "Tool": {
    "Specs": [
      {
        "Power": 1,
        "GatherType": "SoftBlocks"
      },
      {
        "Power": 0.5,
        "GatherType": "Soils"
      },
      {
        "Power": 0.5,
        "GatherType": "Rocks"
      }
    ],
    "DurabilityLossBlockTypes": [
      {
        "BlockSets": ["Stone", "Rock", "Ores"],
        "DurabilityLossOnHit": 0.25
      }
    ]
  }
}
```

### Pattern 3: Interactive Tool Items

From `Tool_Repair_Kit_Iron.json` - Items that open custom UIs:

```json
{
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "OpenCustomUI",
          "Page": {
            "Id": "ItemRepair",
            "RepairPenalty": 0.1
          }
        }
      ]
    }
  }
}
```

## Troubleshooting Items

### Common Issues

**Item doesn't appear in creative menu:**
- Check `Categories` array
- Verify category exists in `ItemCategory`
- Ensure `ItemLevel` is set

**Item can't be crafted:**
- Verify `Recipe.BenchRequirement` bench exists
- Check `Input` items are obtainable
- Confirm `KnowledgeRequired` is learned

**Item has no icon:**
- Check `Icon` path exists
- Verify PNG file in `Common/` folder
- Check `IconProperties` scaling

**Item doesn't deal damage:**
- Verify `InteractionVars` configured
- Check `DamageCalculator.BaseDamage` set
- Ensure `PlayerAnimationsId` exists

**Item breaks instantly:**
- Check `MaxDurability` is set
- Verify `DurabilityLossOnHit` not too high
- Ensure `CanBeRepaired` if needed

## Related Guides

### Item Creation Guides
- [Creating Items](02_Items.md) - Complete item creation guide
- [Item Properties Reference](175_Item_Properties_Reference.md) - Every property documented
- [Item Tool Specs](07_Item_Tool_Specs.md) - Tool efficiency specs
- [Item Sound Sets](108_Item_Sound_Sets.md) - Audio configuration
- [Item Groups](78_Item_Groups.md) - Item grouping
- [Item Categories](79_Item_Categories.md) - Category system

### Specific Item Types
- [Weapons](17_Weapons.md) - Weapon items
- [Armor](02_Items.md#armor) - Armor items
- [Food Items](08_Food_Items.md) - Consumable food
- [Potions & Effects](04_Potions_and_Effects.md) - Potion items
- [Blocks & Portals](06_Blocks_and_Portals.md) - Block items

### Item Integration
- [Advanced Item Interactions](21_Advanced_Item_Interactions.md) - Complex interactions
- [Interaction Types](109_Interaction_Types_List.md) - All interaction types
- [Crafting Recipes](16_Crafting.md) - Recipe system
- [Loot Tables](18_Loot_Tables.md) - Item drops
- [Trading & Quests](05_Trading_and_Quests.md) - Item economy

### System Integration
- [Asset Types Reference](173_Asset_Types_Reference.md) - All asset types
- [Entity Stats](31_Entity_Stats.md) - Stat system
- [Resource Types](19_Resource_Types.md) - Material resources

## Summary

The Hytale item system is a comprehensive framework that connects:

✅ **3,330+ unique items** with individual properties  
✅ **12 interaction types** for player engagement  
✅ **15+ integration points** with other systems  
✅ **Complete lifecycle management** from creation to destruction  
✅ **Performance optimizations** for thousands of entities  
✅ **Flexible inheritance system** for easy content creation  

Understanding how items work holistically allows you to create better, more integrated content that works seamlessly with all of Hytale's systems.

---

**Previous:** [Asset Types Reference](173_Asset_Types_Reference.md) | **Next:** [Item Properties Reference](175_Item_Properties_Reference.md)
