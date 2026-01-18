# Raycast Targeting

Learn how to use raycast targeting for spells, interactions, and line-of-sight-based targeting.

## Overview

Raycast targeting fires a ray from the player's camera/view in the direction they're looking, detecting the first entity or block it hits. Raycasts are ideal for spells, ranged interactions, and precise targeting systems. They follow the player's view direction and can target blocks or entities within range.

## Location
Used in `Type: "Selector"` interactions with `"Id": "Raycast"`.

## Basic Raycast Structure

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": {
      "Y": 1.6
    },
    "Distance": 5
  },
  "HitEntity": {
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "Root",
        "Duration": 5.0
      }
    ]
  },
  "HitBlock": {
    "Interactions": [
      {
        "Type": "SendMessage",
        "Key": "server.spells.hit.block"
      }
    ]
  }
}
```

## Raycast Properties

### Id

```json
{
  "Id": "Raycast"
}
```

**Required.** Must be `"Raycast"` to use raycast targeting.

### Distance

```json
{
  "Distance": 5
}
```

Maximum range of the raycast in blocks. Ray stops at first hit or reaches this distance.

### Offset

```json
{
  "Offset": {
    "X": 0,
    "Y": 1.6,
    "Z": 0
  }
}
```

Position offset from entity origin where raycast starts:
- **`X`** - Left/right offset (blocks, negative = left, positive = right)
- **`Y`** - Up/down offset (blocks, negative = down, positive = up)
  - **Typical:** `1.6` for eye-level (player view height)
- **`Z`** - Forward/back offset (blocks, negative = forward, positive = back)

### TargetType

```json
{
  "TargetType": "Entity"
}
```

Optional filter for what can be hit:
- **`"Entity"`** - Only hits entities
- **`"Block"`** - Only hits blocks
- Omit for both entities and blocks

### BlockTag

```json
{
  "BlockTag": "Type=Cloth"
}
```

Optional tag filter for blocks. Only blocks matching this tag can be hit. Uses tag pattern matching.

## Hit Handlers

### HitEntity

```json
{
  "HitEntity": {
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "Root",
        "Entity": "Target",
        "Duration": 5.0
      }
    ]
  }
}
```

Interactions executed when an entity is hit by the raycast.

**Entity Reference:**
- Use `"Entity": "Target"` to reference the hit entity
- Omit `Entity` for self (spell caster)

### HitBlock

```json
{
  "HitBlock": {
    "Interactions": [
      {
        "Type": "SendMessage",
        "Key": "server.spells.hit.block"
      }
    ]
  }
}
```

Interactions executed when a block is hit by the raycast.

### HitEntityRules

```json
{
  "HitEntityRules": [
    {
      "Matchers": [
        { "Type": "NPC" },
        { "Type": "Vulnerable" }
      ],
      "Next": {
        "Interactions": [
          {
            "Type": "DamageEntity",
            "Entity": "Target",
            "DamageCalculator": {
              "BaseDamage": {
                "Physical": 15
              }
            }
          }
        ]
      }
    }
  ]
}
```

Array of conditional rules with matchers. Each rule checks if the hit entity matches the criteria and executes different interactions.

**Matcher Types:**
- **`"Type": "NPC"`** - Target is an NPC
- **`"Type": "Player"`** - Target is a player
- **`"Type": "Vulnerable"`** - Target can take damage
- **`"Invert": true`** - Invert the matcher (exclude this type)

### Failed / FailOn

```json
{
  "FailOn": "Entity",
  "Failed": {
    "Type": "ApplyEffect",
    "EffectId": "Stoneskin",
    "Entity": "User"
  }
}
```

**`FailOn`** - When to trigger Failed:
- **`"Entity"`** - Fails if entity is hit
- **`"Block"`** - Fails if block is hit
- Other conditions may exist

**`Failed`** - Interactions executed when raycast fails (no hit or FailOn condition).

## Complete Examples

### Example 1: Basic Spell Raycast

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": {
      "Y": 1.6
    },
    "Distance": 5
  },
  "HitEntity": {
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "Root",
        "Entity": "Target",
        "Duration": 5.0
      }
    ]
  },
  "HitBlock": {
    "Interactions": [
      {
        "Type": "SendMessage",
        "Key": "server.spells.hit.block"
      }
    ]
  }
}
```

Root spell that roots entities or shows message if block is hit.

### Example 2: Spell with Conditional Targeting

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": {
      "Y": 1.6
    },
    "Distance": 10
  },
  "HitEntityRules": [
    {
      "Matchers": [
        { "Type": "NPC" },
        { "Type": "Vulnerable" }
      ],
      "Next": {
        "Interactions": [
          {
            "Type": "ApplyEffect",
            "EffectId": "Root",
            "Entity": "Target",
            "Duration": 5.0
          },
          {
            "Type": "DamageEntity",
            "Entity": "Target",
            "DamageCalculator": {
              "BaseDamage": {
                "Physical": 20
              }
            }
          }
        ]
      }
    },
    {
      "Matchers": [
        { "Type": "Player" }
      ],
      "Next": {
        "Interactions": [
          {
            "Type": "ApplyEffect",
            "EffectId": "Heal_Over_Time",
            "Entity": "Target",
            "Duration": 10.0
          }
        ]
      }
    }
  ]
}
```

Roots and damages NPCs, heals players.

### Example 3: Player-Only Targeting

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": {
      "Y": 1.6
    },
    "Distance": 5
  },
  "HitEntityRules": [
    {
      "Matchers": [
        { "Type": "Player" }
      ],
      "Next": {
        "Interactions": [
          {
            "Type": "SendMessage",
            "Message": "Matched Player"
          },
          {
            "Type": "ApplyEffect",
            "EffectId": "Red_Flash",
            "Entity": "Target"
          }
        ]
      }
    },
    {
      "Matchers": [
        { "Type": "Player", "Invert": true }
      ],
      "Next": {
        "Interactions": [
          {
            "Type": "SendMessage",
            "Message": "Matched non-Player"
          }
        ]
      }
    }
  ]
}
```

Different interactions for players vs NPCs.

### Example 4: Self-Target with Fallback

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": {
      "Y": 1.6
    },
    "Distance": 10
  },
  "HitEntityRules": [
    {
      "Matchers": [
        { "Type": "Vulnerable" }
      ],
      "Next": {
        "Interactions": [
          {
            "Type": "EffectCondition",
            "Entity": "Target",
            "EntityEffectIds": ["Immune"],
            "Match": "None",
            "Next": {
              "Type": "Serial",
              "Interactions": [
                {
                  "Type": "ChangeStat",
                  "Entity": "Target",
                  "StatModifiers": {
                    "Immunity": 25
                  }
                },
                {
                  "Type": "ApplyEffect",
                  "EffectId": "Root",
                  "Entity": "Target"
                }
              ]
            }
          }
        ]
      }
    }
  ],
  "FailOn": "Entity",
  "Failed": {
    "Type": "ApplyEffect",
    "EffectId": "Stoneskin",
    "Entity": "User"
  }
}
```

Applies effect to target if vulnerable and not immune. If no valid target, applies to self.

### Example 5: Block-Only Raycast

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": {
      "Y": 1.6
    },
    "Distance": 10,
    "TargetType": "Block",
    "BlockTag": "Type=Cloth"
  },
  "HitBlock": {
    "Interactions": [
      {
        "Type": "ChangeBlock",
        "Changes": {
          "Cloth_Block_Wool_Blue": "Cloth_Block_Wool_Red",
          "Cloth_Block_Wool_Red": "Cloth_Block_Wool_Blue"
        }
      }
    ]
  }
}
```

Only targets cloth blocks, changes their color.

### Example 6: Entity-Only Raycast

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": {
      "Y": 1.6
    },
    "Distance": 10,
    "TargetType": "Entity"
  },
  "HitEntity": {
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "Freeze",
        "Entity": "Target",
        "Duration": 3.0
      },
      {
        "Type": "DamageEntity",
        "Entity": "Target",
        "DamageCalculator": {
          "BaseDamage": {
            "Ice": 15
          }
        }
      }
    ]
  }
}
```

Only targets entities, applies freeze and ice damage.

### Example 7: Damage Raycast

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": {
      "Y": 1.6
    },
    "Distance": 5
  },
  "HitEntity": {
    "Interactions": [
      {
        "Type": "SendMessage",
        "Message": "Hit entity"
      },
      {
        "Type": "ApplyEffect",
        "Entity": "Target",
        "EffectId": "Red_Flash"
      },
      {
        "Type": "DamageEntity",
        "Entity": "Target",
        "DamageCalculator": {
          "BaseDamage": {
            "Physical": 6
          }
        },
        "DamageEffects": {
          "Knockback": {
            "Type": "Point",
            "Force": 6.5,
            "VelocityType": "Set",
            "VelocityConfig": {
              "AirResistance": 0.97,
              "GroundResistance": 0.94
            }
          },
          "WorldSoundEventId": "SFX_Sword_T2_Impact",
          "WorldParticles": [
            {
              "SystemId": "Impact_Sword_Basic"
            }
          ]
        }
      }
    ]
  }
}
```

Damages entity with knockback, sound, and particles.

## Raycast with ApplyForce

Some interactions use raycast properties directly in `ApplyForce`:

```json
{
  "Type": "ApplyForce",
  "RaycastHeightOffset": 0.5,
  "RaycastDistance": 3,
  "Direction": {
    "X": 0,
    "Y": 2,
    "Z": -3
  },
  "Force": 15.0,
  "WaitForGround": true,
  "WaitForCollision": true
}
```

**Raycast Properties:**
- **`RaycastHeightOffset`** - Vertical offset for ground detection
- **`RaycastDistance`** - Distance for collision detection

Used for movement abilities that check for ground/collision.

## Matchers Reference

### Common Matchers

| Matcher | Description | Example |
|---------|-------------|---------|
| `"Type": "NPC"` | Target is an NPC | `{ "Type": "NPC" }` |
| `"Type": "Player"` | Target is a player | `{ "Type": "Player" }` |
| `"Type": "Vulnerable"` | Target can take damage | `{ "Type": "Vulnerable" }` |
| `"Type": "Player", "Invert": true` | Target is NOT a player | `{ "Type": "Player", "Invert": true }` |

### Combining Matchers

All matchers in a rule must pass (AND logic):

```json
{
  "Matchers": [
    { "Type": "NPC" },
    { "Type": "Vulnerable" }
  ]
}
```

Target must be BOTH an NPC AND vulnerable.

### Invert Matcher

```json
{
  "Matchers": [
    {
      "Type": "Player",
      "Invert": true
    }
  ]
}
```

Target must NOT be a player (targets NPCs only).

## Common Raycast Patterns

### Pattern 1: Heal Allies, Damage Enemies

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": { "Y": 1.6 },
    "Distance": 10
  },
  "HitEntityRules": [
    {
      "Matchers": [
        { "Type": "NPC" },
        { "Type": "Vulnerable" }
      ],
      "Next": {
        "Interactions": [
          {
            "Type": "DamageEntity",
            "Entity": "Target",
            "DamageCalculator": {
              "BaseDamage": {
                "Fire": 20
              }
            }
          }
        ]
      }
    },
    {
      "Matchers": [
        { "Type": "Player" }
      ],
      "Next": {
        "Interactions": [
          {
            "Type": "Heal",
            "Entity": "Target",
            "Amount": 50
          }
        ]
      }
    }
  ]
}
```

Damages NPCs, heals players.

### Pattern 2: Buff Self if No Target

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": { "Y": 1.6 },
    "Distance": 10
  },
  "HitEntityRules": [
    {
      "Matchers": [
        { "Type": "Vulnerable" }
      ],
      "Next": {
        "Interactions": [
          {
            "Type": "ApplyEffect",
            "EffectId": "Root",
            "Entity": "Target",
            "Duration": 5.0
          }
        ]
      }
    }
  ],
  "FailOn": "Entity",
  "Failed": {
    "Type": "ApplyEffect",
    "EffectId": "Stoneskin",
    "Entity": "User"
  }
}
```

Roots target if found, buffs self if no valid target.

### Pattern 3: Conditional Block Interaction

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": { "Y": 1.6 },
    "Distance": 5,
    "BlockTag": "Type=Cloth"
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
```

Only affects cloth blocks.

## Tips for Raycast Targeting

1. **Offset Y** - Use `1.6` for eye-level (player view height)
2. **Distance** - Set appropriate ranges (5-10 for spells, 10-20 for long-range)
3. **TargetType** - Use to filter entities vs blocks if needed
4. **BlockTag** - Use tag patterns to filter specific block types
5. **HitEntityRules** - Use for conditional targeting (different effects for NPCs/players)
6. **Entity reference** - Always use `"Entity": "Target"` for hit entity
7. **Failed fallback** - Use `Failed` for self-targeting or error handling
8. **Line of sight** - Raycast automatically checks line of sight (no walls)
9. **First hit** - Raycast stops at first entity/block hit
10. **Matchers** - Combine matchers for complex targeting logic

## Distance Guidelines

| Use Case | Distance | Example |
|----------|----------|---------|
| Melee spells | 2-5 | Close-range abilities |
| Standard spells | 5-10 | Most spells |
| Long-range spells | 10-20 | Sniping abilities |
| Interaction tools | 5-10 | Building/placing tools |

## Common Use Cases

### Spells
- Healing spells (target allies)
- Damage spells (target enemies)
- Buff/debuff spells (target entities)
- Utility spells (target blocks/entities)

### Tools
- Building tools (target blocks)
- Interaction tools (target entities/blocks)
- Mining tools (target blocks)
- Targeting tools (select entities)

### Abilities
- Teleportation (target position)
- Summoning (target location)
- Buffing (target allies)
- Debuffing (target enemies)

---

**Previous:** [Min/Max Ranges](157_Min_Max_Ranges.md) | **Next:** Back to [README](../README.md)
