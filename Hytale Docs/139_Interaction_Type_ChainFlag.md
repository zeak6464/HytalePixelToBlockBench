# Interaction Type: ChainFlag

Set flags for combo chains to trigger special moves.

## Overview

`ChainFlag` sets flags during combo chains that can trigger special interactions defined in `Chaining` interactions. Used for conditional combo finishers and special moves.

## Example from Game Files

### Chain Flag Interaction

Chain flag interactions set flags for combo systems. These are used for combo mechanics, attack chains, and sequential action systems.

## Basic Structure

```json
{
  "Type": "ChainFlag",
  "ChainId": "Debug_Combo",
  "Flag": "Held_Second"
}
```

## Properties

### ChainId

```json
{
  "ChainId": "Debug_Combo"
}
```

ID of the combo chain. Must match `ChainId` in the `Chaining` interaction.

### Flag

```json
{
  "Flag": "Held_Second"
}
```

Flag name to set. Triggers special interaction if defined in `Chaining.Flags`.

## Complete Examples

### Example 1: Set Flag

```json
{
  "Type": "ChainFlag",
  "ChainId": "Debug_Combo",
  "Flag": "Held_Second"
}
```

Sets flag for combo chain.

### Example 2: In Combo Chain

```json
{
  "Type": "FirstClick",
  "Held": {
    "Type": "Simple",
    "Effects": {
      "ItemAnimationId": "Hook_Left"
    },
    "Next": {
      "Type": "ChainFlag",
      "ChainId": "Combo_Mixed",
      "Flag": "Held_Second"
    }
  }
}
```

In combo, sets flag when held.

### Example 3: Trigger Special Move

In `Chaining` interaction:
```json
{
  "Type": "Chaining",
  "ChainId": "Combo_Mixed",
  "Next": [ ... ],
  "Flags": {
    "Held_Second": {
      "Type": "Simple",
      "Effects": {
        "ItemAnimationId": "StabDashCharged"
      },
      "Next": {
        "Type": "ApplyForce",
        "Direction": { "Z": -5 },
        "Force": 30
      }
    }
  }
}
```

When flag is set, triggers special dash attack.

## Tips

1. **ChainId match** - Must match `ChainId` in `Chaining` interaction
2. **Flag names** - Use descriptive names (Held_Second, Special_Move, etc.)
3. **Conditional combos** - Use flags for branching combo paths
4. **Special moves** - Define special interactions in `Chaining.Flags`

---

**Previous:** [SendMessage](138_Interaction_Type_SendMessage.md) | **Next:** [FirstClick](140_Interaction_Type_FirstClick.md)
