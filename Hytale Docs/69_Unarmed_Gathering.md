# Unarmed Gathering

Learn how to configure gathering behaviors for unarmed (no tool) interactions with blocks.

## Overview

Unarmed gathering defines what happens when players interact with blocks without tools. The `Power` value determines gathering efficiency (higher = faster breaking).

## Location

- Gathering configs: `Server/Item/Unarmed/Gathering/`
- Interaction configs: `Server/Item/Unarmed/Interactions/`

## All Gathering Types with Power Values

| GatherType | Power | Description |
|------------|-------|-------------|
| `SoftBlocks` | 1.0 | Full speed - dirt, sand, leaves, etc. |
| `Soils` | 0.1 | Slow - grass, packed dirt |
| `Benches` | 0.5 | Medium - crafting stations |
| `Cloths` | 0.4 | Medium - fabric/cloth blocks |
| `Branches` | 0.3 | Slow-medium - small wood |
| `Rocks` | 0.035 | Very slow - stone blocks |
| `Woods` | 0.03 | Very slow - tree trunks |
| `VolcanicRocks` | 0.001 | Extremely slow - volcanic stone |
| `DungeonBlocks` | 0.001 | Extremely slow - dungeon structures |
| `OreCopper` | 0.001 | Extremely slow - copper ore |
| `OreIron` | 0.001 | Extremely slow - iron ore |
| `OreSilver` | 0.001 | Extremely slow - silver ore |
| `OreGold` | 0.001 | Extremely slow - gold ore |
| `OreCobalt` | 0.001 | Extremely slow - cobalt ore |
| `OreThorium` | 0.001 | Extremely slow - thorium ore |
| `OreMithril` | 0.001 | Extremely slow - mithril ore |
| `OreAdamantite` | 0.001 | Extremely slow - adamantite ore |

### Power Value Meaning

- **1.0** - Full gathering speed (can break easily with hands)
- **0.5** - Half speed (takes twice as long)
- **0.1** - Very slow (10x longer)
- **0.035** - Extremely slow (nearly impossible unarmed)
- **0.001** - Practically requires a tool

## Example from Game Files

### SoftBlocks (Full Speed)

From `Server/Item/Unarmed/Gathering/SoftBlocks.json`:

```json
{
  "Power": 1
}
```

### Rocks (Very Slow)

From `Server/Item/Unarmed/Gathering/Rocks.json`:

```json
{
  "Power": 0.035
}
```

### Ores (Requires Tool)

From `Server/Item/Unarmed/Gathering/OreCopper.json`:

```json
{
  "Power": 0.001
}
```

## Unarmed Interactions

`Server/Item/Unarmed/Interactions/` defines what happens when using/attacking without a tool.

### Empty.json (No Item Held)

```json
{
  "Interactions": {
    "Wielding": "Double_Jump",
    "Primary": {
      "Type": "Serial",
      "Interactions": ["UseBlock", "Unarmed_Attack"]
    },
    "Use": {
      "Type": "Serial", 
      "Interactions": ["UseBlock", "UseEntity", "BreakBlock"]
    },
    "Pick": "PickBlock"
  }
}
```

Defines:
- **Wielding** - Double jump ability
- **Primary** - Try to use block, then attack
- **Use** - Try to use block/entity, then break
- **Pick** - Pick up blocks

### Block.json (Holding a Block)

```json
{
  "Interactions": {
    "Primary": "Block_Primary",
    "Secondary": "Block_Secondary"
  }
}
```

### Item.json (Holding an Item)

```json
{
  "Interactions": {
    "Primary": "Block_Primary"
  }
}
```

## Using Gather Types in Blocks

Blocks reference gather types via `Gathering.Breaking.GatherType`:

```json
{
  "BlockType": {
    "Gathering": {
      "Breaking": {
        "GatherType": "Rocks",
        "ItemId": "Rock_Stone_Cobble"
      }
    }
  }
}
```

The unarmed power for `Rocks` (0.035) makes stone very slow to break without a pickaxe.

## Tips for Unarmed Gathering

1. **Balance power values** - Higher power = easier to break unarmed
2. **Use 0.001 for tool-required blocks** - Ores, dungeons, etc.
3. **Use 1.0 for soft blocks** - Dirt, leaves, plants
4. **Consider gameplay flow** - Some blocks should be breakable early game

---

**Previous:** [Player Tools Menu](68_Player_Tools_Menu.md) | **Next:** [Item Qualities](70_Item_Qualities.md)
