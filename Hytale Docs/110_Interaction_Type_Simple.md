# Interaction Type: Simple

The most basic interaction type for playing effects, animations, sounds, and delays.

## Overview

`Simple` is the fundamental interaction type. It plays effects (animations, sounds, particles), applies delays, and chains to other interactions. Almost every interaction starts with or includes a `Simple` interaction.

## Location
Used in `Server/Item/Interactions/` and referenced in item/block definitions.

## Example from Game Files

### Simple Interaction

From `Server/Item/Interactions/Tests/Simple.json`:

```1:8:Server/Item/Interactions/Tests/Simple.json
{
  "Type": "Simple",
  "RunTime": 0.5,
  "Next": {
    "Type": "Simple",
    "RunTime": 0.5
  }
}
```

This shows a simple interaction with a runtime of 0.5 seconds that chains to another simple interaction.

## Basic Structure

```json
{
  "Type": "Simple",
  "Effects": {
    "ItemAnimationId": "SwingLeft",
    "WorldSoundEventId": "SFX_Swing",
    "LocalSoundEventId": "SFX_Player_Swing"
  },
  "RunTime": 0.111
}
```

## Example from Game Files

### Basic Simple Interaction

From `Server/Item/Interactions/Tests/Simple.json`:

```1:8:Server/Item/Interactions/Tests/Simple.json
{
  "Type": "Simple",
  "RunTime": 0.5,
  "Next": {
    "Type": "Simple",
    "RunTime": 0.5
  }
}
```

A simple delay that chains to another delay.

### Simple with Effects

From `Server/Item/Interactions/Weapons/Wand/Root_Cast.json`:

```1:17:Server/Item/Interactions/Weapons/Wand/Root_Cast.json
{
  "Type": "Simple",
  "RunTime": 0.2,
  "Effects": {
    "ItemAnimationId": "CastLeftCharged",
    "Particles": [
      {
        "SystemId": "NatureBeam",
        "TargetNodeName": "Handle",
        "RotationOffset": {
          "Roll": 180
        }
      }
    ],
    "WorldSoundEventId": "SFX_Skeleton_Mage_Spellbook_Impact",
    "LocalSoundEventId": "SFX_Skeleton_Mage_Spellbook_Impact"
  },
  "Next": {
```

Shows a `Simple` interaction with animation, particles, sounds, and a delay before chaining to the next interaction.

## Properties

### Effects

All visual and audio effects are defined in the `Effects` object:

```json
{
  "Effects": {
    "ItemAnimationId": "SwingLeft",
    "ItemPlayerAnimationsId": "Default",
    "WorldSoundEventId": "SFX_Swing",
    "LocalSoundEventId": "SFX_Player_Swing",
    "WorldParticles": [
      {
        "SystemId": "Impact_Blade_01"
      }
    ],
    "CameraEffect": "Unarmed_Swing_Horizontal",
    "ClearSoundEventOnFinish": true,
    "ClearAnimationOnFinish": true
  }
}
```

**Effect Properties:**
- **`ItemAnimationId`** - Animation to play on the item
- **`ItemPlayerAnimationsId`** - Animation set to use (usually "Default")
- **`WorldSoundEventId`** - 3D sound that plays in world
- **`LocalSoundEventId`** - Sound that plays locally for player
- **`WorldParticles`** - Array of particle systems to spawn
  - **`SystemId`** - Particle system ID
- **`CameraEffect`** - Camera shake/effect to apply
- **`ClearSoundEventOnFinish`** - Stop sound when animation finishes
- **`ClearAnimationOnFinish`** - Stop animation when finished

### RunTime

```json
{
  "RunTime": 0.111
}
```

Delay in seconds before the interaction completes and proceeds to `Next`.

### Next

```json
{
  "Next": {
    "Type": "DamageEntity",
    "DamageCalculator": {
      "BaseDamage": {
        "Physical": 10
      }
    }
  }
}
```

Next interaction to execute after this one completes.

### Failed

```json
{
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.interactions.failed"
  }
}
```

Interaction to execute if this interaction fails.

## Complete Examples

### Example 1: Basic Animation

```json
{
  "Type": "Simple",
  "Effects": {
    "ItemAnimationId": "SwingLeft"
  }
}
```

Plays swing animation, no delay.

### Example 2: Animation with Sound

```json
{
  "Type": "Simple",
  "Effects": {
    "ItemAnimationId": "SwingLeft",
    "WorldSoundEventId": "SFX_Unarmed_Swing",
    "LocalSoundEventId": "SFX_Player_Unarmed_Swing_Left"
  },
  "RunTime": 0.111
}
```

Plays swing animation, sound effects, waits 0.111s.

### Example 3: With Delay and Chain

```json
{
  "Type": "Simple",
  "Effects": {
    "ItemAnimationId": "SwingLeft"
  },
  "RunTime": 0.111,
  "Next": {
    "Type": "Simple",
    "Effects": {
      "WorldSoundEventId": "SFX_Impact"
    }
  }
}
```

Plays animation, waits, then plays impact sound.

### Example 4: Particle Effects

```json
{
  "Type": "Simple",
  "Effects": {
    "ItemAnimationId": "Cast",
    "WorldParticles": [
      {
        "SystemId": "Magic_Cast"
      },
      {
        "SystemId": "Sparkles"
      }
    ],
    "CameraEffect": "Spell_Cast"
  }
}
```

Plays cast animation with particles and camera shake.

### Example 5: Clear Effects

```json
{
  "Type": "Simple",
  "RunTime": 0.1,
  "Next": {
    "Type": "RefillContainer",
    "Effects": {
      "ItemAnimationId": "Interact",
      "ClearSoundEventOnFinish": true,
      "ClearAnimationOnFinish": true,
      "LocalSoundEventId": "SFX_WATER_MoveIn"
    }
  }
}
```

Plays interaction animation, clears sound/animation when done.

## Common Patterns

### Preparation Delay

```json
{
  "Type": "Simple",
  "Effects": {
    "ItemAnimationId": "SwingDown"
  },
  "$Comment": "Prepare Delay",
  "RunTime": 0.111
}
```

Add a small delay before action (wind-up).

### Padding

```json
{
  "Type": "Simple",
  "$Comment": "Pad the interaction length",
  "RunTime": 0.112
}
```

Extend interaction duration with empty delay.

### Chain Starter

```json
{
  "Type": "Simple",
  "Effects": {
    "ItemAnimationId": "Start"
  },
  "Next": {
    "Type": "Parallel",
    "Interactions": [
      {
        "Interactions": [
          {
            "Type": "Selector",
            "Selector": { ... }
          }
        ]
      }
    ]
  }
}
```

Start interaction chain with animation/sound.

## Tips for Simple Interactions

1. **Always use for effects** - Use `Simple` for any animations, sounds, or particles
2. **RunTime delays** - Use `RunTime` for wind-up delays and timing
3. **Clear effects** - Use `ClearSoundEventOnFinish`/`ClearAnimationOnFinish` for clean transitions
4. **Chain with Next** - Use `Next` to chain interactions sequentially
5. **Failed branches** - Use `Failed` for error handling
6. **Padding** - Use empty `Simple` with `RunTime` to pad interaction duration
7. **Comments** - Use `$Comment` to document interaction purpose
8. **Effects first** - Define effects before chaining to other interactions

## Related Types

- **Serial** - Execute multiple `Simple` interactions sequentially
- **Parallel** - Execute multiple interactions simultaneously
- **Condition** - Branch based on game state before/after `Simple`

---

**Previous:** [Interaction Types List](109_Interaction_Types_List.md) | **Next:** [Serial](111_Interaction_Type_Serial.md)
