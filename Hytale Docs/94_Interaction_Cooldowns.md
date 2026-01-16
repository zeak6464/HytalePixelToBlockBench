# Interaction Cooldowns

Learn how to configure cooldowns for interactions to prevent spam and balance gameplay.

## Overview

Cooldowns prevent interactions from being triggered too frequently. They can be global per interaction type, per-block, or per-entity, and can track multiple cooldown timers simultaneously.

## Location
Cooldowns are configured in interaction definitions via `Cooldown` property.

## Basic Cooldown Structure

```json
{
  "Cooldown": {
    "Id": "MyInteraction",
    "Cooldown": 5.0
  },
  "Interactions": [
    {
      "Type": "Simple"
    }
  ]
}
```

- **`Id`** - Unique cooldown identifier (shared across interactions with same ID)
- **`Cooldown`** - Time in seconds before interaction can trigger again

## Per-Interaction Cooldown

```json
{
  "Interactions": {
    "Use": {
      "Cooldown": {
        "Id": "BlockUse",
        "Cooldown": 2.0
      },
      "Interactions": [
        {
          "Type": "ChangeState"
        }
      ]
    }
  }
}
```

Cooldown applies to this specific interaction.

## Per-Entity Cooldown

```json
{
  "Collision": {
    "Cooldown": {
      "Id": "Collision",
      "Cooldown": 1.0
    },
    "Interactions": [
      {
        "Type": "ApplyEffect"
      }
    ]
  }
}
```

Cooldown is tracked per entity (prevents rapid-fire on same entity).

## Collision Cooldowns

Common pattern for collision interactions:

```json
{
  "CollisionEnter": {
    "Cooldown": {
      "Id": "PortalEnter",
      "Cooldown": 5.0
    },
    "Interactions": [
      {
        "Type": "HubPortal"
      }
    ]
  }
}
```

Prevents portal spam (5 second cooldown between uses).

## Multiple Cooldowns

Different interactions can have different cooldowns:

```json
{
  "Interactions": {
    "Use": {
      "Cooldown": {
        "Id": "UseAction",
        "Cooldown": 1.0
      },
      "Interactions": [ ... ]
    },
    "Collision": {
      "Cooldown": {
        "Id": "CollisionTrigger",
        "Cooldown": 0.5
      },
      "Interactions": [ ... ]
    }
  }
}
```

## Tips for Interaction Cooldowns

1. **Balance** - Shorter cooldowns (0.5-2s) for frequent actions, longer (5-10s) for powerful abilities
2. **Cooldown IDs** - Share IDs across related interactions to prevent spam
3. **Collision** - Always use cooldowns on collision interactions
4. **Game feel** - Test cooldowns for comfortable gameplay flow

---

**Previous:** [Interaction Selectors](93_Interaction_Selectors.md) | **Next:** [Interaction Chains](95_Interaction_Chains.md)
