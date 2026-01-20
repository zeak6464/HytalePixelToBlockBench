# Entity Effects

Learn how to apply effects to entities (players and NPCs) using interactions and effect definitions.

## Overview

Entity effects modify entity stats, apply visual effects, change movement, or trigger behaviors. They can be temporary (status effects) or permanent modifications.

## Location
- Effect application: `ApplyEffect` interaction type
- Effect definitions: `Server/Entity/Effects/`

## Example from Game Files

### Food Effect Condition

From `Server/Entity/Effects/Food/Buff/_Deprecated/Food_EffectCondition_Buff_Small.json`:

```1:15:Server/Entity/Effects/Food/Buff/_Deprecated/Food_EffectCondition_Buff_Small.json
{
  "Type": "EffectCondition",
  "EntityEffectIds": [
    "Food_Buff_Small_T1",
    "Food_Buff_Small_T2",
    "Food_Buff_Small_T3"
  ],
  "Match": "None",
  "Next": "Consume_Charge",
  "Failed": {
    "Type": "Simple",
    "RunTime": 0
  }
}
```

This shows an effect condition interaction that checks if an entity has specific effects before proceeding.

## Basic Effect Application

```json
{
  "Type": "ApplyEffect",
  "EffectId": "Burn",
  "Duration": 10.0
}
```

Applies effect to target entity for duration.

## ApplyEffect Properties

### EffectId

```json
{
  "EffectId": "Burn"
}
```

References effect definition ID.

### Duration

```json
{
  "Duration": 10.0
}
```

Effect duration in seconds. Omit for permanent effects.

### OverlapBehavior

```json
{
  "OverlapBehavior": "Overwrite"
}
```

How to handle overlapping effects:
- **`Overwrite`** - Replace existing effect
- **`Stack`** - Stack multiple instances
- **`Extend`** - Extend duration if already active

## Effect Inline Definition

```json
{
  "Type": "ApplyEffect",
  "EffectId": {
    "Duration": 5.0,
    "OverlapBehavior": "Overwrite",
    "ApplicationEffects": {
      "HorizontalSpeedMultiplier": 0
    }
  }
}
```

Define effect inline without separate file.

### ApplicationEffects

```json
{
  "ApplicationEffects": {
    "HorizontalSpeedMultiplier": 0,
    "HealthRegeneration": -5.0,
    "ParticleSystemId": "Effect_Burn"
  }
}
```

Effects applied:
- **`HorizontalSpeedMultiplier`** - Movement speed modifier
- **`HealthRegeneration`** - Health regen rate
- **`ParticleSystemId`** - Visual particle effect

## Common Effect Patterns

### Movement Slow

```json
{
  "Type": "ApplyEffect",
  "EffectId": {
    "Duration": 3.0,
    "ApplicationEffects": {
      "HorizontalSpeedMultiplier": 0.5
    }
  }
}
```

50% movement speed for 3 seconds.

### Damage Over Time

```json
{
  "Type": "ApplyEffect",
  "EffectId": {
    "Duration": 10.0,
    "ApplicationEffects": {
      "HealthRegeneration": -2.0
    }
  }
}
```

-2 health/second for 10 seconds (20 total damage).

### Complete Stop

```json
{
  "Type": "ApplyEffect",
  "EffectId": {
    "Duration": 0.2,
    "ApplicationEffects": {
      "HorizontalSpeedMultiplier": 0
    }
  }
}
```

Stops movement completely for 0.2 seconds (trap effect).

## Combining with Interactions

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
  "HitEntity": {
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "Freeze",
        "Duration": 5.0
      }
    ]
  }
}
```

Apply effect to targeted entity.

## Tips for Entity Effects

1. **Duration balance** - Short durations (0.2-5s) for temporary debuffs, longer for persistent effects
2. **OverlapBehavior** - Use `Overwrite` for single-instance effects, `Stack` for stacking buffs
3. **Inline vs definitions** - Use inline for simple effects, separate files for complex/reusable effects
4. **Stat modifiers** - Effects can modify regeneration rates, not direct stat values

---

**Previous:** [Interaction Chains](95_Interaction_Chains.md) | **Next:** [Inventory Operations](97_Inventory_Operations.md)
