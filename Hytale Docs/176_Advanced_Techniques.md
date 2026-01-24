# Advanced Techniques

Master advanced modding patterns, complex interactions, and optimization techniques used throughout Hytale's game systems.

## Overview

This guide covers advanced techniques for creating sophisticated mod behavior:
- Complex interaction chains (Parallel, Serial, Chaining)
- Conditional logic patterns
- Advanced recipe systems
- Dynamic content with InteractionVars
- State management patterns
- Performance optimization

All examples are from actual game files.

---

## Complex Interaction Chains

### Parallel Interactions

Execute multiple interactions simultaneously. From `Weapon_Staff_Crystal_Ice.json`:

```json
{
  "Type": "Parallel",
  "Interactions": [
    {
      "Interactions": [
        {
          "Type": "ChangeStat",
          "StatModifiers": {
            "Stamina": -5
          }
        }
      ]
    },
    {
      "Interactions": [
        {
          "Type": "ChangeStat",
          "Behaviour": "Set",
          "StatModifiers": {
            "StaminaRegenDelay": -1.5
          }
        }
      ]
    },
    {
      "Interactions": [
        {
          "Type": "Projectile",
          "RunTime": 0.25,
          "Config": "Projectile_Config_Ice_Ball",
          "Effects": {
            "ItemAnimationId": "CastSummonCharged"
          }
        }
      ]
    }
  ]
}
```

**When used:**
- Ice staff consumes stamina
- Sets stamina regen delay
- Launches projectile
- **All happen at the same time**

**Use cases:**
- Apply multiple stat changes simultaneously
- Trigger visual effects + gameplay effects
- Synchronize sounds, particles, and mechanics

---

### Serial Interactions

Execute interactions in sequence (one after another). From `Weapon_Deployable_Healing_Totem.json`:

```json
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
```

**Execution order:**
1. Play throw animation (0.25 seconds)
2. **Wait for animation to finish**
3. Launch healing totem projectile

**Use cases:**
- Animation → action sequences
- Charging → release patterns
- Multi-step item behaviors

---

### Chained Effects with Clear Pattern

From `Potion_Health_Greater.json` - clear old effect before applying new:

```json
{
  "Type": "ClearEntityEffect",
  "EntityEffectId": "Potion_Health_Lesser_Regen",
  "Next": {
    "Type": "Serial",
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "Potion_Health_Instant_Greater"
      },
      {
        "Type": "ApplyEffect",
        "EffectId": "Potion_Health_Greater_Regen"
      }
    ]
  }
}
```

**Pattern:**
1. Clear lesser potion effect (if active)
2. Apply instant health boost
3. Apply health regeneration over time

**Use cases:**
- Upgrading effects (lesser → greater potions)
- Replacing old buffs with new ones
- Preventing effect stacking

---

## Advanced Conditional Patterns

### Success/Failure Branching

From `Weapon_Staff_Crystal_Ice.json` - check inventory before casting:

```json
{
  "Type": "ModifyInventory",
  "ItemToRemove": {
    "Id": "Ingredient_Ice_Essence",
    "Quantity": 1
  },
  "AdjustHeldItemDurability": -0.5,
  "Next": {
    "Type": "Parallel",
    "Interactions": [
      // ... cast spell effects ...
    ]
  },
  "Failed": "Staff_Cast_Fail"
}
```

**Logic:**
- **If** player has Ice Essence → Remove essence, consume durability, cast spell
- **Else** → Run "Staff_Cast_Fail" interaction (likely shows message)

**Use cases:**
- Resource-consuming abilities
- Item requirements for actions
- Conditional unlocks

---

### Multi-Condition Checks

Stack multiple conditions for complex logic:

```json
{
  "Type": "StatsCondition",
  "Stats": {
    "Mana": 50
  },
  "Next": {
    "Type": "EffectCondition",
    "HasEffect": "Buff_MagicBoost",
    "Next": {
      // Player has mana AND magic boost
      "Type": "LaunchProjectile",
      "Config": "Projectile_Empowered_Fireball"
    },
    "Failed": {
      // Player has mana but NO magic boost
      "Type": "LaunchProjectile",
      "Config": "Projectile_Normal_Fireball"
    }
  },
  "Failed": {
    "Type": "SendMessage",
    "Message": "Not enough mana!"
  }
}
```

**Decision tree:**
1. Check mana ≥ 50
   - **Yes** → Check for magic boost effect
     - **Has boost** → Launch empowered fireball
     - **No boost** → Launch normal fireball
   - **No mana** → Send failure message

---

## Dynamic Content with InteractionVars

### Reusable Interaction Templates

From `Weapon_Staff_Crystal_Ice.json` - define variables for customization:

```json
"InteractionVars": {
  "Staff_Cast_Summon_Cost": {
    "Interactions": [
      {
        "Parent": "Staff_Cast_Cost",
        "StatModifiers": {
          "Mana": 0
        }
      }
    ]
  },
  "Staff_Cast_Summon_StaminaCost": {
    "Interactions": [
      {
        "Type": "Simple",
        "RunTime": 0
      }
    ]
  },
  "Staff_Cast_Summon_Launch": {
    "Interactions": [
      {
        "Type": "ModifyInventory",
        "ItemToRemove": {
          "Id": "Ingredient_Ice_Essence",
          "Quantity": 1
        },
        "Next": {
          // ... launch projectile ...
        }
      }
    ]
  }
}
```

**Then reference by ID:**

```json
"Interactions": {
  "Primary": "Root_Ice_Staff_Primary_Wrapper",
  "Secondary": "Root_Ice_Staff_Primary_Entry"
}
```

**Benefits:**
- Reuse interaction logic across items
- Override specific parts (e.g., mana cost)
- Cleaner, more maintainable files
- Inherit from parent templates

**Pattern:**
1. Define base interaction in parent/template
2. Reference by ID string
3. Override specific properties in child items

---

## Advanced Recipe Systems

### Tiered Crafting Requirements

From `Potion_Health_Greater.json`:

```json
"Recipe": {
  "KnowledgeRequired": false,
  "Input": [
    {
      "ItemId": "Potion_Empty",
      "Quantity": 3
    },
    {
      "ItemId": "Potion_Health_Lesser",
      "Quantity": 1
    },
    {
      "ItemId": "Plant_Fruit_Berries_Red",
      "Quantity": 12
    },
    {
      "ItemId": "Plant_Crop_Health2",
      "Quantity": 3
    },
    {
      "ItemId": "Plant_Crop_Health3",
      "Quantity": 2
    }
  ],
  "BenchRequirement": [
    {
      "Id": "Alchemybench",
      "Type": "Crafting",
      "Categories": [
        "Alchemy_Potions"
      ],
      "RequiredTierLevel": 2
    }
  ],
  "OutputQuantity": 3,
  "TimeSeconds": 1
}
```

**Key features:**
- Requires lesser potion as ingredient (progression)
- Multiple plant ingredients (resource gathering)
- Tier 2 alchemy bench required (gated content)
- Batch output (3 potions from 1 recipe)

**Pattern for progression:**
- Crude → Bronze → Iron → Steel (generic); for **pickaxe** tiers post–Update 2 see [Tools](14_Tools.md) and [Patch Notes Update 2](Patch_Notes_Update_2.md) (Copper T2, Iron T4, Thorium T6).
- Lesser → Greater → Superior
- Tier 1 bench → Tier 2 bench → Tier 3 bench

---

### Multi-Stage Crafting

Create items that require pre-crafted components:

```json
// Stage 1: Craft base components
"Recipe": {
  "Input": [
    { "ItemId": "Ingredient_Bar_Iron", "Quantity": 5 },
    { "ItemId": "Wood_Oak_Plank", "Quantity": 2 }
  ],
  "BenchRequirement": [{ "Id": "Workbench" }]
}

// Stage 2: Combine components into final item
"Recipe": {
  "Input": [
    { "ItemId": "Component_Iron_Frame", "Quantity": 1 },
    { "ItemId": "Component_Mechanism", "Quantity": 1 },
    { "ItemId": "Rock_Gem_Ruby", "Quantity": 1 }
  ],
  "BenchRequirement": [
    {
      "Id": "Arcanebench",
      "RequiredTierLevel": 3
    }
  ]
}
```

**Forces players to:**
1. Gather raw materials
2. Craft intermediate components
3. Unlock advanced benches
4. Craft final powerful item

---

## State Management Patterns

### NPC State Transitions

From `Kweebec_Merchant.json`:

```json
"Instructions": [
  {
    "Sensor": { "Type": "State", "State": "Idle" },
    "Instructions": [
      {
        "Sensor": { "Type": "Player", "Range": 8 },
        "Actions": [
          {
            "Type": "PlayAnimation",
            "Slot": "Status",
            "Animation": "Wave"
          },
          {
            "Type": "State",
            "State": "Watching"
          }
        ]
      }
    ]
  },
  {
    "Sensor": { "Type": "State", "State": "Watching" },
    "Instructions": [
      {
        "Sensor": {
          "Type": "Not",
          "Sensor": { "Type": "Player", "Range": 12 }
        },
        "Actions": [
          {
            "Type": "State",
            "State": "Idle"
          }
        ]
      }
    ]
  }
]
```

**State machine:**
- **Idle** → Player approaches (8m) → Wave + switch to **Watching**
- **Watching** → Player leaves (12m) → Switch to **Idle**

**Benefits:**
- Clean separation of behaviors
- Prevents conflicting actions
- Easy to debug and extend

---

### Block State Transitions

Pattern for multi-state blocks:

```json
{
  "Type": "UseBlock",
  "Next": {
    "Type": "BlockCondition",
    "Property": "State",
    "Value": "Unlit",
    "Next": {
      "Type": "ChangeBlock",
      "Property": "State",
      "Value": "Lit"
    },
    "Failed": {
      "Type": "ChangeBlock",
      "Property": "State",
      "Value": "Unlit"
    }
  }
}
```

**Logic:**
- Click unlit torch → Light it
- Click lit torch → Extinguish it

---

## Performance Optimization

### Minimize Nested Interactions

**Bad** (deep nesting, hard to read):

```json
{
  "Type": "Condition",
  "Next": {
    "Type": "Parallel",
    "Interactions": [
      {
        "Interactions": [
          {
            "Type": "Serial",
            "Interactions": [
              {
                "Type": "ApplyEffect",
                "Next": {
                  // ... too deep ...
                }
              }
            ]
          }
        ]
      }
    ]
  }
}
```

**Good** (use InteractionVars):

```json
"InteractionVars": {
  "ApplyBuffs": {
    "Interactions": [
      { "Type": "ApplyEffect", "EffectId": "Buff1" },
      { "Type": "ApplyEffect", "EffectId": "Buff2" }
    ]
  },
  "LaunchAttack": {
    "Interactions": [
      { "Type": "LaunchProjectile", "Config": "Fireball" }
    ]
  }
},
"Interactions": {
  "Primary": {
    "Interactions": [
      {
        "Type": "Parallel",
        "Interactions": [
          { "Interactions": [{ "Parent": "ApplyBuffs" }] },
          { "Interactions": [{ "Parent": "LaunchAttack" }] }
        ]
      }
    ]
  }
}
```

---

### Batch Operations

**Bad** (multiple individual actions):

```json
{
  "Type": "Serial",
  "Interactions": [
    { "Type": "ChangeStat", "StatModifiers": { "Health": 10 } },
    { "Type": "ChangeStat", "StatModifiers": { "Mana": 20 } },
    { "Type": "ChangeStat", "StatModifiers": { "Stamina": 15 } }
  ]
}
```

**Good** (single action with multiple stats):

```json
{
  "Type": "ChangeStat",
  "StatModifiers": {
    "Health": 10,
    "Mana": 20,
    "Stamina": 15
  }
}
```

---

### Limit Particle Spam

**Bad** (particles every frame):

```json
{
  "Type": "ApplyEffect",
  "EffectId": "Trail_Every_Frame"  // 60+ particles/second
}
```

**Good** (controlled particle rate):

```json
{
  "Particles": [
    {
      "SystemId": "Magic_Trail",
      "TargetNodeName": "Hand",
      // Particle system itself controls spawn rate
    }
  ]
}
```

Use particle system configurations to control spawn rates, not interaction loops.

---

## Common Advanced Patterns

### Pattern 1: Combo Chains

Track player actions to unlock combo moves:

```json
{
  "Type": "FirstClick",
  "Next": {
    "Type": "DamageEntity",
    "Damage": 10,
    "DamageType": "Melee",
    "Next": "ComboStep2"
  }
}
```

```json
"InteractionVars": {
  "ComboStep2": {
    "Interactions": [
      {
        "Type": "FirstClick",
        "Next": {
          "Type": "DamageEntity",
          "Damage": 15,
          "Next": "ComboStep3"
        }
      }
    ]
  },
  "ComboStep3": {
    "Interactions": [
      {
        "Type": "FirstClick",
        "Next": {
          "Type": "DamageEntity",
          "Damage": 25,
          "DamageType": "MeleeFinisher"
        }
      }
    ]
  }
}
```

**Result:** Click 3 times within window for 10 → 15 → 25 damage combo.

---

### Pattern 2: Cooldown Management

From `Weapon_Deployable_Healing_Totem.json`:

```json
"Interactions": {
  "Primary": {
    "Interactions": [
      // ... deployment logic ...
    ],
    "Cooldown": {
      "Id": "DeployableUtility",
      "Cooldown": 10
    }
  }
}
```

**Shares cooldown across all deployable utility items** - prevents spam.

**Use shared cooldowns for:**
- Item families (all healing items share cooldown)
- Powerful abilities (prevent stacking ultimates)
- Resource management (global mana potion cooldown)

---

### Pattern 3: Resource Consumption with Feedback

```json
{
  "Type": "ModifyInventory",
  "ItemToRemove": { "Id": "Ingredient_Essence", "Quantity": 5 },
  "Next": {
    "Type": "Parallel",
    "Interactions": [
      {
        "Interactions": [
          {
            "Type": "SendMessage",
            "Message": "Spell cast! (-5 Essence)"
          }
        ]
      },
      {
        "Interactions": [
          { "Type": "LaunchProjectile", "Config": "Fireball" }
        ]
      }
    ]
  },
  "Failed": {
    "Type": "SendMessage",
    "Message": "Not enough essence!"
  }
}
```

**Always provide feedback:**
- Success message + effect
- Failure message when resources missing

---

### Pattern 4: Multi-Target Effects

Apply effects to multiple entities:

```json
{
  "Type": "ApplyEffect",
  "EffectId": "Heal_Area",
  "Selector": {
    "Type": "Area",
    "Range": 10,
    "EntityTypes": ["Player", "NPC"],
    "MaxCount": 5
  }
}
```

**Heals up to 5 players/NPCs within 10 blocks.**

---

## Debugging Advanced Interactions

### Add Debug Messages

During development:

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "SendMessage",
      "Message": "DEBUG: Checking mana..."
    },
    {
      "Type": "StatsCondition",
      "Stats": { "Mana": 50 },
      "Next": {
        "Type": "SendMessage",
        "Message": "DEBUG: Mana OK, casting spell"
      },
      "Failed": {
        "Type": "SendMessage",
        "Message": "DEBUG: Mana check failed"
      }
    }
  ]
}
```

Remove debug messages for production.

---

### Test Edge Cases

Always test:
- ✅ Resource exactly at threshold (Mana = 50 when cost is 50)
- ✅ Multiple rapid clicks (spam prevention)
- ✅ Conflicting effects (two buffs modifying same stat)
- ✅ Inventory full when giving items
- ✅ Player death mid-interaction
- ✅ Network lag scenarios (multiplayer)

---

## Best Practices Summary

1. **Use InteractionVars** for reusable logic
2. **Prefer Parallel** for simultaneous effects
3. **Use Serial** for animation → action sequences
4. **Always handle Failed** cases
5. **Batch stat changes** into single operations
6. **Share cooldowns** between related items
7. **Provide feedback** on success and failure
8. **Test edge cases** thoroughly
9. **Minimize nesting** depth (max 3-4 levels)
10. **Document complex logic** with `$Comment` fields

---

## Related Guides

- **[Interaction Types List](109_Interaction_Types_List.md)** - All available interaction types
- **[Interaction Chains](95_Interaction_Chains.md)** - Basic interaction chain patterns
- **[System Integration](177_System_Integration.md)** - How systems work together
- **[Item Properties Reference](175_Item_Properties_Reference.md)** - Complete property list

---

**Previous:** [Item Properties Reference](175_Item_Properties_Reference.md) | **Next:** [System Integration](177_System_Integration.md)
