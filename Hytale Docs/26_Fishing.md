# Fishing

Learn how to create fishing traps, fish items, bait, and fishing mechanics.

## Overview

Fishing in Hytale uses fishing traps that can be placed in water. Traps automatically catch fish over time, and can be baited to catch fish faster or better quality fish.

## Location
- Fishing Traps: `Server/Item/Items/Tool/`
- Fish Items: `Server/Item/Items/Fish/`
- Bait: `Server/Item/Items/Tool/`
- Drop Lists: `Server/Drops/Traps/`

## Creating Fishing Traps

Fishing traps are placeable blocks that catch fish over time using a farming-style growth system.

### Basic Fishing Trap Structure

Create `Server/Item/Items/Tool/Tool_Fishing_Trap_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Tool_Fishing_Trap_MyCustom.name",
    "Description": "server.items.Tool_Fishing_Trap_MyCustom.description"
  },
  "Icon": "Icons/ItemsGenerated/Tool_Fishing_Trap_MyCustom.png",
  "Categories": ["Furniture.Containers"],
  "ItemLevel": 10,
  "Recipe": {
    "TimeSeconds": 5,
    "Input": [
      {
        "ResourceTypeId": "Wood_All",
        "Quantity": 10
      },
      {
        "ItemId": "Ingredient_Fibre",
        "Quantity": 20
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Id": "Farmingbench",
        "Categories": ["Farming"],
        "RequiredTierLevel": 2
      }
    ]
  },
  "BlockType": {
    "Effect": ["WindFractal"],
    "VariantRotation": "NESW",
    "Support": {
      "Down": [
        {
          "FluidId": "Water_Source"
        },
        {
          "FluidId": "Water"
        }
      ]
    },
    "Material": "Solid",
    "DrawType": "Model",
    "CustomModel": "Blocks/Farming/Trap_Fishing.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/Farming/Trap_Fishing.png",
        "Weight": 1
      }
    ],
    "BlockEntity": {
      "Components": {
        "FarmingBlock": {}
      }
    },
    "State": {
      "Definitions": {
        "default": {
          "HitboxType": "Tool_Fishing_Trap",
          "BlockParticleSetId": "Water"
        },
        "StageFinal": {
          "CustomModel": "Blocks/Farming/Trap_Fishing_Full.blockymodel",
          "CustomModelTexture": [
            {
              "Texture": "Blocks/Farming/Trap_Fishing_Full.png",
              "Weight": 1
            }
          ],
          "HitboxType": "Tool_Fishing_Trap_Full",
          "InteractionHint": "server.interactionHints.harvest",
          "Interactions": {
            "Use": {
              "Interactions": [
                {
                  "Type": "HarvestCrop"
                }
              ]
            }
          },
          "Gathering": {
            "Harvest": {
              "DropList": "Drops_Fishing_Trap_MyCustom"
            },
            "Soft": {
              "ItemId": "Tool_Fishing_Trap_MyCustom",
              "IsWeaponBreakable": false
            }
          }
        }
      }
    },
    "Farming": {
      "Stages": {
        "Default": [
          {
            "Duration": {
              "Min": 40000,
              "Max": 60000
            },
            "Type": "BlockState",
            "State": "default",
            "SoundEventId": "SFX_Water_MoveOut"
          },
          {
            "Type": "BlockState",
            "State": "StageFinal",
            "SoundEventId": "SFX_Water_MoveIn"
          }
        ],
        "Baited": [
          {
            "Duration": {
              "Min": 20000,
              "Max": 40000
            },
            "Type": "BlockState",
            "State": "Baited",
            "SoundEventId": "SFX_Water_MoveOut"
          },
          {
            "Type": "BlockState",
            "State": "Baited_StageFinal",
            "SoundEventId": "SFX_Water_MoveIn"
          }
        ]
      },
      "StartingStageSet": "Default",
      "StageSetAfterHarvest": "Default",
      "ActiveGrowthModifiers": ["Water", "LightLevel"]
    },
    "Gathering": {
      "Soft": {
        "ItemId": "Tool_Fishing_Trap_MyCustom",
        "IsWeaponBreakable": false
      }
    }
  }
}
```

## Key Fishing Trap Properties

### Water Support

Traps must be placed on water:

```json
"Support": {
  "Down": [
    {
      "FluidId": "Water_Source"
    },
    {
      "FluidId": "Water"
    }
  ]
}
```

### Farming Block Component

Required for the growth system:

```json
"BlockEntity": {
  "Components": {
    "FarmingBlock": {}
  }
}
```

### Growth Stages

Fishing traps use the farming system with two stage sets:

#### Default Stage Set (No Bait)

```json
"Default": [
  {
    "Duration": {
      "Min": 40000,  // ~11 hours
      "Max": 60000   // ~16.7 hours
    },
    "Type": "BlockState",
    "State": "default"
  },
  {
    "Type": "BlockState",
    "State": "StageFinal"  // Trap is full, ready to harvest
  }
]
```

#### Baited Stage Set (With Bait)

```json
"Baited": [
  {
    "Duration": {
      "Min": 20000,  // Faster with bait (~5.5 hours)
      "Max": 40000   // (~11 hours)
    },
    "Type": "BlockState",
    "State": "Baited"
  },
  {
    "Type": "BlockState",
    "State": "Baited_StageFinal"
  }
]
```

### Growth Modifiers

```json
"ActiveGrowthModifiers": ["Water", "LightLevel"]
```

- **`Water`** - Trap must be in water to progress
- **`LightLevel`** - Growth may be affected by light level

## Creating Bait

Bait items are used to improve fishing trap catches.

### Basic Bait Structure

Create `Server/Item/Items/Tool/Tool_Trap_Bait_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Tool_Trap_Bait_MyCustom.name",
    "Description": "server.items.Tool_Trap_Bait_MyCustom.description"
  },
  "Quality": "Common",
  "Icon": "Icons/ItemsGenerated/Tool_Trap_Bait_MyCustom.png",
  "Model": "Resources/Plants/Wild_Berry.blockymodel",
  "Texture": "Resources/Plants/Wild_Berry_Texture.png",
  "Categories": ["Items.Tools"],
  "Recipe": {
    "TimeSeconds": 1,
    "Input": [
      {
        "ResourceTypeId": "Fruits",
        "Quantity": 3
      },
      {
        "ItemId": "Ingredient_Life_Essence",
        "Quantity": 5
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Id": "Farmingbench",
        "Categories": ["Farming"],
        "RequiredTierLevel": 2
      }
    ],
    "OutputQuantity": 2
  },
  "Interactions": {
    "Secondary": "Root_ChangeFarmingStage_Set"
  },
  "InteractionVars": {
    "ChangeFarmingStage_Set": {
      "Interactions": [
        {
          "Type": "BlockCondition",
          "Matchers": [
            {
              "Block": {
                "Id": "Tool_Fishing_Trap_MyCustom",
                "State": "default"
              }
            }
          ],
          "Next": {
            "Type": "ChangeFarmingStage",
            "Stage": 0,
            "StageSet": "Baited",
            "Effects": {
              "ItemAnimationId": "Till"
            },
            "Next": {
              "Type": "ModifyInventory",
              "AdjustHeldItemQuantity": -1
            },
            "RunTime": 0.15
          }
        }
      ]
    }
  },
  "Tags": {
    "Type": ["Tool"],
    "Family": ["Bait"]
  }
}
```

### Bait Interaction

Bait uses `ChangeFarmingStage` to switch the trap to the "Baited" stage set:

```json
{
  "Type": "ChangeFarmingStage",
  "Stage": 0,
  "StageSet": "Baited",
  "Effects": {
    "ItemAnimationId": "Till"
  },
  "Next": {
    "Type": "ModifyInventory",
    "AdjustHeldItemQuantity": -1  // Consumes 1 bait
  }
}
```

## Creating Fish Items

Fish items are spawnable NPC items that can be placed in water.

### Basic Fish Item Structure

Create `Server/Item/Items/Fish/Fish_MyCustom_Item.json`:

```json
{
  "Parent": "Template_Fish_Item",
  "Quality": "Common",
  "TranslationProperties": {
    "Name": "server.npcRoles.MyCustom_Fish.name"
  },
  "Model": "NPC/Swimming_Wildlife/MyCustom/Models/Model.blockymodel",
  "Texture": "NPC/Swimming_Wildlife/MyCustom/Models/Texture.png",
  "Animation": "NPC/Swimming_Wildlife/Bluegill/Animations/Swim/Swim.blockyanim",
  "Icon": "Icons/ItemsGenerated/Fish_MyCustom_Item.png",
  "IconProperties": {
    "Scale": 0.3,
    "Rotation": [22.5, 40, 22.5],
    "Translation": [4, -21]
  },
  "InteractionVars": {
    "SpawnNPC_Entity": {
      "Interactions": [
        {
          "Parent": "SpawnNPC_Entity_Default",
          "EntityId": "MyCustom_Fish",
          "SpawnOffset": {
            "X": 0,
            "Y": 1,
            "Z": 0
          },
          "Effects": {
            "WorldSoundEventId": "SFX_Water_MoveOut"
          }
        }
      ]
    }
  },
  "ResourceTypes": [
    {
      "Id": "Fish",
      "Quantity": 1
    }
  ],
  "Tags": {
    "Type": ["SpawnNPC"],
    "Family": ["Fish"]
  },
  "ItemSoundSetId": "ISS_Items_Splatty"
}
```

### Fish Item Properties

- **`SpawnNPC_Entity`** - When placed, spawns the fish NPC
- **`EntityId`** - Must match an NPC Role ID
- **`ResourceTypes`** - Should include `"Id": "Fish"` for recipes
- **`Tags`** - Should include `"Type": ["SpawnNPC"]` and `"Family": ["Fish"]`

### Fish Quality Variants

Fish can have quality-based states:

```json
{
  "State": {
    "Epic": {
      "Quality": "Epic",
      "Variant": true,
      "ItemEntity": {
        "ParticleSystemId": "Drop_Epic"
      },
      "ResourceTypes": [
        {
          "Id": "Fish",
          "Quantity": 1
        },
        {
          "Id": "Fish_Rare"
        }
      ]
    },
    "Legendary": {
      "Quality": "Legendary",
      "Variant": true,
      "ItemEntity": {
        "ParticleSystemId": "Drop_Legendary"
      },
      "ResourceTypes": [
        {
          "Id": "Fish",
          "Quantity": 1
        },
        {
          "Id": "Fish_Legendary"
        }
      ]
    }
  }
}
```

## Creating Fishing Drop Lists

Fishing traps use drop lists to determine what they catch.

### Basic Fishing Drop List

Create `Server/Drops/Traps/Drops_Fishing_Trap_MyCustom.json`:

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "$Comment": "Common Fish",
        "Type": "Choice",
        "Weight": 100,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Fish_Bluegill_Item",
              "QuantityMin": 1,
              "QuantityMax": 1
            }
          },
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Fish_Catfish_Item",
              "QuantityMin": 1,
              "QuantityMax": 1
            }
          }
        ]
      },
      {
        "$Comment": "Rare Fish",
        "Type": "Choice",
        "Weight": 10,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Fish_Salmon_Item",
              "QuantityMin": 1,
              "QuantityMax": 1
            }
          }
        ]
      },
      {
        "$Comment": "Resources",
        "Type": "Choice",
        "Weight": 99,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Rock_Salt",
              "QuantityMin": 1,
              "QuantityMax": 4
            }
          }
        ]
      },
      {
        "$Comment": "Junk",
        "Type": "Choice",
        "Weight": 98,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Rubble_Stone",
              "QuantityMin": 1,
              "QuantityMax": 5
            }
          },
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Ingredient_Stick",
              "QuantityMin": 1,
              "QuantityMax": 5
            }
          }
        ]
      }
    ]
  }
}
```

### Baited Drop List

Baited traps can have different drop lists:

Create `Server/Drops/Traps/Drops_Fishing_Trap_MyCustom_Baited.json`:

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Choice",
        "Weight": 100,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Fish_Salmon_Item",  // Better fish with bait
              "QuantityMin": 1,
              "QuantityMax": 1
            }
          },
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Fish_Trout_Rainbow_Item",
              "QuantityMin": 1,
              "QuantityMax": 1
            }
          }
        ]
      }
    ]
  }
}
```

Then reference in trap state:

```json
"Baited_StageFinal": {
  "Gathering": {
    "Harvest": {
      "DropList": "Drops_Fishing_Trap_MyCustom_Baited"
    }
  }
}
```

## Fish Resource Types

Fish items should have resource types:

```json
"ResourceTypes": [
  {
    "Id": "Fish",
    "Quantity": 1
  },
  {
    "Id": "Fish_Rare"  // Optional: for rare fish
  }
]
```

These allow fish to be used in recipes that require `ResourceTypeId: "Fish"`.

## Complete Example: Custom Fishing Trap

### 1. Trap Item

`Server/Item/Items/Tool/Tool_Fishing_Trap_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Tool_Fishing_Trap_MyCustom.name"
  },
  "Icon": "Icons/ItemsGenerated/Tool_Fishing_Trap_MyCustom.png",
  "Categories": ["Furniture.Containers"],
  "BlockType": {
    "Support": {
      "Down": [
        { "FluidId": "Water_Source" },
        { "FluidId": "Water" }
      ]
    },
    "CustomModel": "Blocks/Farming/Trap_Fishing.blockymodel",
    "BlockEntity": {
      "Components": {
        "FarmingBlock": {}
      }
    },
    "Farming": {
      "Stages": {
        "Default": [
          {
            "Duration": { "Min": 40000, "Max": 60000 },
            "Type": "BlockState",
            "State": "default"
          },
          {
            "Type": "BlockState",
            "State": "StageFinal"
          }
        ],
        "Baited": [
          {
            "Duration": { "Min": 20000, "Max": 40000 },
            "Type": "BlockState",
            "State": "Baited"
          },
          {
            "Type": "BlockState",
            "State": "Baited_StageFinal"
          }
        ]
      },
      "StartingStageSet": "Default",
      "ActiveGrowthModifiers": ["Water"]
    },
    "State": {
      "Definitions": {
        "StageFinal": {
          "Gathering": {
            "Harvest": {
              "DropList": "Drops_Fishing_Trap_MyCustom"
            }
          }
        }
      }
    }
  }
}
```

### 2. Drop List

`Server/Drops/Traps/Drops_Fishing_Trap_MyCustom.json`:

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Choice",
        "Weight": 100,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Fish_MyCustom_Item",
              "QuantityMin": 1,
              "QuantityMax": 2
            }
          }
        ]
      }
    ]
  }
}
```

## Tips for Creating Fishing Systems

1. **Use FarmingBlock component** - Required for trap growth system
2. **Set water support** - Traps must be placeable on water
3. **Configure growth durations** - Baited traps should catch faster
4. **Create varied drop lists** - Include fish, resources, and junk
5. **Use different drop lists for baited** - Bait should improve catch quality
6. **Define fish resource types** - Allows fish to be used in recipes
7. **Test trap placement** - Ensure traps can be placed in water only

---

**Previous:** [Damage Types](25_Damage_Types.md) | **Next:** [Animations](27_Animations.md)
