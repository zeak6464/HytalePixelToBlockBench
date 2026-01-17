# Interaction Type: Condition

Conditional branching based on game state, stats, entity type, or other conditions.

## Overview

`Condition` checks various game states and executes different interactions based on the result. Supports `Next` for success and optional `Failed`/`Else` for failure cases.

## Basic Structure

```json
{
  "Type": "Condition",
  "RequiredGameMode": "Adventure",
  "Next": {
    "Type": "BreakBlock"
  },
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.interactions.adventure.only"
  }
}
```

## Properties

### Condition Checks

```json
{
  "Condition": {
    "EntityType": "Player",
    "Crouching": true,
    "Stat": "Health",
    "Comparison": "GreaterThan",
    "Value": 50,
    "GameMode": "Adventure"
  }
}
```

**Common Condition Properties:**
- **`EntityType`** - "Player" or entity type
- **`Crouching`** - Boolean, is entity crouching
- **`Stat`** - Stat name to check (Health, Mana, etc.)
- **`Comparison`** - "GreaterThan", "LessThan", "Equals", etc.
- **`Value`** - Value to compare against
- **`GameMode`** - "Adventure", "Creative", etc.

### Next

```json
{
  "Next": {
    "Type": "ApplyEffect",
    "EffectId": "Buff"
  }
}
```

Interaction to execute if condition passes. Can be inline object or string reference.

### Failed / Else

```json
{
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.interactions.failed"
  }
}
```

Optional interaction to execute if condition fails.

### RequiredGameMode

```json
{
  "RequiredGameMode": "Adventure"
}
```

Shortcut for checking game mode.

## Complete Examples

### Example 1: Game Mode Check

```json
{
  "Type": "Condition",
  "RequiredGameMode": "Adventure",
  "Next": {
    "Type": "BreakBlock"
  }
}
```

Only breaks blocks in Adventure mode.

### Example 2: Entity Type Check

```json
{
  "Type": "Condition",
  "Condition": {
    "EntityType": "Player"
  },
  "Next": {
    "Type": "ApplyEffect",
    "EffectId": "PlayerOnly_Buff"
  },
  "Failed": {
    "Type": "Simple"
  }
}
```

Applies effect only to players.

### Example 3: Stat Check

```json
{
  "Type": "Condition",
  "Condition": {
    "Stat": "Health",
    "Comparison": "LessThan",
    "Value": 25
  },
  "Next": {
    "Type": "ApplyEffect",
    "EffectId": "LowHealth_Regen"
  }
}
```

Applies regen when health below 25.

### Example 4: Crouching Check

```json
{
  "Type": "Condition",
  "Condition": {
    "Crouching": true
  },
  "Next": {
    "Type": "ApplyEffect",
    "EffectId": "Stealth"
  }
}
```

Applies stealth when crouching.

### Example 5: Complex Condition

```json
{
  "Type": "Condition",
  "Condition": {
    "EntityType": "Player",
    "Stat": "Mana",
    "Comparison": "GreaterThanOrEqual",
    "Value": 50,
    "GameMode": "Adventure"
  },
  "Next": {
    "Type": "LaunchProjectile",
    "ProjectileId": "Fireball"
  },
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.spells.insufficient.mana"
  }
}
```

Checks multiple conditions before casting spell.

## Tips

1. **RequiredGameMode shortcut** - Use `RequiredGameMode` for simple game mode checks
2. **Nested conditions** - Nest conditions for complex logic (AND conditions)
3. **Failed branches** - Always provide `Failed` for better UX
4. **String references** - Use string IDs for `Next` instead of inline objects when possible

---

**Previous:** [Parallel](112_Interaction_Type_Parallel.md) | **Next:** [Chaining](114_Interaction_Type_Chaining.md)
