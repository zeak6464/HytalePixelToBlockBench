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
    "Description": "server.items.Weapon_Sword_Iron.description"
  }
}
```

**Keys:**
- `Name` - Item display name
- `Description` - Item description

**Note:** `Flavor` and `ShortDescription` are not verified in game files. Most items only use `Name`.

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
    "Translation": [0, -13.2]
  }
}
```

**Properties:**
- `Scale` - Size multiplier (default: 1.0)
- `Rotation` - X, Y, Z rotation in degrees
- `Translation` - Array format `[X, Y]` for pixel offset (not `Offset` object)

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
- `99` or `999` - Currency, small items

**Note:** The property is `MaxStack`, not `MaxStackSize`. Item dropability, storage, and hotbar placement are determined by item type and interactions, not simple boolean properties.

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

**Note:** Durability loss on block breaking is configured through `Tool.DurabilityLossBlockTypes` arrays, not a simple property. Item repair is handled through repair kits and benches. See `Tool_Repair_Kit` items for the repair system implementation.

## Equipment

### Armor (Object)

Armor configuration is defined within an `Armor` object, not as top-level properties.

```json
{
  "Armor": {
    "ArmorSlot": "Chest",
    "BaseDamageResistance": {
      "Physical": [
        {
          "Amount": 0.09,
          "CalculationType": "Multiplicative"
        }
      ]
    },
    "DamageResistance": {
      "Physical": [
        {
          "Amount": 0.09,
          "CalculationType": "Multiplicative"
        }
      ]
    }
  }
}
```

**ArmorSlot Values:**
- `"Head"` - Helmet slot
- `"Chest"` - Chest armor slot
- `"Hands"` - Gauntlets/gloves slot
- `"Legs"` - Leg armor slot

**Note:** There is no top-level `EquipmentSlot` property. Use `Armor.ArmorSlot` for armor items.

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

### DamageResistance (within Armor object)

Damage reduction is configured as arrays within the `Armor` object, not as simple numeric values.

```json
{
  "Armor": {
    "DamageResistance": {
      "Physical": [
        {
          "Amount": 0.09,
          "CalculationType": "Multiplicative"
        }
      ]
    },
    "DamageClassEnhancement": {
      "Light": [
        {
          "Amount": 0.05,
          "CalculationType": "Multiplicative"
        }
      ],
      "Signature": [
        {
          "Amount": 0.05,
          "CalculationType": "Multiplicative"
        }
      ]
    }
  }
}
```

**Verified Damage Types in armor files:**
- `Physical`, `Projectile` - in `DamageResistance`
- `Light`, `Signature` - in `DamageClassEnhancement`

**CalculationType Values:**
- `"Multiplicative"` - Percentage-based reduction

### PlayerAnimationsId (String)

Player animation set for item (documented later in Animation section).

## Player Stats

**Note:** Item stat bonuses and modifiers are configured through:
- `Armor.StatModifiers` for armor stat bonuses
- Interactions with `EntityStatsOnHit` for combat stats
- Entity effects for temporary buffs

There is no simple `ItemStats` object with properties like `Strength`, `Dexterity`, `CriticalChance`, or `AttackSpeed`. See the [Entity Stats](28_Entity_Stats.md) and [Potions & Effects](04_Potions_and_Effects.md) guides.

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

**Note:** Equip/unequip sounds are handled through `ItemSoundSetId` which references a sound set containing all item sounds, not individual `EquipSoundEventId` or `UnequipSoundEventId` properties.

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

Configuration for placeable blocks. Properties are nested within `BlockType`.

```json
{
  "BlockType": {
    "DrawType": "Model",
    "CustomModel": "Blocks/Furniture/Chest_Small.blockymodel",
    "Opacity": "Transparent",
    "HitboxType": "Chest_Small",
    "Flags": {
      "IsStackable": false,
      "IsUsable": true
    },
    "BlockSoundSetId": "Wood",
    "BlockParticleSetId": "Wood"
  }
}
```

**DrawType Values:**
- `"Cube"` - Standard cube block
- `"Model"` - Custom 3D model

**Note:** There is no top-level `PlacedBlock` or `ContainerSize` property. Container configuration is handled through block entity components and interactions.

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
    "OutputQuantity": 1
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

## Interactions

### Interactions (Object/Array)

Item usage behaviors.

```json
{
  "Interactions": {
    "Use": {
      "Type": "Serial",
      "Interactions": [
        {
          "Type": "ModifyInventory",
          "AdjustHeldItemQuantity": -1
        },
        {
          "Type": "ChangeStat",
          "EntityStatId": "Health",
          "Amount": 20
        }
      ]
    }
  }
}
```

**Common Types:**
See [Interaction Types List](109_Interaction_Types_List.md)

### InteractionVars (Object)

Variable interaction definitions.

```json
{
  "InteractionVars": {
    "Primary_Attack": {
      "Interactions": [
        {
          "Parent": "Weapon_Sword_Primary_Swing_Left",
          "DamageCalculator": {
            "BaseDamage": { "Physical": 15 }
          }
        }
      ]
    }
  }
}
```

## Tool Specifications

### Tool (Object)

Tool configuration uses a `Tool` object with `Specs` array, not `ToolEffectiveness` or `ToolTier` properties.

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

**Specs Properties:**
- `Power` - Gathering power (higher = faster breaking)
- `GatherType` - Which block categories this power applies to (`SoftBlocks`, `Soils`, `Rocks`, `Woods`, `Ores`, etc.)

**Note:** Tool tiers are implemented through the parent template hierarchy and `ItemLevel`, not a simple `ToolTier` integer.

## Food Properties

**Note:** Food items do not use a simple `FoodStats` object. Food effects (hunger restoration, buffs) are implemented through:
- `Interactions` with `Type: "Consume"` 
- `ChangeStat` interactions for hunger/saturation
- `ApplyEffect` for food buffs

See [Food Items](13_Food_Items.md) for actual food configuration patterns.

## Effects

**Note:** Items do not have simple `PassiveEffects` or `ActiveEffects` arrays. Effects are applied through:
- `Interactions` with `Type: "ApplyEffect"` for use-triggered effects
- `Armor.StatModifiers` for equipped stat bonuses
- Entity effect system for complex buff management

See [Potions & Effects](04_Potions_and_Effects.md) and [Interaction Type ApplyEffect](120_Interaction_Type_ApplyEffect.md) for effect configuration.

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

❌ **Wrong:** `"MaxStack": 64` on a weapon  
✅ **Correct:** `"MaxStack": 1` for equipment (or omit for default 1)

❌ **Wrong:** Using `"EquipmentSlot"` as top-level property  
✅ **Correct:** Use `"Armor": { "ArmorSlot": "Head" }` for armor

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
  "Armor": {
    "ArmorSlot": "Chest",
    "BaseDamageResistance": { "Physical": [{ "Amount": 0.05 }] }
  }
}

// Child
{
  "Armor": {
    "BaseDamageResistance": { "Physical": [{ "Amount": 0.10 }] }
    // ArmorSlot: "Chest" (inherited)
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
