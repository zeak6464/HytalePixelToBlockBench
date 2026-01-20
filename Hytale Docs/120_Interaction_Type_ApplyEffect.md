# Interaction Type: ApplyEffect

Apply status effects to entities with duration, overlap behavior, and stat modifiers.

## Overview

`ApplyEffect` applies status effects (buffs/debuffs) to entities. Supports inline effects or references to effect definitions, with duration, overlap behavior, and stat modifications.

## Example from Game Files

### Apply Effect Interaction

Apply effect interactions apply status effects to entities. These are commonly used for buffs, debuffs, potions, food effects, and other temporary status modifications.

## Basic Structure

```json
{
  "Type": "ApplyEffect",
  "EffectId": "Burn",
  "Duration": 10.0,
  "Entity": "Target"
}
```

Or inline:

```json
{
  "Type": "ApplyEffect",
  "EffectId": {
    "Duration": 5.0,
    "OverlapBehavior": "Overwrite",
    "ApplicationEffects": {
      "HorizontalSpeedMultiplier": 0.5,
      "HealthRegeneration": -2.0
    }
  }
}
```

## Properties

### EffectId (Reference)

```json
{
  "EffectId": "Burn"
}
```

Reference to effect definition in `Server/Entity/Effects/`.

### EffectId (Inline)

```json
{
  "EffectId": {
    "Duration": 5.0,
    "OverlapBehavior": "Overwrite",
    "ApplicationEffects": {
      "HorizontalSpeedMultiplier": 0.5,
      "HealthRegeneration": -2.0,
      "ParticleSystemId": "Effect_Burn"
    }
  }
}
```

Inline effect definition:
- **`Duration`** - Effect duration in seconds (omit for permanent)
- **`OverlapBehavior`** - "Overwrite", "Stack", or "Extend"
- **`ApplicationEffects`** - Stat modifiers and visual effects

### Duration

```json
{
  "Duration": 10.0
}
```

Effect duration in seconds when using reference `EffectId`. Omit for permanent effects.

### Entity

```json
{
  "Entity": "Target"
}
```

Target entity:
- **`"Target"`** - Selected entity (from Selector)
- **`"Self"`** - Entity performing interaction
- Omit for self

### OverlapBehavior

```json
{
  "OverlapBehavior": "Overwrite"
}
```

How to handle overlapping effects:
- **`"Overwrite"`** - Replace existing effect
- **`"Stack"`** - Stack multiple instances
- **`"Extend"`** - Extend duration if already active

## ApplicationEffects

```json
{
  "ApplicationEffects": {
    "HorizontalSpeedMultiplier": 0.5,
    "HealthRegeneration": -2.0,
    "ParticleSystemId": "Effect_Burn"
  }
}
```

**Common Properties:**
- **`HorizontalSpeedMultiplier`** - Movement speed (0-1, 0 = stop)
- **`HealthRegeneration`** - Health regen rate (negative = damage over time)
- **`ManaRegeneration`** - Mana regen rate
- **`StaminaRegeneration`** - Stamina regen rate
- **`ParticleSystemId`** - Visual particle effect
- **`SoundEventId`** - Continuous sound effect

## Complete Examples

### Example 1: Reference Effect

```json
{
  "Type": "ApplyEffect",
  "EffectId": "Burn",
  "Duration": 10.0
}
```

Applies Burn effect for 10 seconds.

### Example 2: Inline Slow

```json
{
  "Type": "ApplyEffect",
  "EffectId": {
    "Duration": 3.0,
    "OverlapBehavior": "Overwrite",
    "ApplicationEffects": {
      "HorizontalSpeedMultiplier": 0.5
    }
  }
}
```

50% movement speed for 3 seconds.

### Example 3: Damage Over Time

```json
{
  "Type": "ApplyEffect",
  "EffectId": {
    "Duration": 10.0,
    "ApplicationEffects": {
      "HealthRegeneration": -2.0,
      "ParticleSystemId": "Poison_Effect"
    }
  }
}
```

-2 health/second for 10 seconds (20 total damage).

### Example 4: Complete Stop

```json
{
  "Type": "ApplyEffect",
  "EffectId": {
    "Duration": 0.2,
    "OverlapBehavior": "Overwrite",
    "ApplicationEffects": {
      "HorizontalSpeedMultiplier": 0
    }
  }
}
```

Stops movement completely for 0.2 seconds (trap effect).

### Example 5: Target Entity

```json
{
  "Type": "ApplyEffect",
  "EffectId": "Freeze",
  "Duration": 5.0,
  "Entity": "Target"
}
```

Applies Freeze to targeted entity (from Selector).

## Tips

1. **Duration** - Use 0.2-5s for temporary effects, 10s+ for persistent buffs
2. **OverlapBehavior** - "Overwrite" for single-instance, "Stack" for stacking buffs
3. **Inline vs reference** - Use inline for simple effects, references for complex/reusable
4. **Movement speed** - Use 0-1 range, 0 = stop, 1 = normal
5. **Damage over time** - Use negative HealthRegeneration for DoT effects

---

**Previous:** [DamageEntity](119_Interaction_Type_DamageEntity.md) | **Next:** [ClearEntityEffect](121_Interaction_Type_ClearEntityEffect.md)
