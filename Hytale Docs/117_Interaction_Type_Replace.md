# Interaction Type: Replace

Dynamic variable replacement for runtime interaction customization.

## Overview

`Replace` allows interaction variables to be dynamically replaced at runtime. Used to customize interactions based on context, item modifiers, or other conditions. Enables flexible interaction systems.

## Example from Game Files

### Replace Interaction

From `Server/Item/Interactions/Weapons/Sword/Attacks/Primary/Swing_Left/Weapon_Sword_Primary_Swing_Left.json`:

```9:18:Server/Item/Interactions/Weapons/Sword/Attacks/Primary/Swing_Left/Weapon_Sword_Primary_Swing_Left.json
  "Next": {
    "Type": "Replace",
    "Var": "Swing_Left_Selector",
    "DefaultOk": true,
    "DefaultValue": {
      "Interactions": [
        "Weapon_Sword_Primary_Swing_Left_Selector"
      ]
    }
  }
```

This shows a Replace interaction that dynamically replaces a variable with different interactions based on conditions.

## Basic Structure

```json
{
  "Type": "Replace",
  "Var": "Block_Swing_Down_Damage",
  "DefaultValue": {
    "Interactions": [
      "Block_Swing_Down_Damage"
    ]
  },
  "DefaultOk": true
}
```

## Properties

### Var

```json
{
  "Var": "Block_Swing_Down_Damage"
}
```

Variable name to replace. Should match an `InteractionVars` key in the item definition.

### DefaultValue

```json
{
  "DefaultValue": {
    "Interactions": [
      "Block_Swing_Down_Damage"
    ]
  }
}
```

Default value if variable is not set. Can be interaction object or string reference.

### DefaultOk

```json
{
  "DefaultOk": true
}
```

If `true`, uses `DefaultValue` if variable not found. If `false`, interaction fails if variable missing.

## Complete Examples

### Example 1: Dynamic Damage

```json
{
  "Type": "Replace",
  "Var": "Block_Swing_Down_Damage",
  "DefaultValue": {
    "Interactions": [
      "Block_Swing_Down_Damage"
    ]
  },
  "DefaultOk": true
}
```

Replaces damage interaction with custom one if variable set.

### Example 2: Item-Specific Interactions

In item definition:
```json
{
  "InteractionVars": {
    "Custom_Attack": {
      "Interactions": [
        {
          "Parent": "Weapon_Sword_Primary_Swing_Left_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 25
            }
          }
        }
      ]
    }
  }
}
```

In interaction:
```json
{
  "Type": "Replace",
  "Var": "Custom_Attack",
  "DefaultValue": {
    "Interactions": ["Weapon_Sword_Primary_Swing_Left_Damage"]
  },
  "DefaultOk": true
}
```

Uses custom attack if defined, otherwise defaults.

### Example 3: Context-Based Replacement

```json
{
  "Type": "Condition",
  "Condition": {
    "Stat": "Health",
    "Comparison": "LessThan",
    "Value": 25
  },
  "Next": {
    "Type": "Replace",
    "Var": "Desperate_Strike",
    "DefaultValue": {
      "Interactions": ["Normal_Strike"]
    },
    "DefaultOk": true
  }
}
```

Uses desperate strike when health low, otherwise normal.

## Tips

1. **DefaultOk** - Set to `true` for safe fallbacks, `false` for required variables
2. **InteractionVars** - Define variables in item definition for customization
3. **String references** - Use string IDs in `DefaultValue` for cleaner code
4. **Parent inheritance** - Combine with `Parent` for extending base interactions
5. **Context-aware** - Use with conditions for dynamic behavior

---

**Previous:** [Wielding](116_Interaction_Type_Wielding.md) | **Next:** [Selector](118_Interaction_Type_Selector.md)
