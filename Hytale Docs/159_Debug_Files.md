# Debug Files

Learn about debug files and test items used for development, testing, and debugging game mechanics.

## Overview

Debug files are special items, interactions, blocks, and configurations used during development to test and debug game mechanics. They're located in `_Debug/` directories throughout the Server folder and use the `"Debug"` or `"Developer"` quality tier.

## Example from Game Files

Debug files are special items and configurations used for development and testing. They use the "Debug" or "Developer" quality tier and are located in `_Debug/` subdirectories throughout the Server folder.

## Location

Debug files are organized in several locations:

- **Debug Items**: `Server/Item/Items/_Debug/`
- **Debug Weapons**: `Server/Item/Items/Weapon/_Debug/`
- **Debug Armor**: `Server/Item/Items/Armor/_Debug/`
- **Debug Interactions**: `Server/Item/Interactions/_Debug/`
- **Debug Blocks**: `Server/Item/Block/Blocks/_Debug/`
- **Debug Projectile Configs**: `Server/ProjectileConfigs/_Debug/`
- **Debug Quality**: `Server/Item/Qualities/Debug.json`
- **Debug Animations**: `Server/Item/Animations/_Debug/`

## Debug Quality

All debug items use either `"Quality": "Debug"` or `"Quality": "Developer"`:

### Debug Quality Definition

`Server/Item/Qualities/Debug.json`:

```json
{
  "QualityValue": 10,
  "ItemTooltipTexture": "UI/ItemQualities/Tooltips/ItemTooltipCommon.png",
  "ItemTooltipArrowTexture": "UI/ItemQualities/Tooltips/ItemTooltipCommonArrow.png",
  "SlotTexture": "UI/ItemQualities/Slots/SlotDeveloper.png",
  "BlockSlotTexture": "UI/ItemQualities/Slots/SlotDeveloper.png",
  "SpecialSlotTexture": "UI/ItemQualities/Slots/SlotDeveloper.png",
  "TextColor": "#ce1624",
  "LocalizationKey": "server.general.qualities.Debug",
  "VisibleQualityLabel": true,
  "RenderSpecialSlot": true,
  "HideFromSearch": true
}
```

**Properties:**
- **`QualityValue`** - Quality tier value (lower = better)
- **`TextColor`** - Color for quality label (#ce1624 = red)
- **`HideFromSearch`** - If `true`, hides item from search results
- **`VisibleQualityLabel`** - Shows quality label on item tooltip

## Debug Item Types

### Debug Raycast Item

Tests raycast targeting and entity/block detection:

`Server/Item/Items/_Debug/Debug_Raycast.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Debug_Raycast.name"
  },
  "Quality": "Debug",
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "Selector",
          "Selector": {
            "Id": "Raycast",
            "Offset": { "Y": 1.6 },
            "Distance": 5,
            "BlockTag": "Type=Cloth"
          },
          "HitEntityRules": [
            {
              "Matchers": [
                { "Type": "Vulnerable" },
                { "Type": "Player" }
              ],
              "Next": {
                "Interactions": [
                  {
                    "Type": "SendMessage",
                    "Message": "Matched Player"
                  }
                ]
              }
            }
          ],
          "HitEntity": {
            "Interactions": [
              {
                "Type": "DamageEntity",
                "DamageCalculator": {
                  "BaseDamage": { "Physical": 6 }
                }
              }
            ]
          },
          "HitBlock": {
            "Interactions": [
              {
                "Type": "ChangeBlock",
                "Changes": {
                  "Cloth_Block_Wool_Blue": "Cloth_Block_Wool_Red"
                }
              }
            ]
          }
        }
      ]
    }
  },
  "Tags": {
    "Type": ["Debug"]
  }
}
```

**Purpose:** Tests raycast selectors, entity matchers, block detection, and interaction chains.

### Debug Combo Item

Tests chaining interactions and combo mechanics:

`Server/Item/Items/_Debug/Debug_Combo.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Debug_Combo.name"
  },
  "Quality": "Debug",
  "PlayerAnimationsId": "Combo_Test",
  "Interactions": {
    "Primary": {
      "ClickQueuingTimeout": 0.2,
      "Interactions": ["Debug_Combo_Primary"]
    },
    "Secondary": {
      "ClickQueuingTimeout": 0.2,
      "Interactions": ["Debug_Combo_Secondary"]
    }
  },
  "Tags": {
    "Type": ["Debug"]
  }
}
```

**Combo Interaction:**

`Server/Item/Interactions/_Debug/Debug_Combo_Primary.json`:

```json
{
  "Type": "Chaining",
  "ChainId": "Debug_Combo",
  "ChainingAllowance": 0.8,
  "Next": [
    {
      "Type": "SendMessage",
      "Message": "First - Primary",
      "RunTime": 0.5,
      "Effects": {
        "ItemAnimationId": "Swing_Right"
      }
    },
    {
      "Type": "FirstClick",
      "Click": {
        "Type": "SendMessage",
        "Message": "Second click - Primary"
      },
      "Held": {
        "Type": "SendMessage",
        "Message": "Second held - Primary"
      }
    }
  ],
  "Flags": {
    "Special_Second": {
      "Type": "SendMessage",
      "Message": "Flag hit!"
    }
  }
}
```

**Purpose:** Tests chaining interactions, click queuing, combo mechanics, and chain flags.

### Debug Continue Item

Tests continuous interactions and sequence mechanics:

`Server/Item/Items/_Debug/Debug_Continue.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Debug_Continue.name"
  },
  "Quality": "Debug",
  "Interactions": {
    "Held": {
      "Interactions": [
        {
          "Type": "ApplyForce",
          "RunTime": 1.5,
          "Direction": { "Z": -1 },
          "Force": 15,
          "WaitForGround": false,
          "VelocityConfig": {
            "AirResistance": 0.96,
            "GroundResistance": 0.96
          }
        }
      ]
    },
    "HeldOffhand": {
      "Interactions": [
        {
          "Type": "ApplyForce",
          "RunTime": 1.5,
          "Direction": { "Y": 1 },
          "Force": 15
        }
      ]
    }
  },
  "Tags": {
    "Type": ["Debug"]
  }
}
```

**Purpose:** Tests continuous interactions (`Held`, `HeldOffhand`), force application, and sequence mechanics.

### Debug Forking Item

Tests parallel interactions and forking mechanics:

`Server/Item/Items/_Debug/Debug_Forking.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Debug_Forking.name"
  },
  "Quality": "Debug",
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "SendMessage",
          "Message": "Interaction start",
          "Next": {
            "Type": "Repeat",
            "ForkInteractions": {
              "Interactions": [
                {
                  "Type": "SendMessage",
                  "Message": "> Fork started"
                },
                {
                  "Type": "Charging",
                  "Next": {
                    "0": {
                      "Type": "SendMessage",
                      "Message": "> Short"
                    },
                    "1.5": {
                      "Type": "SendMessage",
                      "Message": "> Long"
                    }
                  }
                }
              ]
            },
            "Next": {
              "Type": "SendMessage",
              "Message": "Fork (charge) complete"
            }
          }
        }
      ]
    }
  },
  "Tags": {
    "Type": ["Debug"]
  }
}
```

**Purpose:** Tests parallel execution (`ForkInteractions`), repeat mechanics, and interaction branching.

### Debug Projectile Item

Tests projectile spawning and configuration:

`Server/Item/Items/_Debug/Debug_Projectile.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Debug_Projectile.name"
  },
  "Quality": "Debug",
  "Interactions": {
    "Primary": {
      "Interactions": [
        "Debug_Projectile",
        {
          "Type": "Simple",
          "RunTime": 0.2
        }
      ],
      "Cooldown": {
        "Id": "Debug_Projectile",
        "Cooldown": 0.2,
        "ClickBypass": false
      }
    }
  },
  "Tags": {
    "Type": ["Debug"]
  }
}
```

**Projectile Interaction:**

`Server/Item/Interactions/_Debug/Debug_Projectile.json`:

```json
{
  "Type": "Projectile",
  "Config": "Projectile_Config_Debug"
}
```

**Debug Projectile Config:**

`Server/ProjectileConfigs/_Debug/Projectile_Config_Debug.json`:

```json
{
  "Model": "Arrow_Crude",
  "Physics": {
    "Type": "Standard",
    "Gravity": 15,
    "TerminalVelocityAir": 50,
    "Bounciness": 0,
    "SticksVertically": true
  },
  "LaunchForce": 25,
  "Interactions": {
    "ProjectileHit": {
      "Interactions": [
        {
          "Type": "DamageEntity",
          "TargetedDamage": {
            "Head": {
              "DamageCalculator": {
                "BaseDamage": { "Physical": 20 }
              }
            }
          },
          "DamageCalculator": {
            "BaseDamage": { "Physical": 6 }
          }
        }
      ]
    }
  }
}
```

**Purpose:** Tests projectile spawning, physics, hit detection, targeted damage (headshots), and projectile interactions.

### Debug Block

Tests block interactions and state changes:

`Server/Item/Items/_Debug/Debug_Block.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Debug_Block.name"
  },
  "Quality": "Developer",
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Cube",
    "VariantRotation": "Debug",
    "Textures": [
      {
        "Weight": 1,
        "Up": "BlockTextures/_Debug/Up.png",
        "Down": "BlockTextures/_Debug/Down.png",
        "North": "BlockTextures/_Debug/North.png",
        "South": "BlockTextures/_Debug/South.png",
        "East": "BlockTextures/_Debug/East.png",
        "West": "BlockTextures/_Debug/West.png"
      }
    ],
    "ParticleColor": "#0000f4"
  },
  "Tags": {
    "Type": ["Debug"]
  }
}
```

**Block Entity:**

`Server/Item/Block/Blocks/_Debug/Debug_Test_Block.json`:

```json
{
  "Textures": [
    {
      "All": "Blocks/_Debug/Texture.png"
    }
  ],
  "Material": "Solid",
  "Light": {
    "Color": "#f0f"
  }
}
```

**Purpose:** Tests block placement, textures, rotation variants, and block entity components.

### Debug Weapon Sticks

Simple weapons for testing combat mechanics:

**Basic Stick:**

`Server/Item/Items/Weapon/_Debug/Debug_Stick.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Debug_Stick.name"
  },
  "Quality": "Developer",
  "PlayerAnimationsId": "Stick",
  "Interactions": {
    "Primary": "Stick_Attack",
    "Secondary": "Stick_Block"
  },
  "InteractionVars": {
    "Stick_Swing_Left_Fast_Damage": {
      "Interactions": [
        {
          "Parent": "Stick_Swing_Left_Fast_Damage"
        }
      ]
    }
  },
  "Tags": {
    "Type": ["Weapon"],
    "Family": ["Staff"]
  },
  "Weapon": {}
}
```

**Parry Stick:**

`Server/Item/Items/Weapon/_Debug/Debug_Stick_Parry.json`:

```json
{
  "Quality": "Developer",
  "Interactions": {
    "Primary": "Stick_Attack",
    "Secondary": {
      "Cooldown": {
        "Id": "Debug_Stick_Parry"
      },
      "Interactions": ["Debug_Stick_Parry"]
    }
  }
}
```

**Other Debug Sticks:**
- **`Debug_Stick_Delay`** - Tests delayed charging mechanics
- **`Debug_Stick_Slow`** - Tests slow/freeze effects
- **`Debug_Stick_Stun`** - Tests stun mechanics

**Purpose:** Tests weapon interactions, blocking, parrying, status effects, and combat mechanics.

### Debug Armor

Tests armor stat modifiers and resistance mechanics:

`Server/Item/Items/Armor/_Debug/Debug_Armor_D_Chest_Of_Damage_Reduction.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Debug_Armor_D_Chest_Of_Damage_Reduction.name"
  },
  "Quality": "Debug",
  "Armor": {
    "ArmorSlot": "Chest",
    "BaseDamageResistance": 8,
    "DamageResistance": {
      "Physical": [
        {
          "Amount": 0.5,
          "CalculationType": "Multiplicative"
        }
      ],
      "Projectile": [
        {
          "Amount": 5,
          "CalculationType": "Additive"
        }
      ]
    },
    "StatModifiers": {
      "Health": [
        {
          "Amount": 6,
          "CalculationType": "Additive"
        }
      ]
    }
  }
}
```

**Debug Armor Types:**
- **`Debug_Armor_D_Chest_Of_Damage_Reduction`** - Damage reduction testing
- **`Debug_Armor_O_Chest_Of_Damage_Enhancement`** - Damage enhancement testing
- **`Debug_Armor_D_Legs_Of_Knockback_Reduction`** - Knockback reduction
- **`Debug_Armor_O_Legs_Of_Knockback_Enhancement`** - Knockback enhancement
- **`Debug_Armor_D_Head_Of_Dash_Reduction`** - Dash cost reduction
- **`Debug_Armor_O_Head_Of_Dash_Exhaustion`** - Dash cost increase
- **`Debug_Armor_D_Hands_Of_Regeneration`** - Health regeneration

**Purpose:** Tests armor stat modifiers, damage resistance calculations (multiplicative vs additive), and stat effects.

### Debug Continue Equip (Hands)

Tests equipment-based continuous interactions on hands slot:

`Server/Item/Items/_Debug/Debug_Continue_Equip.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Debug_Continue_Equip.name"
  },
  "Quality": "Debug",
  "Categories": ["Items.Armors"],
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "EquipItem"
        }
      ]
    },
    "Equipped": {
      "Interactions": [
        {
          "Type": "ApplyForce",
          "RunTime": 1.5,
          "Direction": { "Y": 1 },
          "Force": 15,
          "WaitForGround": false
        }
      ]
    }
  },
  "Armor": {
    "ArmorSlot": "Hands"
  }
}
```

**Purpose:** Tests equipment-triggered interactions (`Equipped`, `Unequipped`), continuous effects from equipped items.

### Debug Continue Equip (Legs)

Tests equipment-based continuous interactions on legs slot with custom animations:

`Server/Item/Items/_Debug/Debug_Continue_Equip_Legs.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Debug_Continue_Equip_Legs.name"
  },
  "Quality": "Debug",
  "Categories": ["Items.Armors"],
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "EquipItem"
        }
      ]
    },
    "Equipped": {
      "Interactions": [
        {
          "Type": "Simple",
          "Effects": {
            "ItemPlayerAnimationsId": {
              "Parent": "Block",
              "Animations": {
                "Dance": {
                  "FirstPerson": "Characters/Animations/Items/Dual_Handed/Block/Idle_FPS.blockyanim",
                  "ThirdPerson": "Characters/Animations/Items/Dual_Handed/Block/Idle.blockyanim",
                  "Speed": 0.5,
                  "Looping": true
                }
              }
            },
            "ItemAnimationId": "Dance"
          },
          "RunTime": 5
        }
      ],
      "Tags": {
        "Allows": ["Movement"]
      }
    }
  },
  "Armor": {
    "ArmorSlot": "Legs"
  }
}
```

**Purpose:** Tests equipped interactions on legs slot, custom animation sets, and animation looping while allowing movement.

### Debug Spawn Marker Block

Tests spawn point mechanics and triggers:

`Server/Item/Items/_Debug/Debug_Spawn_Marker_Block.json`:

Tests spawn marker placement and NPC/entity spawning.

### Test Spawn Marker Trigger

Tests spawn marker trigger interactions:

`Server/Item/Items/_Debug/Test_Spawn_Marker_Trigger.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Test_Spawn_Marker_Trigger.name"
  },
  "Quality": "Developer",
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "TriggerSpawnMarkers",
          "Range": 30,
          "Effects": {
            "ItemAnimationId": "Block"
          }
        }
      ]
    }
  },
  "Tags": {
    "Type": ["Weapon"]
  }
}
```

**Purpose:** Tests `TriggerSpawnMarkers` interaction for triggering spawn markers within range.

### Portal Prefab

Tests portal and prefab systems:

`Server/Item/Items/_Debug/Portal_Prefab.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Portal_Prefab.name"
  },
  "Quality": "Developer",
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Model",
    "VariantRotation": "NESW",
    "CustomModel": "Blocks/Decorative_Sets/Temple_Dark/Statue_Owl.blockymodel",
    "HitboxType": "Statue_Small",
    "Opacity": "Transparent",
    "Flags": {
      "IsUsable": true
    }
  }
}
```

**Purpose:** Tests portal block placement, model rendering, and usable block flags.

## Test Items

### Test Camera Item

Tests camera perspective changes:

`Server/Item/Items/_Debug/Test_Camera_Item.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Test_Camera_Item.name"
  },
  "Quality": "Developer",
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "Simple",
          "Effects": {
            "ItemAnimationId": "SwingLeft"
          },
          "Next": {
            "Type": "Camera",
            "Action": "ForcePerspective",
            "Perspective": "Third",
            "CameraInteractionTime": 5
          }
        }
      ]
    }
  }
}
```

**Purpose:** Tests camera perspective switching (first-person to third-person).

### Test Backpack

Tests armor/equipment systems:

`Server/Item/Items/_Debug/Test_Backpack.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Test_Backpack.name"
  },
  "Quality": "Developer",
  "Categories": ["Items.Armors"],
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "EquipItem"
        }
      ]
    }
  },
  "Tags": {
    "Type": ["Armor"]
  }
}
```

**Purpose:** Tests armor equipment, model attachments, and inventory systems.

## Using Debug Files

### Purpose

Debug files serve multiple purposes:

1. **Testing Mechanics** - Test specific game mechanics in isolation
2. **Development Tools** - Quick access to test features during development
3. **Examples** - Reference implementations for complex interaction patterns
4. **Debugging** - Identify issues with game systems
5. **Learning** - Understand how complex mechanics work

### Common Patterns

**1. SendMessage for Testing**

Debug items often use `SendMessage` to show what's happening:

```json
{
  "Type": "SendMessage",
  "Message": "Interaction triggered"
}
```

**2. Visual Feedback**

Debug items use visual effects to show activation:

```json
{
  "Type": "ApplyEffect",
  "EffectId": {
    "Duration": 0.1,
    "ApplicationEffects": {
      "EntityTopTint": "#FF0000"
    }
  }
}
```

**3. Quality: Debug**

All debug items use:

```json
{
  "Quality": "Debug"  // or "Developer"
}
```

**4. Tags**

Debug items are tagged for easy identification:

```json
{
  "Tags": {
    "Type": ["Debug"]
  }
}
```

## Tips for Using Debug Files

1. **Reference Examples** - Use debug files as examples for complex mechanics
2. **Test Interactions** - Use debug items to test your own interactions
3. **Learning Tool** - Study debug files to understand advanced patterns
4. **Development** - Create your own debug items for testing specific mechanics
5. **Don't Ship** - Debug items are for development only - don't include them in releases

## Common Debug Item Locations

| Item | Location | Purpose |
|------|----------|---------|
| `Debug_Raycast` | `Server/Item/Items/_Debug/` | Tests raycast targeting |
| `Debug_Combo` | `Server/Item/Items/_Debug/` | Tests combo/chaining interactions |
| `Debug_Continue` | `Server/Item/Items/_Debug/` | Tests continuous interactions |
| `Debug_Forking` | `Server/Item/Items/_Debug/` | Tests parallel/forking interactions |
| `Debug_Projectile` | `Server/Item/Items/_Debug/` | Tests projectile mechanics |
| `Debug_Block` | `Server/Item/Items/_Debug/` | Tests block interactions |
| `Debug_Stick` | `Server/Item/Items/Weapon/_Debug/` | Basic weapon testing |
| `Debug_Armor_*` | `Server/Item/Items/Armor/_Debug/` | Tests armor stat modifiers |

---

**Previous:** [Raycast Targeting](158_Raycast_Targeting.md) | **Next:** [World Configuration](160_World_Configuration.md)
