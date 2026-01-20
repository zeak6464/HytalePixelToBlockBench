# Item Animations (Detailed)

Learn how to configure detailed animation sets for items including first-person, third-person, combat, and movement animations.

## Overview

Item animations define how characters animate when holding or using items. Animations include:
- Idle, walk, run, sprint
- Combat (swings, stabs, guards)
- Swimming, climbing, flying
- First-person and third-person variants

## Location
`Server/Item/Animations/`

## Example from Game Files

### Sword Animations

From `Server/Item/Animations/Sword.json`:

```1:52:Server/Item/Animations/Sword.json
{
  "Animations": {
    "Idle": {
      "FirstPerson": "Characters/Animations/Items/Main_Handed/Sword/Idle_FPS.blockyanim",
      "Speed": 0.5,
      "ThirdPerson": "Characters/Animations/Items/Main_Handed/Sword/Idle.blockyanim",
      "Looping": true
    },
    "Walk": {
      "FirstPerson": "Characters/Animations/Items/Main_Handed/Sword/Walk_FPS.blockyanim",
      "Speed": 1.0,
      "ThirdPerson": "Characters/Animations/Items/Main_Handed/Sword/Walk.blockyanim",
      "Looping": true
    },
    "Run": {
      "FirstPerson": "Characters/Animations/Items/Main_Handed/Sword/Run_FPS.blockyanim",
      "Speed": 0.9,
      "ThirdPerson": "Characters/Animations/Items/Main_Handed/Sword/Run.blockyanim",
      "Looping": true
    },
    "SwingLeft": {
      "ThirdPerson": "Characters/Animations/Items/Main_Handed/Sword/Attacks/Swing_Left/Swing_Left.blockyanim",
      "ThirdPersonMoving": "Characters/Animations/Items/Main_Handed/Sword/Attacks/Swing_Left/Swing_Left_Moving.blockyanim",
      "ThirdPersonFace": "Characters/Animations/Expressions/Frown.blockyanim",
      "FirstPerson": "Characters/Animations/Items/Main_Handed/Sword/Attacks/Swing_Left/Swing_Left_FPS.blockyanim",
      "KeepPreviousFirstPersonAnimation": true,
      "Speed": 1,
      "ClipsGeometry": true
    }
  }
}
```

This shows a comprehensive item animation configuration with first-person and third-person animations for movement, combat, swimming, climbing, and more.

## Basic Animation Set Structure

Create `Server/Item/Animations/MyCustom.json`:

```json
{
  "Animations": {
    "Idle": {
      "FirstPerson": "Characters/Animations/Items/MyCustom/Idle_FPS.blockyanim",
      "ThirdPerson": "Characters/Animations/Items/MyCustom/Idle.blockyanim",
      "Speed": 0.5,
      "Looping": true
    }
  }
}
```

## Animation Properties

### FirstPerson / ThirdPerson

```json
{
  "FirstPerson": "path/to/FirstPerson.blockyanim",
  "ThirdPerson": "path/to/ThirdPerson.blockyanim"
}
```

Animation paths for first-person and third-person views.

### Speed

```json
{
  "Speed": 1.0
}
```

Playback speed multiplier. `1.0` = normal speed.

### Looping

```json
{
  "Looping": true
}
```

Whether animation loops continuously.

### BlendingDuration

```json
{
  "BlendingDuration": 0.1
}
```

Transition time when switching to this animation (seconds).

## Movement Animations

### Basic Movement

```json
{
  "Idle": { ... },
  "Walk": { ... },
  "Run": { ... },
  "Sprint": { ... },
  "Crouch": { ... },
  "CrouchWalk": { ... }
}
```

Standard movement states.

### Jumping

```json
{
  "Jump": {
    "ThirdPerson": "...",
    "Speed": 1,
    "Looping": false,
    "KeepPreviousFirstPersonAnimation": true
  },
  "JumpWalk": { ... },
  "JumpRun": { ... }
}
```

Jump animations with optional first-person retention.

### Falling

```json
{
  "Fall": {
    "BlendingDuration": 0.4,
    "Looping": true,
    "KeepPreviousFirstPersonAnimation": true
  },
  "FallFar": { ... }
}
```

Fall animations with smooth blending.

## Combat Animations

### Basic Attacks

```json
{
  "SwingLeft": {
    "ThirdPerson": "...",
    "FirstPerson": "...",
    "Speed": 1,
    "ClipsGeometry": true
  },
  "SwingRight": { ... },
  "SwingDown": { ... },
  "Stab": { ... }
}
```

Melee attack animations.

### Guard/Block

```json
{
  "Guard": {
    "Looping": true,
    "BlendingDuration": 0.1
  },
  "GuardBash": { ... }
}
```

Defensive animations.

### Charged Attacks

```json
{
  "SwingLeftCharging": {
    "Looping": false,
    "Speed": 0.64
  },
  "SwingLeftCharged": {
    "BlendingDuration": 0,
    "Speed": 1
  }
}
```

Charged/combo attack sequences.

## Advanced Animations

### Swimming

```json
{
  "SwimIdle": { ... },
  "Swim": { ... },
  "SwimFast": { ... },
  "SwimDive": { ... }
}
```

Underwater movement animations.

### Climbing

```json
{
  "ClimbIdle": { ... },
  "ClimbUp": { ... },
  "ClimbDown": { ... },
  "ClimbLeft": { ... },
  "ClimbRight": { ... }
}
```

Climbing movement animations.

### Flying

```json
{
  "FlyIdle": { ... },
  "Fly": { ... },
  "FlyBackward": { ... },
  "FlyFast": { ... }
}
```

Creative mode flying animations.

## Animation Features

### KeepPreviousFirstPersonAnimation

```json
{
  "KeepPreviousFirstPersonAnimation": true
}
```

Retains previous first-person animation (useful for jumps).

### ClipsGeometry

```json
{
  "ClipsGeometry": true
}
```

Animation can clip through geometry (for attack swings).

### ThirdPersonMoving

```json
{
  "ThirdPersonMoving": "path/to/Moving.blockyanim"
}
```

Separate animation for moving while performing action.

### ThirdPersonFace

```json
{
  "ThirdPersonFace": "Characters/Animations/Expressions/Angry.blockyanim"
}
```

Facial expression animation.

## Camera Configuration

```json
{
  "Camera": {
    "Pitch": {
      "TargetNodes": ["Head", "RShoulder"]
    },
    "Yaw": {
      "TargetNodes": ["Belly"]
    }
  }
}
```

Camera target nodes for this animation set.

## Wiggle Weights

```json
{
  "WiggleWeights": {
    "Pitch": 2.5,
    "Roll": 0.1,
    "X": 2.5,
    "Y": 0.1,
    "Z": 0.1
  }
}
```

Camera shake/wiggle settings.

## Using Item Animations

Reference in item definitions:

```json
{
  "PlayerAnimationsId": "Sword"
}
```

## Tips for Creating Item Animations

1. **First/third person** - Always provide both variants
2. **Speed matching** - Match animation speed to gameplay feel
3. **Blending** - Use blending for smooth transitions
4. **Combat timing** - Sync attack animations with damage windows
5. **Looping** - Use looping for continuous states (idle, walk)

---

**Previous:** [Fluid FX](65_Fluid_FX.md) | **Next:** [Reticles](67_Reticles.md)
