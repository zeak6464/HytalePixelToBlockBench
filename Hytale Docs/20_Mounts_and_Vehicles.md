# Mounts & Vehicles

Learn how to create gliders and other vehicles that modify player movement.

## Overview

Gliders are items that allow players to glide through the air, modifying fall speed and adding horizontal movement. This guide focuses on gliders, the primary vehicle type in Hytale.

## Location
`Server/Item/Items/Glider/`

## Basic Glider Structure

Create `Server/Item/Items/Glider/Glider_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Glider_MyCustom.name"
  },
  "Icon": "Icons/ItemsGenerated/Glider_MyCustom.png",
  "Model": "Items/Glider/Glider_Inhand.blockymodel",
  "Texture": "Items/Glider/MyCustom_Glider_Texture.png",
  "DroppedItemAnimation": "Items/Animations/Dropped/Dropped_Diagonal_Left.blockyanim",
  "Quality": "Rare",
  "ItemLevel": 40,
  "MaxStack": 1,
  "Categories": ["Items.Tools"],
  "Glider": {
    "FallSpeedMultiplier": 50,
    "HorizontalSpeedMultiplier": 0.6,
    "TerminalVelocity": -4,
    "Speed": 10
  },
  "Interactions": {
    "Primary": {
      "RequireNewClick": true,
      "Interactions": [
        {
          "Type": "StatsCondition",
          "Costs": {
            "Stamina": 0.01
          },
          "Next": {
            "Type": "EffectCondition",
            "EntityEffectIds": ["Stamina_Broken"],
            "Match": "None",
            "Next": {
              "Type": "ToggleGlider"
            }
          }
        }
      ]
    },
    "Secondary": {
      "Interactions": [
        {
          "Type": "StatsCondition",
          "Costs": {
            "GlidingActive": 1
          },
          "Next": {
            "Type": "ApplyForce",
            "Direction": {
              "X": 0,
              "Y": 0,
              "Z": -1
            },
            "AdjustVertical": false,
            "WaitForGround": false,
            "Force": 15,
            "Next": {
              "Type": "ChangeStat",
              "StatModifiers": {
                "Stamina": -1
              }
            }
          }
        }
      ]
    },
    "SwapFrom": {
      "Interactions": [
        {
          "Type": "StatsCondition",
          "Costs": {
            "GlidingActive": 1
          },
          "Next": {
            "Type": "ToggleGlider"
          }
        },
        {
          "Type": "ChangeActiveSlot"
        }
      ]
    }
  },
  "ItemAppearanceConditions": {
    "GlidingActive": [
      {
        "Condition": [1, 1],
        "Model": "Items/Glider/Glider.blockymodel",
        "Texture": "Items/Glider/MyCustom_Glider_Texture.png"
      }
    ]
  },
  "IconProperties": {
    "Scale": 0.25,
    "Rotation": [212.0, 326.0, 229.0],
    "Translation": [-11.0, -39.0]
  },
  "Tags": {
    "Type": ["Glider"]
  }
}
```

## Glider Properties

The `Glider` object defines flight characteristics:

```json
"Glider": {
  "FallSpeedMultiplier": 50,
  "HorizontalSpeedMultiplier": 0.6,
  "TerminalVelocity": -4,
  "Speed": 10
}
```

### Property Descriptions

| Property | Description | Example Values |
|----------|-------------|----------------|
| `FallSpeedMultiplier` | How much fall speed is reduced (higher = slower fall) | `50` (slow), `20` (faster) |
| `HorizontalSpeedMultiplier` | Horizontal movement speed multiplier | `0.6` (standard), `1.0` (fast) |
| `TerminalVelocity` | Maximum fall speed (negative = downward) | `-4` (slow), `-10` (fast) |
| `Speed` | Overall glider speed | `10` (standard), `20` (fast) |

## Glider Interactions

### Primary (Toggle Glider)

```json
"Primary": {
  "RequireNewClick": true,
  "Interactions": [
    {
      "Type": "StatsCondition",
      "Costs": {
        "Stamina": 0.01
      },
      "Next": {
        "Type": "EffectCondition",
        "EntityEffectIds": ["Stamina_Broken"],
        "Match": "None",
        "Next": {
          "Type": "ToggleGlider"
        }
      }
    }
  ]
}
```

- **`ToggleGlider`** - Activates/deactivates glider
- **`Stamina` cost** - Costs stamina to activate
- **`EffectCondition`** - Prevents use when `Stamina_Broken`

### Secondary (Boost While Gliding)

```json
"Secondary": {
  "Interactions": [
    {
      "Type": "StatsCondition",
      "Costs": {
        "GlidingActive": 1
      },
      "Next": {
        "Type": "ApplyForce",
        "Direction": {
          "X": 0,
          "Y": 0,
          "Z": -1
        },
        "Force": 15,
        "Next": {
          "Type": "ChangeStat",
          "StatModifiers": {
            "Stamina": -1
          }
        }
      }
    }
  ]
}
```

- **`GlidingActive`** - Only works while gliding
- **`ApplyForce`** - Adds forward boost
- **`Stamina` cost** - Costs stamina per boost

### SwapFrom (Deactivate on Switch)

```json
"SwapFrom": {
  "Interactions": [
    {
      "Type": "StatsCondition",
      "Costs": {
        "GlidingActive": 1
      },
      "Next": {
        "Type": "ToggleGlider"
      }
    },
    {
      "Type": "ChangeActiveSlot"
    }
  ]
}
```

Automatically deactivates glider when switching items.

## Glider Visual States

The glider model changes when active:

```json
"ItemAppearanceConditions": {
  "GlidingActive": [
    {
      "Condition": [1, 1],
      "Model": "Items/Glider/Glider.blockymodel",
      "Texture": "Items/Glider/MyCustom_Glider_Texture.png"
    }
  ]
}
```

- **`Condition: [1, 1]`** - When `GlidingActive` stat equals 1
- **`Model`** - Deployed glider model
- **`Texture`** - Deployed glider texture

## Glider Stats

Gliders use special stats:
- **`GlidingActive`** - Tracks if glider is currently active (0 or 1)
- **`Stamina`** - Consumed during gliding and boosts

## Complete Example: Standard Glider

```json
{
  "TranslationProperties": {
    "Name": "server.items.Glider.name"
  },
  "Icon": "Icons/ItemsGenerated/Template_Glider.png",
  "Model": "Items/Glider/Glider_Inhand.blockymodel",
  "Texture": "Items/Glider/Feran_Glider_Texture.png",
  "Quality": "Rare",
  "ItemLevel": 40,
  "MaxStack": 1,
  "Categories": ["Items.Tools"],
  "Glider": {
    "FallSpeedMultiplier": 50,
    "HorizontalSpeedMultiplier": 0.6,
    "TerminalVelocity": -4,
    "Speed": 10
  },
  "Interactions": {
    "Primary": {
      "RequireNewClick": true,
      "Interactions": [
        {
          "Type": "StatsCondition",
          "Costs": { "Stamina": 0.01 },
          "Next": {
            "Type": "EffectCondition",
            "EntityEffectIds": ["Stamina_Broken"],
            "Match": "None",
            "Next": {
              "Type": "ToggleGlider"
            }
          }
        }
      ]
    }
  },
  "ItemAppearanceConditions": {
    "GlidingActive": [
      {
        "Condition": [1, 1],
        "Model": "Items/Glider/Glider.blockymodel",
        "Texture": "Items/Glider/Feran_Glider_Texture.png"
      }
    ]
  },
  "Tags": {
    "Type": ["Glider"]
  }
}
```

## Glider Tier Examples

### Basic Glider

```json
{
  "Glider": {
    "FallSpeedMultiplier": 30,
    "HorizontalSpeedMultiplier": 0.5,
    "TerminalVelocity": -6,
    "Speed": 8
  },
  "Quality": "Uncommon"
}
```

### Advanced Glider

```json
{
  "Glider": {
    "FallSpeedMultiplier": 70,
    "HorizontalSpeedMultiplier": 0.8,
    "TerminalVelocity": -3,
    "Speed": 15
  },
  "Quality": "Legendary"
}
```

## Tips for Creating Gliders

1. **Balance fall speed** - Higher `FallSpeedMultiplier` = slower fall
2. **Test horizontal movement** - Adjust `HorizontalSpeedMultiplier` for feel
3. **Set stamina costs** - Balance activation and boost costs
4. **Use visual states** - Show deployed glider when active
5. **Handle deactivation** - Use `SwapFrom` to deactivate on item switch
6. **Prevent broken stamina** - Use `EffectCondition` to prevent use when stamina broken

---

**Previous:** [Materials & Resources](19_Materials_and_Resources.md) | **Next:** [Advanced Item Interactions](21_Advanced_Item_Interactions.md)
