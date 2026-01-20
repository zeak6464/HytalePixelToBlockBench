# Interaction Type: ChangeState

Change block states (on/off, open/closed, activated/deactivated).

## Overview

`ChangeState` switches blocks between their defined states. Useful for doors, switches, lanterns, traps, and any blocks with multiple states.

## Example from Game Files

### Change State Interaction

Change state interactions modify block states. These are used for block state transitions, activating/deactivating blocks, and state-based mechanics.

## Basic Structure

```json
{
  "Type": "ChangeState",
  "Changes": {
    "default": "Yellow",
    "White": "Yellow",
    "Red": "Yellow"
  }
}
```

## Properties

### Changes

```json
{
  "Changes": {
    "default": "activated",
    "activated": "default"
  }
}
```

Map of source state IDs to target state IDs. Key is current state, value is new state.

## Complete Examples

### Example 1: Toggle Switch

```json
{
  "Type": "ChangeState",
  "Changes": {
    "default": "activated",
    "activated": "default"
  }
}
```

Toggles between default and activated states.

### Example 2: Lantern Colors

```json
{
  "Type": "ChangeState",
  "Changes": {
    "default": "Yellow",
    "White": "Yellow",
    "Red": "Yellow",
    "Blue": "Yellow",
    "Cyan": "Yellow",
    "Green": "Yellow",
    "Pink": "Yellow",
    "Purple": "Yellow"
  }
}
```

Changes any lantern color to yellow.

### Example 3: Door Open/Close

```json
{
  "Type": "ChangeState",
  "Changes": {
    "closed": "open",
    "open": "closed"
  }
}
```

Toggles door between open and closed.

### Example 4: With Block Condition

```json
{
  "Type": "BlockCondition",
  "Matchers": [
    {
      "Block": {
        "Id": "Furniture_Human_Ruins_Lantern"
      }
    }
  ],
  "Next": {
    "Type": "ChangeState",
    "Changes": {
      "default": "Yellow"
    }
  },
  "Failed": "Block_Secondary"
}
```

Only changes state if block matches lantern type.

## Tips

1. **Bidirectional** - Map both directions for toggle functionality
2. **Multiple sources** - Map multiple source states to same target for "reset" behavior
3. **State IDs** - Must match block state definition IDs exactly
4. **Combine with BlockCondition** - Validate block type before changing state

---

**Previous:** [ChangeBlock](128_Interaction_Type_ChangeBlock.md) | **Next:** [BreakBlock](130_Interaction_Type_BreakBlock.md)
