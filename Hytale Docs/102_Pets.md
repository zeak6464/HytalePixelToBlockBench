# Creating Pets

Learn how to create pets that follow players, including tameable animals and interactive companions.

## Overview

Pets are NPCs that follow and accompany players. There are two main approaches:
1. **Tameable Animals** - NPCs that follow players when they hold specific items (simple, uses templates)
2. **Interactive Pets** - NPCs that become pets when interacted with (advanced, custom instructions)

## Location
- Pet roles: `Server/NPC/Roles/Creature/` or `Server/NPC/Roles/Pets/`
- Pet models: `Server/Models/Pets/`

---

## Method 1: Tameable Animals (Simple)

### Overview

Animals follow players when they hold specific items. This method uses the `Template_Animal_Neutral` template with `LovedItems` and `AttractiveItems` properties.

### Basic Tameable Pet Structure

Create `Server/NPC/Roles/Creature/MyCustom/MyCustom_Pet.json`:

```json
{
  "Type": "Variant",
  "Reference": "Template_Animal_Neutral",
  "Modify": {
    "Appearance": "MyCustom_Animal",
    "LovedItems": [
      "Food_Apple",
      "Plant_Crop_Carrot_Item"
    ],
    "AttractiveItems": [
      "Tool_Feedbag"
    ],
    "ChanceToTurnFriendly": 30,
    "ChanceToTurnFriendlyWithAttractiveItem": 60,
    "WeightGreet": 1,
    "WeightFollowItem": 20,
    "WeightFollow": 40,
    "WeightIgnore": 10,
    "MaxHealth": 50,
    "MaxSpeed": 8,
    "NameTranslationKey": {
      "Compute": "NameTranslationKey"
    }
  },
  "Parameters": {
    "NameTranslationKey": {
      "Value": "server.npcRoles.MyCustom_Pet.name",
      "Description": "Translation key for NPC name display"
    }
  }
}
```

### Key Properties for Tameable Pets

#### LovedItems

```json
{
  "LovedItems": [
    "Plant_Crop_Carrot_Item",
    "Food_Apple"
  ]
}
```

- Items that **guarantee** the NPC will follow the player
- NPC becomes friendly when player holds these items
- Spawns heart particles when following
- More reliable than `AttractiveItems`

#### AttractiveItems

```json
{
  "AttractiveItems": [
    "Tool_Feedbag"
  ]
}
```

- Items that have a **chance** to make NPC follow
- Less reliable than `LovedItems`
- NPC will be interested but may lose interest after timeout
- Useful for items like feedbags or general food

#### Friendliness Properties

```json
{
  "ChanceToTurnFriendly": 30,
  "ChanceToTurnFriendlyWithAttractiveItem": 60,
  "FriendlyOverrideTimeWithAttractiveItem": 10
}
```

- **`ChanceToTurnFriendly`** - % chance to become friendly normally (0-100)
- **`ChanceToTurnFriendlyWithAttractiveItem`** - % chance with attractive item held
- **`FriendlyOverrideTimeWithAttractiveItem`** - How long friendly attitude lasts (seconds)

#### Behavior Weights

```json
{
  "WeightGreet": 1,
  "WeightFollowItem": 20,
  "WeightFollow": 40,
  "WeightIgnore": 10
}
```

Controls how often NPC performs behaviors:
- **`WeightGreet`** - How often to greet players
- **`WeightFollowItem`** - How often to follow item holders
- **`WeightFollow`** - How often to follow friendly players
- **`WeightIgnore`** - How often to ignore players

Higher weights = more likely behavior. These are relative weights used in random selection.

#### Timid Property

```json
{
  "Timid": true
}
```

Makes the pet more likely to flee from threats (hostile NPCs, players with weapons).

### Complete Example: Tameable Rabbit

`Server/NPC/Roles/Creature/Livestock/Bunny.json`:

```json
{
  "Type": "Variant",
  "Reference": "Template_Animal_Neutral",
  "Modify": {
    "Appearance": "Bunny",
    "FlockArray": [
      "Bunny",
      "Rabbit"
    ],
    "LovedItems": [
      "Plant_Crop_Carrot_Item"
    ],
    "DropList": "Drop_Bunny",
    "MaxHealth": 25,
    "MaxSpeed": 5,
    "AlertedActionRange": 10,
    "ChanceToTurnFriendly": 50,
    "ChanceToTurnFriendlyWithAttractiveItem": 80,
    "WeightGreet": 1,
    "WeightFollowItem": 40,
    "WeightFollow": 40,
    "WeightIgnore": 10,
    "Timid": true,
    "IsMemory": true,
    "MemoriesNameOverride": "Bunny",
    "NameTranslationKey": {
      "Compute": "NameTranslationKey"
    }
  },
  "Parameters": {
    "NameTranslationKey": {
      "Value": "server.npcRoles.Bunny.name",
      "Description": "Translation key for NPC name display"
    }
  }
}
```

Rabbits follow players holding carrots and spawn heart particles.

---

## Method 2: Interactive Pets (Advanced)

### Overview

Interactive pets become companions when interacted with. They use custom states and instructions for more control over behavior.

### Basic Interactive Pet Structure

Create `Server/NPC/Roles/Pets/MyCustom_Pet.json`:

```json
{
  "Type": "Generic",
  "Appearance": "Corgi",
  "StartState": "Idle",
  "MotionControllerList": [
    {
      "Type": "Walk",
      "MaxWalkSpeed": 3,
      "RunThreshold": 0.7,
      "RunThresholdRange": 0.1,
      "Gravity": 10,
      "MaxFallSpeed": 20,
      "Acceleration": 10
    }
  ],
  "MaxHealth": 50,
  "Instructions": [
    {
      "Instructions": [
        {
          "Sensor": {
            "Type": "State",
            "State": "Idle"
          },
          "Instructions": [
            {
              "Sensor": {
                "Type": "Player",
                "Range": 15,
                "LockOnTarget": true
              },
              "BodyMotion": {
                "Type": "Seek",
                "SlowDownDistance": 5,
                "StopDistance": 3,
                "RelativeSpeed": 1
              }
            }
          ]
        },
        {
          "Sensor": {
            "Type": "State",
            "State": "Pet"
          },
          "Instructions": [
            {
              "Sensor": {
                "Type": "Target"
              },
              "BodyMotion": {
                "Type": "MaintainDistance",
                "DesiredDistanceRange": [2, 3],
                "RelativeForwardsSpeed": 1
              }
            }
          ]
        }
      ]
    }
  ],
  "InteractionInstruction": {
    "Instructions": [
      {
        "Sensor": {
          "Type": "HasInteracted"
        },
        "Instructions": [
          {
            "Sensor": {
              "Type": "Not",
              "Sensor": {
                "Type": "State",
                "State": "Pet"
              }
            },
            "Actions": [
              {
                "Type": "LockOnInteractionTarget"
              },
              {
                "Type": "State",
                "State": "Pet"
              }
            ]
          }
        ]
      }
    ]
  },
  "NameTranslationKey": "server.npcRoles.MyCustom_Pet.name"
}
```

### Key Components for Interactive Pets

#### InteractionInstruction

Handles what happens when the player interacts with the NPC:

```json
{
  "InteractionInstruction": {
    "Instructions": [
      {
        "Sensor": {
          "Type": "HasInteracted"
        },
        "Instructions": [
          {
            "Sensor": {
              "Type": "Not",
              "Sensor": {
                "Type": "State",
                "State": "Pet"
              }
            },
            "Actions": [
              {
                "Type": "LockOnInteractionTarget"
              },
              {
                "Type": "State",
                "State": "Pet"
              }
            ]
          }
        ]
      }
    ]
  }
}
```

- **`HasInteracted`** - Triggers when player interacts with NPC
- **`LockOnInteractionTarget`** - Locks onto the player who interacted
- **`State: "Pet"`** - Enters pet state to enable following

#### Pet State Following

When in "Pet" state, the pet follows its locked target:

```json
{
  "Sensor": {
    "Type": "State",
    "State": "Pet"
  },
  "Instructions": [
    {
      "Sensor": {
        "Type": "Target"
      },
      "BodyMotion": {
        "Type": "MaintainDistance",
        "DesiredDistanceRange": [2, 3],
        "RelativeForwardsSpeed": 1,
        "RelativeBackwardsSpeed": 1
      }
    }
  ]
}
```

- **`MaintainDistance`** - Maintains distance from target (pet follows player)
- **`DesiredDistanceRange`** - Min/max distance to maintain (blocks)
- **`RelativeForwardsSpeed`** - Speed when moving forward (1 = match player)
- **`RelativeBackwardsSpeed`** - Speed when moving backward

#### Seek Behavior (Optional)

For pets that approach players before becoming pets:

```json
{
  "Sensor": {
    "Type": "Player",
    "Range": 15,
    "LockOnTarget": true
  },
  "BodyMotion": {
    "Type": "Seek",
    "SlowDownDistance": 5,
    "StopDistance": 3,
    "RelativeSpeed": 1
  }
}
```

- **`Seek`** - Moves toward target
- **`SlowDownDistance`** - Distance at which to slow down
- **`StopDistance`** - Distance at which to stop

---

## Pet Model Configuration

### Basic Pet Model

Create `Server/Models/Pets/MyCustom_Pet.json`:

```json
{
  "Model": "NPC/Pets/MyCustom_Pet/Models/Model.blockymodel",
  "Texture": "NPC/Pets/MyCustom_Pet/Models/Texture.png",
  "EyeHeight": 0.7,
  "HitBox": {
    "Max": {
      "X": 0.3,
      "Y": 0.9,
      "Z": 0.3
    },
    "Min": {
      "X": -0.3,
      "Y": 0,
      "Z": -0.3
    }
  },
  "AnimationSets": {
    "Idle": {
      "Animations": [
        {
          "Animation": "NPC/Pets/MyCustom_Pet/Animations/Idle.blockyanim"
        }
      ]
    },
    "Walk": {
      "Animations": [
        {
          "Animation": "NPC/Pets/MyCustom_Pet/Animations/Walk.blockyanim"
        }
      ]
    },
    "Run": {
      "Animations": [
        {
          "Animation": "NPC/Pets/MyCustom_Pet/Animations/Run.blockyanim"
        }
      ]
    },
    "Jump": {
      "Animations": [
        {
          "Animation": "NPC/Pets/MyCustom_Pet/Animations/Jump.blockyanim",
          "BlendingDuration": 0.1,
          "Looping": false
        }
      ]
    },
    "Alerted": {
      "Animations": [
        {
          "Animation": "NPC/Pets/MyCustom_Pet/Animations/Alerted.blockyanim",
          "BlendingDuration": 0.1
        }
      ]
    },
    "Hurt": {
      "Animations": [
        {
          "Animation": "NPC/Pets/MyCustom_Pet/Animations/Hurt.blockyanim",
          "BlendingDuration": 0.1,
          "Looping": false
        }
      ]
    },
    "Death": {
      "Animations": [
        {
          "Animation": "NPC/Pets/MyCustom_Pet/Animations/Death.blockyanim",
          "Looping": false
        }
      ]
    }
  }
}
```

### Required Animations

Pets should have these animations:
- **`Idle`** - Standing still (looping)
- **`Walk`** - Walking (looping)
- **`Run`** - Running (looping)
- **`Jump`** - Jumping (non-looping)
- **`Alerted`** - Alert/curious state (optional)
- **`Hurt`** - Taking damage (optional)
- **`Death`** - Death animation (optional)

---

## Following Behaviors

### MaintainDistance

Best for pets that follow at a specific distance:

```json
{
  "BodyMotion": {
    "Type": "MaintainDistance",
    "DesiredDistanceRange": [2, 3],
    "StrafingDurationRange": [1, 1],
    "StrafingFrequencyRange": [2, 2],
    "MoveThreshold": 0.2,
    "RelativeForwardsSpeed": 1,
    "RelativeBackwardsSpeed": 1
  }
}
```

- **`DesiredDistanceRange`** - Min/max distance from target (blocks)
- **`StrafingDurationRange`** - How long to strafe before adjusting
- **`StrafingFrequencyRange`** - How often to strafe
- **`MoveThreshold`** - Minimum distance change to trigger movement
- **`RelativeForwardsSpeed`** - Speed multiplier when moving forward (1 = match player speed)
- **`RelativeBackwardsSpeed`** - Speed multiplier when moving backward

### Seek

For pets that approach players:

```json
{
  "BodyMotion": {
    "Type": "Seek",
    "SlowDownDistance": 5,
    "StopDistance": 3,
    "RelativeSpeed": 1
  }
}
```

- **`SlowDownDistance`** - Distance at which to start slowing down
- **`StopDistance`** - Distance at which to stop
- **`RelativeSpeed`** - Speed multiplier (1 = match player speed)

### Teleport

For pets that teleport to player (use sparingly):

```json
{
  "BodyMotion": {
    "Type": "Teleport"
  }
}
```

Instantly teleports to target location. Use only for special cases.

---

## Flocking Pets

### Overview

Pets can flock together, following a leader (the player's pet). Other NPCs can join the flock.

### Flocking Pet Configuration

```json
{
  "Type": "Generic",
  "Appearance": "Wolf_Black",
  "FlockAllowedNPC": ["Test_Dog_Tame"],
  "FlockCanLead": true,
  "FlockInfluenceRange": 4,
  "Instructions": [
    {
      "Instructions": [
        {
          "Sensor": {
            "Type": "And",
            "Sensors": [
              {
                "Type": "FlockLeader"
              },
              {
                "Type": "Self",
                "Filters": [
                  {
                    "Type": "Flock",
                    "FlockStatus": "Member"
                  }
                ]
              }
            ]
          },
          "BodyMotion": {
            "Type": "Seek",
            "SlowDownDistance": 7,
            "StopDistance": 3
          }
        }
      ]
    },
    {
      "Instructions": [
        {
          "Sensor": {
            "Type": "Player",
            "Range": 3,
            "Filters": [
              {
                "Type": "Flock",
                "FlockStatus": "NotMember"
              }
            ]
          },
          "Actions": [
            {
              "Type": "JoinFlock",
              "ForceJoin": true
            }
          ]
        }
      ]
    }
  ]
}
```

### Flocking Properties

- **`FlockAllowedNPC`** - Array of NPC IDs that can join the flock
- **`FlockCanLead`** - Whether this NPC can be a flock leader
- **`FlockInfluenceRange`** - Range at which flock members follow leader

---

## Comparison: Tameable vs Interactive Pets

| Feature | Tameable Animals | Interactive Pets |
|---------|------------------|------------------|
| **Activation** | Hold specific item | Interact with NPC |
| **Persistence** | Temporary (while item held) | Permanent (until state changes) |
| **Complexity** | Simple (uses template) | Complex (custom instructions) |
| **Best For** | Livestock, wild animals | True companions, pets |
| **Following** | Automatic (template handles it) | Custom (MaintainDistance/Seek) |
| **State Management** | Handled by template | Manual state management |

---

## Tips for Creating Pets

1. **Start with Template_Animal_Neutral** - For simple tameable pets
2. **Use LovedItems** - Guaranteed following behavior (more reliable than AttractiveItems)
3. **Set appropriate weights** - Balance follow vs ignore behaviors for natural feel
4. **Pet state management** - Interactive pets need careful state handling
5. **Follow distance** - Adjust `DesiredDistanceRange` for comfortable following (2-5 blocks typical)
6. **Speed matching** - Set `RelativeSpeed: 1` to match player movement
7. **Animations** - Ensure pet models have all necessary animations (Idle, Walk, Run, Jump minimum)
8. **Health balance** - Pets should have reasonable health (25-100 typical)
9. **Timid pets** - Use `Timid: true` for skittish animals
10. **Memory system** - Use `IsMemory: true` for pets that remember players

---

## Complete Example: Interactive Pet

`Server/NPC/Roles/_Core/Tests/Test_Pet.json`:

Complete example showing pet interaction, state management, and following behavior with multiple states for complex pet behavior.

---

**Previous:** [Guns & Spellbooks](101_Guns_and_Spellbooks.md) | **Next:** Back to [README](README.md)
