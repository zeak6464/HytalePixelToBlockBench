# Unarmed Gathering

Learn how to configure gathering behaviors for unarmed (no tool) interactions with blocks.

## Overview

Unarmed gathering defines what happens when players interact with blocks without tools. This includes breaking blocks, gathering resources, and interaction behaviors.

## Location
`Server/Item/Unarmed/Gathering/`

## Unarmed Gathering Structure

Unarmed gathering is typically configured in block definitions via `Gathering.Breaking` properties. The system determines what can be gathered with bare hands vs requiring tools.

## Block Gathering Configuration

In block/item definitions:

```json
{
  "BlockType": {
    "Gathering": {
      "Breaking": {
        "GatherType": "SoftBlocks"
      }
    }
  }
}
```

## GatherTypes for Unarmed

- **`SoftBlocks`** - Can be broken with hands (dirt, sand, etc.)
- **`Rocks`** - Requires pickaxe
- **`Woods`** - Requires axe
- **`Unbreakable`** - Cannot be broken

## Tips for Unarmed Gathering

1. **GatherType** - Set appropriate gather types for unarmed breaking
2. **Tool requirements** - Use tool-specific gather types for harder blocks
3. **Game balance** - Balance what can be gathered unarmed vs with tools

---

**Previous:** [Player Tools Menu](68_Player_Tools_Menu.md) | **Next:** [Item Qualities](70_Item_Qualities.md)
