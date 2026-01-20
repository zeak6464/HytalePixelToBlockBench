# Animations

Learn how to configure animations for NPCs, players, and items.

## Overview

Animations in Hytale are defined using `.blockyanim` files and configured in model files. Animation sets define which animations play for different states and actions.

## Location
- Animation files: `Common/Characters/Animations/`, `Common/NPC/`, `Common/Items/Animations/`
- Animation configuration: `Server/Models/`, `Server/Item/Animations/`

## Example from Game Files

### Emberwulf NPC Animations

From `Server/Models/Beast/Emberwulf.json`:

```40:50:Server/Models/Beast/Emberwulf.json
  "AnimationSets": {
    "Idle": {
      "Animations": [
        {
          "Animation": "NPC/Beast/Emberwulf/Animations/Default/Idle.blockyanim"
        }
      ]
    },
    "Walk": {
      "Animations": [
        {
```

This shows an NPC model with animation sets for different behaviors (Idle, Walk, etc.).

## NPC Animation Sets

NPC models define animation sets for different behaviors.

### Basic Animation Set Structure

In `Server/Models/Intelligent/MyCustom/MyCustom_NPC.json`:

```json
{
  "AnimationSets": {
    "Idle": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Idle.blockyanim",
          "Speed": 0.5,
          "Looping": true
        }
      ]
    },
    "Walk": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Walk.blockyanim",
          "Speed": 1.0
        }
      ]
    },
    "Run": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Run.blockyanim",
          "Speed": 0.8,
          "SoundEventId": "SFX_Goblin_Run"
        }
      ]
    },
    "Jump": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Jump.blockyanim",
          "BlendingDuration": 0.1,
          "Looping": false
        }
      ]
    },
    "Alerted": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Alerted.blockyanim",
          "Looping": false
        }
      ]
    },
    "Hurt": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Hurt.blockyanim",
          "Looping": false
        }
      ]
    },
    "Death": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Death.blockyanim",
          "Looping": false
        }
      ]
    }
  }
}
```

## Animation Properties

### Speed

```json
{
  "Animation": "NPC/MyCustom/Walk.blockyanim",
  "Speed": 1.0
}
```

- **`1.0`** - Normal speed
- **`0.5`** - Half speed (slower)
- **`1.5`** - 1.5x speed (faster)

### Looping

```json
{
  "Animation": "NPC/MyCustom/Idle.blockyanim",
  "Looping": true
}
```

- **`true`** - Animation repeats continuously (for Idle, Walk, Run)
- **`false`** - Animation plays once (for Jump, Hurt, Death)

### Blending Duration

```json
{
  "Animation": "NPC/MyCustom/Jump.blockyanim",
  "BlendingDuration": 0.1
}
```

Time in seconds for smooth transition between animations (usually 0.1-0.2).

### Sound Events

```json
{
  "Animation": "NPC/MyCustom/Run.blockyanim",
  "SoundEventId": "SFX_Goblin_Run"
}
```

Sound played during the animation (e.g., footsteps, idle sounds).

### Multiple Animations (Random Selection)

```json
{
  "Animations": [
    {
      "Animation": "NPC/MyCustom/Jump.blockyanim",
      "Speed": 0.8
    },
    {
      "Animation": "NPC/MyCustom/Jump_Far.blockyanim",
      "Speed": 0.8
    }
  ]
}
```

Multiple animations randomly selected for variety.

## Common NPC Animation Sets

| Animation Set | Use Case | Looping | Example |
|---------------|----------|---------|---------|
| `Idle` | Standing still | `true` | `"Idle.blockyanim"` |
| `Walk` | Walking | `true` | `"Walk.blockyanim"` |
| `WalkBackward` | Walking backward | `true` | `"Walk_Backward.blockyanim"` |
| `Run` | Running | `true` | `"Run.blockyanim"` |
| `Crouch` | Crouching | `true` | `"Crouch.blockyanim"` |
| `CrouchWalk` | Crouch walking | `true` | `"Crouch_Walk.blockyanim"` |
| `Jump` | Jumping | `false` | `"Jump.blockyanim"` |
| `Fall` | Falling | `true` | `"Fall.blockyanim"` |
| `Alerted` | Alert/aware state | `false` | `"Alerted.blockyanim"` |
| `Hurt` | Taking damage | `false` | `"Hurt.blockyanim"` |
| `Death` | Death animation | `false` | `"Death.blockyanim"` |
| `Spawn` | Spawning in | `false` | `"Spawn.blockyanim"` |
| `Despawn` | Despawning | `false` | `"Despawn.blockyanim"` |

## Player Animations

Player animations are configured in `Server/Models/Human/Player.json` and item animation files.

### Item Animations

Item-specific animations are defined in `Server/Item/Animations/`:

`Server/Item/Animations/Default.json`:

```json
{
  "Animations": {
    "Idle": {
      "FirstPerson": "Characters/Animations/Default/Idle_FPS.blockyanim",
      "Speed": 0.5,
      "Looping": true
    },
    "Walk": {
      "Speed": 1.0,
      "Looping": true,
      "FirstPerson": "Characters/Animations/Default/Walk_FPS.blockyanim"
    },
    "Run": {
      "Speed": 0.9,
      "Looping": true,
      "FirstPerson": "Characters/Animations/Default/Run_FPS.blockyanim"
    }
  }
}
```

### Item Animation IDs

Items reference animation sets via `PlayerAnimationsId`:

```json
{
  "PlayerAnimationsId": "Sword",  // Uses animations from Item/Animations/Sword.json
  "PlayerAnimationsId": "Pickaxe",
  "PlayerAnimationsId": "Spellbook",
  "PlayerAnimationsId": "Item"  // Default item animations
}
```

## Animation in Interactions

Animations can be triggered by item interactions:

```json
{
  "Effects": {
    "ItemAnimationId": "CastHurlCharged"
  }
}
```

Common animation IDs:
- `"CastHurlCharged"` - Spellbook casting
- `"CastSummonCharged"` - Staff casting
- `"CastLeftCharged"` - Wand casting
- `"Till"` - Hoe tilling
- `"Interact"` - General interaction

## Footstep Intervals

For walk/run animations, define footstep timing:

```json
{
  "Animation": "Characters/Animations/Default/Walk.blockyanim",
  "FootstepIntervals": [
    25,
    75
  ]
}
```

- **`[25, 75]`** - Footsteps occur at 25% and 75% through the animation cycle
- Used for syncing footstep sounds

## Complete Example: Custom NPC Animations

`Server/Models/Intelligent/MyCustom/MyCustom_NPC.json`:

```json
{
  "Parent": "Goblin",
  "Model": "NPC/Intelligent/Goblin/Models/Model.blockymodel",
  "Texture": "NPC/Intelligent/Goblin/Models/Model_Textures/Moldy.png",
  "AnimationSets": {
    "Idle": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Idle.blockyanim",
          "Speed": 0.5,
          "Looping": true
        }
      ]
    },
    "Walk": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Walk.blockyanim",
          "Speed": 0.9,
          "SoundEventId": "SFX_MyCustom_Footsteps"
        }
      ]
    },
    "Run": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Run.blockyanim",
          "Speed": 1.1,
          "SoundEventId": "SFX_MyCustom_Run"
        }
      ]
    },
    "Jump": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Jump.blockyanim",
          "BlendingDuration": 0.1,
          "Looping": false
        }
      ]
    },
    "Alerted": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Alerted.blockyanim",
          "Looping": false,
          "Speed": 1.0
        }
      ]
    },
    "Hurt": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Hurt.blockyanim",
          "Looping": false
        }
      ]
    },
    "Death": {
      "Animations": [
        {
          "Animation": "NPC/Intelligent/MyCustom/Animations/Death.blockyanim",
          "Looping": false
        }
      ]
    }
  }
}
```

## Animation Path Reference

### Character Animations

- `Characters/Animations/Default/Idle.blockyanim`
- `Characters/Animations/Default/Walk.blockyanim`
- `Characters/Animations/Default/Run.blockyanim`
- `Characters/Animations/Default/Jump.blockyanim`

### NPC Animations

- `NPC/Intelligent/{Type}/Animations/Default/Idle.blockyanim`
- `NPC/Intelligent/{Type}/Animations/Default/Walk.blockyanim`
- `NPC/Creature/{Type}/Animations/Default/Run.blockyanim`

### Item Animations

- `Items/Animations/Fly/Fly_Vertical.blockyanim`
- `Items/Animations/Dropped/Dropped_Diagonal_Left.blockyanim`

## Tips for Configuring Animations

1. **Set appropriate speeds** - Match animation speed to movement speed
2. **Use looping correctly** - Idle/Walk/Run loop, Jump/Hurt/Death don't
3. **Add blending duration** - Smooth transitions between animations
4. **Include sound events** - Footsteps, idle sounds enhance immersion
5. **Define all common sets** - Idle, Walk, Run, Jump, Hurt, Death at minimum
6. **Test animation transitions** - Ensure smooth blending between states
7. **Use multiple animations** - Random selection adds variety

---

**Previous:** [Fishing](26_Fishing.md) | **Next:** [Entity Stats](28_Entity_Stats.md)
