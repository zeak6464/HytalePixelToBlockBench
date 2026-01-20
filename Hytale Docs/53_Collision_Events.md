# Collision Events

Learn how to create collision triggers for blocks that react when entities walk on them, including player-only collision pads.

## Overview

Collision events allow blocks to react when entities (players or NPCs) collide with them. There are two main collision types: `CollisionEnter` (triggers once when entering) and `Collision` (triggers continuously while colliding). You can use special hitbox types like `Pad_Portal` to make collisions player-only.

## Location
- Block interactions: `Server/Item/Items/` (in block definitions)
- Hitbox definitions: `Server/Item/Block/Hitboxes/`

## Example from Game Files

### Player-Only Portal Hitbox

From `Server/Item/Block/Hitboxes/Furniture/Pad_Portal.json`:

```1:16:Server/Item/Block/Hitboxes/Furniture/Pad_Portal.json
{
  "Boxes": [
    {
      "Min": {
        "X": -0.5,
        "Y": 0,
        "Z": -0.5
      },
      "Max": {
        "X": 1.5,
        "Y": 0.45,
        "Z": 1.5
      }
    }
  ]
}
```

This shows the hitbox definition for `Pad_Portal`, which only collides with players, used in portal pads and launchpads.

## Collision Event Types

### CollisionEnter

Triggers **once** when an entity first enters the collision area:

```json
{
  "Interactions": {
    "CollisionEnter": {
      "Interactions": [
        {
          "Type": "PortalReturn"
        }
      ]
    }
  }
}
```

### Collision

Triggers **continuously** while an entity is colliding (useful for traps, damage over time):

```json
{
  "Interactions": {
    "Collision": {
      "Cooldown": {
        "Id": "DamageCooldown",
        "Cooldown": 1.0
      },
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": {
            "DamageCalculator": {
              "BaseDamage": {
                "Physical": 10
              }
            }
          }
        }
      ]
    }
  }
}
```

## Player-Only Collision

### Using Pad_Portal Hitbox Type

The `Pad_Portal` hitbox type makes collision events **player-only**. NPCs and other entities will not trigger the collision.

```json
{
  "BlockType": {
    "HitboxType": "Pad_Portal",
    "Interactions": {
      "CollisionEnter": {
        "Interactions": [
          {
            "Type": "YourAction"
          }
        ]
      }
    }
  }
}
```

**Key Point:** When you use `"HitboxType": "Pad_Portal"`, the `CollisionEnter` or `Collision` interactions will **only trigger for players**, not NPCs or other entities.

### Pad_Portal Hitbox Definition

The `Pad_Portal` hitbox is defined in `Server/Item/Block/Hitboxes/Furniture/Pad_Portal.json`:

```json
{
  "Boxes": [
    {
      "Min": {
        "X": -0.5,
        "Y": 0,
        "Z": -0.5
      },
      "Max": {
        "X": 1.5,
        "Y": 0.45,
        "Z": 1.5
      }
    }
  ]
}
```

This creates a flat pad (from Y=0 to Y=0.45) that only detects players.

## Collision with Cooldowns

Add cooldowns to prevent collision from triggering too frequently:

```json
{
  "CollisionEnter": {
    "Cooldown": {
      "Id": "CollisionEnter",
      "Cooldown": 5.0
    },
    "Interactions": [
      {
        "Type": "ApplyForce",
        "Force": 65
      }
    ]
  }
}
```

**Cooldown Properties:**
- **`Id`** - Unique identifier for the cooldown
- **`Cooldown`** - Time in seconds between triggers

## Complete Examples

### Example 1: Player-Only Portal (CollisionEnter)

```json
{
  "TranslationProperties": {
    "Name": "server.items.Portal_MyCustom.name"
  },
  "Categories": ["Blocks.Portals"],
  "BlockType": {
    "DrawType": "Model",
    "Material": "Solid",
    "Opacity": "Transparent",
    "CustomModel": "Blocks/Miscellaneous/Platform_Magic_Exit.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/Miscellaneous/Platform_Magic_Blue2.png",
        "Weight": 1
      }
    ],
    "HitboxType": "Pad_Portal",
    "Interactions": {
      "CollisionEnter": {
        "Interactions": [
          {
            "Type": "PortalReturn",
            "Next": {
              "Type": "Simple",
              "Effects": {
                "LocalSoundEventId": "SFX_Portal_Neutral_Teleport_Local"
              }
            }
          }
        ]
      }
    }
  },
  "Tags": {
    "Type": ["Portal"]
  }
}
```

### Example 2: Damage Trap (Collision - All Entities)

```json
{
  "BlockType": {
    "HitboxType": "Block_Three_Quarter",
    "Interactions": {
      "Collision": {
        "Cooldown": {
          "Id": "DamageCooldown",
          "Cooldown": 1.0
        },
        "Interactions": [
          {
            "Type": "Condition",
            "RequiredGameMode": "Adventure",
            "Next": {
              "Type": "ApplyEffect",
              "EffectId": {
                "Locale": "spikes",
                "Duration": 0.1,
                "OverlapBehavior": "Overwrite",
                "DamageCalculator": {
                  "BaseDamage": {
                    "Physical": 10
                  }
                }
              }
            }
          }
        ]
      }
    }
  }
}
```

This trap damages **all entities** that walk on it (players and NPCs).

### Example 3: Player-Only Launch Pad

```json
{
  "BlockType": {
    "HitboxType": "Pad_Portal",
    "Interactions": {
      "CollisionEnter": {
        "Cooldown": {
          "Cooldown": 5.0
        },
        "Interactions": [
          {
            "Type": "ApplyForce",
            "Direction": {
              "X": 0,
              "Y": 1,
              "Z": 0
            },
            "Force": 65,
            "VelocityConfig": {
              "AirResistance": 0.97,
              "GroundResistance": 0.94,
              "Threshold": 3.0,
              "Style": "Exp"
            },
            "ChangeVelocityType": "Set"
          }
        ]
      }
    }
  }
}
```

Only players will trigger the launch pad.

### Example 4: Snapjaw Trap (CollisionEnter + State Change)

```json
{
  "BlockType": {
    "State": {
      "Definitions": {
        "Closed": {
          "Interactions": {
            "CollisionEnter": {
              "Interactions": [
                {
                  "Type": "Simple"
                }
              ]
            },
            "Collision": {
              "Cooldown": {
                "Id": "Collision",
                "Cooldown": 0
              },
              "Interactions": [
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
              ]
            }
          }
        }
      }
    },
    "Interactions": {
      "CollisionEnter": {
        "Cooldown": {
          "Id": "CollisionEnter",
          "Cooldown": 0
        },
        "Interactions": [
          {
            "Type": "Condition",
            "RequiredGameMode": "Adventure",
            "Next": {
              "Type": "ApplyEffect",
              "EffectId": {
                "Duration": 0.2,
                "OverlapBehavior": "Overwrite",
                "ApplicationEffects": {
                  "HorizontalSpeedMultiplier": 0
                },
                "DamageCalculator": {
                  "BaseDamage": {
                    "Physical": 25
                  }
                }
              },
              "Next": {
                "Type": "ChangeState",
                "Changes": {
                  "default": "Closed"
                }
              }
            }
          }
        ]
      }
    }
  }
}
```

## Hitbox Types

### Common Hitbox Types

Different hitbox types determine collision shape and behavior:

| Hitbox Type | Description | Player-Only |
|-------------|-------------|-------------|
| **`Pad_Portal`** | Flat pad (Y: 0-0.45) | **Yes** |
| **`Block_Half`** | Half-height block | No |
| **`Block_Three_Quarter`** | 3/4 height block | No |
| **`Block_Full`** | Full block | No |

### Hitbox Type Structure

Hitboxes are defined in `Server/Item/Block/Hitboxes/`:

```json
{
  "Boxes": [
    {
      "Min": {
        "X": -0.5,
        "Y": 0,
        "Z": -0.5
      },
      "Max": {
        "X": 1.5,
        "Y": 0.45,
        "Z": 1.5
      }
    }
  ]
}
```

**Coordinates:**
- **X, Y, Z** - Relative to block center
- **Min** - Minimum corner of the box
- **Max** - Maximum corner of the box

## Collision vs CollisionEnter

### When to Use Each

| Event | Trigger Frequency | Best For |
|-------|-------------------|----------|
| **CollisionEnter** | Once when entering | Portals, teleportation, one-time effects |
| **Collision** | Continuously while colliding | Damage over time, traps, continuous effects |

### CollisionEnter Example

```json
{
  "CollisionEnter": {
    "Interactions": [
      {
        "Type": "PortalReturn"
      }
    ]
  }
}
```

Triggers once when entity steps on the block.

### Collision Example

```json
{
  "Collision": {
    "Cooldown": {
      "Id": "Damage",
      "Cooldown": 1.0
    },
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": {
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 5
            }
          }
        }
      }
    ]
  }
}
```

Triggers every 1 second while entity is on the block.

## Player-Only Patterns

### Pattern 1: Portal Pad

```json
{
  "HitboxType": "Pad_Portal",
  "Interactions": {
    "CollisionEnter": {
      "Interactions": [
        {
          "Type": "TeleportConfigInstance"
        }
      ]
    }
  }
}
```

### Pattern 2: Launch Pad

```json
{
  "HitboxType": "Pad_Portal",
  "Interactions": {
    "CollisionEnter": {
      "Interactions": [
        {
          "Type": "ApplyForce",
          "Direction": { "Y": 1 },
          "Force": 65
        }
      ]
    }
  }
}
```

### Pattern 3: Exit Trigger

```json
{
  "HitboxType": "Pad_Portal",
  "Interactions": {
    "CollisionEnter": {
      "Interactions": [
        {
          "Type": "ExitInstance"
        }
      ]
    }
  }
}
```

## All-Entity Patterns

### Pattern 1: Damage Trap

```json
{
  "HitboxType": "Block_Three_Quarter",
  "Interactions": {
    "Collision": {
      "Cooldown": {
        "Id": "Damage",
        "Cooldown": 1.0
      },
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": {
            "DamageCalculator": {
              "BaseDamage": {
                "Physical": 10
              }
            }
          }
        }
      ]
    }
  }
}
```

### Pattern 2: Slow Effect

```json
{
  "Interactions": {
    "Collision": {
      "Cooldown": {
        "Id": "Slow",
        "Cooldown": 0.5
      },
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": {
            "Duration": 0.5,
            "OverlapBehavior": "Extend",
            "ApplicationEffects": {
              "SpeedMultiplier": 0.5
            }
          }
        }
      ]
    }
  }
}
```

## Collision with Conditions

Add conditions to collision events:

```json
{
  "CollisionEnter": {
    "Interactions": [
      {
        "Type": "Condition",
        "RequiredGameMode": "Adventure",
        "Next": {
          "Interactions": [
            {
              "Type": "ApplyEffect",
              "EffectId": "MyEffect"
            }
          ]
        }
      }
    ]
  }
}
```

## Tips for Creating Collision Events

1. **Use Pad_Portal for player-only** - `HitboxType: "Pad_Portal"` makes collisions player-only
2. **Use CollisionEnter for one-time** - Portals, teleportation, single triggers
3. **Use Collision for continuous** - Damage over time, traps, ongoing effects
4. **Add cooldowns** - Prevent collision from triggering too frequently
5. **Test hitbox shapes** - Different hitbox types affect collision detection
6. **Combine with conditions** - Add game mode checks or other conditions
7. **Use state-based collision** - Change collision behavior based on block state

---

**Previous:** [Farming Modifiers](52_Farming_Modifiers.md) | **Next:** Back to [README](../README.md)
