# Interaction Selectors

Learn how to use selectors to target entities, blocks, or positions for interactions.

## Overview

Selectors allow interactions to choose targets dynamically using raycasts, entity detection, or position-based targeting. They enable interactions that respond to what the player is looking at or nearby.

## Location
Selectors are used in interaction definitions via `Type: "Selector"`.

## Basic Selector Structure

```json
{
  "Type": "Selector",
  "Selector": {
    "Type": "Raycast",
    "Range": 10
  },
  "Interactions": [
    {
      "Type": "Simple",
      "Effects": {
        "LocalSoundEventId": "SFX_Target_Hit"
      }
    }
  ]
}
```

## Selector Types

### Raycast

```json
{
  "Selector": {
    "Type": "Raycast",
    "Range": 10
  }
}
```

Targets what the player is looking at (blocks or entities) within range.

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

### Entity

```json
{
  "Selector": {
    "Type": "Entity",
    "Range": 5
  }
}
```

Targets nearby entities within range.

### Position

```json
{
  "Selector": {
    "Type": "Position",
    "Position": {
      "X": 0,
      "Y": 0,
      "Z": 0
    }
  }
}
```

Targets a specific world position.

## Selector Interaction Pattern

```json
{
  "Type": "Selector",
  "Selector": {
    "Type": "Raycast",
    "Range": 5
  },
  "Interactions": [
    {
      "Type": "ApplyEffect",
      "EffectId": "TargetedEffect"
    }
  ]
}
```

## Combining Selectors with Conditions

```json
{
  "Type": "Selector",
  "Selector": {
    "Type": "Raycast",
    "Range": 10
  },
  "Interactions": [
    {
      "Type": "Condition",
      "Condition": {
        "EntityType": "NPC"
      },
      "Interactions": [
        {
          "Type": "Heal",
          "Amount": 50
        }
      ]
    }
  ]
}
```

Only heals NPCs targeted by raycast.

## Tips for Interaction Selectors

1. **Raycast** - Most common for player interactions (targeting blocks/entities)
2. **Range** - Set appropriate ranges for gameplay balance
3. **Combine with conditions** - Use selectors + conditions for smart targeting
4. **Entity detection** - Use Entity selector for area-of-effect interactions

---

**Previous:** [Interaction Conditions](92_Interaction_Conditions.md) | **Next:** [Interaction Cooldowns](94_Interaction_Cooldowns.md)
