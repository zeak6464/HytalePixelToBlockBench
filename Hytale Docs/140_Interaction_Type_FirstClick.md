# Interaction Type: FirstClick

Handle first-click vs held actions in combo chains.

## Overview

`FirstClick` distinguishes between clicking (tap) and holding actions in combo chains. Provides `Click` for tap actions and `Held` for charge/hold actions.

## Basic Structure

```json
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
      "ChainId": "Combo",
      "Flag": "Held_Second"
    }
  }
}
```

## Properties

### Click

```json
{
  "Click": {
    "Type": "Simple",
    "Effects": { ... }
  }
}
```

Interaction to execute if button is clicked (tapped, not held).

### Held

```json
{
  "Held": {
    "Type": "Simple",
    "Effects": { ... },
    "Next": { ... }
  }
}
```

Interaction to execute if button is held (charge/hold action).

## Complete Examples

### Example 1: Click vs Hold

```json
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
    }
  }
}
```

Click = swing, hold = hook.

### Example 2: In Combo Chain

```json
{
  "Type": "Chaining",
  "ChainId": "Combo_Mixed",
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
  ]
}
```

In combo, click = swing, hold = hook + flag.

### Example 3: Branching Paths

```json
{
  "Type": "FirstClick",
  "Click": {
    "Type": "Simple",
    "Effects": {
      "ItemAnimationId": "Quick_Stab"
    },
    "Next": "Quick_Combo_Next"
  },
  "Held": {
    "Type": "Simple",
    "Effects": {
      "ItemAnimationId": "Charged_Stab"
    },
    "Next": "Charged_Combo_Next"
  }
}
```

Different combo paths based on click vs hold.

## Tips

1. **Combo chains** - Use within `Chaining` for varied combo paths
2. **Flag setting** - Set flags in `Held` for special move triggers
3. **Different animations** - Use distinct animations for click vs hold
4. **Branching** - Use for combo paths that branch based on input

---

**Previous:** [ChainFlag](139_Interaction_Type_ChainFlag.md) | **Next:** [StatsCondition](141_Interaction_Type_StatsCondition.md)
