# Creating Food Items

Learn how to create consumable food items that restore health, mana, or stamina.

## Overview

Food items are consumable items that apply effects when eaten. Unlike potions which often have instant effects, food can provide regeneration over time or instant healing.

## Location
`Server/Item/Items/Food/`

## Basic Food Structure

Create `Server/Item/Items/Food/Food_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Food_MyCustom.name",
    "Description": "server.items.Food_MyCustom.description"
  },
  "Parent": "Template_Food",
  "Quality": "Common",
  "Icon": "Icons/ItemsGenerated/Food_MyCustom.png",
  "BlockType": {
    "Material": "Empty",
    "DrawType": "Model",
    "Opacity": "Semitransparent",
    "CustomModel": "Items/Consumables/Food/Bread.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Items/Consumables/Food/Bread_Texture.png",
        "Weight": 1
      }
    ],
    "HitboxType": "Food_Medium",
    "RandomRotation": "YawStep1",
    "Gathering": {
      "Harvest": {},
      "Soft": {}
    },
    "CustomModelScale": 0.5,
    "BlockParticleSetId": "Dust",
    "ParticleColor": "#e4cb69"
  },
  "Interactions": {
    "Secondary": "Root_Secondary_Consume_Food_T2"
  },
  "InteractionVars": {
    "Effect": {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": "Food_Instant_Heal_Bread"
        }
      ]
    },
    "ConsumeSFX": {
      "Interactions": [
        {
          "Parent": "Consume_SFX",
          "Effects": {
            "LocalSoundEventId": "SFX_Consume_Bread_Local"
          }
        }
      ]
    },
    "ConsumedSFX": {
      "Interactions": [
        {
          "Parent": "Consumed_SFX",
          "Effects": {
            "LocalSoundEventId": "SFX_Consume_Bread_Local"
          }
        }
      ]
    }
  },
  "InteractionConfig": {
    "Priorities": {
      "Secondary": 1
    }
  },
  "Consumable": true,
  "MaxStack": 25,
  "DropOnDeath": true
}
```

## Key Food Properties

### Consumable Flag
```json
"Consumable": true
```
This marks the item as consumable, allowing it to be eaten.

### Interaction Types

Food uses the `"Secondary"` interaction with different tier consumption patterns:

- **`Root_Secondary_Consume_Food_T1`** - Basic consumption (simple foods)
- **`Root_Secondary_Consume_Food_T2`** - Standard consumption (most foods)
- **`Root_Secondary_Consume_Food_T3`** - Advanced consumption (elaborate meals)

### Effect Types

#### Instant Healing Food

```json
"InteractionVars": {
  "Effect": {
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "Food_Instant_Heal_Bread"
      }
    ]
  }
}
```

The `EffectId` can reference:
- A status effect (like `Food_Instant_Heal_Bread`)
- An inline effect definition (see below)

#### Inline Effect Definition

```json
"Effect": {
  "Interactions": [
    {
      "Type": "ApplyEffect",
      "EffectId": {
        "StatModifiers": {
          "Health": 50
        },
        "ValueType": "Absolute",
        "Duration": 0.1,
        "OverlapBehavior": "Extend"
      }
    }
  ]
}
```

#### Regeneration Food

```json
"Effect": {
  "Interactions": [
    {
      "Type": "ApplyEffect",
      "EffectId": {
        "StatModifiers": {
          "Health": 2
        },
        "Duration": 30,
        "DamageCalculatorCooldown": 2,
        "OverlapBehavior": "Extend"
      }
    }
  ]
}
```

### Food Visual Properties

#### Hitbox Types
- `Food_Large` - Large food items (e.g., whole fish)
- `Food_Medium` - Medium food items (e.g., bread, cooked meat)
- `Food_Small` - Small food items (e.g., berries, small fruits)

#### Particle Colors
```json
"ParticleColor": "#e4cb69"  // Yellow-brown for bread
"ParticleColor": "#f3ba9b"  // Pink for raw meat
"ParticleColor": "#cabc3f"  // Yellow for corn
```

### Sound Effects

Food items have two sound event variables:

```json
"ConsumeSFX": {
  "Interactions": [
    {
      "Parent": "Consume_SFX",
      "Effects": {
        "LocalSoundEventId": "SFX_Consume_Bread_Local"
      }
    }
  ]
},
"ConsumedSFX": {
  "Interactions": [
    {
      "Parent": "Consumed_SFX",
      "Effects": {
        "LocalSoundEventId": "SFX_Consume_Bread_Local"
      }
    }
  ]
}
```

- **`ConsumeSFX`** - Plays when consumption starts
- **`ConsumedSFX`** - Plays when consumption completes

## Food vs Potions

| Feature | Food | Potions |
|---------|------|---------|
| **Consumption Speed** | Slower (animation) | Instant |
| **Effect Type** | Usually regeneration or small instant | Instant or powerful effects |
| **Stack Size** | Typically 25 | Typically 10 |
| **Quality** | Common to Rare | Common to Legendary |
| **Crafting** | Cooking bench | Alchemy bench |
| **ItemLevel** | Usually lower (2-15) | Usually higher (10-50) |

## Creating Cooked Food

Cooked food is typically crafted from raw ingredients at a cooking bench:

```json
{
  "Recipe": {
    "Input": [
      {
        "ItemId": "Ingredient_Dough",
        "Quantity": 1
      },
      {
        "ResourceTypeId": "Fuel",
        "Quantity": 3
      }
    ],
    "Output": [
      {
        "ItemId": "Food_Bread"
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Id": "Cookingbench",
        "Categories": [
          "Baked"
        ]
      }
    ],
    "TimeSeconds": 5
  }
}
```

### Cooking Bench Categories
- `Prepared` - Prepared dishes
- `Baked` - Baked goods (bread, pastries)
- `Ingredients` - Cooking ingredients

## Resource Types for Food

Food items can have resource types:

```json
"ResourceTypes": [
  {
    "Id": "Foods"
  }
]
```

Other food-related resource types:
- `Meats` - Raw or cooked meat
- `Foods` - General food items

## Complete Examples

### Raw Meat (Simple Food)

```json
{
  "TranslationProperties": {
    "Name": "server.items.Food_Beef_Raw.name"
  },
  "Parent": "Template_Food",
  "Quality": "Common",
  "Icon": "Icons/ItemsGenerated/Food_Beef_Raw.png",
  "BlockType": {
    "Material": "Empty",
    "DrawType": "Model",
    "CustomModel": "Items/Consumables/Food/Beef.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Items/Consumables/Food/Beef_Raw.png",
        "Weight": 1
      }
    ],
    "ParticleColor": "#f3ba9b"
  },
  "InteractionVars": {
    "Effect": {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": "Food_Instant_Heal_T1"
        }
      ]
    }
  },
  "ResourceTypes": [
    {
      "Id": "Meats"
    }
  ],
  "MaxStack": 25,
  "DropOnDeath": true
}
```

### Cooked Bread (Recipe Food)

```json
{
  "TranslationProperties": {
    "Name": "server.items.Food_Bread.name"
  },
  "Parent": "Template_Food",
  "Interactions": {
    "Secondary": "Root_Secondary_Consume_Food_T2"
  },
  "Quality": "Uncommon",
  "Icon": "Icons/ItemsGenerated/Food_Bread.png",
  "BlockType": {
    "Material": "Empty",
    "DrawType": "Model",
    "Opacity": "Semitransparent",
    "CustomModel": "Items/Consumables/Food/Bread.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Items/Consumables/Food/Bread_Texture.png",
        "Weight": 1
      }
    ],
    "HitboxType": "Food_Medium",
    "RandomRotation": "YawStep1",
    "Gathering": {
      "Harvest": {},
      "Soft": {}
    },
    "CustomModelScale": 0.5,
    "BlockParticleSetId": "Dust",
    "ParticleColor": "#e4cb69"
  },
  "InteractionVars": {
    "Effect": {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": "Food_Instant_Heal_Bread"
        }
      ]
    }
  },
  "Recipe": {
    "Input": [
      {
        "ItemId": "Ingredient_Dough",
        "Quantity": 1
      },
      {
        "ResourceTypeId": "Fuel",
        "Quantity": 3
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Id": "Cookingbench",
        "Categories": ["Baked"]
      }
    ],
    "TimeSeconds": 5
  },
  "MaxStack": 25,
  "DropOnDeath": true
}
```

## Food Item Levels

| Tier | ItemLevel | Quality | Example |
|------|-----------|---------|---------|
| Basic | 2-5 | Common | Raw meat, simple fruits |
| Cooked | 5-10 | Uncommon | Bread, cooked meat |
| Prepared | 10-15 | Rare | Elaborate meals |

## Tips for Creating Food

1. **Use Parent inheritance** - Start with `Template_Food` and override what you need
2. **Set appropriate ItemLevel** - Lower for basic foods, higher for elaborate meals
3. **Choose consumption tier** - Use T1 for simple foods, T2 for standard, T3 for elaborate
4. **Balance healing** - Instant heal vs regeneration over time
5. **Set MaxStack** - Usually 25 for food (vs 10 for potions)
6. **Add DropOnDeath** - Set `"DropOnDeath": true` so food drops when player dies
7. **Use appropriate sounds** - Match sound events to food type (bread sounds, meat sounds, etc.)
8. **Define recipes** - Create cooking recipes at the `Cookingbench`

---

**Previous:** [Server Modding](12_Server_Modding.md) | **Next:** [Tools](14_Tools.md)
