# Interaction Type: UseBlock

Interact with blocks (containers, benches, interactive blocks).

## Overview

`UseBlock` triggers block interactions like opening containers, using benches, or activating interactive blocks. Used when right-clicking/interacting with blocks.

## Example from Game Files

### Use Block Interaction

Use block interactions interact with blocks. These are used for activating blocks, using block features, and block-based interactions.

## Basic Structure

```json
{
  "Type": "UseBlock",
  "Failed": "Block_Attack"
}
```

## Properties

### Failed

```json
{
  "Failed": "Block_Attack"
}
```

Interaction to execute if block interaction fails (block has no Use interaction).

## Complete Examples

### Example 1: Basic Use

```json
{
  "Type": "Simple",
  "Next": {
    "Type": "UseBlock",
    "Failed": "Block_Attack"
  }
}
```

Uses block, falls back to attack if block has no Use interaction.

### Example 2: Container Use

```json
{
  "Type": "UseBlock"
}
```

Opens container (if block is a container).

### Example 3: Bench Use

```json
{
  "Type": "UseBlock"
}
```

Uses processing bench (if block is a bench).

## Tips

1. **Failed fallback** - Always provide `Failed` for better UX
2. **Block interactions** - Block must define Use interaction in its state
3. **Context-aware** - Different blocks have different Use behaviors
4. **Combine with Simple** - Add animation/sound before using

---

**Previous:** [BreakBlock](130_Interaction_Type_BreakBlock.md) | **Next:** [BlockCondition](132_Interaction_Type_BlockCondition.md)
