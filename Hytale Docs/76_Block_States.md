# Block States

Learn how to configure block states that allow blocks to change appearance and behavior dynamically.

## Overview

Block states allow blocks to have multiple visual and functional configurations. States can change models, animations, interactions, and properties. Common uses include doors, traps, crops, and interactive blocks.

## Location
Block states are configured in `BlockType.State.Definitions`.

## Basic Block State Structure

```json
{
  "BlockType": {
    "State": {
      "Id": "mycustom",
      "Definitions": {
        "default": {
          "CustomModel": "Blocks/MyCustom/Default.blockymodel"
        },
        "activated": {
          "CustomModel": "Blocks/MyCustom/Activated.blockymodel",
          "Interactions": {
            "Use": {
              "Interactions": [
                {
                  "Type": "ChangeState",
                  "Changes": {
                    "activated": "default"
                  }
                }
              ]
            }
          }
        }
      }
    }
  }
}
```

## State Properties

### Default State

```json
{
  "default": {
    "CustomModel": "...",
    "Interactions": { ... }
  }
}
```

The initial state when the block is placed.

### State-Specific Properties

Each state can override:
- **Models** - `CustomModel`, `CustomModelTexture`
- **Animations** - `CustomModelAnimation`
- **Interactions** - `Interactions`, `InteractionSoundEventId`
- **Particles** - `Particles`
- **Sound** - `AmbientSoundEventId`, `SoundEventId`
- **Hitboxes** - `HitboxType`
- **Gathering** - `Gathering` properties
- **Loops** - `Looping` for animations

## Changing States

### ChangeState Interaction

```json
{
  "Type": "ChangeState",
  "Changes": {
    "default": "activated",
    "activated": "default"
  }
}
```

Switches between states. Can be bidirectional.

### Runtime Changes

```json
{
  "Type": "ChangeState",
  "Changes": {
    "default": "On"
  },
  "RunTime": 3,
  "Next": {
    "Type": "ChangeState",
    "Changes": {
      "On": "default"
    }
  }
}
```

Changes state temporarily and reverts after `RunTime` seconds.

### UpdateBlockState

```json
{
  "Type": "ChangeState",
  "Changes": {
    "default": "On"
  },
  "UpdateBlockState": true
}
```

Forces block state update visually.

## Complete Example: Trap with States

```json
{
  "BlockType": {
    "State": {
      "Definitions": {
        "default": {
          "Interactions": {
            "CollisionEnter": {
              "Interactions": [
                {
                  "Type": "ApplyEffect",
                  "EffectId": { ... },
                  "Next": {
                    "Type": "ChangeState",
                    "Changes": {
                      "default": "Closed"
                    }
                  }
                }
              ]
            }
          }
        },
        "Closed": {
          "CustomModelAnimation": "Blocks/Trap/Closed.blockyanim",
          "Interactions": {
            "Use": {
              "Interactions": [
                {
                  "Type": "ChangeState",
                  "Changes": {
                    "Closed": "default"
                  }
                }
              ]
            }
          }
        }
      }
    }
  }
}
```

## State Animation

```json
{
  "Closed": {
    "CustomModelAnimation": "Blocks/Trap/Closed.blockyanim",
    "Looping": false
  }
}
```

Animations that play when in this state.

## Tips for Block States

1. **Default state** - Always define a `default` state
2. **State changes** - Use `ChangeState` interactions to switch states
3. **Temporary states** - Use `RunTime` for timed state changes
4. **State-specific interactions** - Each state can have different interactions
5. **Animations** - Use model animations for smooth transitions

---

**Previous:** [Block Variants](75_Block_Variants.md) | **Next:** [Block Migrations](77_Block_Migrations.md)
