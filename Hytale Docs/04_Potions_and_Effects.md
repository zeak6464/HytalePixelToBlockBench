# Creating Potions & Status Effects

Learn how to create consumable potions and status effects (buffs/debuffs).

## Creating Potions

### Overview

Potions are consumable items that grant temporary or instant effects. They use status effects to modify player stats, health, mana, or stamina.

### Location
`Server/Item/Items/Potion/`

### Basic Potion Structure

Create `Server/Item/Items/Potion/Potion_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Potion_MyCustom.name"
  },
  "Parent": "Potion_Template",
  "Categories": ["Items.Potions"],
  "Icon": "Icons/ItemsGenerated/Potion_MyCustom.png",
  "InteractionVars": {
    "Effect": {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": "MyCustom_Effect"
        }
      ]
    }
  },
  "BlockType": {
    "CustomModel": "Items/Consumables/Potions/Potion.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Items/Consumables/Potions/Potion_Textures/Red.png",
        "Weight": 1
      }
    ],
    "ParticleColor": "#ff3730",
    "Light": {
      "Color": "#522"
    }
  },
  "Consumable": true,
  "MaxStack": 10
}
```

### Instant Healing Potion

```json
{
  "TranslationProperties": {
    "Name": "server.items.Potion_Health_Instant.name"
  },
  "Parent": "Potion_Template",
  "InteractionVars": {
    "Effect": {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": {
            "StatModifiers": {
              "Health": 50
            },
            "ValueType": "Absolute",
            "StatusEffectIcon": "UI/StatusEffects/Health_Potion.png",
            "Duration": 0.1,
            "OverlapBehavior": "Extend"
          }
        }
      ]
    },
    "Stat_Check": {
      "Interactions": [
        {
          "Parent": "Stat_Check",
          "Costs": {
            "Health": 100
          },
          "ValueType": "Percent",
          "LessThan": true
        }
      ]
    }
  },
  "BlockType": {
    "CustomModel": "Items/Consumables/Potions/Potion.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Items/Consumables/Potions/Potion_Textures/Red.png"
      }
    ],
    "ParticleColor": "#ff3730"
  },
  "Consumable": true,
  "MaxStack": 10
}
```

### Regeneration Potion

```json
{
  "InteractionVars": {
    "Effect": {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": {
            "StatModifiers": {
              "Health": 5
            },
            "Duration": 20,
            "DamageCalculatorCooldown": 2,
            "StatusEffectIcon": "UI/StatusEffects/Health_Potion.png",
            "OverlapBehavior": "Extend"
          }
        }
      ]
    }
  }
}
```

### Potion Types

| Type | StatModifiers | Use Case |
|------|---------------|----------|
| Instant | `Health: 50`, `Duration: 0.1` | Immediate healing |
| Regeneration | `Health: 5`, `Duration: 20`, `DamageCalculatorCooldown: 2` | Over-time healing |
| Buff | Stat increases (speed, damage, etc.) | Temporary stat boosts |
| Debuff | Negative effects | Status ailments |

### Potion Textures

Available textures in `Items/Consumables/Potions/Potion_Textures/`:
- `Red.png` - Health potions
- `Blue.png` - Mana potions
- `Green.png` - Stamina potions
- `Pink.png` - Regeneration potions
- `Yellow.png` - Buff potions

### Potion Recipes

```json
{
  "Recipe": {
    "Input": [
      {
        "ItemId": "Potion_Empty",
        "Quantity": 3
      },
      {
        "ItemId": "Plant_Fruit_Berries_Red",
        "Quantity": 6
      },
      {
        "ItemId": "Plant_Crop_Health1",
        "Quantity": 1
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Alchemybench",
        "Type": "Crafting",
        "Categories": ["Alchemy_Potions"]
      }
    ],
    "OutputQuantity": 3,
    "TimeSeconds": 1
  }
}
```

---

## Creating Status Effects

### Overview

Status effects are temporary modifiers applied to entities. They can modify stats, apply damage over time, change visuals, or disable abilities.

### Location
`Server/Entity/Effects/`

### Basic Status Effect Structure

Create `Server/Entity/Effects/Status/MyCustom_Buff.json`:

```json
{
  "Duration": 30,
  "StatModifiers": {
    "Health": 20,
    "Mana": 50
  },
  "StatusEffectIcon": "UI/StatusEffects/Health_Potion.png",
  "OverlapBehavior": "Extend"
}
```

### Buff Effect (Positive)

```json
{
  "Duration": 60,
  "StatModifiers": {
    "Health": 50
  },
  "StatusEffectIcon": "UI/StatusEffects/Health_Potion.png",
  "OverlapBehavior": "Extend"
}
```

### Debuff Effect (Negative)

```json
{
  "Duration": 10,
  "Debuff": true,
  "DamageCalculator": {
    "BaseDamage": {
      "Poison": 5
    }
  },
  "DamageCalculatorCooldown": 2,
  "ApplicationEffects": {
    "EntityTopTint": "#00ff00",
    "EntityBottomTint": "#008000",
    "Particles": [
      {
        "SystemId": "Effect_Poison"
      }
    ]
  },
  "StatusEffectIcon": "UI/StatusEffects/Poison.png",
  "OverlapBehavior": "Overwrite"
}
```

### Root/Immobilize Effect

```json
{
  "Duration": 5,
  "ApplicationEffects": {
    "MovementEffects": {
      "DisableAll": true
    },
    "EntityTopTint": "#008000",
    "EntityBottomTint": "#000000",
    "ModelOverride": {
      "Model": "VFX/Spells/Roots/Model.blockymodel",
      "Texture": "VFX/Spells/Roots/Model.png"
    },
    "Particles": [
      {
        "SystemId": "Effect_Poison"
      }
    ]
  },
  "KnockbackMultiplier": 0
}
```

### Burn Effect (Damage Over Time)

```json
{
  "Duration": 5,
  "Debuff": true,
  "DamageCalculator": {
    "BaseDamage": {
      "Fire": 10
    }
  },
  "DamageCalculatorCooldown": 1,
  "ApplicationEffects": {
    "EntityTopTint": "#cf2302",
    "EntityBottomTint": "#100600",
    "ScreenEffect": "ScreenEffects/Fire.png",
    "Particles": [
      {
        "SystemId": "Effect_Fire"
      }
    ],
    "WorldSoundEventId": "SFX_Effect_Burn_World",
    "LocalSoundEventId": "SFX_Effect_Burn_Local"
  },
  "StatusEffectIcon": "UI/StatusEffects/Burn.png",
  "OverlapBehavior": "Overwrite"
}
```

### Status Effect Properties

| Property | Description | Values |
|----------|-------------|--------|
| `Duration` | How long effect lasts (seconds) | `5`, `30`, `60` |
| `Debuff` | Is this a negative effect? | `true`, `false` |
| `OverlapBehavior` | How overlapping effects behave | `"Extend"`, `"Overwrite"`, `"Stack"` |
| `StatModifiers` | Stats to modify | `Health`, `Mana`, `Stamina`, etc. |
| `DamageCalculator` | Damage dealt over time | Per `DamageCalculatorCooldown` |
| `StatusEffectIcon` | UI icon path | `"UI/StatusEffects/Health_Potion.png"` |

### Overlap Behaviors

- **`"Extend"`** - Adds duration to existing effect
- **`"Overwrite"`** - Replaces existing effect
- **`"Stack"`** - Multiple instances can stack

---

**Previous:** [Creating NPCs](03_NPCs.md) | **Next:** [Trading & Quests](05_Trading_and_Quests.md)
