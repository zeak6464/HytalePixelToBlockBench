# Interaction Type: Chaining

Combo system for sequential actions with timing windows and special flag triggers.

## Overview

`Chaining` enables combo systems where players can chain actions together within a timing window. Supports multiple steps, held actions, and special flag-triggered combos.

## Basic Structure

```json
{
  "Type": "Chaining",
  "ChainId": "Debug_Combo",
  "ChainingAllowance": 0.8,
  "Next": [
    {
      "Type": "SendMessage",
      "Message": "First - Primary",
      "Effects": {
        "ItemAnimationId": "Swing_Right"
      }
    },
    {
      "Type": "FirstClick",
      "Click": {
        "Type": "SendMessage",
        "Message": "Second click"
      },
      "Held": {
        "Type": "SendMessage",
        "Message": "Second held"
      }
    }
  ],
  "Flags": {
    "Special_Second": {
      "Type": "SendMessage",
      "Message": "Flag hit!"
    }
  }
}
```

## Properties

### ChainId

```json
{
  "ChainId": "Debug_Combo"
}
```

Unique identifier for this combo chain. Used with `ChainFlag` to trigger special moves.

### ChainingAllowance

```json
{
  "ChainingAllowance": 0.8
}
```

Time window in seconds to perform the next action in the chain.

### Next

```json
{
  "Next": [
    {
      "Type": "Simple",
      "Effects": { ... }
    },
    {
      "Type": "FirstClick",
      "Click": { ... },
      "Held": { ... }
    }
  ]
}
```

Array of chain steps. Each step can be a regular interaction or `FirstClick` for click/hold options.

### FirstClick

```json
{
  "Type": "FirstClick",
  "Click": {
    "Type": "Simple",
    "Effects": { "ItemAnimationId": "Swing_Left" }
  },
  "Held": {
    "Type": "Simple",
    "Effects": { "ItemAnimationId": "Hook_Left" },
    "Next": {
      "Type": "ChainFlag",
      "ChainId": "Debug_Combo",
      "Flag": "Held_Second"
    }
  }
}
```

- **`Click`** - Action if clicked (tapped)
- **`Held`** - Action if held (charges)

### Flags

```json
{
  "Flags": {
    "Special_Second": {
      "Type": "SendMessage",
      "Message": "Flag hit!"
    }
  }
}
```

Special interactions triggered by `ChainFlag` with matching `ChainId` and `Flag`.

## Complete Examples

### Example 1: Basic 3-Hit Combo

```json
{
  "Type": "Chaining",
  "ChainId": "Combo_3Hit",
  "ChainingAllowance": 1.0,
  "Next": [
    {
      "Type": "Simple",
      "Effects": {
        "ItemAnimationId": "Swing_Right"
      }
    },
    {
      "Type": "Simple",
      "Effects": {
        "ItemAnimationId": "Swing_Left"
      }
    },
    {
      "Type": "Simple",
      "Effects": {
        "ItemAnimationId": "Uppercut"
      }
    }
  ]
}
```

Three-hit combo with 1 second window between each.

### Example 2: Click vs Hold

```json
{
  "Type": "Chaining",
  "ChainId": "Combo_Mixed",
  "ChainingAllowance": 1.25,
  "Next": [
    {
      "Type": "Simple",
      "Effects": {
        "ItemAnimationId": "Swing_Right"
      }
    },
    {
      "Type": "FirstClick",
      "Click": {
        "Type": "Simple",
        "Effects": {
          "ItemAnimationId": "Swing_Left"
        }
      },
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
  ],
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

Combo with click (swing) or hold (hook + dash) options.

### Example 3: String References

```json
{
  "Type": "Chaining",
  "ChainId": "Sword_Combo",
  "ChainingAllowance": 1.25,
  "Next": [
    "Sword_Swing_Left_Fast",
    "Sword_Swing_Right_Fast",
    "Sword_Uppercut"
  ]
}
```

References other interaction IDs instead of inline definitions.

## Tips

1. **Timing windows** - `ChainingAllowance` determines how forgiving combos are (0.8-1.5s typical)
2. **ChainId uniqueness** - Use unique IDs per combo to avoid conflicts
3. **Click vs Hold** - Use `FirstClick` for varied combo paths
4. **Flags for specials** - Use `ChainFlag` + `Flags` for conditional combo finishers
5. **String references** - Reference other interactions by ID for cleaner code

---

**Previous:** [Condition](113_Interaction_Type_Condition.md) | **Next:** [Charging](115_Interaction_Type_Charging.md)
