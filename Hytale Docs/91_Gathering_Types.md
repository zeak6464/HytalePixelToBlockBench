# Gathering Types

Learn how gathering types determine what tools are required to break blocks and gather resources.

## Overview

Gathering types categorize blocks by what tool is needed to break them. Each gather type corresponds to specific tool types (pickaxes for Rocks, axes for Woods, etc.).

**Update 2 (Jan 2026):** **Adamantite** now requires a **Thorium or Cobalt** pickaxe to mine. Lower-tier pickaxes deal much less damage to higher-tier ores; upgrading significantly reduces hits required. See [Tools](14_Tools.md) and [Patch Notes Update 2](Patch_Notes_Update_2.md).

## Location
Gathering types are configured in `BlockType.Gathering.Breaking.GatherType` in block definitions.

## Example from Game Files

### Stone Block Gathering Type

From `Server/Item/Items/Rock/Stone/Rock_Stone.json`:

```18:22:Server/Item/Items/Rock/Stone/Rock_Stone.json
    "Gathering": {
      "Breaking": {
        "GatherType": "Rocks",
        "ItemId": "Rock_Stone_Cobble"
      }
    },
```

This shows a gathering type configuration where stone blocks require a pickaxe (Rocks gather type) to break and drop stone cobble.

## Basic Gathering Type Configuration

```json
{
  "BlockType": {
    "Gathering": {
      "Breaking": {
        "GatherType": "Rocks"
      }
    }
  }
}
```

## Common Gathering Types

### Rocks

```json
{
  "GatherType": "Rocks"
}
```

Requires **pickaxe** tools. Used for:
- Stone blocks
- Ores
- Rock formations

### Woods

```json
{
  "GatherType": "Woods"
}
```

Requires **axe/hatchet** tools. Used for:
- Wooden blocks
- Trees
- Logs

### Soils

```json
{
  "GatherType": "Soils"
}
```

Requires **shovel** tools. Used for:
- Dirt
- Sand
- Gravel

### SoftBlocks

```json
{
  "GatherType": "SoftBlocks"
}
```

Can be broken with **bare hands** or any tool. Used for:
- Weak blocks
- Player-removable blocks
- Decorative blocks

### Benches

```json
{
  "GatherType": "Benches"
}
```

Special gathering type for crafting benches and furniture.

### DungeonBlocks

```json
{
  "GatherType": "DungeonBlocks"
}
```

Special gathering type for dungeon-specific blocks.

### Branches

```json
{
  "GatherType": "Branches"
}
```

Special gathering type for branch/plant blocks.

## Tool Power vs Gather Type

Tools define power for each gather type:

```json
{
  "Tool": {
    "Specs": [
      {
        "Power": 1.0,
        "GatherType": "Rocks"
      },
      {
        "Power": 0.25,
        "GatherType": "Woods"
      }
    ]
  }
}
```

- **`Power: 1.0`** - 100% effective (full speed)
- **`Power: 0.5`** - 50% effective (half speed)
- **`Power: 0.25`** - 25% effective (very slow)

## Unarmed Gathering

See the [Unarmed Gathering](69_Unarmed_Gathering.md) guide for configuring what can be gathered without tools.

## Tips for Gathering Types

1. **Tool matching** - Match gather types to appropriate tool types
2. **Balance** - Use SoftBlocks for weak/easily broken blocks
3. **Special types** - Use specialized gather types (Benches, DungeonBlocks) for special mechanics

---

**Previous:** [Block Entity Components](90_Block_Entity_Components.md) | **Next:** [Interaction Conditions](92_Interaction_Conditions.md)
