# Item Properties Reference

Complete reference of all available item properties in Hytale.

## Overview

This guide documents every property that can be used in item JSON files, organized by category with examples and valid values.

## Core Properties

### Id (String, Required)

Unique identifier for the item.

```json
{
  "Id": "Weapon_Sword_Iron"
}
```

**Rules:**
- Must be unique across all items
- Use alphanumeric characters and underscores
- Convention: `Type_Subtype_Material` or `Category_ItemName`

### Parent (String)

Inherit properties from a template item.

```json
{
  "Parent": "Template_Weapon_Sword"
}
```

**Common Templates:**
- `Template_Weapon_Sword`, `Template_Weapon_Bow`, `Template_Weapon_Mace`
- `Template_Tool_Pickaxe`, `Template_Tool_Axe`, `Template_Tool_Shovel`
- `Template_Armor_Head`, `Template_Armor_Chest`
- `Template_Food`, `Template_Potion`

### Type (String)

Explicit type declaration (usually inherited from Parent).

```json
{
  "Type": "Item"
}
```

**Valid Values:**
- `"Item"` - Standard item
- `"Variant"` - Variant of parent
- `"Abstract"` - Template only (not spawnable)

## Translation Properties

### TranslationProperties (Object)

Localized text references.

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Sword_Iron.name",
    "Description": "server.items.Weapon_Sword_Iron.description",
    "Flavor": "server.items.Weapon_Sword_Iron.flavor"
  }
}
```

**Keys:**
- `Name` - Item display name
- `Description` - Item description
- `Flavor` - Flavor text (lore)
- `ShortDescription` - Abbreviated description

## Visual Properties

### Icon (String)

Path to item icon image.

```json
{
  "Icon": "Icons/ItemsGenerated/Weapon_Sword_Iron.png"
}
```

**Format:** PNG file, typically 64x64 or 128x128 pixels

### IconProperties (Object)

Icon display customization.

```json
{
  "IconProperties": {
    "Scale": 1.5,
    "Rotation": {
      "X": 15,
      "Y": 45,
      "Z": 0
    },
    "Offset": {
      "X": 0,
      "Y": -2
    }
  }
}
```

**Properties:**
- `Scale` - Size multiplier (default: 1.0)
- `Rotation` - X, Y, Z rotation in degrees
- `Offset` - X, Y pixel offset

### Model (String)

Path to 3D model file.

```json
{
  "Model": "Items/Weapons/Sword/Iron.blockymodel"
}
```

**Format:** `.blockymodel` file

### Texture (String)

Path to texture file for model.

```json
{
  "Texture": "Items/Weapons/Sword/Iron_Texture.png"
}
```

**Format:** PNG file with texture atlas

### DroppedItemAnimation (String)

Animation when item is on the ground.

```json
{
  "DroppedItemAnimation": "Items/Animations/Dropped/Dropped_Diagonal_Left.blockyanim"
}
```

**Common Values:**
- `Items/Animations/Dropped/Dropped_Diagonal_Left.blockyanim`
- `Items/Animations/Dropped/Dropped_Diagonal_Right.blockyanim`
- `Items/Animations/Dropped/Dropped_Flat.blockyanim`

## Quality and Level

### Quality (String)

Item rarity tier.

```json
{
  "Quality": "Rare"
}
```

**Valid Values (in order):**
- `"Common"` - White/gray
- `"Uncommon"` - Green
- `"Rare"` - Blue
- `"Epic"` - Purple
- `"Legendary"` - Orange/gold

### ItemLevel (Integer)

Item's power level.

```json
{
  "ItemLevel": 25
}
```

**Range:** Typically 1-100, affects stat scaling and requirements

## Stack and Storage

### MaxStack (Integer)

Maximum items per inventory slot.

```json
{
  "MaxStack": 64
}
```

**Defaults:**
- `64` - Standard stackable items
- `1` - Tools, weapons, armor (non-stackable)
- `16` - Throwable items
- `40` - Arrows
- `99` or `999` - Currency, small items

**Note:** The property is `MaxStack`, not `MaxStackSize`. Item dropability, storage, and hotbar placement are not configured through simple boolean properties. These behaviors are determined by item type and other complex properties.

## Durability

### MaxDurability (Integer)

Maximum durability points.

```json
{
  "MaxDurability": 250
}
```

**Common Values:**
- Wood: 60-80
- Stone: 120-150
- Iron: 200-250
- Steel: 400-500
- Legendary: 1000+

### DurabilityLossOnHit (Float)

Durability lost per combat hit.

```json
{
  "DurabilityLossOnHit": 1.0
}
```

### DurabilityLossOnBlockBreak (Float)

Durability lost per block broken.

```json
{
  "DurabilityLossOnBlockBreak": 0.5
}
```

**Note:** Item repair is handled through repair kits and benches, not through item-level properties. See `Tool_Repair_Kit` items for the repair system implementation.

## Equipment

### EquipmentSlot (String)

Which slot the item equips to.

```json
{
  "EquipmentSlot": "Head"
}
```

**Valid Values:**
- `"Head"` - Helmet slot
- `"Chest"` - Chest armor slot
- `"Hands"` - Gauntlets/gloves slot
- `"Legs"` - Leg armor slot
- `"Back"` - Cape/backpack slot
- `"Trinket"` - Accessory slot

**Note:** Armor classification is primarily determined by the item parent template and other properties, not a simple `ArmorType` string.

### CosmeticsToHide (Array)

From `Armor_Wool_Legs.json` - Player cosmetics hidden when armor is equipped (within `Armor` object):

```json
{
  "Armor": {
    "ArmorSlot": "Legs",
    "CosmeticsToHide": [
      "Pants",
      "Shoes"
    ]
  }
}
```

**Common Values:**
- `"Pants"`, `"Shoes"` (for leg armor)
- `"Shirt"` (for chest armor)
- Character creator cosmetics

## Combat Stats

### DamageResistance (Object)

Damage reduction by type.

```json
{
  "DamageResistance": {
    "Physical": 10,
    "Fire": 5,
    "Ice": 3,
    "Lightning": 0,
    "Poison": 2,
    "Void": -5
  }
}
```

**Damage Types:**
- `Physical`, `Fire`, `Ice`, `Lightning`, `Poison`, `Void`, `Nature`, `Arcane`

**Values:**
- Positive = resistance (reduce damage)
- Negative = weakness (increase damage)
- `0` = neutral

### PlayerAnimationsId (String)

Player animation set for item (documented later in Animation section).

## Player Stats

**Note:** Item stat bonuses and modifiers are primarily configured through interactions and entity effects, not through a simple `ItemStats` property. See the [Entity Stats](31_Entity_Stats.md) and [Potions & Effects](04_Potions_and_Effects.md) guides for stat modification.

## Categories

### Categories (Array)

Item organization categories.

```json
{
  "Categories": [
    "Items.Weapons",
    "Items.Melee",
    "Furniture.Storage"
  ]
}
```

**Common Categories:**
See [Item Categories Guide](79_Item_Categories.md) for complete list.

## Sound

### ItemSoundSetId (String)

Sound set for item usage.

```json
{
  "ItemSoundSetId": "ISS_Weapons_Sword_Metal"
}
```

**Common Sound Sets:**
- Weapons: `ISS_Weapons_Sword_Metal`, `ISS_Weapons_Bow_Wood`
- Tools: `ISS_Tools_Pickaxe`, `ISS_Tools_Axe`
- Armor: `ISS_Armor_Heavy`, `ISS_Armor_Leather`, `ISS_Armor_Cloth`

### EquipSoundEventId (String)

Sound when equipping item.

```json
{
  "EquipSoundEventId": "SFX_Armor_Equip_Heavy"
}
```

### UnequipSoundEventId (String)

Sound when unequipping item.

```json
{
  "UnequipSoundEventId": "SFX_Armor_Unequip_Heavy"
}
```

## Animation

### PlayerAnimationsId (String)

Player animation set for item.

```json
{
  "PlayerAnimationsId": "Sword"
}
```

**Common Values:**
- Weapons: `"Sword"`, `"Bow"`, `"Mace"`, `"Dagger"`, `"Staff"`
- Tools: `"Pickaxe"`, `"Axe"`, `"Shovel"`, `"Hoe"`
- Special: `"Block"` (for armor/shields)

### Reticle (String)

Crosshair/reticle type.

```json
{
  "Reticle": "DefaultMelee"
}
```

**Common Values:**
- `"DefaultMelee"` - Melee weapons
- `"DefaultRanged"` - Bows, crossbows
- `"DefaultTool"` - Tools
- `"None"` - No reticle

## Block Properties

### BlockType (Object)

Configuration for placeable blocks.

```json
{
  "BlockType": {
    "Type": "Normal",
    "PlacedBlock": "Rock_Stone",
    "PlacementSound": "SFX_Block_Place_Stone",
    "BreakSound": "SFX_Block_Break_Stone"
  }
}
```

**Type Values:**
- `"Normal"` - Standard block
- `"Container"` - Chest/storage
- `"Crafting"` - Crafting bench
- `"Furniture"` - Decorative furniture
- `"Door"` - Door block
- `"Bed"` - Bed block

### PlacedBlock (String)

Block ID when placed in world.

```json
{
  "PlacedBlock": "Rock_Stone"
}
```

### ContainerSize (Integer)

Number of inventory slots (for containers).

```json
{
  "ContainerSize": 27
}
```

**Common Sizes:**
- Small chest: 9-18
- Medium chest: 27
- Large chest: 54
- Barrel: 27

## Crafting

### Recipe (Object)

How the item is crafted.

```json
{
  "Recipe": {
    "Input": [
      {
        "ItemId": "Ingredient_Bar_Iron",
        "Quantity": 6
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Categories": ["Weapon_Sword"],
        "Id": "Weapon_Bench"
      }
    ],
    "KnowledgeRequired": true,
    "TimeSeconds": 3,
    "Output": {
      "Quantity": 1
    }
  }
}
```

**Input Types:**
- `ItemId` - Specific item
- `ResourceTypeId` - Any resource of type
- `TagPattern` - Items matching tags

**BenchRequirement Types:**
- `"Crafting"` - Crafting benches
- `"StructuralCrafting"` - Builder benches

**RequiredTierLevel:**
```json
{
  "BenchRequirement": [{
    "Id": "Workbench",
    "Type": "Crafting",
    "Categories": ["Workbench_Tools"],
    "RequiredTierLevel": 2
  }]
}
```
Requires the bench to be at a specific tier level to craft the item.

## Interactions

### Interactions (Object/Array)

Item usage behaviors.

```json
{
  "Interactions": {
    "Type": "Consume",
    "Next": {
      "Type": "GiveHealth",
      "Amount": 20
    }
  }
}
```

**Common Types:**
See [Interaction Types List](109_Interaction_Types_List.md)

### InteractionVars (Object)

Variable interaction definitions with damage, stats, and effects.

```json
{
  "InteractionVars": {
    "Primary_Attack": {
      "Interactions": [
        {
          "Parent": "Weapon_Sword_Primary_Swing_Left",
          "DamageCalculator": {
            "BaseDamage": { "Physical": 15 },
            "RandomPercentageModifier": 0.1
          },
          "EntityStatsOnHit": [
            {
              "EntityStatId": "SignatureEnergy",
              "Amount": 3
            }
          ],
          "DamageEffects": {
            "WorldSoundEventId": "SFX_Sword_T2_Impact",
            "LocalSoundEventId": "SFX_Sword_T2_Impact"
          },
          "StaminaCost": {
            "Value": 10,
            "CostType": "Damage"
          }
        }
      ]
    }
  }
}
```

**InteractionVars Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Parent` | String | Inherit from parent interaction template |
| `DamageCalculator` | Object | Damage configuration |
| `EntityStatsOnHit` | Array | Stats granted on successful hit |
| `DamageEffects` | Object | Sound/visual effects on damage |
| `StaminaCost` | Object | Stamina cost configuration |

**EntityStatsOnHit:**
```json
{
  "EntityStatsOnHit": [
    {
      "EntityStatId": "SignatureEnergy",
      "Amount": 3
    }
  ]
}
```
Grants stats to the attacker when they successfully hit an entity.

**DamageEffects:**
```json
{
  "DamageEffects": {
    "WorldSoundEventId": "SFX_Sword_T2_Impact",
    "LocalSoundEventId": "SFX_Sword_T2_Impact"
  }
}
```
Sound effects played on hit (world = audible to all, local = only attacker).

**StaminaCost:**
```json
{
  "StaminaCost": {
    "Value": 10,
    "CostType": "Damage"
  }
}
```
- `Value` - Amount of stamina consumed
- `CostType` - When cost is applied: `"Damage"`, `"Use"`, `"Start"`

**RandomPercentageModifier:**
```json
{
  "RandomPercentageModifier": 0.1
}
```
Adds damage variance (0.1 = ±10% random damage).

## Tool Specifications

### ToolEffectiveness (Object)

Tool efficiency by type.

```json
{
  "ToolEffectiveness": {
    "Pickaxe": 1.0,
    "Axe": 0.5,
    "Shovel": 0.0
  }
}
```

**Values:**
- `1.0` - Full efficiency
- `0.5` - Half efficiency
- `0.0` - Cannot mine

### ToolTier (Integer)

Tool power tier.

```json
{
  "ToolTier": 3
}
```

**Tiers:**
- 1 - Wood/basic
- 2 - Stone/copper
- 3 - Iron/bronze
- 4 - Steel
- 5+ - Advanced materials

### Tool.Specs (Array)

Detailed tool gathering specifications with power, quality, and gather types:

```json
{
  "Tool": {
    "Specs": [
      {
        "Power": 0.5,
        "GatherType": "Rocks",
        "Quality": 3
      },
      {
        "Power": 0.25,
        "GatherType": "OreIron"
      },
      {
        "Power": 0.125,
        "GatherType": "OreThorium"
      }
    ]
  }
}
```

**Spec Properties:**
| Property | Type | Description |
|----------|------|-------------|
| `Power` | Float | Mining speed multiplier (0.0-1.0) |
| `GatherType` | String | Type of material this spec applies to |
| `Quality` | Integer | Tool quality level for this spec |

**GatherType Values:**
- **General:** `Rocks`, `Soil`, `Wood`, `Plants`
- **Ores:** `OreCopper`, `OreIron`, `OreSilver`, `OreGold`, `OreThorium`, `OreCobalt`, `OreAdamantite`, `OreMithril`
- **Special:** `Crystal`, `Ice`, `Sand`

**Example: Iron Pickaxe with Ore Specs**
```json
{
  "Tool": {
    "Specs": [
      { "Power": 0.5, "GatherType": "Rocks", "Quality": 3 },
      { "Power": 0.25, "GatherType": "OreIron" },
      { "Power": 0.125, "GatherType": "OreThorium" }
    ]
  }
}
```

## Food Properties

### FoodStats (Object)

Food consumption values.

```json
{
  "FoodStats": {
    "Hunger": 5,
    "Saturation": 3,
    "EatingTime": 1.5
  }
}
```

**Properties:**
- `Hunger` - Hunger restored
- `Saturation` - Saturation restored
- `EatingTime` - Seconds to consume

## Effects

### PassiveEffects (Array)

Always-active effects when equipped.

```json
{
  "PassiveEffects": [
    {
      "EffectId": "Effect_Fire_Resistance",
      "Strength": 1.0,
      "Permanent": true
    }
  ]
}
```

### ActiveEffects (Array)

Effects applied on use.

```json
{
  "ActiveEffects": [
    {
      "EffectId": "Effect_Speed_Boost",
      "Duration": 10.0,
      "Strength": 1.5
    }
  ]
}
```

## Advanced Properties

### Tags (Object)

From `Bench_WorkBench.json` - Categorization tags:

```json
{
  "Tags": {
    "Type": ["Bench"]
  }
}
```

**Usage:**
- Organize items by type
- Filter in searches/queries
- Reference in tag patterns

### Metadata (Object)

From `Bench_Furnace.json` - Custom data in recipe outputs:

```json
{
  "Recipe": {
    "Output": [
      {
        "ItemId": "Bench_Furnace",
        "Metadata": {
          "BlockState": {
            "Type": "processingBench",
            "FuelContainer": {
              "Capacity": 2
            }
          }
        }
      }
    ]
  }
}
```

**Usage:**
- Store block state data
- Pass custom properties to spawned items
- Configure complex item behaviors

### ItemAppearanceConditions (Object)

From `Template_Glider.json` - Change item appearance based on stats:

```json
{
  "ItemAppearanceConditions": {
    "GlidingActive": [
      {
        "Condition": [1, 1],
        "Model": "Items/Glider/Glider.blockymodel",
        "Texture": "Items/Glider/Feran_Glider_Texture.png"
      }
    ]
  }
}
```

**Usage:**
- Dynamic item appearance
- Visual feedback for item states
- Conditional model/texture switching

## Property Validation

### Required Properties

**Minimum viable item:**
```json
{
  "Parent": "Template_Item",
  "TranslationProperties": {
    "Name": "server.items.MyItem.name"
  },
  "Icon": "Icons/MyItem.png"
}
```

### Recommended Properties

**Well-configured item:**
```json
{
  "Parent": "Template_Weapon_Sword",
  "TranslationProperties": {
    "Name": "server.items.MyItem.name",
    "Description": "server.items.MyItem.description"
  },
  "Icon": "Icons/MyItem.png",
  "Model": "Items/MyItem.blockymodel",
  "Texture": "Items/MyItem_Texture.png",
  "Quality": "Rare",
  "ItemLevel": 25,
  "MaxDurability": 200
}
```

## Common Property Mistakes

❌ **Wrong:** `"Quality": "Legendary"` with `"ItemLevel": 1`  
✅ **Correct:** Match quality to item level (Legendary = 50+)

❌ **Wrong:** `"MaxStackSize": 64` on a weapon  
✅ **Correct:** `"MaxStackSize": 1` for equipment

❌ **Wrong:** `"EquipmentSlot": "Head"` without `"ArmorType"`  
✅ **Correct:** Include `"ArmorType": "Heavy"` for armor

❌ **Wrong:** Recipe with non-existent `"ItemId"`  
✅ **Correct:** Verify all referenced items exist

## Property Inheritance Rules

### Override Behavior

```json
// Parent: Template_Weapon_Sword
{
  "MaxDurability": 200,
  "AttackSpeed": 1.0
}

// Child: Weapon_Sword_Fast
{
  "Parent": "Template_Weapon_Sword",
  "AttackSpeed": 1.5  // Overrides parent
  // MaxDurability: 200 (inherited)
}
```

### Merging Objects

```json
// Parent
{
  "ItemStats": {
    "Strength": 5,
    "Dexterity": 3
  }
}

// Child
{
  "ItemStats": {
    "Strength": 8  // Override
    // Dexterity: 3 (inherited)
  }
}
```

### Replacing Arrays

```json
// Parent
{
  "Categories": ["Items.Weapons"]
}

// Child
{
  "Categories": ["Items.Weapons", "Items.Special"]  // Replaces entire array
}
```

## Related Guides

- [Item System Overview](174_Item_System_Overview.md) - How items work together
- [Creating Items](02_Items.md) - Item creation guide
- [Interaction Types](109_Interaction_Types_List.md) - All interaction types
- [Item Categories](79_Item_Categories.md) - Category system
- [Asset Types Reference](173_Asset_Types_Reference.md) - All asset types

---

**Previous:** [Item System Overview](174_Item_System_Overview.md) | **Next:** [Advanced Techniques](176_Advanced_Techniques.md)
