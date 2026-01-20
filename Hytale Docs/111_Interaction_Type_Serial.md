# Interaction Type: Serial

Execute multiple interactions sequentially, one after another.

## Overview

`Serial` executes interactions in the order they appear in the `Interactions` array. Each interaction completes before the next one starts. This is useful for chaining multiple actions that must happen in sequence.

## Example from Game Files

### Serial Interaction

From `Server/Item/Items/Weapon/Deployable/Weapon_Deployable_Healing_Totem.json`:

The Serial interaction type executes interactions in sequence, one after another. This is used in deployable items like totems to chain multiple actions together.

## Basic Structure

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "ClearEntityEffect",
      "EntityEffectId": "Meat_Buff_T1"
    },
    {
      "Type": "ClearEntityEffect",
      "EntityEffectId": "Meat_Buff_T2"
    },
    {
      "Type": "ApplyEffect",
      "EffectId": "Meat_Buff_T3"
    }
  ]
}
```

## Properties

### Interactions

```json
{
  "Interactions": [
    {
      "Type": "ApplyEffect",
      "EffectId": "Buff1"
    },
    {
      "Type": "ApplyEffect",
      "EffectId": "Buff2"
    }
  ]
}
```

Array of interactions to execute sequentially.

## Complete Examples

### Example 1: Clear Old Effects

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "ClearEntityEffect",
      "EntityEffectId": "Meat_Buff_T1"
    },
    {
      "Type": "ClearEntityEffect",
      "EntityEffectId": "Meat_Buff_T2"
    },
    {
      "Type": "ApplyEffect",
      "EffectId": "Meat_Buff_T3"
    }
  ]
}
```

Clears tier 1 and 2 effects, then applies tier 3.

### Example 2: Stat Changes

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "ChangeStatWithModifier",
      "StatModifiers": {
        "Stamina": -2
      },
      "ValueType": "Absolute"
    },
    {
      "Type": "ChangeStat",
      "StatModifiers": {
        "StaminaRegenDelay": -0.7
      },
      "Behaviour": "Set",
      "ValueType": "Absolute"
    }
  ]
}
```

Reduces stamina, then sets regen delay.

### Example 3: Complex Chain

```json
{
  "Type": "Serial",
  "Interactions": [
    {
      "Type": "Simple",
      "Effects": {
        "WorldSoundEventId": "SFX_Start"
      }
    },
    {
      "Type": "ApplyEffect",
      "EffectId": "Boost",
      "Duration": 5.0
    },
    {
      "Type": "Simple",
      "RunTime": 2.0
    },
    {
      "Type": "SendMessage",
      "Key": "server.effects.boost.expired"
    }
  ]
}
```

Plays sound, applies effect, waits 2s, shows message.

## Tips

1. **Order matters** - Interactions execute in array order
2. **Each waits** - Each interaction completes before the next
3. **Use for dependencies** - When one action must complete before another
4. **Combine with Simple** - Use Simple for delays between actions

---

**Previous:** [Simple](110_Interaction_Type_Simple.md) | **Next:** [Parallel](112_Interaction_Type_Parallel.md)
