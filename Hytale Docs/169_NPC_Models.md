# NPC Models

Learn how to configure NPC visual appearances, animations, hitboxes, and attachments using `Server/Models/`.

## Overview

NPC models define the visual appearance and animation sets for NPCs. They configure the 3D model, textures, hitbox dimensions, animations for all states, attachments (clothes, accessories), and particles.

## Location

`Server/Models/{Category}/`

Categories:
- **`Beast/`** - Hostile creatures (wolves, spiders, emberwulf)
- **`Livestock/`** - Farm animals (chicken, sheep, pig)
- **`Critter/`** - Small creatures (frogs, mice, squirrels)
- **`Flying_Beast/`** - Flying hostile creatures
- **`Flying_Critter/`** - Flying small creatures
- **`Flying_Wildlife/`** - Flying wildlife
- **`Swimming_Beast/`** - Aquatic hostile creatures
- **`Swimming_Critter/`** - Aquatic small creatures
- **`Swimming_Wildlife/`** - Aquatic wildlife
- **`Wildlife/`** - Wild animals (deer, mosshorn)
- **`Intelligent/`** - Intelligent NPCs (goblins, trorks, outlanders)
- **`Undead/`** - Undead creatures (skeletons)
- **`Elemental/`** - Elemental creatures
- **`Void/`** - Void creatures
- **`Human/`** - Human NPCs
- **`Pets/`** - Pet companions
- **`Projectiles/`** - Projectile visuals
- **`Deployables/`** - Deployable entities (turrets, totems)

## Basic NPC Model Structure

`Server/Models/Livestock/Chicken.json`:

```json
{
  "Model": "NPC/Livestock/Chicken/Models/Model.blockymodel",
  "Texture": "NPC/Livestock/Chicken/Models/Texture.png",
  "EyeHeight": 0.7,
  "CrouchOffset": -0.2,
  "HitBox": {
    "Max": { "X": 0.35, "Y": 0.9, "Z": 0.35 },
    "Min": { "X": -0.35, "Y": 0, "Z": -0.35 }
  },
  "MinScale": 0.8,
  "MaxScale": 1.2,
  "AnimationSets": {
    "Idle": {
      "Animations": [
        {
          "Animation": "NPC/Livestock/Chicken/Animations/Default/Idle.blockyanim"
        }
      ]
    }
  }
}
```

## Model Properties

### Model & Texture

```json
{
  "Model": "NPC/Livestock/Chicken/Models/Model.blockymodel",
  "Texture": "NPC/Livestock/Chicken/Models/Texture.png"
}
```

- **`Model`** - Path to `.blockymodel` file
- **`Texture`** - Path to texture file

### HitBox

```json
{
  "HitBox": {
    "Max": { "X": 0.35, "Y": 0.9, "Z": 0.35 },
    "Min": { "X": -0.35, "Y": 0, "Z": -0.35 }
  }
}
```

Collision box dimensions (Min/Max corners).

### Scale

```json
{
  "MinScale": 0.8,
  "MaxScale": 1.2
}
```

Random scale range for visual variety.

### Eye Height

```json
{
  "EyeHeight": 0.7,
  "CrouchOffset": -0.2
}
```

- **`EyeHeight`** - Eye position height (for camera/targeting)
- **`CrouchOffset`** - Height offset when crouching

### Parent Inheritance

```json
{
  "Parent": "Skeleton",
  "Texture": "NPC/Undead/Skeleton/Models/Model_Textures/White.png"
}
```

Inherit from a parent model and override specific properties.

## Animation Sets

Animation sets define animations for different NPC states:

```json
{
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
          "Animation": "NPC/Beast/Emberwulf/Animations/Default/Walk.blockyanim",
          "SoundEventId": "SFX_Emberwulf_Walk",
          "Speed": 1
        }
      ]
    },
    "Run": {
      "Animations": [
        {
          "Animation": "NPC/Beast/Emberwulf/Animations/Default/Run.blockyanim",
          "SoundEventId": "SFX_Emberwulf_Run",
          "Speed": 1
        }
      ]
    }
  }
}
```

### Animation Properties

```json
{
  "Animation": "path/to/animation.blockyanim",
  "SoundEventId": "SFX_Walk",
  "Speed": 1.0,
  "BlendingDuration": 0.1,
  "Looping": true
}
```

- **`Animation`** - Path to animation file
- **`SoundEventId`** - Sound to play with animation
- **`Speed`** - Playback speed multiplier
- **`BlendingDuration`** - Transition blend time (seconds)
- **`Looping`** - Whether animation loops

### Common Animation Sets

| Set | Description |
|-----|-------------|
| `Idle` | Standing still |
| `Walk` | Walking forward |
| `WalkBackward` | Walking backward |
| `Run` | Running forward |
| `RunBackward` | Running backward |
| `Sprint` | Sprinting |
| `Crouch` | Crouching idle |
| `CrouchWalk` | Crouch walking |
| `Jump` | Jumping |
| `Fall` | Falling |
| `Alerted` | Alert/awareness |
| `Hurt` | Taking damage |
| `Death` | Death animation |
| `Sleep` | Sleeping |
| `Wake` | Waking up |
| `Spawn` | Spawn animation |
| `Swim*` | Swimming animations |
| `Fly*` | Flying animations |
| `IdlePassive` | Random idle behaviors |

### Passive Animation Timing

```json
{
  "IdlePassive": {
    "Animations": [
      { "Animation": "Flavor/Look_Left.blockyanim" },
      { "Animation": "Flavor/Howl.blockyanim" }
    ],
    "NextAnimationDelay": {
      "Min": 2,
      "Max": 10
    }
  }
}
```

## Default Attachments

Add cosmetics, accessories, or equipment to NPCs:

```json
{
  "DefaultAttachments": [
    {
      "Model": "NPC/Intelligent/Goblin/Models/Attachments/Cosmetics/Rag/Rag_Chest.blockymodel",
      "Texture": "NPC/Intelligent/Goblin/Models/Attachments/Cosmetics/Rag/Rag_Chest_Brown.png"
    },
    {
      "Model": "Cosmetics/Head/PirateCaptain.blockymodel",
      "Texture": "Cosmetics/Head/PirateCaptain_Textures/PirateCaptain_Greyscale_Texture.png",
      "GradientSet": "Faded_Leather",
      "GradientId": "Black"
    }
  ]
}
```

**Attachment Properties:**
- **`Model`** - Attachment model path
- **`Texture`** - Attachment texture
- **`GradientSet`** / **`GradientId`** - Color gradient for grayscale textures

## Particles

Attach particle effects to NPCs:

```json
{
  "Particles": [
    {
      "SystemId": "Fire_Crest",
      "TargetNodeName": "Fire_Spawner",
      "RotationOffset": {}
    },
    {
      "SystemId": "Campfire_New_Cartoon",
      "TargetNodeName": "Fire_Mouth_Spawner"
    }
  ]
}
```

**Particle Properties:**
- **`SystemId`** - Particle system ID
- **`TargetNodeName`** - Model bone/node to attach to
- **`RotationOffset`** - Rotation offset

## Camera Configuration

For mountable NPCs or camera control:

```json
{
  "Camera": {
    "Pitch": {
      "AngleRange": {
        "Max": 30,
        "Min": -45
      },
      "TargetNodes": ["Head"]
    },
    "Yaw": {
      "AngleRange": {
        "Max": 45,
        "Min": -45
      },
      "TargetNodes": ["Head"]
    }
  }
}
```

## Complete Examples

### Simple Livestock (Chicken)

```json
{
  "Model": "NPC/Livestock/Chicken/Models/Model.blockymodel",
  "Texture": "NPC/Livestock/Chicken/Models/Texture.png",
  "EyeHeight": 0.7,
  "HitBox": {
    "Max": { "X": 0.35, "Y": 0.9, "Z": 0.35 },
    "Min": { "X": -0.35, "Y": 0, "Z": -0.35 }
  },
  "MinScale": 0.8,
  "MaxScale": 1.2,
  "AnimationSets": {
    "Idle": {
      "Animations": [{ "Animation": "Default/Idle.blockyanim" }]
    },
    "Walk": {
      "Animations": [{ "Animation": "Default/Walk.blockyanim", "SoundEventId": "SFX_Chicken_Walk" }]
    },
    "Death": {
      "Animations": [{ "Animation": "Damage/Death.blockyanim", "Looping": false, "SoundEventId": "SFX_Chicken_Death" }]
    }
  }
}
```

### Intelligent NPC with Attachments (Goblin)

```json
{
  "Parent": "Goblin",
  "Model": "NPC/Intelligent/Goblin/Models/Model.blockymodel",
  "Texture": "NPC/Intelligent/Goblin/Models/Model_Textures/Moldy.png",
  "DefaultAttachments": [
    {
      "Model": "Attachments/Cosmetics/Rag/Rag_Chest.blockymodel",
      "Texture": "Attachments/Cosmetics/Rag/Rag_Chest_Brown.png"
    },
    {
      "Model": "Attachments/Cosmetics/Hair/Goblin_Hair.blockymodel",
      "Texture": "Attachments/Cosmetics/Hair/Goblin_Hair_Textures/BrownLight.png"
    }
  ]
}
```

### Beast with Particles (Emberwulf)

```json
{
  "Model": "NPC/Beast/Emberwulf/Models/Model.blockymodel",
  "Texture": "NPC/Beast/Emberwulf/Models/Texture.png",
  "EyeHeight": 1.4,
  "HitBox": {
    "Max": { "X": 0.9, "Y": 1.7, "Z": 0.9 },
    "Min": { "X": -0.9, "Y": 0, "Z": -0.9 }
  },
  "MinScale": 0.8,
  "MaxScale": 1.2,
  "Particles": [
    { "SystemId": "Fire_Crest", "TargetNodeName": "Fire_Spawner" },
    { "SystemId": "Campfire_New_Cartoon", "TargetNodeName": "Fire_Mouth_Spawner" }
  ],
  "AnimationSets": {
    "Howl": {
      "Animations": [
        { "Animation": "Flavor/Howl.blockyanim", "Looping": false, "SoundEventId": "SFX_Emberwulf_Alerted" }
      ]
    }
  }
}
```

### Projectile Model

```json
{
  "HitBox": {
    "Max": { "X": 0.075, "Y": 0.075, "Z": 0.075 },
    "Min": { "X": -0.075, "Y": -0.075, "Z": -0.075 }
  },
  "MinScale": 1,
  "MaxScale": 1,
  "Trails": [
    {
      "PositionOffset": { "X": 0.5, "Y": 0, "Z": 0 },
      "TargetNodeName": "Handle",
      "TrailId": "Arrow",
      "RotationOffset": { "Yaw": 90, "Pitch": 0, "Roll": 45 }
    }
  ],
  "DefaultAttachments": [
    {
      "Model": "Items/Weapons/Arrow/Arrow.blockymodel",
      "Texture": "Items/Weapons/Arrow/Arrow_Crude.png"
    }
  ],
  "Model": "Items/Projectiles/Projectile.blockymodel",
  "Texture": "Items/Projectiles/Projectile_default.png",
  "AnimationSets": {
    "Idle": {
      "Animations": [{ "Animation": "Items/Animations/Fly/Fly_Vertical.blockyanim" }]
    }
  }
}
```

---

**Previous:** [Objective System](168_Objective_System.md) | **Next:** [Off-Hand Item Stats](170_Offhand_Item_Stats.md)
