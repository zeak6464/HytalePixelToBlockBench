# Plants & Farming

Learn how to create crops, seeds, trees, and farming mechanics.

## Overview

Farming in Hytale involves creating seeds that can be planted, which grow through stages until harvestable. Plants can be crops, trees, or decorative flora.

### Update 2 (Jan 2026) — Farming changes

- **Eternal crops** no longer break from accidental weapon damage. Breaking fully grown Eternal crops drops **Seeds** back.
- **Tilled soil** lifetime increased to **1.2–1.5 days**; soil decay under fully grown crops fixed.
- **Offhand:** you can hold a **torch in offhand** while using Hoe and Seeds.
- **Harvested crops** can be **placed on the ground when crouching**.
- **Petals** are craftable at the **furniture bench** (Textiles). Renewable (e.g. Yellow Petals from Sunflower Seeds). **Lighter cloth block** variants craftable with Petals.
- **Hoes** unlock at different farming bench tiers; recipe costs adjusted. See [Tools](14_Tools.md) and [Patch Notes Update 2](Patch_Notes_Update_2.md).

## Location
- Seeds: `Server/Item/Items/Plant/Crop/{CropName}/`
- Crop Blocks: `Server/Item/Items/Plant/Crop/{CropName}/`

## Example from Game Files

### Crop Item Template

From `Server/Item/Items/Plant/Crop/_Template/Template_Crop_Item.json`:

```1:65:Server/Item/Items/Plant/Crop/_Template/Template_Crop_Item.json
{
  "TranslationProperties": {
    "Name": "server.items.Template_Crop.name"
  },
  "Consumable": true,
  "Icon": "Icons/ItemsGenerated/Plant_Crop_Lettuce.png",
  "IconProperties": {
    "Scale": 0.7,
    "Translation": [
      -3,
      -7.8
    ],
    "Rotation": [
      42.8,
      29.3,
      0
    ]
  },
  "Texture": "Resources/Ingredients/Cabbage_Lettuce.png",
  "Model": "Resources/Ingredients/Cabbage.blockymodel",
  "Quality": "Template",
  "Categories": [
    "Items.Foods"
  ],
  "ResourceTypes": [
    {
      "Id": "Vegetables"
    }
  ],
  "PlayerAnimationsId": "Item",
  "Tags": {
    "Type": [
      "Plant"
    ],
    "Family": [
      "Crop"
    ]
  },
  "ItemEntity": {
    "ParticleSystemId": null
  },
  "Interactions": {
    "Secondary": "Root_Secondary_Consume_Food_T1"
  },
  "InteractionVars": {
    "Effect": {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": "Food_Instant_Heal_T1"
        },
        "HealthRegen_TierCheck_T1"
      ]
    }
  },
  "Utility": {
    "Compatible": true
  },
  "InteractionConfig": {
    "Priorities": {
      "Secondary": 1
    }
  },
  "ItemSoundSetId": "ISS_Items_Foliage"
}
```

This shows a complete crop item template with consumption mechanics, effects, and resource types.
- Trees: `Server/Item/Items/Plant/`

## Creating Seeds

Seeds are items that can be placed on tilled soil to create crop blocks.

### Basic Seed Structure

Create `Server/Item/Items/Plant/Crop/MyCustom/Plant_Seeds_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Plant_Seeds_MyCustom.name",
    "Description": "server.items.Plant_Seeds_MyCustom.description"
  },
  "Parent": "Template_Seeds",
  "Quality": "Junk",
  "InteractionVars": {
    "SeedId": {
      "Interactions": [
        {
          "Parent": "Seed_Place",
          "BlockTypeToPlace": "Plant_Crop_MyCustom_Block"
        }
      ]
    }
  },
  "Texture": "Resources/Plants/SeedBag_Textures/MyCustom.png",
  "Icon": "Icons/ItemsGenerated/Plant_Seeds_MyCustom.png",
  "IconProperties": {
    "Scale": 1.1,
    "Rotation": [0.0, 30.0, 15.0],
    "Translation": [2.5, -11.0]
  },
  "Recipe": {
    "TimeSeconds": 0,
    "Input": [
      {
        "ItemId": "Ingredient_Life_Essence",
        "Quantity": 2
      }
    ],
    "Output": [
      {
        "ItemId": "Plant_Seeds_MyCustom",
        "Quantity": 1
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Id": "Farmingbench",
        "Categories": ["Seeds"]
      }
    ]
  },
  "ItemLevel": 2
}
```

### Key Seed Properties

- **`Parent: "Template_Seeds"`** - Base seed template
- **`BlockTypeToPlace`** - The crop block that appears when seed is placed
- **`Quality: "Junk"`** - Seeds are typically junk quality

## Creating Crop Blocks

Crop blocks are placeable blocks that grow through stages.

### Basic Crop Block Structure

Create `Server/Item/Items/Plant/Crop/MyCustom/Plant_Crop_MyCustom_Block.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Plant_Crop_MyCustom_StageFinal.name"
  },
  "Parent": "Template_Crop_Block",
  "BlockType": {
    "RandomRotation": "YawStep90",
    "CustomModelTexture": [
      {
        "Texture": "Resources/Plants/Seeds_Textures/MyCustom.png",
        "Weight": 1
      }
    ],
    "BlockSoundSetId": "Seeds",
    "BlockEntity": {
      "Components": {
        "FarmingBlock": {}
      }
    },
    "State": {
      "Definitions": {
        "Stage1": {
          "CustomModel": "Blocks/Foliage/Plants/Dense_Small_3Rows.blockymodel",
          "CustomModelTexture": [
            {
              "Texture": "Blocks/Foliage/Plants/Dense_Small_3Rows_MyCustom_01.png",
              "Weight": 1
            }
          ],
          "Effect": ["WindAttached"],
          "Gathering": {
            "Soft": {
              "IsWeaponBreakable": false,
              "DropList": "Drops_Plant_Crop_MyCustom_Stage1"
            }
          },
          "HitboxType": "Plant_Small",
          "BlockParticleSetId": "Grass",
          "BlockSoundSetId": "Plant",
          "ParticleColor": "#93d72c"
        },
        "StageFinal": {
          "CustomModel": "Blocks/Foliage/Plants/Dense_Tall.blockymodel",
          "CustomModelTexture": [
            {
              "Texture": "Blocks/Foliage/Plants/Dense_Tall_Textures/MyCustom.png",
              "Weight": 1
            }
          ],
          "Effect": ["WindAttached"],
          "InteractionHint": "server.interactionHints.harvest",
          "Gathering": {
            "Harvest": {
              "DropList": "Drops_Plant_Crop_MyCustom_StageFinal_Harvest"
            },
            "Soft": {
              "IsWeaponBreakable": false,
              "DropList": "Drops_Plant_Crop_MyCustom_StageFinal"
            }
          },
          "BlockParticleSetId": "Grass",
          "ParticleColor": "#efcc7b",
          "BlockSoundSetId": "Plant",
          "HitboxType": "Plant_Tall_Small"
        }
      }
    },
    "Farming": {
      "Stages": {
        "Default": [
          {
            "Duration": {
              "Min": 10500,
              "Max": 10500
            },
            "Type": "BlockState",
            "State": "default"
          },
          {
            "Duration": {
              "Min": 25300,
              "Max": 25300
            },
            "Type": "BlockState",
            "SoundEventId": "SFX_Crops_Grow",
            "State": "Stage1"
          },
          {
            "Duration": {
              "Min": 25300,
              "Max": 25300
            },
            "Type": "BlockState",
            "SoundEventId": "SFX_Crops_Grow",
            "State": "StageFinal"
          }
        ],
        "Harvested": [
          {
            "Duration": {
              "Min": 25300,
              "Max": 25300
            },
            "Type": "BlockState",
            "State": "Stage1"
          }
        ]
      },
      "StartingStageSet": "Default"
    },
    "Support": {
      "Down": [
        {
          "TagId": "Type=Soil"
        }
      ]
    },
    "Material": "Empty",
    "HitboxType": "Plant_Seed"
  }
}
```

## Farming Block Component

All growing crops need the `FarmingBlock` component:

```json
"BlockEntity": {
  "Components": {
    "FarmingBlock": {}
  }
}
```

This enables the growth system.

## Growth Stages Configuration

The `Farming.Stages` section defines how the crop grows:

```json
"Farming": {
  "Stages": {
    "Default": [
      {
        "Duration": {
          "Min": 10500,  // Minimum seconds (2.9 hours)
          "Max": 10500   // Maximum seconds
        },
        "Type": "BlockState",
        "State": "default"  // Initial state
      },
      {
        "Duration": {
          "Min": 25300,
          "Max": 25300
        },
        "Type": "BlockState",
        "SoundEventId": "SFX_Crops_Grow",
        "State": "Stage1"  // Transitions to Stage1
      },
      {
        "Type": "BlockState",
        "SoundEventId": "SFX_Crops_Grow_Stage_Complete",
        "State": "StageFinal"  // Final harvestable stage
      }
    ]
  },
  "StartingStageSet": "Default"
}
```

### Stage Types

- **`Type: "BlockState"`** - Transitions to a defined block state
- **`Duration`** - Time before transitioning (in seconds)
- **`SoundEventId`** - Sound played when stage advances
- **`State`** - The state name (must match `State.Definitions` keys)

### Growth Duration

Duration is in **seconds**:
- `10500` = ~2.9 hours
- `25300` = ~7 hours
- `86400` = 24 hours (1 day)

## Crop States

Each growth stage is defined in `BlockType.State.Definitions`:

### Stage 1 (Early Growth)

```json
"Stage1": {
  "CustomModel": "Blocks/Foliage/Plants/Dense_Small_3Rows.blockymodel",
  "CustomModelTexture": [
    {
      "Texture": "Blocks/Foliage/Plants/Dense_Small_3Rows_MyCustom_01.png",
      "Weight": 1
    }
  ],
  "HitboxType": "Plant_Small",
  "Gathering": {
    "Soft": {
      "DropList": "Drops_Plant_Crop_MyCustom_Stage1"
    }
  }
}
```

### Final Stage (Harvestable)

```json
"StageFinal": {
  "CustomModel": "Blocks/Foliage/Plants/Dense_Tall.blockymodel",
  "InteractionHint": "server.interactionHints.harvest",
  "Gathering": {
    "Harvest": {
      "DropList": "Drops_Plant_Crop_MyCustom_StageFinal_Harvest"
    },
    "Soft": {
      "DropList": "Drops_Plant_Crop_MyCustom_StageFinal"
    }
  },
  "HitboxType": "Plant_Tall_Small"
}
```

### Harvest vs Soft Gathering

- **`Harvest`** - Used when player uses harvest action (right-click with empty hand)
- **`Soft`** - Used when breaking with any tool/weapon

## Soil Requirements

Crops need to be placed on soil:

```json
"Support": {
  "Down": [
    {
      "TagId": "Type=Soil"
    }
  ]
}
```

## Creating Tree Saplings

Tree saplings work similarly but grow into larger prefab structures.

### Tree Sapling Structure

Create `Server/Item/Items/Plant/Plant_Sapling_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Plant_Sapling_MyCustom.name"
  },
  "Icon": "Icons/ItemsGenerated/Plant_MyCustom_Sapling.png",
  "Categories": ["Blocks.Plants"],
  "BlockType": {
    "DrawType": "Model",
    "CustomModel": "Blocks/Foliage/Tree/Sapling.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/Foliage/Tree/Sapling_Textures/MyCustom.png",
        "Weight": 1
      }
    ],
    "BlockEntity": {
      "Components": {
        "FarmingBlock": {}
      }
    },
    "Farming": {
      "Stages": {
        "Default": [
          {
            "Block": "Plant_Sapling_MyCustom",
            "Duration": {
              "Min": 40000,
              "Max": 60000
            },
            "Type": "BlockType"
          },
          {
            "Prefabs": [
              {
                "Path": "Trees/MyCustom/Stage_0/MyCustom_Stage0_001.prefab.json",
                "Weight": 1
              }
            ],
            "Duration": {
              "Min": 40000,
              "Max": 60000
            },
            "Type": "Prefab",
            "ReplaceMaskTags": ["Soil"],
            "SoundEventId": "SFX_Crops_Grow"
          }
        ]
      }
    }
  }
}
```

### Tree Growth Stages

Trees can transition to prefabs instead of block states:

```json
{
  "Prefabs": [
    {
      "Path": "Trees/MyCustom/Stage_1/MyCustom_Stage1_001.prefab.json",
      "Weight": 1
    }
  ],
  "Type": "Prefab",
  "ReplaceMaskTags": ["Soil"]
}
```

- **`Type: "Prefab"`** - Grows into a prefab structure
- **`ReplaceMaskTags`** - Blocks with these tags are replaced when prefab spawns
- **`Path`** - Path to the prefab file

## Drop Lists for Crops

Create drop lists for each growth stage:

### Early Stage Drops

`Server/Drops/Plants/Crop/MyCustom/Drops_Plant_Crop_MyCustom_Stage1.json`:

```json
{
  "Container": {
    "Type": "Single",
    "Item": {
      "ItemId": "Ingredient_Fibre",
      "QuantityMin": 0,
      "QuantityMax": 1
    }
  }
}
```

### Harvest Drops

`Server/Drops/Plants/Crop/MyCustom/Drops_Plant_Crop_MyCustom_StageFinal_Harvest.json`:

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Plant_Crop_MyCustom_Item",
          "QuantityMin": 2,
          "QuantityMax": 5
        }
      },
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Plant_Seeds_MyCustom",
          "QuantityMin": 0,
          "QuantityMax": 2,
          "Chance": 0.7
        }
      }
    ]
  }
}
```

## Common Crop Models

Available plant models in `Blocks/Foliage/Plants/`:
- `Dense_Small_3Rows.blockymodel` - Small crop (stage 1)
- `Dense_Medium_3Rows.blockymodel` - Medium crop (stage 2)
- `Dense_Tall.blockymodel` - Tall crop (final stage)
- `Seeds.blockymodel` - Seed stage

## Hitbox Types

| HitboxType | Size | Use Case |
|------------|------|----------|
| `Plant_Seed` | Tiny | Initial seed stage |
| `Plant_Small` | Small | Early growth stages |
| `Plant_Large` | Medium | Mid growth stages |
| `Plant_Full` | Large | Late growth stages |
| `Plant_Tall_Small` | Tall | Final harvestable stage |

## Tips for Creating Crops

1. **Define all states** - Create visual states for each growth stage
2. **Set appropriate durations** - Balance growth time with gameplay
3. **Create drop lists** - Define what drops at each stage
4. **Use Harvest gathering** - Allow proper harvesting with right-click
5. **Match textures** - Use consistent texture paths across stages
6. **Add FarmingBlock component** - Required for growth system
7. **Test growth cycles** - Ensure crops grow through all stages properly

---

**Previous:** [Tools](14_Tools.md) | **Next:** [Furniture](16_Furniture.md)
