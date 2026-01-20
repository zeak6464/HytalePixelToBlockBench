# Interaction Type: OpenContainer

Open container UI (chests, barrels, inventory blocks).

## Overview

`OpenContainer` opens the container UI for blocks that store items. Used when interacting with chests, barrels, and other storage blocks.

## Example from Game Files

### Open Container Interaction

Open container interactions open container blocks. These are used for chests, storage containers, and inventory-based blocks.

## Basic Structure

```json
{
  "Type": "OpenContainer"
}
```

## Properties

None required. The container block must have container component configured.

## Complete Examples

### Example 1: Basic Container

```json
{
  "Type": "OpenContainer"
}
```

Opens container UI for the targeted block.

### Example 2: With Animation

```json
{
  "Type": "Simple",
  "Effects": {
    "ItemAnimationId": "Open",
    "WorldSoundEventId": "SFX_Chest_Open"
  },
  "Next": {
    "Type": "OpenContainer"
  }
}
```

Plays open animation/sound, then opens container.

## Tips

1. **Block component** - Block must have container component configured
2. **Animation** - Add Simple interaction before for opening animation
3. **Sounds** - Include sound effects for better UX
4. **Use with UseBlock** - Typically used in block Use interactions

---

**Previous:** [TeleportConfigInstance](148_Interaction_Type_TeleportConfigInstance.md) | **Next:** [OpenProcessingBench](150_Interaction_Type_OpenProcessingBench.md)
