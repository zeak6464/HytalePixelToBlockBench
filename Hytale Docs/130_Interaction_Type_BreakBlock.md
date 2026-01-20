# Interaction Type: BreakBlock

Break or destroy blocks in the world.

## Overview

`BreakBlock` breaks blocks at the target location. Handles block destruction, drop spawning, and harvesting mechanics.

## Example from Game Files

### Break Block Interaction

Break block interactions destroy blocks in the world. These are used for mining, demolition, and block removal mechanics.

## Basic Structure

```json
{
  "Type": "BreakBlock",
  "Harvest": true
}
```

## Properties

### Harvest

```json
{
  "Harvest": true
}
```

If `true`, block drops its harvest items (from drop table). If `false`, no drops.

## Complete Examples

### Example 1: Basic Break

```json
{
  "Type": "BreakBlock",
  "Harvest": true
}
```

Breaks block and spawns drops.

### Example 2: Mine Block

```json
{
  "Type": "BreakBlock",
  "Harvest": true
}
```

Mines block with drops (pickaxe).

### Example 3: Without Drops

```json
{
  "Type": "BreakBlock",
  "Harvest": false
}
```

Breaks block without drops.

### Example 4: With Selector

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Distance": 5,
    "TargetType": "Block"
  },
  "HitBlock": {
    "Interactions": [
      {
        "Type": "BreakBlock",
        "Harvest": true
      }
    ]
  }
}
```

Breaks targeted block within 5 blocks.

## Tips

1. **Harvest** - Use `true` for mining/gathering, `false` for destruction
2. **Drop tables** - Block must have drop table configured for harvest to work
3. **Tool requirements** - May require specific tools (check Gathering types)
4. **Combine with Selector** - Use Selector for ranged block breaking

---

**Previous:** [ChangeState](129_Interaction_Type_ChangeState.md) | **Next:** [UseBlock](131_Interaction_Type_UseBlock.md)
