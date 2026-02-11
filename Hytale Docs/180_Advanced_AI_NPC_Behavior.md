# Advanced AI & NPC Behavior Systems

Master NPC AI in Hytale: state machines, sensors, behavior trees, combat AI, and complex decision-making.

## Overview

Hytale's NPC AI system is built on three core concepts:
1. **States** - What the NPC is doing (Idle, Chase, Combat, Trading)
2. **Sensors** - How the NPC perceives the world (Player nearby? Taking damage?)
3. **Instructions** - What the NPC does in response (Attack, Flee, Trade)

This guide covers advanced AI techniques used in Hytale's most sophisticated NPCs.

All examples from actual game files.

---

## State Machine Architecture

### Understanding NPC States

States represent what an NPC is currently doing. From `Template_Trork_Melee.json`:

```json
{
  "StartState": "Idle",
  "BusyStates": [ "Chase", "Search", "ReturnHome" ]
}
```

**Common States:**
- **Idle** - Default, peaceful behavior
- **Alerted** - Noticed something, investigating
- **Chase** - Pursuing a target
- **Combat** - Fighting
- **Search** - Lost target, looking around
- **Flee** - Running away
- **ReturnHome** - Going back to spawn point
- **$Interaction** - Trading/talking with player

---

### State Machine: Kweebec Merchant

From `Kweebec_Merchant.json` - Complete 3-state system:

```
    Idle
     |
     | (Player within 8m)
     v
  Watching
     |
     | (Player clicks)
     v
$Interaction
     |
     | (Shop closes)
     v
  Watching
     |
     | (Player leaves 12m)
     v
    Idle
```

**Implementation:**

```json
{
  "StartState": "Idle",
  "BusyStates": [ "$Interaction" ],
  "Instructions": [
    {
      "Instructions": [
        {
          "$Comment": "Idle state - no player nearby",
          "Sensor": {
            "Type": "State",
            "State": "Idle"
          },
          "Instructions": [
            {
              "Sensor": {
                "Type": "Player",
                "Range": 8
              },
              "Actions": [
                {
                  "Type": "PlayAnimation",
                  "Slot": "Status",
                  "Animation": "Wave"
                },
                {
                  "Type": "State",
                  "State": "Watching"
                }
              ]
            }
          ]
        },
        {
          "$Comment": "Watching state - player is nearby",
          "Sensor": {
            "Type": "State",
            "State": "Watching"
          },
          "Instructions": [
            {
              "Sensor": {
                "Type": "Player",
                "Range": 12
              },
              "HeadMotion": {
                "Type": "Watch"
              }
            },
            {
              "Sensor": {
                "Type": "Not",
                "Sensor": {
                  "Type": "Player",
                  "Range": 12
                }
              },
              "Actions": [
                {
                  "Type": "State",
                  "State": "Idle"
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

**State transitions:**
1. **Idle** - NPC stands still
2. Player approaches (8m) → Wave animation + transition to **Watching**
3. **Watching** - NPC's head tracks player
4. Player leaves (12m) → transition back to **Idle**

---

### State Machine: Combat NPC

From `Template_Trork_Melee.json` - Complex combat flow:

```
    Idle
     |
     | (Sees player)
     v
   Alerted
     |
     | (Confirm threat)
     v
    Chase
     |
     | (Get close)
     v
   Combat
     |
     | (Target dies/escapes)
     v
   Search
     |
     | (Timeout / Too far)
     v
 ReturnHome
     |
     v
    Idle
```

**Key Parameters:**

```json
{
  "Parameters": {
    "ViewRange": {
      "Value": 15,
      "Description": "View range"
    },
    "ViewSector": {
      "Value": 180,
      "Description": "View sector (FOV)"
    },
    "HearingRange": {
      "Value": 8,
      "Description": "Hearing range"
    },
    "AlertedRange": {
      "Value": 50,
      "Description": "Extended range when alerted"
    },
    "LeashDistance": {
      "Value": 30,
      "Description": "Max distance from spawn"
    },
    "AttackDistance": {
      "Value": 3,
      "Description": "Attack range"
    }
  }
}
```

---

## Advanced Sensors

### Sensor Types

#### 1. Player Sensor

```json
{
  "Sensor": {
    "Type": "Player",
    "Range": 10
  }
}
```

**Detects:** Any player within range  
**Use:** Detection, triggers, proximity-based behavior

---

#### 2. Target Sensor

```json
{
  "Sensor": {
    "Type": "Target",
    "Range": 5
  }
}
```

**Detects:** Current target within range  
**Use:** Combat, tracking specific entity

**Difference from Player:**
- **Player** - Detects ANY player
- **Target** - Checks current locked target (combat-specific)

---

#### 3. State Sensor

```json
{
  "Sensor": {
    "Type": "State",
    "State": "Combat"
  }
}
```

**Detects:** NPC is in specific state  
**Use:** State-specific behaviors

---

#### 4. Beacon Sensor

```json
{
  "Sensor": {
    "Type": "Beacon",
    "Message": "TrorkEnemySighted",
    "TargetSlot": "LockedTarget"
  }
}
```

**Detects:** Messages from other NPCs  
**Use:** Group coordination, alerts

**Example:** Trork sees player → sends "TrorkEnemySighted" → nearby Trorks converge

---

#### 5. CanInteract Sensor

```json
{
  "Sensor": {
    "Type": "CanInteract",
    "ViewSector": 180
  }
}
```

**Detects:** Player looking at NPC (within view sector)  
**Use:** Show interaction prompts

---

#### 6. HasInteracted Sensor

```json
{
  "Sensor": {
    "Type": "HasInteracted"
  }
}
```

**Detects:** Player pressed interact button  
**Use:** Trigger shop/dialogue

---

### Sensor Modifiers

#### Not

```json
{
  "Sensor": {
    "Type": "Not",
    "Sensor": {
      "Type": "Player",
      "Range": 10
    }
  }
}
```

**Inverts sensor** - True when NO player within 10m

---

#### And

```json
{
  "Sensor": {
    "Type": "And",
    "Sensors": [
      {
        "Type": "Player",
        "Range": 10
      },
      {
        "Type": "State",
        "State": "Idle"
      }
    ]
  }
}
```

**All must be true** - Player nearby AND in Idle state

---

#### Or

```json
{
  "Sensor": {
    "Type": "Or",
    "Sensors": [
      {
        "Type": "State",
        "State": "Chase"
      },
      {
        "Type": "State",
        "State": "Combat"
      }
    ]
  }
}
```

**Any can be true** - Chasing OR fighting

---

### Complex Sensor Chains

#### Example 1: Beacon Alert System

From `Template_Trork_Melee.json`:

```json
{
  "Sensor": {
    "Type": "And",
    "Sensors": [
      {
        "Type": "Beacon",
        "Message": "TrorkEnemySighted",
        "TargetSlot": "LockedTarget"
      },
      {
        "Type": "Not",
        "Sensor": {
          "Type": "Or",
          "Sensors": [
            {
              "Type": "State",
              "State": "Converge"
            },
            {
              "Type": "State",
              "State": "Chase"
            }
          ]
        }
      }
    ]
  },
  "Actions": [
    {
      "Type": "State",
      "State": "Converge"
    }
  ]
}
```

**Logic:**
- **IF** beacon message received
- **AND** not already converging/chasing
- **THEN** start converging

**Result:** Multiple Trorks coordinate when one spots player

---

#### Example 2: Merchant Interaction Guard

From `Kweebec_Merchant.json`:

```json
{
  "Sensor": {
    "Type": "Not",
    "Sensor": {
      "Type": "CanInteract",
      "ViewSector": 180
    }
  },
  "Actions": [
    {
      "Type": "SetInteractable",
      "Interactable": false
    }
  ]
}
```

**Logic:**
- **IF** player NOT looking at merchant (outside 180° view)
- **THEN** disable interaction prompt

**Result:** Can only trade when facing merchant

---

## Actions

### State Transition

```json
{
  "Type": "State",
  "State": "Combat"
}
```

**Changes NPC state** - Most common action

---

### Play Animation

```json
{
  "Type": "PlayAnimation",
  "Slot": "Status",
  "Animation": "Wave"
}
```

**Slots:**
- **Status** - Emotional/status animations (wave, sleep)
- **Movement** - Locomotion (walk, run)
- **Combat** - Attack animations

---

### Set Interactable

```json
{
  "Type": "SetInteractable",
  "Interactable": true,
  "Hint": "server.interactionHints.trade"
}
```

**Shows/hides interaction prompt** with custom hint text

---

### Inventory Operation

```json
{
  "Type": "Inventory",
  "Operation": "EquipHotbar",
  "Slot": 0
}
```

**Equips weapon** from hotbar slot

---

### Lock On Target

```json
{
  "Type": "LockOnInteractionTarget"
}
```

**Sets current target** - Used for combat/tracking

---

### Release Target

```json
{
  "Type": "ReleaseTarget"
}
```

**Clears target** - End combat/tracking

---

### Open Barter Shop

```json
{
  "Type": "OpenBarterShop",
  "Shop": "Kweebec_Merchant"
}
```

**Opens trading UI** with specified shop

---

### Timeout (Delayed Action)

```json
{
  "Type": "Timeout",
  "Delay": [ 2, 2 ],
  "Action": {
    "Type": "PlayAnimation",
    "Slot": "Status"
  }
}
```

**Executes action after delay** (2-2 second range)  
**Use:** Timed transitions, animation cleanup

---

### Sequence (Multiple Actions)

```json
{
  "Type": "Sequence",
  "Actions": [
    {
      "Type": "ReleaseTarget"
    },
    {
      "Type": "State",
      "State": "Idle"
    }
  ]
}
```

**Executes actions in order** - End combat, then return to idle

---

## Motion Control

### BodyMotion

```json
{
  "BodyMotion": {
    "Type": "Nothing"
  }
}
```

**Types:**
- **Nothing** - Stand still
- **Walk** - Walk to destination
- **Wander** - Random movement
- **FollowPath** - Path-based patrol

---

### HeadMotion

```json
{
  "HeadMotion": {
    "Type": "Watch"
  }
}
```

**Types:**
- **Watch** - Look at target/player
- **Nothing** - Don't track

**Use:** Merchant watching player, enemy tracking target

---

## Advanced Instruction Patterns

### Continue Flag

```json
{
  "Continue": true,
  "Sensor": {
    "Type": "Player",
    "Range": 10
  },
  "Actions": [
    {
      "Type": "PlayAnimation",
      "Slot": "Status",
      "Animation": "Alert"
    }
  ]
}
```

**`Continue: true`** - Check additional instructions even if this one triggers  
**`Continue: false` (default)** - Stop checking after first match

**Use:** Multiple simultaneous behaviors (track player + check health)

---

### ActionsBlocking

```json
{
  "ActionsBlocking": true,
  "Sensor": {
    "Type": "Player",
    "Range": 8
  },
  "Actions": [
    {
      "Type": "PlayAnimation",
      "Slot": "Status",
      "Animation": "Wave"
    },
    {
      "Type": "State",
      "State": "Watching"
    }
  ]
}
```

**`ActionsBlocking: true`** - NPC can't do anything else until actions complete  
**Use:** Animation sequences that shouldn't be interrupted

---

### Spawn beacons + “edible critters” (Goblin Ogre pattern)

`Template_Goblin_Ogre.json` demonstrates a full “call something in, then react to it” loop:

- **Randomized idle picks** using a `Random` action (including local substates like `.Default` / `.Guard`):

```315:377:Server/NPC/Roles/Intelligent/Aggressive/Goblin/Templates/Template_Goblin_Ogre.json
            {
              "Sensor": {
                "Type": "State",
                "State": ".Default"
              },
              "Instructions": [
                {
                  "Actions": [
                    {
                      "Type": "Random",
                      "Actions": [
                        {
                          "Weight": 30,
                          "Action": {
                            "Type": "State",
                            "State": ".Guard"
                          }
                        },
                        {
                          "Weight": 30,
                          "Action": {
                            "Type": "State",
                            "State": "Sleep"
                          }
                        },
                        {
                          "Weight": 20,
                          "Action": {
                            "Type": "State",
                            "State": "Eat"
                          }
                        },
                        {
                          "Weight": 20,
                          "Action": {
                            "Type": "State",
                            "State": "CallRat"
                          }
                        }
                      ]
                    }
                  ]
                }
              ]
            },
```

- **Spawning an “edible” NPC** via a spawn beacon trigger:

```469:481:Server/NPC/Roles/Intelligent/Aggressive/Goblin/Templates/Template_Goblin_Ogre.json
            {
              "Continue": true,
              "Sensor": {
                "Type": "Any",
                "Once": true
              },
              "Actions": [
                {
                  "Type": "TriggerSpawnBeacon",
                  "BeaconSpawn": { "Compute": "FoodNPCBeacon" },
                  "Range": 15
                }
              ]
            },
```

Related: [NPC Spawn Beacons](185_NPC_Spawn_Beacons.md)

### Nested Instructions

From `Kweebec_Merchant.json`:

```json
{
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
                "Range": 8
              },
              "Actions": [ /* ... */ ]
            }
          ]
        },
        {
          "Sensor": {
            "Type": "State",
            "State": "Watching"
          },
          "Instructions": [ /* ... */ ]
        }
      ]
    }
  ]
}
```

**Structure:**
- **Level 1**: Always-active container
- **Level 2**: State-specific branches
- **Level 3**: Behavior within each state

**Result:** Clean state machine with isolated behaviors per state

---

## Combat AI Patterns

### Attack Sequence

From `Template_Trork_Melee.json`:

```json
{
  "AttackSequence": {
    "Value": "Component_Instruction_Attack_Sequence_Trork_Warrior",
    "Description": "The attack sequence to use in combat"
  },
  "AttackDistance": {
    "Value": 3,
    "Description": "The distance at which an NPC will execute attacks"
  }
}
```

**Attack sequences** define combo chains referenced by component ID

---

### Leashing (Return Home)

```json
{
  "LeashDistance": {
    "Value": 30,
    "Description": "Max distance from spawn"
  },
  "LeashMinPlayerDistance": {
    "Value": 7,
    "Description": "Min distance from player before giving up"
  },
  "LeashTimer": {
    "Value": [ 8, 8 ],
    "Description": "Time before giving up chase"
  },
  "HardLeashDistance": {
    "Value": 160,
    "Description": "Absolute maximum distance"
  }
}
```

**Prevents NPCs from chasing forever:**
1. Chase player up to LeashDistance (30m)
2. If player escapes MinPlayerDistance (7m) for LeashTimer (8s)
3. NPC gives up, returns home
4. HardLeashDistance (160m) forces immediate return

---

### Damage Calculation

```json
{
  "_InteractionVars": {
    "Melee_Damage": {
      "Interactions": [
        {
          "Parent": "NPC_Attack_Melee_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 23
            }
          }
        }
      ]
    }
  }
}
```

**Defines attack damage** via interaction vars

---

## Combat Action Evaluator

The **Combat Action Evaluator** is an advanced system that allows NPCs to make smarter, more dynamic combat decisions. Instead of simple sequential attack patterns, the evaluator considers:

- **Conditions** - Target state, health, distance, environmental factors
- **Weighted Choices** - Probability-based action selection
- **Cooldowns** - Per-action cooldown timers
- **Resource Costs** - Stamina or other resource requirements

### Basic Combat Action Evaluator

```json
{
  "CombatActionEvaluator": {
    "Actions": [
      {
        "Weight": 50,
        "Cooldown": 2,
        "Action": {
          "Type": "Attack",
          "Attack": "Melee_Swing"
        },
        "Conditions": [
          {
            "Type": "TargetDistance",
            "Max": 3
          }
        ]
      },
      {
        "Weight": 30,
        "Cooldown": 5,
        "Action": {
          "Type": "Attack",
          "Attack": "Heavy_Slam"
        },
        "Conditions": [
          {
            "Type": "TargetDistance",
            "Max": 4
          },
          {
            "Type": "Health",
            "Min": 0.3
          }
        ]
      },
      {
        "Weight": 20,
        "Cooldown": 8,
        "Action": {
          "Type": "Dodge",
          "Direction": "Back"
        },
        "Conditions": [
          {
            "Type": "Health",
            "Max": 0.5
          }
        ]
      }
    ]
  }
}
```

### Evaluator Flow

1. **Check Conditions** - Filter actions where all conditions are met
2. **Check Cooldowns** - Filter actions currently on cooldown
3. **Weighted Random** - Pick from remaining actions by weight
4. **Execute** - Perform the selected action
5. **Apply Cooldown** - Start the cooldown timer

This results in more varied combat that responds to player actions and combat state.

---

## Social AI Patterns

### Merchant Complete Flow

From `Kweebec_Merchant.json`:

**1. Approach Detection:**

```json
{
  "Sensor": {
    "Type": "Player",
    "Range": 8
  },
  "Actions": [
    {
      "Type": "PlayAnimation",
      "Slot": "Status",
      "Animation": "Wave"
    },
    {
      "Type": "State",
      "State": "Watching"
    }
  ]
}
```

**2. Player Tracking:**

```json
{
  "Continue": true,
  "Sensor": {
    "Type": "Player",
    "Range": 12
  },
  "HeadMotion": {
    "Type": "Watch"
  }
}
```

**3. Interaction Check:**

```json
{
  "Sensor": {
    "Type": "CanInteract",
    "ViewSector": 180
  },
  "Actions": [
    {
      "Type": "SetInteractable",
      "Interactable": true,
      "Hint": "server.interactionHints.trade"
    }
  ]
}
```

**4. Trade Activation:**

```json
{
  "Sensor": {
    "Type": "HasInteracted"
  },
  "Actions": [
    {
      "Type": "LockOnInteractionTarget"
    },
    {
      "Type": "OpenBarterShop",
      "Shop": "Kweebec_Merchant"
    },
    {
      "Type": "State",
      "State": "$Interaction"
    }
  ]
}
```

**5. Post-Trade Cleanup:**

```json
{
  "Actions": [
    {
      "Type": "Timeout",
      "Delay": [ 1, 1 ],
      "Action": {
        "Type": "Sequence",
        "Actions": [
          {
            "Type": "ReleaseTarget"
          },
          {
            "Type": "State",
            "State": "Watching"
          }
        ]
      }
    }
  ]
}
```

---

## Parameters & Compute

### Parameter System

From `Template_Trork_Melee.json`:

```json
{
  "Parameters": {
    "ViewRange": {
      "Value": 15,
      "Description": "View range"
    },
    "AttackDistance": {
      "Value": 3,
      "Description": "The distance at which an NPC will execute attacks"
    }
  },
  "ViewRange": { "Compute": "ViewRange" },
  "AttackDistance": { "Compute": "AttackDistance" }
}
```

**Benefits:**
- Define values once as parameters
- Reference with `{ "Compute": "ParameterName" }`
- Override in variants
- Documentation built-in

---

### Variant System

From `Trork_Warrior.json`:

```json
{
  "Type": "Variant",
  "Reference": "Template_Trork_Melee",
  "Modify": {
    "Appearance": "Trork_Warrior",
    "MaxHealth": 61,
    "_InteractionVars": {
      "Melee_Damage": {
        "Interactions": [
          {
            "Parent": "NPC_Attack_Melee_Damage",
            "DamageCalculator": {
              "BaseDamage": {
                "Physical": 23
              }
            }
          }
        ]
      }
    }
  }
}
```

**Pattern:**
1. Create template with shared behavior
2. Create variants with only differences
3. Reuse combat AI across all Trork types

---

## Complete AI Example: Guard NPC

### Requirements
- Patrol between points when idle
- Detect player at 15m (180° FOV)
- Alert nearby guards via beacon
- Chase player up to 30m from patrol
- Attack at 3m range
- Return to patrol if player escapes

### Implementation

```json
{
  "Type": "Generic",
  "StartState": "Patrol",
  "BusyStates": [ "Chase", "Combat", "ReturnHome" ],
  "DefaultPlayerAttitude": "Hostile",
  "Parameters": {
    "ViewRange": {
      "Value": 15,
      "Description": "Detection range"
    },
    "AlertRange": {
      "Value": 25,
      "Description": "Beacon alert range"
    },
    "AttackDistance": {
      "Value": 3,
      "Description": "Melee range"
    },
    "LeashDistance": {
      "Value": 30,
      "Description": "Max chase distance from patrol"
    }
  },
  "Instructions": [
    {
      "Instructions": [
        {
          "$Comment": "Patrol State",
          "Sensor": {
            "Type": "State",
            "State": "Patrol"
          },
          "Instructions": [
            {
              "$Comment": "Detect player",
              "Sensor": {
                "Type": "And",
                "Sensors": [
                  {
                    "Type": "Player",
                    "Range": { "Compute": "ViewRange" }
                  },
                  {
                    "Type": "CanSee",
                    "ViewSector": 180
                  }
                ]
              },
              "Actions": [
                {
                  "Type": "PlayAnimation",
                  "Slot": "Status",
                  "Animation": "Alert"
                },
                {
                  "Type": "Beacon",
                  "Message": "GuardAlert",
                  "Range": { "Compute": "AlertRange" }
                },
                {
                  "Type": "LockOnTarget"
                },
                {
                  "Type": "State",
                  "State": "Chase"
                }
              ]
            },
            {
              "$Comment": "Respond to ally alerts",
              "Sensor": {
                "Type": "Beacon",
                "Message": "GuardAlert",
                "TargetSlot": "LockedTarget"
              },
              "Actions": [
                {
                  "Type": "State",
                  "State": "Chase"
                }
              ]
            },
            {
              "$Comment": "Follow patrol path",
              "Sensor": {
                "Type": "Any"
              },
              "BodyMotion": {
                "Type": "FollowPath"
              }
            }
          ]
        },
        {
          "$Comment": "Chase State",
          "Sensor": {
            "Type": "State",
            "State": "Chase"
          },
          "Instructions": [
            {
              "$Comment": "Close enough to attack",
              "Sensor": {
                "Type": "Target",
                "Range": { "Compute": "AttackDistance" }
              },
              "Actions": [
                {
                  "Type": "State",
                  "State": "Combat"
                }
              ]
            },
            {
              "$Comment": "Target escaped (too far from patrol)",
              "Sensor": {
                "Type": "Or",
                "Sensors": [
                  {
                    "Type": "DistanceFromHome",
                    "Min": { "Compute": "LeashDistance" }
                  },
                  {
                    "Type": "Not",
                    "Sensor": {
                      "Type": "Target",
                      "Range": 50
                    }
                  }
                ]
              },
              "Actions": [
                {
                  "Type": "ReleaseTarget"
                },
                {
                  "Type": "State",
                  "State": "ReturnHome"
                }
              ]
            },
            {
              "$Comment": "Keep chasing",
              "Sensor": {
                "Type": "Any"
              },
              "BodyMotion": {
                "Type": "Chase",
                "Speed": 1.2
              }
            }
          ]
        },
        {
          "$Comment": "Combat State",
          "Sensor": {
            "Type": "State",
            "State": "Combat"
          },
          "Instructions": [
            {
              "$Comment": "Target moved away",
              "Sensor": {
                "Type": "Not",
                "Sensor": {
                  "Type": "Target",
                  "Range": { "Compute": "AttackDistance" }
                }
              },
              "Actions": [
                {
                  "Type": "State",
                  "State": "Chase"
                }
              ]
            },
            {
              "$Comment": "Execute attack sequence",
              "Sensor": {
                "Type": "Any"
              },
              "Component": "Component_Guard_Attack_Sequence"
            }
          ]
        },
        {
          "$Comment": "Return Home State",
          "Sensor": {
            "Type": "State",
            "State": "ReturnHome"
          },
          "Instructions": [
            {
              "$Comment": "Reached patrol area",
              "Sensor": {
                "Type": "DistanceFromHome",
                "Max": 2
              },
              "Actions": [
                {
                  "Type": "State",
                  "State": "Patrol"
                }
              ]
            },
            {
              "$Comment": "Walk home",
              "Sensor": {
                "Type": "Any"
              },
              "BodyMotion": {
                "Type": "ReturnHome"
              }
            }
          ]
        }
      ]
    }
  ]
}
```

---

## Best Practices

### ✅ Do

1. **Use state machines** for complex NPCs (3+ states)
2. **Name states clearly** - "Combat" not "State3"
3. **Add $Comment fields** for documentation
4. **Use Parameters** for reusable values
5. **Nest instructions by state** for organization
6. **Set BusyStates** to prevent interruptions
7. **Use Continue: true** for simultaneous behaviors
8. **Clean up animations** with timeouts
9. **Release targets** when done
10. **Test edge cases** (player escapes, NPC stuck)

---

### ❌ Don't

1. **Don't forget leashing** - NPCs will chase forever
2. **Don't make detection instant** - Add alert state
3. **Don't skip CanInteract** - Players can trade from behind
4. **Don't forget state transitions** - NPCs get stuck
5. **Don't use ActionsBlocking** everywhere - NPC freezes
6. **Don't overlap ranges** - 8m detection, 12m tracking creates gaps
7. **Don't forget Not sensors** - Positive + negative checks
8. **Don't hardcode values** - Use parameters

---

## Common AI Patterns

### Pattern 1: Alert → Investigate → Return
```
Idle → (Hears noise) → Alert → (No threat) → Idle
```

### Pattern 2: Detect → Call Allies → Attack
```
Patrol → (Sees player) → Send beacon → Chase → Combat
```

### Pattern 3: Greet → Track → Forget
```
Idle → (Player nearby) → Wave → Watch → (Player leaves) → Idle
```

### Pattern 4: Combat → Flee → Heal → Return
```
Combat → (Low health) → Flee → (Find cover) → Heal → Rejoin
```

---

## Performance Tips

1. **Limit sensor ranges** - Don't check 100m constantly
2. **Use ViewSector** - 180° FOV cheaper than 360°
3. **Avoid nested Ands/Ors** - Max 2-3 levels deep
4. **Cache target checks** - Once per second, not per frame
5. **Use BusyStates** - Skip inactive instruction trees
6. **Minimize beacons** - Don't spam every frame

---

## Debug Commands

Use these commands in-game to visualize and debug NPC behavior:

```
/npc debug set <flag>
/npc debug presets    -- lists all available flags
```

### Available Debug Flags

| Flag | Description |
|------|-------------|
| `VisSensorRanges` | Shows sensor detection ranges and view sectors |
| `VisMarkedTargets` | Displays current locked targets |
| `VisAiming` | Shows what NPCs are aiming at |
| `VisLeash` | Displays the leash position and distance |
| `VisFlock` | Shows flocking behavior and flock-mate connections |

### VisFlock Debug

The `VisFlock` flag is particularly useful for understanding pack behavior:
- Shows connections between flock members
- Displays the flock leader
- Visualizes influence ranges
- Helps debug why NPCs aren't grouping correctly

### Example: Debugging a Bear

```
/npc debug set VisSensorRanges
```

- **Sleeping Bear**: Shows only one active sensor (noise detection)
- **Awake Bear**: Displays hearing range, vision cone, and absolute detection range

---

## Known Limitations

The NPC system has some documented limitations:

1. **Pathfinding** - NPCs may struggle with complex terrain or multi-level structures
2. **Performance** - Large sensor ranges or many NPCs can impact performance
3. **Scripting** - Complex behaviors may require many nested components
4. **State Transitions** - Rapidly changing states can cause animation glitches

### Mitigation Strategies

- Use reasonable sensor ranges (15-30m for most NPCs)
- Limit beacon message frequency
- Use `BusyStates` to prevent unwanted state changes
- Test NPC behavior in different scenarios

---

## Official Resources

- **Generated NPC Documentation:** https://hytalemodding.dev/en/docs/official-documentation/npc-doc
- **NPC Tutorial:** https://hytalemodding.dev/en/docs/official-documentation/npc/1-know-your-enemy
- **Video Tutorials:**
  - [Part 1](https://youtu.be/vsbYytAI-_o) - [Part 2](https://youtu.be/Za_wipUM-i8) - [Part 3](https://youtu.be/n44o2ABVhy4)
  - [Part 4](https://youtu.be/jg-IUZopAi8) - [Part 5](https://youtu.be/tSVeuUguCwA) - [Part 6](https://youtu.be/hgelFDVhmYw)

---

## Related Guides

- **[Creating NPCs](03_NPCs.md)** - Basic NPC creation
- **[NPC Sensors](81_NPC_Sensors.md)** - Sensor types reference
- **[NPC Instructions](82_NPC_Instructions.md)** - Instruction types
- **[NPC State Transitions](80_NPC_State_Transitions.md)** - State system
- **[NPC Components](84_NPC_Components.md)** - Reusable behavior components
- **[NPC Spawn Beacons](185_NPC_Spawn_Beacons.md)** - Spawn beacon triggers
- **[System Integration](177_System_Integration.md)** - NPC + shop + quests

---

**Previous:** [Advanced Particle Systems](179_Advanced_Particle_Systems.md) | **Next:** [Procedural World Generation](181_Procedural_World_Generation.md)
