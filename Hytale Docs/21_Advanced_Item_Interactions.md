# Advanced Item Interactions

Learn advanced patterns for creating complex item interactions, conditional logic, and interaction chains.

## Overview

Item interactions in Hytale support complex logic including conditions, branching, parallel execution, and state management. This guide covers advanced patterns beyond basic interactions.

## Location
`Server/Item/Interactions/`

## Interaction Types

### 1. Conditional Interactions

Interactions can branch based on conditions:

```json
{
  "Type": "Condition",
  "RequiredGameMode": "Adventure",
  "Next": {
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "MyEffect"
      }
    ]
  },
  "Failed": {
    "Interactions": [
      {
        "Type": "SendMessage",
        "Message": "Can only use in Adventure mode"
      }
    ]
  }
}
```

### 2. StatsCondition

Check and consume player stats:

```json
{
  "Type": "StatsCondition",
  "Costs": {
    "Mana": 100,
    "Stamina": 10
  },
  "Next": {
    "Interactions": [
      {
        "Type": "LaunchProjectile",
        "ProjectileId": "Fireball"
      }
    ]
  },
  "Failed": {
    "Interactions": [
      {
        "Type": "SendMessage",
        "Message": "Not enough mana!"
      }
    ]
  }
}
```

### 3. EffectCondition

Check if player has specific effects:

```json
{
  "Type": "EffectCondition",
  "EntityEffectIds": ["Stamina_Broken", "Poisoned"],
  "Match": "Any",  // "Any", "None", "All"
  "Next": {
    "Interactions": [
      {
        "Type": "RemoveEffect",
        "EffectId": "Poisoned"
      }
    ]
  }
}
```

### 4. Random Chance

Add randomness to interactions:

```json
{
  "Type": "Random",
  "Chance": 0.3,  // 30% chance
  "Next": {
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "CriticalHit"
      }
    ]
  },
  "Failed": {
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "NormalHit"
      }
    ]
  }
}
```

## Interaction Chains

### Serial Execution

Execute interactions in sequence:

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "ApplyForce",
      "Direction": { "X": 0, "Y": 2, "Z": 0 },
      "Force": 20
    },
    {
      "Type": "ChangeStat",
      "StatModifiers": {
        "Stamina": -2
      }
    },
    {
      "Type": "Simple",
      "RunTime": 1,
      "Effects": {
        "Particles": [
          {
            "SystemId": "Impact_Feathers_Black"
          }
        ]
      }
    }
  ]
}
```

### Parallel Execution

Execute multiple interactions simultaneously:

```json
{
  "Type": "Parallel",
  "Interactions": [
    {
      "Interactions": [
        {
          "Type": "PlayAnimation",
          "Animation": "Cast"
        }
      ]
    },
    {
      "Interactions": [
        {
          "Type": "LaunchProjectile",
          "ProjectileId": "Fireball"
        }
      ]
    },
    {
      "Interactions": [
        {
          "Type": "ChangeStat",
          "StatModifiers": {
            "Mana": -50
          }
        }
      ]
    }
  ]
}
```

## Selector (Targeting)

Use selectors to target entities or blocks:

### Raycast Selector

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": { "Y": 1.6 },
    "Distance": 5,
    "TargetType": "Entity"
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
            "Entity": "Target",
            "EffectId": "Root"
          }
        ]
      }
    }
  ],
  "HitBlock": {
    "Interactions": [
      {
        "Type": "SendMessage",
        "Message": "Hit a block"
      }
    ]
  }
}
```

### Selector Matchers

| Matcher | Description |
|---------|-------------|
| `"Type": "NPC"` | Target is an NPC |
| `"Type": "Vulnerable"` | Target can take damage |
| `"Type": "Attitude"` | Target has specific attitude |
| `"Type": "Player"` | Target is a player |

## Interaction Variables

Define reusable interaction variables in items:

```json
{
  "InteractionVars": {
    "MyCustom_Attack": {
      "Interactions": [
        {
          "Parent": "Weapon_Sword_Primary_Swing_Left_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 20
            }
          }
        }
      ]
    },
    "MyCustom_Cast": {
      "Interactions": [
        {
          "Type": "LaunchProjectile",
          "ProjectileId": "Fireball"
        }
      ]
    }
  }
}
```

Then reference in interactions:

```json
{
  "Interactions": {
    "Primary": "MyCustom_Attack"
  }
}
```

## Replace Pattern

Dynamically replace interaction variables:

```json
{
  "Type": "Replace",
  "Var": "MyCustom_Cast_Cost",
  "DefaultValue": {
    "Interactions": ["Spellbook_Cast_Cost"]
  }
}
```

## Complex Example: Conditional Spell

```json
{
  "Type": "StatsCondition",
  "Costs": {
    "Mana": 50
  },
  "Next": {
    "Type": "Selector",
    "Selector": {
      "Id": "Raycast",
      "Distance": 10
    },
    "HitEntityRules": [
      {
        "Matchers": [
          { "Type": "NPC" },
          { "Type": "Vulnerable" }
        ],
        "Next": {
          "Type": "Random",
          "Chance": 0.7,
          "Next": {
            "Type": "Serial",
            "Interactions": [
              {
                "Type": "ApplyEffect",
                "Entity": "Target",
                "EffectId": "Root"
              },
              {
                "Type": "ChangeStat",
                "Entity": "Target",
                "StatModifiers": {
                  "Health": -25
                }
              },
              {
                "Type": "ApplyEffect",
                "Entity": "Player",
                "EffectId": "Mana_Siphon"
              }
            ]
          },
          "Failed": {
            "Type": "ApplyEffect",
            "Entity": "Target",
            "EffectId": "Minor_Damage"
          }
        }
      }
    ],
    "HitBlock": {
      "Interactions": [
        {
          "Type": "SpawnParticles",
          "SystemId": "Explosion_Medium"
        }
      ]
    }
  },
  "Failed": {
    "Interactions": [
      {
        "Type": "SendMessage",
        "Message": "Not enough mana!"
      }
    ]
  }
}
```

This interaction:
1. Checks if player has 50 mana
2. Raycasts to find target
3. If NPC is vulnerable, 70% chance to:
   - Root target
   - Deal damage
   - Apply mana siphon to player
4. 30% chance for minor damage only
5. If hits block, spawn explosion particles
6. If not enough mana, show message

## Tips for Advanced Interactions

1. **Use conditions** - Check game state before executing actions
2. **Chain interactions** - Combine multiple effects in sequence
3. **Handle failures** - Provide feedback when conditions aren't met
4. **Use selectors** - Target entities or blocks precisely
5. **Add randomness** - Create varied outcomes with `Random`
6. **Test thoroughly** - Complex interactions need extensive testing
7. **Organize with variables** - Use `InteractionVars` for reusable patterns

---

**Previous:** [Mounts & Vehicles](20_Mounts_and_Vehicles.md) | **Next:** [Dungeons & Structures](22_Dungeons_and_Structures.md)
