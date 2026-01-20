# Creating Items

Learn how to create custom weapons, armor, tools, and other items with crafting recipes.

> **💡 New to the item system?** See the [Item System Overview](174_Item_System_Overview.md) for a complete guide to how ALL 3,330+ items work together!

## Location
`Server/Item/Items/`

## Example from Game Files

### Iron Sword

From `Server/Item/Items/Weapon/Sword/Weapon_Sword_Iron.json`:

```1:30:Server/Item/Items/Weapon/Sword/Weapon_Sword_Iron.json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Sword_Iron.name",
    "Description": "server.items.Weapon_Sword_Iron.description"
  },
  "Parent": "Template_Weapon_Sword",
  "Quality": "Uncommon",
  "ItemLevel": 20,
  "Model": "Items/Weapons/Sword/Iron.blockymodel",
  "Texture": "Items/Weapons/Sword/Iron_Texture.png",
  "Icon": "Icons/ItemsGenerated/Weapon_Sword_Iron.png",
  "DroppedItemAnimation": "Items/Animations/Dropped/Dropped_Diagonal_Left.blockyanim",
  "PlayerAnimationsId": "Sword",
  "Reticle": "DefaultMelee",
  "ItemSoundSetId": "ISS_Weapons_Sword_Metal",
  "Recipe": {
    "TimeSeconds": 3,
    "Input": [
      {
        "ItemId": "Ingredient_Bar_Iron",
        "Quantity": 6
      },
      {
        "ItemId": "Ingredient_Leather_Light",
        "Quantity": 3
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Categories": ["Weapon_Sword"],
        "Id": "Weapon_Bench"
      }
    ],
    "KnowledgeRequired": false
  },
```

This shows a complete weapon item with recipe, quality, and all required properties.

## Basic Item Structure

Create a new JSON file (e.g., `Weapon/Sword/Weapon_Sword_MyCustom.json`):

```json
{
  "Parent": "Template_Weapon_Sword",
  "TranslationProperties": {
    "Name": "server.items.Weapon_Sword_MyCustom.name"
  },
  "Model": "Items/Weapons/Sword/Iron.blockymodel",
  "Texture": "Items/Weapons/Sword/Iron_Texture.png",
  "Icon": "Icons/ItemsGenerated/Weapon_Sword_Iron.png",
  "Quality": "Rare",
  "ItemLevel": 30,
  "MaxDurability": 150,
  "DurabilityLossOnHit": 0.15
}
```

## Available Templates (Parents)

| Template | Location | Use Case |
|----------|----------|----------|
| `Template_Weapon_Sword` | `Weapon/Sword/` | One-handed swords |
| `Template_Weapon_Longsword` | `Weapon/Longsword/` | Two-handed swords |
| `Template_Weapon_Axe` | `Weapon/Axe/` | One-handed axes |
| `Template_Weapon_Battleaxe` | `Weapon/Battleaxe/` | Two-handed axes |
| `Template_Weapon_Crossbow` | `Weapon/Crossbow/` | Crossbows |

## Overriding Damage Values

```json
{
  "Parent": "Template_Weapon_Sword",
  "InteractionVars": {
    "Swing_Left_Damage": {
      "Interactions": [
        {
          "Parent": "Weapon_Sword_Primary_Swing_Left_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 15
            }
          }
        }
      ]
    },
    "Swing_Right_Damage": {
      "Interactions": [
        {
          "Parent": "Weapon_Sword_Primary_Swing_Right_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 16
            }
          }
        }
      ]
    }
  }
}
```

## Adding Crafting Recipes

### Basic Recipe

```json
{
  "Recipe": {
    "TimeSeconds": 3.5,
    "KnowledgeRequired": false,
    "Input": [
      {
        "ItemId": "Ingredient_Bar_Iron",
        "Quantity": 6
      },
      {
        "ItemId": "Ingredient_Leather_Light",
        "Quantity": 3
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Categories": ["Weapon_Sword"],
        "Id": "Weapon_Bench"
      }
    ]
  }
}
```

### Multi-Output Recipe

```json
{
  "Recipe": {
    "Input": [
      {
        "ItemId": "Ore_Iron",
        "Quantity": 1
      }
    ],
    "Output": [
      {
        "ItemId": "Ingredient_Bar_Iron",
        "Quantity": 1
      },
      {
        "ItemId": "Ingredient_Scrap_Iron",
        "Quantity": 2,
        "Chance": 0.3
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Furnace",
        "Type": "Crafting"
      }
    ],
    "TimeSeconds": 5
  }
}
```

### Knowledge-Locked Recipe

```json
{
  "Recipe": {
    "Input": [
      {
        "ItemId": "Ingredient_Bar_Iron",
        "Quantity": 5
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Weapon_Bench",
        "Type": "Crafting",
        "Categories": ["Weapon_Sword"]
      }
    ],
    "KnowledgeRequired": true,
    "RequiredMemoriesLevel": 3,
    "TimeSeconds": 3
  }
}
```

### Resource Type Requirements

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
    "TimeSeconds": 5
  }
}
```

## Creating Armor

### Overview

Armor items provide protection and stat bonuses when equipped. They use the same base structure as other items but include an `Armor` property that defines protection values and stat modifiers.

### Location
`Server/Item/Items/Armor/{Material}/{Armor}_{Material}_{Slot}.json`

### Basic Armor Structure

Create `Server/Item/Items/Armor/MyCustom/Armor_MyCustom_Chest.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Armor_MyCustom_Chest.name"
  },
  "Quality": "Uncommon",
  "ItemLevel": 20,
  "PlayerAnimationsId": "Block",
  "ItemSoundSetId": "ISS_Armor_Heavy",
  "Categories": [
    "Items.Armors"
  ],
  "Icon": "Icons/ItemsGenerated/Armor_MyCustom_Chest.png",
  "Model": "Items/Armors/MyCustom/Chest.blockymodel",
  "Texture": "Items/Armors/MyCustom/Chest_Texture.png",
  "Recipe": {
    "Input": [
      {
        "ItemId": "Ingredient_Bar_Iron",
        "Quantity": 16
      },
      {
        "ItemId": "Ingredient_Leather_Light",
        "Quantity": 7
      },
      {
        "ItemId": "Ingredient_Fabric_Scrap_Linen",
        "Quantity": 6
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Armor_Bench",
        "Type": "Crafting",
        "Categories": [
          "Armor_Chest"
        ]
      }
    ],
    "KnowledgeRequired": false,
    "TimeSeconds": 3
  },
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "EquipItem"
        }
      ]
    },
    "Secondary": {
      "Interactions": [
        {
          "Type": "EquipItem"
        }
      ]
    }
  },
  "Armor": {
    "ArmorSlot": "Chest",
    "BaseDamageResistance": 0,
    "CosmeticsToHide": [
      "Overtop",
      "Cape"
    ],
    "DamageResistance": {
      "Physical": [
        {
          "Amount": 0.09,
          "CalculationType": "Multiplicative"
        }
      ],
      "Projectile": [
        {
          "Amount": 0.09,
          "CalculationType": "Multiplicative"
        }
      ]
    },
    "StatModifiers": {
      "Health": [
        {
          "Amount": 17,
          "CalculationType": "Additive"
        }
      ]
    }
  },
  "MaxDurability": 100,
  "DurabilityLossOnHit": 0.5,
  "Tags": {
    "Type": [
      "Armor"
    ],
    "Family": [
      "MyCustom"
    ]
  }
}
```

### Armor Slots

| Slot | Description | Crafting Category | Example Cosmetics Hidden |
|------|-------------|-------------------|-------------------------|
| `Head` | Helmet/headgear | `"Armor_Head"` | `EarAccessory`, `Haircut`, `HeadAccessory` |
| `Chest` | Chestplate/armor | `"Armor_Chest"` | `Overtop`, `Cape` |
| `Legs` | Leggings/boots | `"Armor_Legs"` | `Pants`, `Shoes` |

**Note:** In Hytale, boots use the `Legs` slot, not a separate `Feet` slot.

### Armor Properties

#### ArmorSlot

Defines which body part the armor covers:

```json
"Armor": {
  "ArmorSlot": "Head"  // or "Chest" or "Legs"
}
```

#### Damage Resistance

Reduces incoming damage by a percentage:

```json
"DamageResistance": {
  "Physical": [
    {
      "Amount": 0.09,  // 9% damage reduction
      "CalculationType": "Multiplicative"
    }
  ],
  "Projectile": [
    {
      "Amount": 0.09,
      "CalculationType": "Multiplicative"
    }
  ],
  "Fire": [
    {
      "Amount": 0.15,
      "CalculationType": "Multiplicative"
    }
  ]
}
```

**Damage Types:**
- `Physical` - Melee weapon damage
- `Projectile` - Ranged weapon damage
- `Fire` - Fire damage
- `Poison` - Poison damage
- `Light` - Light magic damage
- `Signature` - Signature magic damage

**CalculationType:**
- `Multiplicative` - Reduces damage by percentage (0.09 = 9% reduction)
- `Additive` - Adds to damage resistance

#### Stat Modifiers

Grants stat bonuses when equipped:

```json
"StatModifiers": {
  "Health": [
    {
      "Amount": 17,
      "CalculationType": "Additive"
    }
  ],
  "Mana": [
    {
      "Amount": 50,
      "CalculationType": "Additive"
    }
  ],
  "Stamina": [
    {
      "Amount": 20,
      "CalculationType": "Additive"
    }
  ]
}
```

#### CosmeticsToHide

Specifies which cosmetic items are hidden when armor is equipped:

```json
"CosmeticsToHide": [
  "Overtop",
  "Cape",
  "HeadAccessory",
  "EarAccessory",
  "Haircut",
  "Pants",
  "Shoes"
]
```

Common values:
- **Head slot**: `"EarAccessory"`, `"Ear"`, `"Haircut"`, `"HeadAccessory"`
- **Chest slot**: `"Overtop"`, `"Cape"`
- **Legs slot**: `"Pants"`, `"Shoes"`

#### Damage Class Enhancement (Optional)

Some armor enhances specific damage types when dealing damage:

```json
"DamageClassEnhancement": {
  "Light": [
    {
      "Amount": 0.06,  // +6% Light damage
      "CalculationType": "Multiplicative"
    }
  ],
  "Signature": [
    {
      "Amount": 0.05,  // +5% Signature damage
      "CalculationType": "Multiplicative"
    }
  ]
}
```

### Using Parent Inheritance

You can inherit from existing armor items:

```json
{
  "Parent": "Armor_Iron_Chest",
  "TranslationProperties": {
    "Name": "server.items.Armor_MyCustom_Chest.name"
  },
  "Quality": "Rare",
  "ItemLevel": 35,
  "Icon": "Icons/ItemsGenerated/Armor_MyCustom_Chest.png",
  "Model": "Items/Armors/MyCustom/Chest.blockymodel",
  "Texture": "Items/Armors/MyCustom/Chest_Texture.png",
  "Armor": {
    "ArmorSlot": "Chest",
    "DamageResistance": {
      "Physical": [
        {
          "Amount": 0.12,
          "CalculationType": "Multiplicative"
        }
      ]
    },
    "StatModifiers": {
      "Health": [
        {
          "Amount": 22,
          "CalculationType": "Additive"
        }
      ]
    }
  },
  "MaxDurability": 120
}
```

### Complete Example: Head Armor

```json
{
  "TranslationProperties": {
    "Name": "server.items.Armor_Iron_Head.name"
  },
  "Quality": "Uncommon",
  "ItemLevel": 20,
  "PlayerAnimationsId": "Block",
  "ItemSoundSetId": "ISS_Armor_Heavy",
  "Categories": ["Items.Armors"],
  "Icon": "Icons/ItemsGenerated/Armor_Iron_Head.png",
  "Model": "Items/Armors/Iron/Head.blockymodel",
  "Texture": "Items/Armors/Iron/Head_Texture.png",
  "Recipe": {
    "Input": [
      {
        "ItemId": "Ingredient_Bar_Iron",
        "Quantity": 9
      },
      {
        "ItemId": "Ingredient_Leather_Light",
        "Quantity": 4
      },
      {
        "ItemId": "Ingredient_Fabric_Scrap_Linen",
        "Quantity": 3
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Armor_Bench",
        "Type": "Crafting",
        "Categories": ["Armor_Head"]
      }
    ],
    "TimeSeconds": 3
  },
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "EquipItem"
        }
      ]
    }
  },
  "Armor": {
    "ArmorSlot": "Head",
    "BaseDamageResistance": 0,
    "CosmeticsToHide": [
      "EarAccessory",
      "Ear",
      "Haircut",
      "HeadAccessory"
    ],
    "DamageResistance": {
      "Physical": [
        {
          "Amount": 0.05,
          "CalculationType": "Multiplicative"
        }
      ],
      "Projectile": [
        {
          "Amount": 0.05,
          "CalculationType": "Multiplicative"
        }
      ]
    },
    "StatModifiers": {
      "Health": [
        {
          "Amount": 9,
          "CalculationType": "Additive"
        }
      ]
    }
  },
  "MaxDurability": 100,
  "DurabilityLossOnHit": 0.5,
  "Tags": {
    "Type": ["Armor"],
    "Family": ["Iron"]
  }
}
```

### Complete Example: Legs/Boots Armor

```json
{
  "TranslationProperties": {
    "Name": "server.items.Armor_Iron_Legs.name"
  },
  "Quality": "Uncommon",
  "ItemLevel": 20,
  "PlayerAnimationsId": "Block",
  "ItemSoundSetId": "ISS_Armor_Heavy",
  "Categories": ["Items.Armors"],
  "Icon": "Icons/ItemsGenerated/Armor_Iron_Legs.png",
  "Model": "Items/Armors/Iron/Legs.blockymodel",
  "Texture": "Items/Armors/Iron/Legs_Texture.png",
  "Recipe": {
    "Input": [
      {
        "ItemId": "Ingredient_Bar_Iron",
        "Quantity": 13
      },
      {
        "ItemId": "Ingredient_Leather_Light",
        "Quantity": 6
      },
      {
        "ItemId": "Ingredient_Fabric_Scrap_Linen",
        "Quantity": 4
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Armor_Bench",
        "Type": "Crafting",
        "Categories": ["Armor_Legs"]
      }
    ],
    "TimeSeconds": 3
  },
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "EquipItem"
        }
      ]
    }
  },
  "Armor": {
    "ArmorSlot": "Legs",
    "BaseDamageResistance": 0,
    "CosmeticsToHide": [
      "Pants",
      "Shoes"
    ],
    "DamageResistance": {
      "Physical": [
        {
          "Amount": 0.07,
          "CalculationType": "Multiplicative"
        }
      ],
      "Projectile": [
        {
          "Amount": 0.07,
          "CalculationType": "Multiplicative"
        }
      ]
    },
    "StatModifiers": {
      "Health": [
        {
          "Amount": 13,
          "CalculationType": "Additive"
        }
      ]
    }
  },
  "MaxDurability": 100,
  "DurabilityLossOnHit": 0.5,
  "Tags": {
    "Type": ["Armor"],
    "Family": ["Iron"]
  }
}
```

### Armor Crafting Categories

Each armor slot has a specific crafting category:

| Armor Slot | Crafting Category |
|------------|-------------------|
| Head | `"Armor_Head"` |
| Chest | `"Armor_Chest"` |
| Hands | `"Armor_Hands"` |
| Legs | `"Armor_Legs"` |
| Shield | `"Shield"` |

## Complete Bench Categories Reference

All available bench categories in Hytale (from `server.lang`):

### Workbench Categories
- **`workbench.crafting`** - General crafting recipes
- **`workbench.survival`** - Survival items and basic tools
- **`workbench.housing`** - Housing and furniture items
- **`workbench.tools`** - Tool crafting
- **`workbench.tinkering`** - Advanced tinkering and mechanisms

### Furniture Bench Categories
- **`furniture.seasonal`** - Seasonal decorative furniture
- **`furniture.storage`** - Storage chests and containers
- **`furniture.lighting`** - Lamps, torches, and light sources
- **`furniture.beds`** - Beds and sleeping furniture
- **`furniture.pottery`** - Pottery and ceramic items
- **`furniture.textiles`** - Cloth and textile items
- **`furniture.village_walls`** - Wooden walls and fences
- **`furniture.misc`** - Miscellaneous furniture

### Farming Bench Categories
- **`farmingbench.farming`** - Farming tools and equipment
- **`farmingbench.decorative`** - Decorative farming items
- **`farmingbench.seeds`** - Crop seeds
- **`farmingbench.planters`** - Planter boxes
- **`farmingbench.saplings`** - Tree saplings
- **`farmingbench.essence`** - Essence of Life (growth items)

### Armor Bench Categories
- **`head`** - Helmets and head armor
- **`chest`** - Chest plates and cuirasses
- **`hands`** - Gauntlets and gloves
- **`legs`** - Leg armor and greaves
- **`shield`** - Shields and bucklers

### Alchemy Bench Categories
- **`combatPotions`** - Combat-enhancing potions
- **`miscPotions`** - Utility and miscellaneous potions
- **`bombs`** - Explosive items

### Cooking Bench Categories
- **`prepared`** - Prepared food dishes
- **`baked`** - Baked goods and breads
- **`ingredients`** - Cooking ingredients

### Weapon Smithing Categories
- **`sword`** - Swords (one-handed)
- **`mace`** - Maces and clubs
- **`battleaxe`** - Battleaxes and two-handed axes
- **`daggers`** - Daggers and knives
- **`bow`** - Bows and crossbows

### General Categories
- **`portals`** - Portal devices
- **`misc`** - Miscellaneous items
- **`all`** - All items (catch-all category)

**Note:** Reference these category IDs in your recipe's `BenchRequirement.Categories` array to place items in the correct crafting bench tabs.

### Sound Sets

Armor uses different sound sets based on material:

| Sound Set | Description |
|-----------|-------------|
| `ISS_Armor_Heavy` | Metal/plate armor sounds |
| `ISS_Armor_Leather` | Leather armor sounds |
| `ISS_Armor_Cloth` | Cloth armor sounds |

### Tips for Creating Armor

1. **Balance protection** - Higher tier armor should have better damage resistance
2. **Match visuals** - Use appropriate models and textures for material type
3. **Set CosmeticsToHide** - Hide relevant cosmetics for immersion
4. **Use inheritance** - Inherit from existing armor to save time
5. **Balance stats** - Health bonuses should scale with armor tier
6. **Consider damage types** - Add specific resistances for themed armor (fire resistance for lava armor, etc.)
7. **Icon properties** - Adjust `IconProperties` for proper icon display

---

## Creating Arrows & Ammo

### Overview

Arrows are ammo items used by bows and crossbows. They're stackable items that can be reloaded into ranged weapons, consumed when shooting, and can also be used as melee weapons when held.

### Location
`Server/Item/Items/Weapon/Arrow/`

### Basic Arrow Structure

Create `Server/Item/Items/Weapon/Arrow/Weapon_Arrow_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Arrow_MyCustom.name"
  },
  "Categories": [
    "Items.Weapons"
  ],
  "Quality": "Uncommon",
  "ItemLevel": 20,
  "PlayerAnimationsId": "Dagger",
  "IconProperties": {
    "Scale": 0.5,
    "Rotation": [
      45.0,
      90.0,
      0.0
    ],
    "Translation": [
      -25.0,
      -25.0
    ]
  },
  "Model": "Items/Weapons/Arrow/Arrow.blockymodel",
  "Texture": "Items/Weapons/Arrow/MyCustom_Texture.png",
  "Icon": "Icons/ItemsGenerated/Weapon_Arrow_MyCustom.png",
  "Interactions": {
    "Primary": "Knife_Attack",
    "Secondary": "Knife_Block"
  },
  "InteractionVars": {
    "Knife_Block_Damage": "Knife_Block_Damage",
    "Knife_Swing_Left_Damage": {
      "Interactions": [
        {
          "Parent": "Knife_Swing_Left_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 10
            },
            "RandomPercentageModifier": 0.1
          },
          "DamageEffects": {
            "WorldSoundEventId": "SFX_Axe_Special_Impact"
          }
        }
      ]
    },
    "Knife_Swing_Right_Damage": {
      "Interactions": [
        {
          "Parent": "Knife_Swing_Right_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 10
            },
            "RandomPercentageModifier": 0.1
          },
          "DamageEffects": {
            "WorldSoundEventId": "SFX_Axe_Special_Impact"
          }
        }
      ]
    },
    "Knife_Throw_Charged_Projectile": {
      "Interactions": [
        {
          "Parent": "Knife_Throw_Charged_Projectile",
          "ProjectileId": "Arrow_HalfCharge"
        }
      ]
    }
  },
  "MaxStack": 40,
  "DroppedItemAnimation": "Items/Animations/Dropped/Dropped_Diagonal_Left.blockyanim",
  "Tags": {
    "Type": [
      "Weapon"
    ],
    "Family": [
      "Arrow"
    ]
  },
  "Weapon": {},
  "ItemSoundSetId": "ISS_Weapons_Arrows"
}
```

### Key Arrow Properties

#### MaxStack

Controls how many arrows can stack in one inventory slot:

```json
"MaxStack": 40  // Common: 40-100 arrows per stack
```

- Lower tier arrows: 100 per stack
- Higher tier arrows: 40 per stack

#### Tags

**Critical:** Arrows must have `"Family": ["Arrow"]` for weapons to recognize them as ammo:

```json
"Tags": {
  "Type": ["Weapon"],
  "Family": ["Arrow"]  // Required for ammo system
}
```

#### PlayerAnimationsId

Arrows use dagger animations when held as melee weapons:

```json
"PlayerAnimationsId": "Dagger"
```

#### Melee Weapon Interactions

Arrows can be used as melee weapons when held. They use knife/dagger interactions:

- `"Primary": "Knife_Attack"` - Melee attacks
- `"Secondary": "Knife_Block"` - Blocking
- Various `Knife_Swing_*` and `Knife_Stab_*` interactions for combat

### Arrow Crafting Recipe

```json
{
  "Recipe": {
    "TimeSeconds": 0.5,
    "KnowledgeRequired": false,
    "Input": [
      {
        "Quantity": 4,
        "ItemId": "Ingredient_Stick"
      },
      {
        "ResourceTypeId": "Rubble",
        "Quantity": 1
      }
    ],
    "OutputQuantity": 4,
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Categories": ["Workbench_Survival"],
        "Id": "Workbench"
      },
      {
        "Type": "Crafting",
        "Categories": ["Weapon_Bow"],
        "Id": "Weapon_Bench"
      }
    ]
  }
}
```

**Crafting Bench Requirements:**
- Can be crafted at `Workbench` or `Weapon_Bench`
- Category: `"Weapon_Bow"` or `"Workbench_Survival"`

### Creating Arrow Projectiles

Arrows need projectile configurations for when they're fired. These are separate from the arrow item itself.

#### Arrow Projectile Config

Create `Server/Projectiles/Arrow_MyCustom.json`:

```json
{
  "Appearance": "Arrow_MyCustom",
  "SticksVertically": true,
  "MuzzleVelocity": 10,
  "TerminalVelocity": 50,
  "Gravity": 20,
  "Bounciness": 0,
  "ImpactSlowdown": 0,
  "TimeToLive": 20,
  "Damage": 4,
  "DeadTime": 0.1,
  "HorizontalCenterShot": 0.1,
  "DepthShot": 1,
  "PitchAdjustShot": true,
  "VerticalCenterShot": 0.1,
  "HitSoundEventId": "SFX_Arrow_NoCharge_Hit",
  "MissSoundEventId": "SFX_Arrow_NoCharge_Miss",
  "HitParticles": {
    "SystemId": "Impact_Blade_01"
  }
}
```

#### Arrow Projectile Model

Create `Server/Models/Projectiles/Weapons/Arrow/Arrow_MyCustom.json`:

```json
{
  "HitBox": {
    "Max": {
      "X": 0.075,
      "Y": 0.075,
      "Z": 0.075
    },
    "Min": {
      "X": -0.075,
      "Y": -0.075,
      "Z": -0.075
    }
  },
  "Trails": [
    {
      "PositionOffset": {
        "X": 0.5,
        "Y": 0,
        "Z": 0
      },
      "TargetNodeName": "Handle",
      "TrailId": "Arrow",
      "RotationOffset": {
        "Yaw": 90,
        "Pitch": 0,
        "Roll": 45
      }
    }
  ],
  "DefaultAttachments": [
    {
      "Model": "Items/Weapons/Arrow/Arrow.blockymodel",
      "Texture": "Items/Weapons/Arrow/MyCustom_Texture.png"
    }
  ],
  "Model": "Items/Projectiles/Projectile.blockymodel",
  "Texture": "Items/Projectiles/Projectile_default.png",
  "AnimationSets": {
    "Idle": {
      "Animations": [
        {
          "Animation": "Items/Animations/Fly/Fly_Vertical.blockyanim"
        }
      ]
    }
  }
}
```

### How Ammo Works with Weapons

Arrows work with the ammo system for ranged weapons:

1. **Ammo Stat**: Weapons like crossbows use an `Ammo` entity stat to track loaded ammo
2. **Reload**: Players reload by using the reload ability (default: Ability3), which:
   - Checks inventory for arrows (via `ModifyInventory` interaction)
   - Removes arrow items from inventory
   - Adds to the weapon's `Ammo` stat
3. **Shooting**: Each shot consumes 1 `Ammo` stat
4. **Unequipping**: When switching weapons, remaining `Ammo` is converted back to arrow items

### Arrow Item ID for Weapons

When creating weapons that use arrows, reference the arrow item ID in reload interactions:

```json
{
  "Type": "ModifyInventory",
  "ItemToRemove": {
    "Id": "Weapon_Arrow_MyCustom",  // Your arrow item ID
    "Quantity": 1
  }
}
```

### Complete Example: Iron Arrow

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Arrow_Iron.name"
  },
  "Categories": ["Items.Weapons"],
  "Quality": "Uncommon",
  "ItemLevel": 20,
  "PlayerAnimationsId": "Dagger",
  "IconProperties": {
    "Scale": 0.5,
    "Rotation": [45.0, 90.0, 0.0],
    "Translation": [-25.0, -25.0]
  },
  "Model": "Items/Weapons/Arrow/Arrow.blockymodel",
  "Texture": "Items/Weapons/Arrow/Iron_Texture.png",
  "Icon": "Icons/ItemsGenerated/Weapon_Arrow_Iron.png",
  "Interactions": {
    "Primary": "Knife_Attack",
    "Secondary": "Knife_Block"
  },
  "InteractionVars": {
    "Knife_Swing_Left_Damage": {
      "Interactions": [
        {
          "Parent": "Knife_Swing_Left_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 10
            }
          }
        }
      ]
    },
    "Knife_Throw_Charged_Projectile": {
      "Interactions": [
        {
          "Parent": "Knife_Throw_Charged_Projectile",
          "ProjectileId": "Arrow_HalfCharge"
        }
      ]
    }
  },
  "MaxStack": 40,
  "Tags": {
    "Type": ["Weapon"],
    "Family": ["Arrow"]
  },
  "Weapon": {},
  "ItemSoundSetId": "ISS_Weapons_Arrows"
}
```

### Arrow Tier Reference

| Tier | MaxStack | Damage (Melee) | Quality |
|------|----------|----------------|---------|
| Crude | 100 | 1 | Common |
| Iron | 40 | 10 | Uncommon |
| Steel | 40 | 15 | Rare |

### Tips for Creating Arrows

1. **Always use `"Family": ["Arrow"]`** - Required for ammo system to work
2. **Set MaxStack appropriately** - Lower tier = higher stack, higher tier = lower stack
3. **Create projectile configs** - Needed for when arrows are fired
4. **Create projectile models** - Visual appearance when flying
5. **Reference in weapon reload** - Update weapon reload interactions to use your arrow ID
6. **Melee capability** - Arrows can be used as melee weapons (dagger-like)
7. **Sound sets** - Use `"ISS_Weapons_Arrows"` for arrow sounds
8. **Icon rotation** - Use standard arrow icon rotation (45, 90, 0)

---

## Creating Benches

### Overview

Benches are crafting stations that allow players to craft items. They're placeable blocks with crafting categories, upgradeable tiers, and animations. Examples include Workbench, Weapon Bench, Armor Bench, and Cooking Bench.

### Location
`Server/Item/Items/Bench/`

### Basic Bench Structure

Create `Server/Item/Items/Bench/Bench_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Bench_MyCustom.name",
    "Description": "server.items.Bench_MyCustom.description"
  },
  "Icon": "Icons/ItemsGenerated/Bench_MyCustom.png",
  "Categories": [
    "Furniture.Benches"
  ],
  "Recipe": {
    "TimeSeconds": 3,
    "Input": [
      {
        "ResourceTypeId": "Wood_Trunk",
        "Quantity": 10
      },
      {
        "ResourceTypeId": "Rock",
        "Quantity": 5
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Categories": ["Workbench_Crafting"],
        "Id": "Workbench"
      }
    ]
  },
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Model",
    "Opacity": "Transparent",
    "CustomModel": "Blocks/Benches/MyCustom.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/Benches/MyCustom_Texture.png",
        "Weight": 1
      }
    ],
    "HitboxType": "Bench_MyCustom",
    "VariantRotation": "NESW",
    "Bench": {
      "Type": "Crafting",
      "Id": "MyCustom_Bench",
      "Categories": [
        {
          "Id": "MyCategory",
          "Icon": "Icons/CraftingCategories/MyCategory.png",
          "Name": "server.benchCategories.mycategory"
        }
      ],
      "LocalOpenSoundEventId": "SFX_Workbench_Open",
      "LocalCloseSoundEventId": "SFX_Workbench_Close",
      "CompletedSoundEventId": "SFX_Workbench_Craft",
      "FailedSoundEventId": "SFX_Generic_Crafting_Failed"
    },
    "State": {
      "Id": "crafting",
      "Definitions": {
        "CraftCompleted": {
          "CustomModelAnimation": "Blocks/Benches/MyCustom_Crafting.blockyanim",
          "Looping": true
        },
        "CraftCompletedInstant": {
          "CustomModelAnimation": "Blocks/Benches/MyCustom_Crafting.blockyanim"
        }
      }
    },
    "Gathering": {
      "Breaking": {
        "GatherType": "Benches"
      }
    },
    "Support": {
      "Down": [
        {
          "FaceType": "Full"
        }
      ]
    },
    "BlockParticleSetId": "Wood",
    "BlockSoundSetId": "Wood"
  },
  "PlayerAnimationsId": "Block",
  "Tags": {
    "Type": ["Bench"]
  },
  "MaxStack": 1,
  "ItemSoundSetId": "ISS_Blocks_Wood"
}
```

### Key Bench Properties

#### Bench Configuration

The `Bench` property in `BlockType` defines crafting behavior:

```json
"Bench": {
  "Type": "Crafting",  // or "StructuralCrafting"
  "Id": "MyCustom_Bench",  // Unique bench ID (used in BenchRequirement)
  "Categories": [
    {
      "Id": "MyCategory",
      "Icon": "Icons/CraftingCategories/MyCategory.png",
      "Name": "server.benchCategories.mycategory"
    }
  ]
}
```

**Bench Types:**
- `"Crafting"` - Standard crafting bench
- `"StructuralCrafting"` - Builder/architect bench for structural items

#### Crafting Categories

Each category appears as a tab in the crafting UI:

```json
"Categories": [
  {
    "Id": "Weapon_Sword",
    "Icon": "Icons/CraftingCategories/Armory/Sword.png",
    "Name": "server.benchCategories.sword"
  },
  {
    "Id": "Weapon_Axe",
    "Icon": "Icons/CraftingCategories/Armory/Axe.png",
    "Name": "server.benchCategories.axe"
  }
]
```

**Category Properties:**
- `Id` - Category identifier (referenced in item recipes)
- `Icon` - UI icon for the category tab
- `Name` - Translation key for category name

#### Tier Levels (Upgrades)

Benches can have multiple upgrade tiers with improved crafting speed:

```json
"TierLevels": [
  {
    "CraftingTimeReductionModifier": 0.0,  // Tier 1: No reduction
    "UpgradeRequirement": {
      "Material": [
        {
          "ItemId": "Ingredient_Bar_Iron",
          "Quantity": 20
        }
      ],
      "TimeSeconds": 3
    }
  },
  {
    "CraftingTimeReductionModifier": 0.15,  // Tier 2: 15% faster
    "UpgradeRequirement": {
      "Material": [
        {
          "ItemId": "Ingredient_Bar_Steel",
          "Quantity": 30
        }
      ],
      "TimeSeconds": 5
    }
  },
  {
    "CraftingTimeReductionModifier": 0.3  // Tier 3: 30% faster (max tier)
  }
]
```

**Tier Properties:**
- `CraftingTimeReductionModifier` - Speed bonus (0.0 = no bonus, 0.3 = 30% faster)
- `UpgradeRequirement` - Materials needed to upgrade to next tier
- Last tier has no upgrade requirement (it's the maximum)

#### State Definitions (Animations)

Define animations for different crafting states:

```json
"State": {
  "Id": "crafting",
  "Definitions": {
    "CraftCompleted": {
      "CustomModelAnimation": "Blocks/Benches/MyCustom_Crafting.blockyanim",
      "Looping": true
    },
    "CraftCompletedInstant": {
      "CustomModelAnimation": "Blocks/Benches/MyCustom_Crafting.blockyanim"
    },
    "Tier2": {
      "CustomModel": "Blocks/Benches/MyCustom2.blockymodel",
      "CustomModelTexture": [
        {
          "Texture": "Blocks/Benches/MyCustom2_Texture.png",
          "Weight": 1
        }
      ],
      "HitboxType": "Bench_MyCustom2"
    }
  }
}
```

**State Types:**
- `CraftCompleted` - Animation when crafting finishes (looping)
- `CraftCompletedInstant` - One-time animation for instant crafts
- `Tier2`, `Tier3` - Visual models for upgraded benches

#### Hitbox Types

Define collision detection:

```json
"HitboxType": "Bench_MyCustom"
```

Common hitbox types:
- `Bench_Workbench`
- `Bench_Weapon`
- `Bench_Armor`
- `Bench_Cooking`
- Custom hitbox types for your bench

#### Sound Events

Configure UI and crafting sounds:

```json
"Bench": {
  "LocalOpenSoundEventId": "SFX_Workbench_Open",
  "LocalCloseSoundEventId": "SFX_Workbench_Close",
  "CompletedSoundEventId": "SFX_Workbench_Craft",
  "FailedSoundEventId": "SFX_Generic_Crafting_Failed",
  "BenchUpgradeSoundEventId": "SFX_Workbench_Upgrade_Start_Default",
  "BenchUpgradeCompletedSoundEventId": "SFX_Workbench_Upgrade_Complete_Default"
}
```

#### Variant Rotation

Allow bench to be rotated when placed:

```json
"VariantRotation": "NESW"  // North, East, South, West rotations
```

### Using Bench IDs in Recipes

Other items reference benches by their `Id`:

```json
{
  "Recipe": {
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Id": "MyCustom_Bench",  // Reference to bench ID
        "Categories": ["MyCategory"]
      }
    ]
  }
}
```

### Complete Example: Simple Crafting Bench

```json
{
  "TranslationProperties": {
    "Name": "server.items.Bench_MyCrafting.name",
    "Description": "server.items.Bench_MyCrafting.description"
  },
  "Icon": "Icons/ItemsGenerated/Bench_MyCrafting.png",
  "Categories": ["Furniture.Benches"],
  "Recipe": {
    "TimeSeconds": 3,
    "Input": [
      {
        "ResourceTypeId": "Wood_Trunk",
        "Quantity": 10
      },
      {
        "ResourceTypeId": "Rock",
        "Quantity": 5
      }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Categories": ["Workbench_Crafting"],
        "Id": "Workbench"
      }
    ]
  },
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Model",
    "Opacity": "Transparent",
    "CustomModel": "Blocks/Benches/MyCrafting.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/Benches/MyCrafting_Texture.png",
        "Weight": 1
      }
    ],
    "HitboxType": "Bench_MyCrafting",
    "VariantRotation": "NESW",
    "Bench": {
      "Type": "Crafting",
      "Id": "MyCrafting_Bench",
      "Categories": [
        {
          "Id": "Custom_Items",
          "Icon": "Icons/CraftingCategories/Custom/Items.png",
          "Name": "server.benchCategories.custom.items"
        }
      ],
      "LocalOpenSoundEventId": "SFX_Workbench_Open",
      "LocalCloseSoundEventId": "SFX_Workbench_Close",
      "CompletedSoundEventId": "SFX_Workbench_Craft"
    },
    "State": {
      "Id": "crafting",
      "Definitions": {
        "CraftCompleted": {
          "CustomModelAnimation": "Blocks/Benches/MyCrafting_Crafting.blockyanim",
          "Looping": true
        }
      }
    },
    "Gathering": {
      "Breaking": {
        "GatherType": "Benches"
      }
    },
    "Support": {
      "Down": [
        {
          "FaceType": "Full"
        }
      ]
    },
    "BlockParticleSetId": "Wood",
    "BlockSoundSetId": "Wood"
  },
  "PlayerAnimationsId": "Block",
  "Tags": {
    "Type": ["Bench"]
  },
  "MaxStack": 1,
  "ItemSoundSetId": "ISS_Blocks_Wood"
}
```

### Common Bench IDs

| Bench ID | Use Case | Example Categories |
|----------|----------|-------------------|
| `Workbench` | General crafting | `Workbench_Survival`, `Workbench_Crafting` |
| `Weapon_Bench` | Weapon crafting | `Weapon_Sword`, `Weapon_Bow` |
| `Armor_Bench` | Armor crafting | `Armor_Head`, `Armor_Chest` |
| `Cookingbench` | Food preparation | `Prepared`, `Baked`, `Ingredients` |
| `Builders` | Structural items | `WoodPlanks`, `Bricks`, `Stairs` |

### Tips for Creating Benches

1. **Unique Bench ID** - Use a unique `Id` in the `Bench` property
2. **Category IDs** - Match category `Id` values to those used in item recipes
3. **MaxStack: 1** - Benches should not stack
4. **Hitbox types** - Create custom hitbox types for custom benches
5. **Tier upgrades** - Add tier levels for progression
6. **Animations** - Add crafting animations for visual feedback
7. **Sound events** - Use appropriate sound events for interactions
8. **Support requirement** - Benches need `Support.Down` to be placed on solid blocks
9. **Gathering type** - Set `GatherType: "Benches"` for proper breaking behavior
10. **Variant rotation** - Enable rotation with `"NESW"` for flexible placement

---

## Quality Tiers

| Quality | Description |
|---------|-------------|
| `Template` | Base template (not droppable) |
| `Common` | Basic quality |
| `Uncommon` | Slightly better |
| `Rare` | Good quality |
| `Epic` | High quality |
| `Legendary` | Best quality |

---

**Next:** [Creating NPCs](03_NPCs.md) → [Potions & Effects](04_Potions_and_Effects.md)
