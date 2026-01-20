# Creating Traps

Learn how to create traps that damage or affect entities when triggered.

## Overview

Traps are placeable blocks that trigger effects (usually damage) when entities walk on them. Common trap types include spike traps, snapjaw traps, and pressure plates.

## Location
`Server/Item/Items/Trap/`

## Example from Game Files

### Wood Spike Trap

From `Server/Item/Items/Trap/Survival_Trap_Spike_Wood.json`:

```59:85:Server/Item/Items/Trap/Survival_Trap_Spike_Wood.json
    "Interactions": {
      "Collision": {
        "Cooldown": {
          "Id": "SlowEffect",
          "Cooldown": 1
        },
        "Interactions": [
          {
            "Type": "Condition",
            "RequiredGameMode": "Adventure",
            "Next": {
              "Type": "ApplyEffect",
              "EffectId": {
                "Locale": "spikes",
                "Duration": 0.1,
                "OverlapBehavior": "Overwrite",
                "DamageCalculator": {
                  "BaseDamage": {
                    "Physical": 10
                  }
                }
              }
            }
          }
        ]
      }
    },
```

This shows a trap with collision detection that applies damage when entities walk on it, with a cooldown to prevent spam.

### Healing Totem Deployable

From `Server/Item/Items/Weapon/Deployable/Weapon_Deployable_Healing_Totem.json`:

```1:68:Server/Item/Items/Weapon/Deployable/Weapon_Deployable_Healing_Totem.json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Deployable_Healing_Totem.name"
  },
  "Categories": [
    "Items.Weapons",
    "Items.Utility",
    "Items.Debug"
  ],
  "Recipe": {
    "TimeSeconds": 3,
    "Input": [
      {
        "ItemId": "Ingredient_Life_Essence",
        "Quantity": 50
      },
      {
        "ItemId": "Ingredient_Bar_Thorium",
        "Quantity": 20
      },
      {
        "ItemId": "Potion_Health_Greater",
        "Quantity": 10
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Arcanebench",
        "Type": "Crafting",
        "Categories": [
          "Arcane_Misc"
        ]
      }
    ]
  },
  "Scale": 0.305,
  "Quality": "Common",
  "ItemLevel": 40,
  "MaxStack": 1,
  "Model": "Items/Deployables/Healing_Totem/Healing_Totem_Projectile.blockymodel",
  "Texture": "Items/Deployables/Healing_Totem/Healing_Totem_Texture.png",
  "PlayerAnimationsId": "Item",
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "Serial",
          "Interactions": [
            {
              "Type": "Simple",
              "RunTime": 0.25,
              "Effects": {
                "ItemPlayerAnimationsId": "Item",
                "ItemAnimationId": "Throw"
              }
            },
            {
              "Type": "Projectile",
              "Config": "Projectile_Config_Healing_Totem_Deploy"
            }
          ]
        }
      ],
      "Cooldown": {
        "Id": "DeployableUtility",
        "Cooldown": 10
      }
    }
  },
```

This shows a deployable item that uses a projectile config to place a totem/trap in the world.

## Basic Trap Structure

Create `Server/Item/Items/Trap/Trap_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Trap_MyCustom.name"
  },
  "Icon": "Icons/ItemsGenerated/Trap_MyCustom.png",
  "Categories": ["Blocks.Deco"],
  "Recipe": {
    "TimeSeconds": 1,
    "Input": [
      {
        "ResourceTypeId": "Wood_Trunk",
        "Quantity": 2
      },
      {
        "ItemId": "Ingredient_Tree_Sap",
        "Quantity": 1
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Workbench",
        "Type": "Crafting",
        "Categories": ["Workbench_Tinkering"]
      }
    ]
  },
  "BlockType": {
    "DrawType": "Model",
    "Opacity": "Transparent",
    "CustomModel": "Blocks/Tinkering/Traps/Spikes.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/Tinkering/Traps/Spikes_MyCustom.png",
        "Weight": 1
      }
    ],
    "HitboxType": "Block_Three_Quarter",
    "RandomRotation": "YawStep90",
    "Gathering": {
      "Breaking": {
        "GatherType": "SoftBlocks"
      }
    },
    "BlockParticleSetId": "Wood",
    "BlockSoundSetId": "Branch",
    "Support": {
      "Down": [
        {
          "FaceType": "Full"
        }
      ]
    },
    "ParticleColor": "#684127",
    "Interactions": {
      "Collision": {
        "Cooldown": {
          "Id": "SlowEffect",
          "Cooldown": 1
        },
        "Interactions": [
          {
            "Type": "Condition",
            "RequiredGameMode": "Adventure",
            "Next": {
              "Type": "ApplyEffect",
              "EffectId": {
                "Locale": "spikes",
                "Duration": 0.1,
                "OverlapBehavior": "Overwrite",
                "DamageCalculator": {
                  "BaseDamage": {
                    "Physical": 10
                  }
                }
              }
            }
          }
        ]
      }
    },
    "VariantRotation": "DoublePipe"
  },
  "PlayerAnimationsId": "Block",
  "Tags": {
    "Type": ["Trap"]
  },
  "ItemSoundSetId": "ISS_Blocks_Wood"
}
```

## Trap Trigger System

Traps use the `Collision` interaction to trigger when entities walk on them:

```json
"Interactions": {
  "Collision": {
    "Cooldown": {
      "Id": "SlowEffect",
      "Cooldown": 1  // Seconds between triggers
    },
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": {
          "Duration": 0.1,
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 10
            }
          }
        }
      }
    ]
  }
}
```

## Trap Damage Configuration

### Inline Damage Effect

```json
{
  "Type": "ApplyEffect",
  "EffectId": {
    "Locale": "spikes",
    "Duration": 0.1,
    "OverlapBehavior": "Overwrite",
    "DamageCalculator": {
      "BaseDamage": {
        "Physical": 10
      }
    }
  }
}
```

### Reference to Status Effect

```json
{
  "Type": "ApplyEffect",
  "EffectId": "Trap_Spike_Damage"
}
```

Then create `Server/Entity/Effects/Status/Trap_Spike_Damage.json`:

```json
{
  "Duration": 0.1,
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 10
    }
  },
  "OverlapBehavior": "Overwrite"
}
```

## Trap Cooldown

Prevent traps from triggering too frequently:

```json
"Cooldown": {
  "Id": "SlowEffect",
  "Cooldown": 1  // 1 second cooldown
}
```

## Game Mode Restrictions

Limit traps to specific game modes:

```json
{
  "Type": "Condition",
  "RequiredGameMode": "Adventure",
  "Next": {
    // Trap effect
  }
}
```

## Trap Types

### Spike Trap

```json
{
  "CustomModel": "Blocks/Tinkering/Traps/Spikes.blockymodel",
  "HitboxType": "Block_Three_Quarter",
  "Interactions": {
    "Collision": {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": {
            "DamageCalculator": {
              "BaseDamage": {
                "Physical": 10
              }
            }
          }
        }
      ]
    }
  }
}
```

### Snapjaw Trap

More complex trap that triggers on entity proximity:

```json
{
  "CustomModel": "Blocks/Tinkering/Traps/Snapjaw.blockymodel",
  "Interactions": {
    "Collision": {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": "Trap_Snapjaw_Bite"
        }
      ]
    }
  }
}
```

### Ice Trap (Slowing)

```json
{
  "Interactions": {
    "Collision": {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": {
            "ApplicationEffects": {
              "MovementEffects": {
                "SpeedMultiplier": 0.3
              }
            },
            "Duration": 5
          }
        }
      ]
    }
  }
}
```

## Trap Crafting

Traps are typically crafted at a `Workbench` with `Tinkering` category:

```json
"Recipe": {
  "TimeSeconds": 1,
  "Input": [
    {
      "ResourceTypeId": "Wood_Trunk",
      "Quantity": 2
    },
    {
      "ItemId": "Ingredient_Tree_Sap",
      "Quantity": 1
    }
  ],
  "BenchRequirement": [
    {
      "Id": "Workbench",
      "Type": "Crafting",
      "Categories": ["Workbench_Tinkering"]
    }
  ]
}
```

## Complete Example: Wooden Spike Trap

```json
{
  "TranslationProperties": {
    "Name": "server.items.Survival_Trap_Spike_Wood.name"
  },
  "Icon": "Icons/ItemsGenerated/Trap_Spikes_Wood.png",
  "Categories": ["Blocks.Deco"],
  "Recipe": {
    "Input": [
      { "ResourceTypeId": "Wood_Trunk", "Quantity": 2 },
      { "ItemId": "Ingredient_Tree_Sap", "Quantity": 1 }
    ],
    "BenchRequirement": [
      {
        "Id": "Workbench",
        "Type": "Crafting",
        "Categories": ["Workbench_Tinkering"]
      }
    ]
  },
  "BlockType": {
    "CustomModel": "Blocks/Tinkering/Traps/Spikes.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/Tinkering/Traps/Spikes_Wood.png",
        "Weight": 1
      }
    ],
    "HitboxType": "Block_Three_Quarter",
    "RandomRotation": "YawStep90",
    "Interactions": {
      "Collision": {
        "Cooldown": {
          "Id": "SlowEffect",
          "Cooldown": 1
        },
        "Interactions": [
          {
            "Type": "Condition",
            "RequiredGameMode": "Adventure",
            "Next": {
              "Type": "ApplyEffect",
              "EffectId": {
                "Duration": 0.1,
                "DamageCalculator": {
                  "BaseDamage": {
                    "Physical": 10
                  }
                }
              }
            }
          }
        ]
      }
    },
    "Gathering": {
      "Breaking": {
        "GatherType": "SoftBlocks"
      }
    }
  },
  "Tags": {
    "Type": ["Trap"]
  }
}
```

## Trap Damage Balancing

| Trap Type | Damage | Cooldown | Use Case |
|-----------|--------|----------|----------|
| Basic Spike | 10 | 1s | Early game |
| Iron Spike | 25 | 0.5s | Mid game |
| Advanced | 50+ | 0.25s | Late game |

## Tips for Creating Traps

1. **Set appropriate cooldowns** - Prevent traps from being too powerful
2. **Use collision interactions** - Triggers when entities walk on trap
3. **Add game mode restrictions** - Limit traps to Adventure mode
4. **Balance damage** - Start conservative and adjust
5. **Use correct hitbox** - `Block_Three_Quarter` for floor traps
6. **Enable rotation** - Allow placement flexibility
7. **Define breaking behavior** - Set what tool breaks the trap

---

**Previous:** [Furniture](16_Furniture.md) | **Next:** [Containers](18_Containers.md)
