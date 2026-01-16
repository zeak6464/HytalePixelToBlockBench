# Interaction Conditions

Learn how to use conditional logic in item and block interactions to create branching behaviors.

## Overview

Interaction conditions allow interactions to check various game states before executing. Conditions can check player stats, entity types, crouching state, game modes, and more.

## Location
Conditions are used in interaction definitions via `Type: "Condition"` or in `Condition` properties.

## Basic Condition Structure

```json
{
  "Type": "Condition",
  "Condition": {
    "EntityType": "Player"
  },
  "Interactions": [
    {
      "Type": "Simple",
      "Effects": {
        "LocalSoundEventId": "SFX_Player_Specific"
      }
    }
  ]
}
```

## Condition Types

### Entity Type

```json
{
  "Condition": {
    "EntityType": "Player"
  }
}
```

Only triggers for players (not NPCs).

### Crouching

```json
{
  "Condition": {
    "Crouching": true
  }
}
```

Only triggers when entity is crouching.

### Stat Check

```json
{
  "Condition": {
    "Stat": "Health",
    "Comparison": "GreaterThan",
    "Value": 50
  }
}
```

Checks entity stat value.

### Game Mode

```json
{
  "Condition": {
    "GameMode": "Adventure"
  }
}
```

Only triggers in specific game mode.

## Condition Interaction Pattern

```json
{
  "Type": "Condition",
  "Condition": {
    "EntityType": "Player"
  },
  "Interactions": [
    {
      "Type": "ApplyEffect",
      "EffectId": "PlayerOnlyEffect"
    }
  ],
  "Else": {
    "Interactions": [
      {
        "Type": "Simple"
      }
    ]
  }
}
```

### Else Branch

```json
{
  "Else": {
    "Interactions": [ ... ]
  }
}
```

Optional `Else` executes when condition fails.

## Combining Conditions

Use nested conditions for complex logic:

```json
{
  "Type": "Condition",
  "Condition": {
    "EntityType": "Player"
  },
  "Interactions": [
    {
      "Type": "Condition",
      "Condition": {
        "Crouching": true
      },
      "Interactions": [
        {
          "Type": "StealthAction"
        }
      ]
    }
  ]
}
```

## Tips for Interaction Conditions

1. **Player-only** - Use `EntityType: "Player"` for player-specific interactions
2. **State checks** - Use stat/game mode checks for conditional behaviors
3. **Nesting** - Combine conditions for complex logic
4. **Else branches** - Use `Else` for fallback behaviors

---

**Previous:** [Gathering Types](91_Gathering_Types.md) | **Next:** [Interaction Selectors](93_Interaction_Selectors.md)
