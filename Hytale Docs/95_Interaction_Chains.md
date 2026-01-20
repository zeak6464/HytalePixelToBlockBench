# Interaction Chains

Learn how to chain multiple interactions together for complex behaviors.

## Overview

Interaction chains execute multiple interactions in sequence. Interactions can be simple lists, conditional branches, or complex nested structures with delays and effects.

## Location
Interaction chains are defined in `Interactions` arrays within interaction definitions.

## Example from Game Files

### Interaction Chain with Replace

From `Server/Item/Interactions/Weapons/Sword/Attacks/Primary/Swing_Left/Weapon_Sword_Primary_Swing_Left.json`:

```1:19:Server/Item/Interactions/Weapons/Sword/Attacks/Primary/Swing_Left/Weapon_Sword_Primary_Swing_Left.json
{
  "Type": "Simple",
  "RunTime": 0.117,
  "Effects": {
    "ItemAnimationId": "SwingLeft",
    "WorldSoundEventId": "SFX_Light_Melee_T2_Swing",
    "LocalSoundEventId": "SFX_Sword_T2_Swing_RL_Local"
  },
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
}
```

This shows an interaction chain where a Simple interaction is followed by a Replace interaction that chains to a selector.

## Basic Interaction Chain

```json
{
  "Interactions": [
    {
      "Type": "Simple",
      "Effects": {
        "LocalSoundEventId": "SFX_Step1"
      }
    },
    {
      "Type": "Simple",
      "Effects": {
        "LocalSoundEventId": "SFX_Step2"
      }
    },
    {
      "Type": "Simple",
      "Effects": {
        "LocalSoundEventId": "SFX_Step3"
      }
    }
  ]
}
```

Interactions execute in order.

## Interaction Chain with Next

```json
{
  "Type": "Simple",
  "Effects": {
    "LocalSoundEventId": "SFX_Start"
  },
  "Next": {
    "Type": "Simple",
    "RunTime": 1.0,
    "Effects": {
      "LocalSoundEventId": "SFX_Delayed"
    },
    "Next": {
      "Type": "Simple",
      "Effects": {
        "LocalSoundEventId": "SFX_End"
      }
    }
  }
}
```

Use `Next` for sequential execution with optional delays.

## RunTime Delays

```json
{
  "Type": "Simple",
  "RunTime": 2.5,
  "Effects": {
    "LocalSoundEventId": "SFX_Delayed"
  }
}
```

`RunTime` delays execution before proceeding to next interaction.

## Conditional Chains

```json
{
  "Interactions": [
    {
      "Type": "Condition",
      "Condition": {
        "Stat": "Health",
        "Comparison": "LessThan",
        "Value": 50
      },
      "Interactions": [
        {
          "Type": "Heal",
          "Amount": 100
        }
      ],
      "Else": {
        "Interactions": [
          {
            "Type": "Simple",
            "Effects": {
              "LocalSoundEventId": "SFX_FullHealth"
            }
          }
        ]
      }
    }
  ]
}
```

Branches based on conditions.

## Complex Chain Example

```json
{
  "Interactions": [
    {
      "Type": "Selector",
      "Selector": {
        "Id": "Raycast",
        "Offset": {
          "Y": 1.6
        },
        "Distance": 10
      },
      "HitEntityRules": [
        {
          "Matchers": [
            {
              "Type": "Player"
            }
          ],
          "Next": {
            "Interactions": [
              {
                "Type": "Simple",
                "Effects": {
                  "LocalSoundEventId": "SFX_PlayerTarget"
                },
                "Next": {
                  "Type": "ApplyEffect",
                  "EffectId": "PlayerEffect",
                  "RunTime": 1.0,
                  "Next": {
                    "Type": "Simple",
                  "Effects": {
                    "LocalSoundEventId": "SFX_Complete"
                  }
                }
              }
            }
          ]
        }
      ]
    }
  ]
}
```

Combines selector, condition, and sequential actions.

## Tips for Interaction Chains

1. **Sequential execution** - Interactions in arrays execute in order
2. **Next property** - Use `Next` for cleaner sequential chains
3. **RunTime delays** - Add delays for timing-sensitive sequences
4. **Nesting** - Combine selectors, conditions, and chains for complex behaviors

---

**Previous:** [Interaction Cooldowns](94_Interaction_Cooldowns.md) | **Next:** [Entity Effects](96_Entity_Effects.md)
