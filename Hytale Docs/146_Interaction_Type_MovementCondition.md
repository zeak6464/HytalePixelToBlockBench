# Interaction Type: MovementCondition

Branch interactions based on entity movement direction.

## Overview

`MovementCondition` branches interactions based on which direction the entity is moving. Useful for directional dodges, movement-based abilities, and context-aware actions.

## Example from Game Files

### Movement Condition Interaction

Movement condition interactions check entity movement state before proceeding. These are used for conditional interactions based on movement, velocity, or position.

## Basic Structure

```json
{
  "Type": "MovementCondition",
  "ForwardLeft": {
    "Type": "Simple"
  },
  "ForwardRight": {
    "Type": "Simple"
  },
  "Left": "Dodge_Left",
  "Right": "Dodge_Right",
  "BackLeft": {
    "Type": "Simple"
  },
  "BackRight": {
    "Type": "Simple"
  }
}
```

## Properties

### Direction Properties

```json
{
  "ForwardLeft": { "Type": "Simple" },
  "ForwardRight": { "Type": "Simple" },
  "Left": "Dodge_Left",
  "Right": "Dodge_Right",
  "Forward": { "Type": "Simple" },
  "Back": { "Type": "Simple" },
  "BackLeft": { "Type": "Simple" },
  "BackRight": { "Type": "Simple" }
}
```

Interactions for each movement direction:
- **`ForwardLeft`** - Moving forward-left
- **`ForwardRight`** - Moving forward-right
- **`Left`** - Moving left
- **`Right`** - Moving right
- **`Forward`** - Moving forward
- **`Back`** - Moving back
- **`BackLeft`** - Moving back-left
- **`BackRight`** - Moving back-right

Can be interaction object or string reference.

## Complete Examples

### Example 1: Directional Dodge

```json
{
  "Type": "Condition",
  "Flying": false,
  "Next": {
    "Type": "MovementCondition",
    "ForwardLeft": {
      "Type": "Simple"
    },
    "ForwardRight": {
      "Type": "Simple"
    },
    "Left": "Dodge_Left",
    "Right": "Dodge_Right",
    "BackLeft": {
      "Type": "Simple"
    },
    "BackRight": {
      "Type": "Simple"
    }
  }
}
```

Different dodge animations based on movement direction.

### Example 2: Directional Attack

```json
{
  "Type": "MovementCondition",
  "Forward": {
    "Type": "Simple",
    "Effects": {
      "ItemAnimationId": "Attack_Forward"
    }
  },
  "Left": {
    "Type": "Simple",
    "Effects": {
      "ItemAnimationId": "Attack_Left"
    }
  },
  "Right": {
    "Type": "Simple",
    "Effects": {
      "ItemAnimationId": "Attack_Right"
    }
  }
}
```

Different attack animations based on movement.

## Tips

1. **Directional actions** - Use for dodge rolls, directional attacks, movement abilities
2. **String references** - Reference other interactions by ID for cleaner code
3. **Not moving** - If no direction matches, interaction may not execute (check behavior)
4. **Combine with Condition** - Check flying/grounded before movement checks

---

**Previous:** [MemoriesCondition](145_Interaction_Type_MemoriesCondition.md) | **Next:** [TeleportInstance](147_Interaction_Type_TeleportInstance.md)
