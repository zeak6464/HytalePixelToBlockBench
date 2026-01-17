# Interaction Type: OpenProcessingBench

Open crafting/processing bench UI (workbenches, furnaces, crafting stations).

## Overview

`OpenProcessingBench` opens the processing/crafting UI for benches. Used when interacting with workbenches, furnaces, and other crafting stations.

## Basic Structure

```json
{
  "Type": "OpenProcessingBench"
}
```

## Properties

None required. The bench block must have processing bench component configured.

## Complete Examples

### Example 1: Basic Bench

```json
{
  "Type": "OpenProcessingBench"
}
```

Opens processing bench UI for the targeted block.

### Example 2: With Animation

```json
{
  "Type": "Simple",
  "Effects": {
    "ItemAnimationId": "Interact",
    "WorldSoundEventId": "SFX_Bench_Open"
  },
  "Next": {
    "Type": "OpenProcessingBench"
  }
}
```

Plays interaction animation/sound, then opens bench.

## Tips

1. **Block component** - Block must have processing bench component configured
2. **Animation** - Add Simple interaction before for interaction animation
3. **Sounds** - Include sound effects for better UX
4. **Use with UseBlock** - Typically used in block Use interactions

---

**Previous:** [OpenContainer](149_Interaction_Type_OpenContainer.md) | **Next:** [Explode](151_Interaction_Type_Explode.md)
