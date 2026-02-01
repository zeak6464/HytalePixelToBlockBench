# Block Sets

Learn how block sets work for categorizing blocks and grouping them for world generation and NPC spawning.

## Overview

Block sets in Hytale categorize blocks by material type (e.g., Stone, Wood, Soil) for world generation, NPC spawning, and game logic. They're defined in `Server/Item/Block/Sets/`.

## Location
`Server/Item/Block/Sets/`

## Example from Game Files

### Wood Block Set

From `Server/Item/Block/Sets/Wood.json`:

```1:4:Server/Item/Block/Sets/Wood.json
{
  "IncludeBlockTypes": [
    "*Wood*"
  ]
}
```

This shows a block set that includes all blocks with "Wood" in their type name.

## Basic Block Set Structure

Create `Server/Item/Block/Sets/MyCustom_Material.json`:

```json
{
  "IncludeBlockTypes": [
    "MyCustom_*",
    "Custom_Block_*"
  ],
  "ExcludeBlockTypes": [
    "*Water*",
    "*Mud"
  ]
}
```

## Block Set Properties

### Include Block Types

```json
{
  "IncludeBlockTypes": [
    "Soil_*",
    "Plant_*"
  ]
}
```

Wildcard patterns for blocks to include:
- **`"Soil_*"`** - All blocks starting with `Soil_`
- **`"Plant_*"`** - All blocks starting with `Plant_`
- **`"Rock_Stone"`** - Specific block ID

### Exclude Block Types

```json
{
  "ExcludeBlockTypes": [
    "Plant_Leaves*",
    "*Water*",
    "*Mud"
  ]
}
```

Wildcard patterns for blocks to exclude:
- **`"Plant_Leaves*"`** - Excludes all leaf blocks
- **`"*Water*"`** - Excludes any block with "Water" in name
- **`"*Mud"`** - Excludes any block ending with "Mud"

## Common Block Sets

### Soil

`Server/Item/Block/Sets/Soil.json`:

```json
{
  "IncludeBlockTypes": [
    "Soil_*",
    "Plant_*"
  ],
  "ExcludeBlockTypes": [
    "Plant_Leaves*",
    "*Water*",
    "*Mud"
  ]
}
```

Includes dirt, grass, sand, and plant blocks.

### Rock/Stone

`Server/Item/Block/Sets/Stone.json`:

```json
{
  "IncludeBlockTypes": [
    "Rock*"
  ]
}
```

Includes all rock blocks (blocks starting with "Rock").

### Wood

`Server/Item/Block/Sets/Wood.json`:

```json
{
  "IncludeBlockTypes": [
    "*Wood*"
  ]
}
```

Includes all blocks with "Wood" in their name.

## Using Block Sets

### In NPC Spawning

NPCs can spawn on specific block sets:

```json
{
  "NPCs": [
    {
      "SpawnBlockSet": "Soil",
      "Id": "Goblin_Scrapper"
    }
  ]
}
```

### In World Generation

Biomes use block sets for terrain:

```json
{
  "Biome": "Plains1",
  "BlockSets": ["Stone", "Dirt", "Grass"]
}
```

### In Block Conditions

Tests and conditions can check block sets:

```json
{
  "Matchers": [
    {
      "Block": {
        "BlockSet": "Soil"
      }
    }
  ]
}
```

## Complete Example: Custom Block Set

### 1. Block Set Definition

`Server/Item/Block/Sets/MyCustom_Magical.json`:

```json
{
  "IncludeBlockTypes": [
    "Magic_*",
    "Rune_*",
    "Crystal_*"
  ],
  "ExcludeBlockTypes": [
    "*Broken*"
  ]
}
```

### 2. Using in NPC Spawn

```json
{
  "NPCs": [
    {
      "SpawnBlockSet": "MyCustom_Magical",
      "Id": "Magic_Elemental"
    }
  ]
}
```

### 3. Using in Biome

```json
{
  "Biome": "Magic_Plains",
  "BlockSets": ["MyCustom_Magical", "Soil"]
}
```

## Tips for Creating Block Sets

1. **Use wildcards** - Pattern matching is more flexible than listing every block
2. **Exclude carefully** - Remove blocks that shouldn't be in the set
3. **Group logically** - Group blocks by material/function
4. **Test matching** - Ensure patterns match intended blocks
5. **Document sets** - Keep track of which blocks belong to which sets
6. **Use in spawning** - Block sets control NPC spawn locations
7. **Reference in generation** - Block sets define biome terrain types

---

**Previous:** [World Generation](38_World_Generation.md) | **Next:** [Block Sound Sets](40_Block_Sound_Sets.md)
