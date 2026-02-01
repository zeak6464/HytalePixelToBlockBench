# Interaction Selectors

Learn how to use selectors to target entities, blocks, or positions for interactions.

## Overview

Selectors allow interactions to choose targets dynamically using raycasts, entity detection, or position-based targeting. They enable interactions that respond to what the player is looking at or nearby.

## Location
Selectors are used in interaction definitions via `Type: "Selector"`.

## Example from Game Files

### Selector Interaction

From `Server/Item/Interactions/Tests/Selector.json`:

```1:16:Server/Item/Interactions/Tests/Selector.json
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
    "YawStartOffset": -15
  }
}
```

This shows a selector interaction that uses a horizontal raycast to target entities or blocks to the left with line of sight checking.

## Basic Selector Structure

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
        "Type": "Simple",
        "Effects": {
          "LocalSoundEventId": "SFX_Target_Hit"
        }
      }
    ]
  }
}
```

## Selector Types

### Raycast

```json
{
  "Selector": {
    "Id": "Raycast",
    "Offset": {
      "Y": 1.6
    },
    "Distance": 5
  }
}
```

Targets what the player is looking at (blocks or entities) within range. Uses `"Id": "Raycast"` not `"Type"`.

**Update 1 Note:** Weapon attacks now check line of sight before applying damage. Use `TestLineOfSight: true` in the Selector configuration:

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Horizontal",
    "Direction": "ToLeft",
    "TestLineOfSight": true,  // Requires clear line of sight to hit
    "EndDistance": 2.5,
    "Length": 30
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
  }
}
```

**Selector Properties for Line of Sight:**
- **`TestLineOfSight: true`** - Requires unobstructed line of sight between attacker and target
- Without line of sight, damage is not applied even if the attack would otherwise hit

### Horizontal (Melee)

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
    "Length": 30
  }
}
```

Targets entities in a horizontal sweep area. Used for melee attacks.

**Note:** Selectors use `"Id"` to specify the selector type, not `"Type"`. Common selector IDs include `"Raycast"` and `"Horizontal"`.

## Selector Interaction Pattern

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
        "EffectId": "TargetedEffect"
      }
    ]
  }
}
```

## Combining Selectors with Conditions

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
        {
          "Type": "NPC"
        }
      ],
      "Next": {
        "Interactions": [
          {
            "Type": "ChangeStat",
            "EntityStatId": "Health",
            "Amount": 50
          }
        ]
      }
    }
  ]
}
```

Only heals NPCs targeted by raycast using `HitEntityRules` with matchers.

## Tips for Interaction Selectors

1. **Raycast** - Most common for player interactions (targeting blocks/entities)
2. **Range** - Set appropriate ranges for gameplay balance
3. **Combine with conditions** - Use selectors + conditions for smart targeting
4. **Entity detection** - Use Entity selector for area-of-effect interactions

---

**Previous:** [Interaction Conditions](92_Interaction_Conditions.md) | **Next:** [Interaction Cooldowns](94_Interaction_Cooldowns.md)
