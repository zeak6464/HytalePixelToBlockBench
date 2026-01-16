# Block Type Lists

Learn how to create lists of block types for categorization and organization.

## Overview

Block type lists are simple JSON arrays that list block IDs. They're used for organization, categorization, and referencing groups of blocks in world generation and game logic. Unlike block sets (which use wildcards), block type lists explicitly list every block ID.

## Location
`Server/BlockTypeList/`

## Basic Block Type List Structure

Create `Server/BlockTypeList/MyCustom_Blocks.json`:

```json
{
  "Blocks": [
    "MyCustom_Block_1",
    "MyCustom_Block_2",
    "MyCustom_Block_3"
  ]
}
```

## Block Type List Properties

### Blocks Array

```json
{
  "Blocks": [
    "Rock_Stone",
    "Rock_Stone_Cobble",
    "Rock_Stone_Brick"
  ]
}
```

Array of block item IDs (strings).

## Common Block Type Lists

### Ores

`Server/BlockTypeList/Ores.json`:

```json
{
  "Blocks": [
    "Ore_Adamantite_Basalt",
    "Ore_Adamantite_Shale",
    "Ore_Adamantite_Slate",
    "Ore_Adamantite_Stone",
    "Ore_Adamantite_Volcanic",
    "Ore_Cobalt_Basalt",
    "Ore_Cobalt_Sandstone",
    "Ore_Cobalt_Shale",
    "Ore_Cobalt_Slate",
    "Ore_Cobalt_Stone",
    "Ore_Cobalt_Volcanic",
    "Ore_Copper_Basalt",
    "Ore_Copper_Sandstone",
    "Ore_Copper_Shale",
    "Ore_Copper_Stone",
    "Ore_Copper_Volcanic",
    "Ore_Gold_Basalt",
    "Ore_Gold_Sandstone",
    "Ore_Gold_Shale",
    "Ore_Gold_Stone",
    "Ore_Gold_Volcanic",
    "Ore_Iron_Basalt",
    "Ore_Iron_Sandstone",
    "Ore_Iron_Shale",
    "Ore_Iron_Slate",
    "Ore_Iron_Stone",
    "Ore_Iron_Volcanic",
    "Ore_Mithril_Basalt",
    "Ore_Mithril_Magma",
    "Ore_Mithril_Slate",
    "Ore_Mithril_Stone",
    "Ore_Mithril_Volcanic",
    "Ore_Onyxium_Basalt",
    "Ore_Onyxium_Sandstone",
    "Ore_Onyxium_Shale",
    "Ore_Onyxium_Stone",
    "Ore_Onyxium_Volcanic",
    "Ore_Silver_Basalt",
    "Ore_Silver_Sandstone",
    "Ore_Silver_Shale",
    "Ore_Silver_Slate",
    "Ore_Silver_Stone",
    "Ore_Silver_Volcanic",
    "Ore_Thorium_Basalt",
    "Ore_Thorium_Sandstone",
    "Ore_Thorium_Shale",
    "Ore_Thorium_Stone",
    "Ore_Thorium_Volcanic"
  ]
}
```

### Soils

`Server/BlockTypeList/Soils.json`:

```json
{
  "Blocks": [
    "Soil_Dirt",
    "Soil_Grass",
    "Soil_Sand",
    "Soil_Clay"
  ]
}
```

### Plants and Trees

`Server/BlockTypeList/PlantsAndTrees.json`:

```json
{
  "Blocks": [
    "Plant_Grass_Tall",
    "Plant_Flower_Blue",
    "Tree_Oak",
    "Tree_Birch"
  ]
}
```

### Rock

`Server/BlockTypeList/Rock.json`:

```json
{
  "Blocks": [
    "Rock_Stone",
    "Rock_Stone_Cobble",
    "Rock_Basalt",
    "Rock_Shale"
  ]
}
```

## Using Block Type Lists

### In World Generation

Block type lists can be referenced in world generation:

```json
{
  "Biome": "Desert",
  "OreGeneration": {
    "BlockList": "Ores",
    "Density": 0.01
  }
}
```

### In Block Spawners

Block spawners can use block type lists:

```json
{
  "BlockSpawner": {
    "BlockList": "Ores",
    "SpawnDepth": [10, 50],
    "SpawnRate": 0.005
  }
}
```

### In Game Logic

Block type lists can be used for filtering:

```json
{
  "Filter": {
    "BlockTypeList": "Ores",
    "Action": "SpawnOreVein"
  }
}
```

## Block Type Lists vs Block Sets

### Block Type Lists

- **Explicit** - Lists every block ID individually
- **Static** - Must manually add new blocks
- **Precise** - Exact control over which blocks are included

### Block Sets

- **Pattern-based** - Uses wildcards (e.g., `"Rock_*"`)
- **Dynamic** - Automatically includes matching blocks
- **Flexible** - Easy to update with new blocks

**When to use Block Type Lists:**
- When you need exact control
- When blocks don't follow a naming pattern
- When you want explicit documentation

**When to use Block Sets:**
- When blocks follow naming patterns
- When you want automatic inclusion of new blocks
- When flexibility is preferred

## Complete Example: Custom Block Collection

### 1. Block Type List Definition

`Server/BlockTypeList/MyCustom_Magical_Blocks.json`:

```json
{
  "Blocks": [
    "Block_Magic_Crystal_Blue",
    "Block_Magic_Crystal_Red",
    "Block_Magic_Rune_Stone",
    "Block_Magic_Enchanted_Wood",
    "Block_Magic_Altar"
  ]
}
```

### 2. Using in World Generation

```json
{
  "Biome": "Magic_Forest",
  "Decoration": {
    "BlockLists": ["MyCustom_Magical_Blocks"],
    "Density": 0.02,
    "SpawnHeight": [50, 100]
  }
}
```

## Tips for Creating Block Type Lists

1. **Keep organized** - Group related blocks together
2. **Use descriptive names** - Clear list names (e.g., `Ores`, `Soils`)
3. **Maintain accuracy** - Ensure all block IDs are valid
4. **Document purpose** - Note what each list is used for
5. **Consider block sets** - Use block sets if patterns work better
6. **Update regularly** - Add new blocks as they're created
7. **Test references** - Verify lists work in world generation

---

**Previous:** [Prefab Lists](49_Prefab_Lists.md) | **Next:** [Model VFX](51_Model_VFX.md)
