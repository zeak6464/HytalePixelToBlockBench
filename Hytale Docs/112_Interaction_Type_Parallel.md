# Interaction Type: Parallel

Execute multiple interactions simultaneously, all at once.

## Overview

`Parallel` executes all interactions in the `Interactions` array at the same time. Useful for combining independent actions like attacking while checking blocks, or applying multiple effects simultaneously.

## Basic Structure

```json
{
  "Type": "Parallel",
  "$Comment": "Make sure this parallel interaction is needed and can't be optimized away",
  "Interactions": [
    {
      "Interactions": [
        {
          "Type": "Selector",
          "Selector": { ... }
        }
      ]
    },
    {
      "Interactions": [
        {
          "Type": "Condition",
          "RequiredGameMode": "Adventure",
          "Next": "Block_Break"
        }
      ]
    }
  ]
}
```

## Properties

### Interactions

```json
{
  "Interactions": [
    {
      "Interactions": [
        { "Type": "Simple", "Effects": { ... } }
      ]
    },
    {
      "Interactions": [
        { "Type": "ApplyEffect", "EffectId": "Buff" }
      ]
    }
  ]
}
```

Array of interaction arrays. Each inner array executes simultaneously.

## Complete Examples

### Example 1: Attack and Block Check

```json
{
  "Type": "Parallel",
  "Interactions": [
    {
      "Interactions": [
        {
          "Type": "Selector",
          "Selector": {
            "Id": "Horizontal",
            "Direction": "ToLeft",
            "TestLineOfSight": true,
            "StartDistance": 0.1,
            "EndDistance": 2.5
          },
          "HitEntity": {
            "Interactions": [
              {
                "Type": "DamageEntity",
                "DamageCalculator": {
                  "BaseDamage": {
                    "Physical": 10
                  }
                }
              }
            ]
          }
        }
      ]
    },
    {
      "Interactions": [
        {
          "Type": "Condition",
          "RequiredGameMode": "Adventure",
          "Next": "Block_Break"
        }
      ]
    }
  ]
}
```

Attacks entities while also checking if blocks should break.

### Example 2: Multiple Effects

```json
{
  "Type": "Parallel",
  "Interactions": [
    {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": "Speed_Boost",
          "Duration": 10.0
        }
      ]
    },
    {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": "Jump_Boost",
          "Duration": 10.0
        }
      ]
    },
    {
      "Interactions": [
        {
          "Type": "Simple",
          "Effects": {
            "WorldSoundEventId": "SFX_Boost"
          }
        }
      ]
    }
  ]
}
```

Applies multiple buffs and plays sound simultaneously.

### Example 3: Replace Variables

```json
{
  "Type": "Parallel",
  "Interactions": [
    {
      "Interactions": [
        {
          "Type": "Replace",
          "Var": "Block_Swing_Down_Damage",
          "DefaultValue": {
            "Interactions": ["Block_Swing_Down_Damage"]
          },
          "DefaultOk": true
        }
      ]
    },
    {
      "Interactions": [
        {
          "Type": "Replace",
          "Var": "Block_Swing_Down_Effect",
          "DefaultValue": {
            "Interactions": ["Block_Swing_Down_Effect"]
          },
          "DefaultOk": true
        }
      ]
    }
  ]
}
```

Sets multiple interaction variables simultaneously.

## Tips

1. **Independent actions** - Use when actions don't depend on each other
2. **Performance** - Can improve responsiveness when multiple things should happen at once
3. **Nested arrays** - Each inner array must contain interactions (note the double nesting)
4. **Comment warning** - Game warns if parallel can be optimized away (may not be needed)

---

**Previous:** [Serial](111_Interaction_Type_Serial.md) | **Next:** [Condition](113_Interaction_Type_Condition.md)
