# Interaction Type: Selector

Targeting system for detecting and interacting with entities and blocks in the world.

## Overview

`Selector` detects entities or blocks within a specified area/range and triggers interactions on hits. Supports raycasts, sweep attacks, area detection, and line of sight checks.

## Basic Structure

```json
{
  "Type": "Selector",
  "RunTime": 0.25,
  "Selector": {
    "Id": "Horizontal",
    "Direction": "ToLeft",
    "TestLineOfSight": true,
    "ExtendTop": 0.5,
    "ExtendBottom": 0.5,
    "StartDistance": 0.1,
    "EndDistance": 2.5,
    "Length": 30,
    "RollOffset": 0,
    "YawOffset": -15
  },
  "HitEntity": {
    "Interactions": [
      {
        "Type": "DamageEntity",
        "DamageCalculator": {
          "BaseDamage": {
            "Physical": 10
          }
        }
      }
    ]
  },
  "HitBlock": {
    "Interactions": [
      {
        "Type": "BreakBlock"
      }
    ]
  }
}
```

## Properties

### Selector

```json
{
  "Selector": {
    "Id": "Horizontal",
    "Direction": "ToLeft",
    "TestLineOfSight": true,
    "ExtendTop": 0.5,
    "ExtendBottom": 0.5,
    "StartDistance": 0.1,
    "EndDistance": 2.5,
    "Length": 30,
    "RollOffset": 0,
    "YawOffset": -15
  }
}
```

**Selector Properties:**
- **`Id`** - Selector identifier
- **`Direction`** - "ToLeft", "ToRight", "ToFront", "ToBack"
- **`TestLineOfSight`** - If `true`, requires clear line of sight
- **`ExtendTop`** - Vertical extension above center (meters)
- **`ExtendBottom`** - Vertical extension below center (meters)
- **`StartDistance`** - Detection start distance from entity
- **`EndDistance`** - Detection end distance from entity
- **`Length`** - Horizontal sweep length (degrees)
- **`RollOffset`** - Roll rotation offset (degrees)
- **`YawOffset`** - Yaw rotation offset (degrees)

### HitEntity

```json
{
  "HitEntity": {
    "Interactions": [
      {
        "Type": "DamageEntity",
        "DamageCalculator": { ... }
      }
    ]
  }
}
```

Interactions to execute when entity is hit.

### HitBlock

```json
{
  "HitBlock": {
    "Interactions": [
      {
        "Type": "BreakBlock"
      }
    ]
  }
}
```

Interactions to execute when block is hit.

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
            "DamageCalculator": { ... }
          }
        ]
      }
    }
  ]
}
```

Array of rules with matchers for conditional entity interactions.

### Effects

```json
{
  "Effects": {
    "CameraEffect": "Unarmed_Swing_Horizontal",
    "WorldSoundEventId": "SFX_Swing"
  }
}
```

Effects to play during selection (camera shake, sounds).

### RunTime

```json
{
  "RunTime": 0.25
}
```

Duration of the selection detection (how long it stays active).

## Complete Examples

### Example 1: Melee Attack

```json
{
  "Type": "Selector",
  "RunTime": 0.25,
  "Selector": {
    "Id": "Horizontal",
    "Direction": "ToLeft",
    "TestLineOfSight": true,
    "ExtendTop": 0.5,
    "ExtendBottom": 0.5,
    "StartDistance": 0.1,
    "EndDistance": 2.5,
    "Length": 30,
    "RollOffset": 0,
    "YawOffset": -15
  },
  "HitEntity": {
    "Interactions": [
      {
        "Type": "DamageEntity",
        "DamageCalculator": {
          "BaseDamage": {
            "Physical": 10
          }
        },
        "Effects": {
          "CameraEffect": "Impact_Light"
        },
        "DamageEffects": {
          "WorldSoundEventId": "SFX_Impact",
          "WorldParticles": [
            {
              "SystemId": "Impact_Blade_01"
            }
          ]
        }
      }
    ]
  }
}
```

Horizontal sweep attack with line of sight check.

### Example 2: Raycast

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Raycast",
    "Offset": { "Y": 1.6 },
    "Distance": 5,
    "TargetType": "Entity"
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

Raycast spell targeting entities or blocks.

### Example 3: Conditional Targeting

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Horizontal",
    "Direction": "ToFront",
    "StartDistance": 0.5,
    "EndDistance": 3.0,
    "Length": 45
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
            "DamageCalculator": {
              "BaseDamage": {
                "Physical": 15
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
            "EffectId": "Friendly_Heal",
            "Duration": 10.0
          }
        ]
      }
    }
  ]
}
```

Different interactions for NPCs (damage) vs players (heal).

## Tips

1. **Line of sight** - Use `TestLineOfSight: true` for realistic attacks (required for weapons)
2. **RunTime** - Match `RunTime` to animation length for accurate hit detection
3. **Direction** - Use appropriate direction for attack type (ToLeft/ToRight for swings)
4. **ExtendTop/Bottom** - Adjust for weapon reach and entity height
5. **HitEntityRules** - Use matchers for conditional targeting logic

---

**Previous:** [Replace](117_Interaction_Type_Replace.md) | **Next:** [DamageEntity](119_Interaction_Type_DamageEntity.md)
