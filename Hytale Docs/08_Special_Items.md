# Special Items

Learn how to create boots with double jump and chests with loot.

## Creating Boots with Double Jump

### Overview

To create boots that grant double jump ability, you'll create a legs armor piece that adds the double jump interaction when equipped. The double jump interaction is already defined in the game, so you just need to reference it in your boots.

### Step 1: Understand the Double Jump Interaction

## Example from Game Files

### Double Jump Interaction

From `Server/Item/RootInteractions/Double_Jump.json`:

The game already has a `Double_Jump` interaction at `Server/Item/RootInteractions/Double_Jump.json` that:
- Costs 2 Stamina
- Applies upward force (jump boost)
- Plays particle effects and sounds

### Step 2: Create Boots Armor Item

Create `Server/Item/Items/Armor/Custom/Armor_Custom_Boots.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Armor_Custom_Boots.name"
  },
  "Quality": "Uncommon",
  "ItemLevel": 20,
  "PlayerAnimationsId": "Block",
  "ItemSoundSetId": "ISS_Armor_Leather",
  "Categories": [
    "Items.Armors"
  ],
  "Icon": "Icons/ItemsGenerated/Armor_Custom_Boots.png",
  "Model": "Items/Armors/Custom/Boots.blockymodel",
  "Texture": "Items/Armors/Custom/Boots_Texture.png",
  "Recipe": {
    "Input": [
      {
        "ItemId": "Ingredient_Leather_Light",
        "Quantity": 4
      },
      {
        "ResourceTypeId": "Wood_Trunk",
        "Quantity": 2
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Armor_Bench",
        "Type": "Crafting",
        "Categories": [
          "Armor_Legs"
        ]
      }
    ],
    "KnowledgeRequired": false,
    "TimeSeconds": 2
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
    },
    "Wielding": "Double_Jump"
  },
  "Armor": {
    "ArmorSlot": "Legs",
    "BaseDamageResistance": 0,
    "CosmeticsToHide": [
      "Shoes",
      "Pants"
    ],
    "StatModifiers": {
      "Health": [
        {
          "Amount": 10,
          "CalculationType": "Additive"
        }
      ]
    },
    "DamageResistance": {
      "Physical": [
        {
          "Amount": 0.05,
          "CalculationType": "Multiplicative"
        }
      ]
    }
  },
  "MaxDurability": 120,
  "Tags": {
    "Type": [
      "Armor"
    ],
    "Family": [
      "Custom"
    ]
  }
}
```

### Key Field: Interactions.Wielding

The crucial addition is the `Wielding` field inside `Interactions`:

```json
"Interactions": {
  "Wielding": "Double_Jump"
}
```

This tells the game that when these boots are equipped (and nothing is wielded in hands), the `Double_Jump` interaction becomes available.

### Alternative: Modifying Double Jump Parameters

If you want custom double jump behavior, you can:

1. **Create a custom double jump interaction** (`Server/Item/Interactions/Custom_Double_Jump.json`):

```json
{
  "Type": "StatsCondition",
  "Costs": {
    "Stamina": 1.5
  },
  "Next": {
    "Type": "Serial",
    "Interactions": [
      {
        "Type": "ApplyForce",
        "Direction": {
          "X": 0,
          "Y": 2.5,
          "Z": 0
        },
        "AdjustVertical": false,
        "WaitForGround": false,
        "Force": 20
      },
      {
        "Type": "ChangeStat",
        "StatModifiers": {
          "Stamina": -1.5
        }
      },
      {
        "Type": "Simple",
        "RunTime": 1,
        "Effects": {
          "Particles": [
            {
              "TargetEntityPart": "Entity",
              "TargetNodeName": "L-Foot",
              "SystemId": "Impact_Feathers_Black"
            },
            {
              "TargetEntityPart": "Entity",
              "TargetNodeName": "R-Foot",
              "SystemId": "Impact_Feathers_Black"
            }
          ]
        }
      }
    ]
  }
}
```

2. **Reference it in your boots**:

```json
"Interactions": {
  "Wielding": "Custom_Double_Jump"
}
```

### Double Jump Parameters Explained

| Parameter | Effect | Default Value |
|-----------|--------|---------------|
| `Stamina` cost | Stamina required to double jump | `2.01` |
| `Force` | Upward jump strength | `15` |
| `Direction.Y` | Vertical direction multiplier | `2` |
| `StaminaRegenDelay` | Delay before stamina regens | `0.3` seconds |

### Example: High Jump Boots

```json
{
  "Type": "StatsCondition",
  "Costs": {
    "Stamina": 3
  },
  "Next": {
    "Type": "ApplyForce",
    "Direction": {
      "X": 0,
      "Y": 3,
      "Z": 0
    },
    "Force": 25
  }
}
```

### Tips for Double Jump Boots

1. **Balance stamina cost** - Higher jumps should cost more stamina
2. **Test jump height** - Adjust `Force` and `Direction.Y` values
3. **Add visual effects** - Use particles at foot nodes for visual feedback
4. **Consider armor stats** - Balance defense vs. mobility
5. **Cosmetic compatibility** - Set `CosmeticsToHide` to hide shoe cosmetics

### Troubleshooting

- **Double jump not working**: Ensure `Interactions.Wielding` is set correctly and boots are equipped in Legs slot
- **Too weak/strong**: Adjust `Force` value (higher = jump higher)
- **Stamina issues**: Modify the `Stamina` cost in the interaction

---

## Spawning Chests with Items

### Overview

Chests can be spawned with items in two ways:
1. **Using Prefabs** - Spawn chests in the world with predefined items
2. **Using Drop Lists** - Reference a drop table that populates when opened

### Method 1: Using Prefabs (Recommended)

Prefabs allow you to place chests in specific locations with pre-configured items.

#### Creating a Chest Prefab

Create `Server/Prefabs/MyCustom/Custom_Chest.prefab.json`:

```json
{
  "version": 8,
  "blockIdVersion": 0,
  "anchorX": 0,
  "anchorY": 0,
  "anchorZ": 0,
  "blocks": [
    {
      "x": 0,
      "y": 0,
      "z": 0,
      "name": "Furniture_Crude_Chest_Small",
      "components": {
        "Components": {
          "container": {
            "Position": {
              "X": 0,
              "Y": 0,
              "Z": 0
            },
            "Custom": false,
            "AllowViewing": true,
            "Droplist": "Drop_MyCustom_Chest",
            "ItemContainer": {
              "Capacity": 18,
              "Items": {}
            }
          }
        }
      },
      "rotation": 1
    }
  ]
}
```

#### Field Explanations

| Field | Description |
|-------|-------------|
| `name` | The chest item ID (e.g., `"Furniture_Crude_Chest_Small"`) |
| `Droplist` | **Drop table ID** that populates the chest when first opened |
| `ItemContainer.Capacity` | Number of item slots (18 for small, 36 for large) |
| `ItemContainer.Items` | Direct items (leave empty `{}` to use Droplist) |
| `rotation` | Chest rotation (0-3 for NESW directions) |

#### Available Chest Types

| Chest ID | Capacity | Description |
|----------|----------|-------------|
| `Furniture_Crude_Chest_Small` | 18 | Basic wooden chest |
| `Furniture_Crude_Chest_Large` | 36 | Large wooden chest |
| `Furniture_Dungeon_Chest_Epic` | 1 | Single-item epic chest |
| `Furniture_Dungeon_Chest_Legendary_Large` | 1 | Single-item legendary chest |
| `Furniture_Goblin_Chest_Small` | 18 | Goblin-themed chest |
| `Furniture_Kweebec_Chest_Small` | 18 | Kweebec-themed chest |

### Method 2: Creating a Drop List for Chests

Create a drop table that will populate the chest when opened.

#### Step 1: Create Drop List

Create `Server/Drops/Prefabs/MyCustom/Drop_MyCustom_Chest.json`:

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
              "ItemId": "Ore_Copper",
              "QuantityMin": 1,
              "QuantityMax": 5
            }
          },
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Ore_Iron",
              "QuantityMin": 1,
              "QuantityMax": 5
            }
          }
        ]
      },
      {
        "Type": "Choice",
        "Weight": 50,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Weapon_Sword_Iron"
            }
          },
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Weapon_Axe_Iron"
            }
          }
        ]
      },
      {
        "Type": "Choice",
        "Weight": 25,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Armor_Iron_Chest"
            }
          }
        ]
      }
    ]
  }
}
```

#### Step 2: Reference in Prefab

In your prefab, reference the drop list:

```json
"Droplist": "Drop_MyCustom_Chest"
```

### Complete Example: Treasure Chest

**1. Create the Drop List** (`Server/Drops/Prefabs/Treasure/Drop_Treasure_Chest.json`):

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Rock_Gem_Emerald",
          "QuantityMin": 2,
          "QuantityMax": 10
        }
      },
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Rock_Gem_Ruby",
          "QuantityMin": 2,
          "QuantityMax": 10
        }
      },
      {
        "Type": "Choice",
        "Weight": 75,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Weapon_Sword_Mithril"
            }
          }
        ]
      }
    ]
  }
}
```

**2. Create the Prefab** (`Server/Prefabs/Treasure/Treasure_Chest.prefab.json`):

```json
{
  "version": 8,
  "blockIdVersion": 0,
  "anchorX": 0,
  "anchorY": 0,
  "anchorZ": 0,
  "blocks": [
    {
      "x": 0,
      "y": 0,
      "z": 0,
      "name": "Furniture_Dungeon_Chest_Legendary_Large",
      "components": {
        "Components": {
          "container": {
            "Position": {
              "X": 0,
              "Y": 0,
              "Z": 0
            },
            "Custom": false,
            "AllowViewing": true,
            "Droplist": "Drop_Treasure_Chest",
            "ItemContainer": {
              "Capacity": 1,
              "Items": {}
            }
          }
        }
      },
      "rotation": 0
    }
  ]
}
```

### Direct Item Assignment (Advanced)

Instead of using a Droplist, you can directly assign items in the prefab:

```json
"ItemContainer": {
  "Capacity": 18,
  "Items": {
    "0": {
      "ItemId": "Weapon_Sword_Iron",
      "Quantity": 1
    },
    "1": {
      "ItemId": "Ore_Copper",
      "Quantity": 10
    }
  }
}
```

**Note:** Direct item assignment is less flexible and items won't be randomly generated. Use Droplist for randomized loot.

### Chest Capacity Reference

| Chest Type | Capacity | Use Case |
|------------|----------|----------|
| Small Chests | 18 slots | Standard containers |
| Large Chests | 36 slots | Extended storage |
| Legendary/Epic | 1 slot | Single rare item |

### Tips for Chest Loot

1. **Use weighted drop lists** - Higher weights = more common items
2. **Mix guaranteed and random** - Use `Type: "Single"` for guaranteed items, `Type: "Choice"` for random
3. **Tier your loot** - Create different drop tables for different difficulty levels
4. **Reference existing drops** - Look at `Server/Drops/Objectives/` for examples
5. **Test spawn locations** - Use the prefab editor to place chests in desired locations

---

## Creating Pickpocket System

### Overview

Creating a Skyrim-like pickpocket system involves using item interactions that:
1. Target NPCs via raycast/selector
2. Check for success/failure conditions
3. Remove items from NPC inventory (if possible)
4. Add items to player inventory
5. Handle detection and failure states

**Note:** Direct NPC inventory access from item interactions may be limited. This system may require NPC-side scripting or may need to work with drop lists instead of live NPC inventories.

### Location
`Server/Item/Items/Tool/` or `Server/Item/Items/MISC/`

### Basic Pickpocket Item Structure

Create `Server/Item/Items/MISC/Tool_Pickpocket.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Tool_Pickpocket.name",
    "Description": "server.items.Tool_Pickpocket.description"
  },
  "Categories": ["Items.Tools"],
  "Quality": "Common",
  "ItemLevel": 10,
  "Icon": "Icons/ItemsGenerated/Tool_Pickpocket.png",
  "Model": "Items/Tools/Pickpocket/Pickpocket.blockymodel",
  "Texture": "Items/Tools/Pickpocket/Pickpocket_Texture.png",
  "PlayerAnimationsId": "Dagger",
  "Interactions": {
    "Primary": "Pickpocket_Attempt"
  },
  "InteractionVars": {
    "Pickpocket_Attempt": {
      "Interactions": [
        {
          "Parent": "Pickpocket_Attempt_Base",
          "SuccessChance": 0.7
        }
      ]
    },
    "Pickpocket_Success": "Pickpocket_Success_Action",
    "Pickpocket_Fail": "Pickpocket_Fail_Action"
  },
  "Tags": {
    "Type": ["Tool"]
  }
}
```

### Pickpocket Interaction: Targeting NPCs

Create `Server/Item/Interactions/Tools/Pickpocket/Pickpocket_Attempt_Base.json`:

```json
{
  "Type": "Simple",
  "RunTime": 1.0,
  "Effects": {
    "ItemAnimationId": "Interact",
    "WorldSoundEventId": "SFX_Pickpocket_Attempt",
    "LocalSoundEventId": "SFX_Pickpocket_Attempt"
  },
  "Next": {
    "Type": "Selector",
    "Selector": {
      "Id": "Raycast",
      "Offset": {
        "Y": 1.6
      },
      "Distance": 3
    },
    "HitEntityRules": [
      {
        "Matchers": [
          {
            "Type": "Vulnerable"
          },
          {
            "Type": "Player",
            "Invert": true
          }
        ],
        "Next": {
          "Type": "Replace",
          "Var": "Pickpocket_Check",
          "DefaultValue": {
            "Interactions": [
              "Pickpocket_Check_Chance"
            ]
          }
        }
      }
    ],
    "HitEntity": {
      "Interactions": [
        {
          "Type": "SendMessage",
          "Message": "Must target an NPC to pickpocket"
        }
      ]
    },
    "HitBlock": {
      "Interactions": [
        {
          "Type": "SendMessage",
          "Message": "Nothing to pickpocket here"
        }
      ]
    }
  }
}
```

### Pickpocket Success/Failure Check

Create `Server/Item/Interactions/Tools/Pickpocket/Pickpocket_Check_Chance.json`:

```json
{
  "Type": "Random",
  "Chance": 0.7,
  "Next": {
    "Type": "Replace",
    "Var": "Pickpocket_Success",
    "DefaultValue": {
      "Interactions": [
        "Pickpocket_Success_Action"
      ]
    }
  },
  "Failed": {
    "Type": "Replace",
    "Var": "Pickpocket_Fail",
    "DefaultValue": {
      "Interactions": [
        "Pickpocket_Fail_Action"
      ]
    }
  }
}
```

**Alternative: Stats-Based Success**

Use player stats for success chance:

```json
{
  "Type": "StatsCondition",
  "Costs": {
    "Stamina": 10
  },
  "Next": {
    "Type": "Random",
    "Chance": 0.7,
    "Next": {
      "Type": "Replace",
      "Var": "Pickpocket_Success"
    }
  },
  "Failed": {
    "Type": "Replace",
    "Var": "Pickpocket_Fail"
  }
}
```

### Pickpocket Success Action

Create `Server/Item/Interactions/Tools/Pickpocket/Pickpocket_Success_Action.json`:

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "Simple",
      "Effects": {
        "WorldSoundEventId": "SFX_Pickpocket_Success",
        "LocalSoundEventId": "SFX_Pickpocket_Success",
        "Particles": [
          {
            "SystemId": "Pickpocket_Success",
            "PositionOffset": {
              "Y": 1.5
            }
          }
        ]
      }
    },
    {
      "Type": "ModifyInventory",
      "ItemToAdd": {
        "ItemId": "Ingredient_Life_Essence",
        "Quantity": 5,
        "Chance": 1.0
      }
    },
    {
      "Type": "SendMessage",
      "Message": "Successfully pickpocketed!"
    }
  ]
}
```

### Pickpocket Failure Action

Create `Server/Item/Interactions/Tools/Pickpocket/Pickpocket_Fail_Action.json`:

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "Simple",
      "Effects": {
        "WorldSoundEventId": "SFX_Pickpocket_Fail",
        "LocalSoundEventId": "SFX_Pickpocket_Fail",
        "Particles": [
          {
            "SystemId": "Pickpocket_Fail",
            "PositionOffset": {
              "Y": 1.5
            }
          }
        ]
      }
    },
    {
      "Type": "ApplyEffect",
      "Entity": "Target",
      "EffectId": "Alert"
    },
    {
      "Type": "OverrideAttitude",
      "Entity": "Target",
      "Attitude": "Hostile",
      "Duration": 60
    },
    {
      "Type": "SendMessage",
      "Message": "You were caught pickpocketing!"
    }
  ]
}
```

### Advanced: Using NPC Drop Lists

Since direct NPC inventory access may be limited, you can use drop lists to simulate pickpocketed items:

**1. Create NPC Drop List** (`Server/Drops/NPCs/Intelligent/MyCustom/Drop_Pickpocket_Villager.json`):

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Choice",
        "Weight": 50,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Ingredient_Life_Essence",
              "QuantityMin": 5,
              "QuantityMax": 15
            }
          },
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Ore_Copper",
              "QuantityMin": 1,
              "QuantityMax": 5
            }
          }
        ]
      },
      {
        "Type": "Choice",
        "Weight": 20,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Weapon_Dagger_Iron"
            }
          }
        ]
      }
    ]
  }
}
```

**2. Use Drop List in Success Action:**

```json
{
  "Type": "GiveItems",
  "DropList": "Drop_Pickpocket_Villager"
}
```

### Factors Affecting Pickpocket Success

You can add multiple conditions:

**Distance Check:**
```json
{
  "Type": "Condition",
  "Crouching": true,
  "Next": {
    // Pickpocket attempt
  },
  "Failed": {
    "Type": "SendMessage",
    "Message": "You must be crouching to pickpocket"
  }
}
```

**Time of Day:**
- Harder during day (NPCs more alert)
- Easier at night

**NPC Alertness:**
- Check if NPC is in combat
- Check if NPC is sleeping
- Check NPC attitude

**Skill Level:**
- Use player stats or levels to modify success chance
- Higher skill = higher success rate

### Complete Example: Pickpocket Tool

**1. Pickpocket Item** (`Server/Item/Items/MISC/Tool_Pickpocket.json`):

```json
{
  "TranslationProperties": {
    "Name": "server.items.Tool_Pickpocket.name"
  },
  "Categories": ["Items.Tools"],
  "Quality": "Common",
  "ItemLevel": 10,
  "Icon": "Icons/ItemsGenerated/Tool_Pickpocket.png",
  "PlayerAnimationsId": "Dagger",
  "Interactions": {
    "Primary": "Pickpocket_Attempt"
  },
  "InteractionVars": {
    "Pickpocket_Attempt": "Pickpocket_Attempt_Base"
  },
  "Tags": {
    "Type": ["Tool"]
  }
}
```

**2. Main Interaction** (`Server/Item/Interactions/Tools/Pickpocket/Pickpocket_Attempt_Base.json`):

```json
{
  "Type": "Condition",
  "Crouching": true,
  "Next": {
    "Type": "Simple",
    "RunTime": 1.0,
    "Effects": {
      "ItemAnimationId": "Interact",
      "WorldSoundEventId": "SFX_Pickpocket_Attempt"
    },
    "Next": {
      "Type": "Selector",
      "Selector": {
        "Id": "Raycast",
        "Distance": 3,
        "Offset": {
          "Y": 1.6
        }
      },
      "HitEntityRules": [
        {
          "Matchers": [
            {
              "Type": "Vulnerable"
            },
            {
              "Type": "Player",
              "Invert": true
            }
          ],
          "Next": {
            "Type": "Random",
            "Chance": 0.7,
            "Next": {
              "Type": "ModifyInventory",
              "ItemToAdd": {
                "ItemId": "Ingredient_Life_Essence",
                "Quantity": 5
              }
            },
            "Failed": {
              "Type": "ApplyEffect",
              "Entity": "Target",
              "EffectId": "Alert"
            }
          }
        }
      ]
    }
  },
  "Failed": {
    "Type": "SendMessage",
    "Message": "You must be crouching to pickpocket"
  }
}
```

### Pickpocket Mechanics Ideas

1. **Success Rate Based on:**
   - Player level/skill
   - NPC awareness state
   - Item value/weight
   - Distance from NPC
   - Time of day

2. **Failure Consequences:**
   - NPC becomes hostile
   - NPC calls for help
   - Player reputation decreases
   - Bounty added
   - NPC remembers player

3. **Detection Factors:**
   - NPC facing direction
   - Other NPCs nearby
   - Light level
   - Player movement

### Limitations & Considerations

**Current Limitations:**
- Direct NPC inventory access from item interactions may not be fully supported
- May need to use drop lists instead of live NPC inventories
- NPC detection/alertness system may need NPC-side scripting

**Workarounds:**
1. Use drop lists that represent pickpocketable items
2. Create NPC variants with different drop lists
3. Use NPC instructions to handle detection
4. Combine item interactions with NPC state changes

### Tips for Pickpocket Systems

1. **Start simple** - Use basic success/failure with drop lists
2. **Test thoroughly** - Pickpocket mechanics are complex
3. **Balance difficulty** - Make it challenging but rewarding
4. **Visual feedback** - Clear success/failure indicators
5. **Sound design** - Distinct sounds for success/failure
6. **NPC reactions** - Make NPCs react appropriately
7. **Progression** - Allow pickpocket skill to improve over time

---

**Previous:** [Creating Spells](07_Spells.md) | **Next:** [Creating UI](09_UI.md)
