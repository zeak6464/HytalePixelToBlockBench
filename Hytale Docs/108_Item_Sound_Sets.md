# Item Sound Sets

Learn how to configure item-specific sound sets for drag, drop, and interaction sounds.

## Overview

Item sound sets define sounds that play when items are dragged, dropped, or interacted with in the inventory. They're separate from block sound sets (footsteps, breaking) and weapon sound sets (swing, impact), focusing on UI and inventory interactions.

## Location
`Server/Audio/ItemSounds/`

## Basic Item Sound Set Structure

Create `Server/Audio/ItemSounds/ISS_MyCustom.json`:

```json
{
  "SoundEvents": {
    "Drop": "SFX_Drop_MyCustom",
    "Drag": "SFX_Drag_MyCustom"
  }
}
```

## Item Sound Set Properties

### Drop

```json
{
  "Drop": "SFX_Drop_Weapons_Books"
}
```

Sound event that plays when the item is dropped (released from drag).

**Sound Event Location:** `Server/Audio/SoundEvents/`

### Drag

```json
{
  "Drag": "SFX_Drag_Weapons_Books"
}
```

Sound event that plays when the item is dragged in the inventory.

### SoundEvents Object

All sound events are grouped in the `SoundEvents` object:

```json
{
  "SoundEvents": {
    "Drop": "SFX_Drop_Weapons_Books",
    "Drag": "SFX_Drag_Weapons_Books"
  }
}
```

## Common Item Sound Sets

### Weapons

`Server/Audio/ItemSounds/ISS_Weapons_Books.json`:

```json
{
  "SoundEvents": {
    "Drop": "SFX_Drop_Weapons_Books",
    "Drag": "SFX_Drag_Weapons_Books"
  }
}
```

Used for spellbooks and magical books.

### Blocks

`Server/Audio/ItemSounds/ISS_Blocks_Wood.json`:

```json
{
  "SoundEvents": {
    "Drop": "SFX_Drop_Blocks_Wood",
    "Drag": "SFX_Drag_Blocks_Wood"
  }
}
```

Used for wooden blocks placed as items.

### Items

`Server/Audio/ItemSounds/ISS_Items_Metal.json`:

```json
{
  "SoundEvents": {
    "Drop": "SFX_Drop_Items_Metal",
    "Drag": "SFX_Drag_Items_Metal"
  }
}
```

Used for metal items (ingots, tools, etc.).

## Linking Sound Sets to Items

### In Item Definition

Reference the sound set in your item:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Spellbook_Fire.name"
  },
  "Model": "Items/Weapons/Spellbook/Book.blockymodel",
  "ItemSoundSetId": "ISS_Weapons_Books"
}
```

**Property:**
- **`ItemSoundSetId`** - ID of the item sound set (file name without `.json` and without `ISS_` prefix)

**Note:** The `ItemSoundSetId` should reference the file name without the `ISS_` prefix:
- File: `Server/Audio/ItemSounds/ISS_Weapons_Books.json`
- ID: `"ISS_Weapons_Books"`

### Complete Example

`Server/Item/Items/Weapon/Spellbook/Weapon_Spellbook_Fire.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Spellbook_Fire.name"
  },
  "Categories": ["Items.Weapons"],
  "Quality": "Common",
  "ItemLevel": 40,
  "Model": "Items/Weapons/Spellbook/Book.blockymodel",
  "Texture": "Items/Weapons/Spellbook/Book_Fire_Texture.png",
  "ItemSoundSetId": "ISS_Weapons_Books",
  "Tags": {
    "Type": ["Weapon"],
    "Family": ["Spellbook"]
  }
}
```

## Sound Set Categories

### Weapon Sound Sets

| Sound Set | Use Case |
|-----------|----------|
| `ISS_Weapons_Books` | Spellbooks, magical books |
| `ISS_Weapons_Wood` | Wooden weapons (bows, staffs) |
| `ISS_Weapons_Stone_Small` | Small stone weapons |
| `ISS_Weapons_Stone_Large` | Large stone weapons |
| `ISS_Weapons_Blade_Large` | Large bladed weapons |
| `ISS_Weapons_Blade_Small` | Small bladed weapons |
| `ISS_Weapons_Blunt_Large` | Large blunt weapons |
| `ISS_Weapons_Blunt_Small` | Small blunt weapons |
| `ISS_Weapons_Arrows` | Arrows, projectiles |
| `ISS_Weapons_Shield_Wood` | Wooden shields |
| `ISS_Weapons_Shield_Metal` | Metal shields |
| `ISS_Weapons_Wand` | Wands, magical tools |

### Block Sound Sets

| Sound Set | Use Case |
|-----------|----------|
| `ISS_Blocks_Wood` | Wooden blocks |
| `ISS_Blocks_Stone` | Stone blocks |
| `ISS_Blocks_Gravel` | Gravel blocks |
| `ISS_Blocks_Soft` | Soft blocks (wool, fabric) |
| `ISS_Blocks_Splatty` | Squishy blocks (slime, etc.) |

### Item Sound Sets

| Sound Set | Use Case |
|-----------|----------|
| `ISS_Items_Metal` | Metal items (ingots, tools) |
| `ISS_Items_Leather` | Leather items |
| `ISS_Items_Cloth` | Cloth items |
| `ISS_Items_Gems` | Gems, crystals |
| `ISS_Items_Ingots` | Ingots, bars |
| `ISS_Items_Potion` | Potions, consumables |
| `ISS_Items_Seeds` | Seeds, plant items |
| `ISS_Items_Foliage` | Leaves, plants |
| `ISS_Items_Shells` | Shells, organic items |
| `ISS_Items_Bones` | Bones, skeletal items |
| `ISS_Items_Clay` | Clay items |
| `ISS_Items_Paper` | Paper, books |
| `ISS_Items_Chest` | Chests, containers |
| `ISS_Items_Gadget` | Gadgets, mechanical items |
| `ISS_Items_Splatty` | Squishy items |

### Armor Sound Sets

| Sound Set | Use Case |
|-----------|----------|
| `ISS_Armor_Cloth` | Cloth armor |
| `ISS_Armor_Leather` | Leather armor |
| `ISS_Armor_Heavy` | Heavy armor (metal, plate) |

### Default

| Sound Set | Use Case |
|-----------|----------|
| `ISS_Default` | Default fallback sound set |

## Complete Examples

### Example 1: Custom Weapon Sound Set

`Server/Audio/ItemSounds/ISS_Weapons_Custom.json`:

```json
{
  "SoundEvents": {
    "Drop": "SFX_Drop_Weapons_Custom",
    "Drag": "SFX_Drag_Weapons_Custom"
  }
}
```

**Using in Item:**

```json
{
  "ItemSoundSetId": "ISS_Weapons_Custom"
}
```

### Example 2: Custom Item Sound Set

`Server/Audio/ItemSounds/ISS_Items_Custom.json`:

```json
{
  "SoundEvents": {
    "Drop": "SFX_Drop_Items_Custom",
    "Drag": "SFX_Drag_Items_Custom"
  }
}
```

**Using in Item:**

```json
{
  "ItemSoundSetId": "ISS_Items_Custom"
}
```

### Example 3: Block Item Sound Set

`Server/Audio/ItemSounds/ISS_Blocks_Custom.json`:

```json
{
  "SoundEvents": {
    "Drop": "SFX_Drop_Blocks_Custom",
    "Drag": "SFX_Drag_Blocks_Custom"
  }
}
```

**Using in Block Item:**

```json
{
  "ItemSoundSetId": "ISS_Blocks_Custom"
}
```

## Creating Sound Events

The sound events referenced in item sound sets must exist in `Server/Audio/SoundEvents/`.

See [Sound Effects](35_Sound_Effects.md) for creating sound events.

## Difference from Block Sound Sets

**Item Sound Sets (`ItemSoundSetId`):**
- Sounds for inventory interactions (drag, drop)
- Applied to items when held in inventory
- Location: `Server/Audio/ItemSounds/`

**Block Sound Sets (`BlockSoundSetId`):**
- Sounds for block interactions (footsteps, breaking, placing)
- Applied to blocks when placed in world
- Location: `Server/Item/Block/Sounds/`

**Both can be used on the same item/block:**
```json
{
  "ItemSoundSetId": "ISS_Blocks_Wood",  // Inventory sounds
  "BlockType": {
    "BlockSoundSetId": "Wood"  // World block sounds
  }
}
```

## Tips for Item Sound Sets

1. **Naming convention** - Use `ISS_` prefix (Item Sound Set)
2. **Category grouping** - Group by material/type (Weapons, Items, Blocks, Armor)
3. **Reuse common sets** - Use existing sound sets for similar items
4. **Create custom sets** - Create new sets for unique item categories
5. **Sound quality** - Match sound type to item material (wood = wood sound, metal = metal sound)
6. **Drag vs Drop** - Different sounds for dragging (continuous) vs dropping (single event)
7. **Fallback** - Use `ISS_Default` for items without specific sounds
8. **Test in game** - Verify sounds feel right in inventory interactions

## File Naming

Item sound set files should follow the pattern:

```
ISS_{Category}_{Material}.json
```

Examples:
- `ISS_Weapons_Books.json`
- `ISS_Items_Metal.json`
- `ISS_Blocks_Wood.json`
- `ISS_Armor_Cloth.json`

The `ItemSoundSetId` in items should match the file name exactly.

---

**Previous:** [Farming Coops](107_Farming_Coops.md) | **Next:** [Interaction Types List](109_Interaction_Types_List.md)
