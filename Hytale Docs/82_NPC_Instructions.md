# NPC Instructions

Learn how NPC instructions create behavior trees and define NPC AI behavior.

## Overview

NPC instructions define behavior trees that control NPC actions. Instructions use sensors to detect conditions and execute actions or body motions based on those conditions. Instructions form a hierarchical decision-making system where the first matching instruction is executed.

**Instructions can contain:**
- **Sensor** - Condition that must be true for instruction to execute
- **BodyMotion** - Movement behavior (walk, flee, wander, etc.)
- **HeadMotion** - Head tracking behavior (watch, observe, aim)
- **Actions** - List of actions to execute (state changes, animations, etc.)
- **Instructions** - Nested child instructions (behavior tree branches)

## Location
Instructions are configured in `Instructions` array in NPC Role definitions.

## Official Documentation Reference
See [Hytale NPC Documentation](https://hytalemodding.dev/en/docs/official-documentation/npc-doc) for complete instruction specifications.

## Example from Game Files

### NPC Instructions from Template

From `Server/NPC/Roles/_Core/Templates/Template_Intelligent.json`:

NPC instructions use sensors to detect conditions and execute actions. For example, when a Path sensor detects a path within range, the NPC follows it using Path BodyMotion.

## Basic Instruction Structure

```json
{
  "Instructions": [
    {
      "Instructions": [
        {
          "Sensor": {
            "Type": "Any"
          },
          "BodyMotion": {
            "Type": "Wander"
          }
        }
      ]
    }
  ]
}
```

## Instruction Hierarchy

Instructions are nested behavior trees:

```json
{
  "Instructions": [
    {
      "Continue": true,
      "Instructions": [
        {
          "Sensor": {
            "Type": "Damage",
            "Combat": true
          },
          "Actions": [
            {
              "Type": "State",
              "State": "Combat"
            }
          ]
        },
        {
          "Sensor": {
            "Type": "Any"
          },
          "BodyMotion": {
            "Type": "Idle"
          }
        }
      ]
    }
  ]
}
```

## Sensor-Action Pattern

```json
{
  "Sensor": {
    "Type": "Mob",
    "Range": 15
  },
  "Actions": [
    {
      "Type": "State",
      "State": "Combat"
    }
  ]
}
```

When sensor detects, execute actions.

## Sensor-BodyMotion Pattern

```json
{
  "Sensor": {
    "Type": "Path",
    "PathType": "AnyPrefabPath"
  },
  "BodyMotion": {
    "Type": "Path",
    "UseNodeViewDirection": true
  }
}
```

When sensor detects, execute movement.

---

## Instruction Types

### Standard Instruction

The default instruction type with sensor, motions, and actions.

```json
{
  "Type": "Instruction",
  "Name": "OptionalName",
  "Tag": "debug_tag",
  "Enabled": true,
  "Sensor": { ... },
  "BodyMotion": { ... },
  "HeadMotion": { ... },
  "Actions": [ ... ],
  "ActionsBlocking": false,
  "ActionsAtomic": false,
  "Instructions": [ ... ],
  "Continue": false,
  "Weight": 1.0,
  "TreeMode": false,
  "InvertTreeModeResult": false
}
```

**Attributes:**
| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `Name` | String | null | Optional name for descriptor |
| `Tag` | String | null | Internal identifier for debugging |
| `Enabled` | Boolean/Computable | true | Whether instruction is enabled |
| `Sensor` | ObjectRef | null | Sensor for condition testing |
| `BodyMotion` | ObjectRef | null | Body motion to execute |
| `HeadMotion` | ObjectRef | null | Head motion to execute |
| `Actions` | ObjectRef | null | Actions to execute |
| `ActionsBlocking` | Boolean | false | Don't execute action unless previous completed |
| `ActionsAtomic` | Boolean | false | Only execute if all actions can execute |
| `Instructions` | Array | null | Nested child instructions |
| `Continue` | Boolean | false | Continue checking after this matches |
| `Weight` | Double | 1.0 | Weight for random instruction selection |
| `TreeMode` | Boolean | false | Treat as behavior tree (continue if children fail) |
| `InvertTreeModeResult` | Boolean | false | Invert result for parent TreeMode |

---

### Random Instruction

Selects one instruction randomly from weighted list.

```json
{
  "Type": "Random",
  "Sensor": { ... },
  "Instructions": [
    {
      "Weight": 2.0,
      "Sensor": { "Type": "Any" },
      "BodyMotion": { "Type": "Wander" }
    },
    {
      "Weight": 1.0,
      "Sensor": { "Type": "Any" },
      "BodyMotion": { "Type": "Nothing" }
    }
  ],
  "ResetOnStateChange": true,
  "ExecuteFor": [5, 10]
}
```

**Additional Attributes:**
| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `ResetOnStateChange` | Boolean | true | Reset when NPC state changes |
| `ExecuteFor` | Double Array | [∞, ∞] | How long to execute before picking another |

---

### Reference Instruction

References a reusable instruction component.

```json
{
  "Type": "Reference",
  "Name": "MyReusableInstruction",
  "Parameters": { ... },
  "Sensor": { ... },
  "BodyMotion": { ... },
  "HeadMotion": { ... },
  "Actions": [ ... ],
  "Instructions": [ ... ]
}
```

**Additional Attributes:**
| Attribute | Type | Description |
|-----------|------|-------------|
| `Name` | String | Required reference name |
| `Parameters` | Parameters | Parameter definitions for the reference |

---

## Body Motion Types

### Nothing
Stay in place, do nothing.

```json
{
  "BodyMotion": {
    "Type": "Nothing"
  }
}
```

---

### Wander
Random movement in short linear pieces.

```json
{
  "BodyMotion": {
    "Type": "Wander",
    "MinWalkTime": 2,
    "MaxWalkTime": 4,
    "MinHeadingChange": 0,
    "MaxHeadingChange": 90,
    "RelaxHeadingChange": true,
    "RelativeSpeed": 0.5,
    "MinMoveDistance": 0.5,
    "StopDistance": 0.5,
    "AvoidBlockDamage": true,
    "RelaxedMoveConstraints": false
  }
}
```

---

### WanderInCircle
Random movement within circle around spawn.

```json
{
  "BodyMotion": {
    "Type": "WanderInCircle",
    "Radius": 10,
    "UseSphere": false
  }
}
```

---

### WanderInRect
Random movement within rectangle around spawn.

```json
{
  "BodyMotion": {
    "Type": "WanderInRect",
    "Width": 10,
    "Depth": 10
  }
}
```

---

### Seek
Move towards target using pathfinding or steering.

```json
{
  "BodyMotion": {
    "Type": "Seek",
    "RelativeSpeed": 1.0,
    "SlowDownDistance": 8,
    "StopDistance": 2,
    "AbortDistance": 96,
    "UseSteering": true,
    "UsePathfinder": true,
    "AdjustRangeByHitboxSize": false
  }
}
```

**Requires:** Sensor that provides target position

---

### Flee
Move away from target.

```json
{
  "BodyMotion": {
    "Type": "Flee",
    "RelativeSpeed": 1.0,
    "SlowDownDistance": 8,
    "StopDistance": 10,
    "HoldDirectionTimeRange": [2, 5],
    "DirectionJitter": 45,
    "ErraticDistance": 4
  }
}
```

**Requires:** Sensor that provides target position

---

### Path
Follow a patrol path.

```json
{
  "BodyMotion": {
    "Type": "Path",
    "StartAtNearestNode": true,
    "MinRelSpeed": 0.5,
    "MaxRelSpeed": 0.5,
    "Direction": "FORWARD",
    "Shape": "LOOP",
    "MinNodeDelay": 0,
    "MaxNodeDelay": 2,
    "UseNodeViewDirection": true
  }
}
```

**Direction:** `FORWARD`, `BACKWARD`, `RANDOM`, `ANY`
**Shape:** `LOOP`, `LINE`, `CHAIN`, `POINTS`

**Requires:** Sensor that provides path

---

### MaintainDistance
Maintain distance from target position.

```json
{
  "BodyMotion": {
    "Type": "MaintainDistance",
    "DesiredDistanceRange": [5, 8],
    "TargetDistanceFactor": 0.5,
    "MoveThreshold": 1,
    "RelativeForwardsSpeed": 1.0,
    "RelativeBackwardsSpeed": 1.0,
    "StrafingDurationRange": [0, 0],
    "StrafingFrequencyRange": [2, 2]
  }
}
```

**Requires:** Sensor that provides target position

---

### Teleport
Teleport NPC to position from sensor.

```json
{
  "BodyMotion": {
    "Type": "Teleport",
    "OffsetRange": [0, 0],
    "MaxYOffset": 5,
    "OffsetSector": 0,
    "Orientation": "Unchanged"
  }
}
```

**Orientation:** `Unchanged`, `UseTarget`, `TowardsTarget`

**Requires:** Sensor that provides target position

---

### TakeOff
Switch from walking to flying motion controller.

```json
{
  "BodyMotion": {
    "Type": "TakeOff",
    "JumpSpeed": 5
  }
}
```

---

### Land
Try to land at position (for flying NPCs).

```json
{
  "BodyMotion": {
    "Type": "Land",
    "GoalLenience": 2
  }
}
```

**Requires:** Sensor that provides target position

---

### Leave
Get away from current position using pathfinding.

```json
{
  "BodyMotion": {
    "Type": "Leave",
    "Distance": 10
  }
}
```

---

### MatchLook
Rotate body to match look direction.

```json
{
  "BodyMotion": {
    "Type": "MatchLook"
  }
}
```

---

### AimCharge
Aim at target for performing a charge attack.

```json
{
  "BodyMotion": {
    "Type": "AimCharge",
    "RelativeTurnSpeed": 1.0
  }
}
```

**Requires:** Sensor that provides target position

---

### Timer (BodyMotion)
Execute motion for specific time.

```json
{
  "BodyMotion": {
    "Type": "Timer",
    "Time": [2, 5],
    "Motion": {
      "Type": "Wander"
    }
  }
}
```

---

### Sequence (BodyMotion)
Sequence of motions.

```json
{
  "BodyMotion": {
    "Type": "Sequence",
    "Looped": true,
    "RestartOnActivate": false,
    "Motions": [
      { "Type": "Wander" },
      { "Type": "Nothing" }
    ]
  }
}
```

---

## Head Motion Types

### Nothing
Don't track anything.

```json
{
  "HeadMotion": {
    "Type": "Nothing"
  }
}
```

---

### Watch
Rotate head to track target.

```json
{
  "HeadMotion": {
    "Type": "Watch",
    "RelativeTurnSpeed": 1.0
  }
}
```

**Requires:** Sensor that provides target position

---

### Aim
Aim at target considering weapon in hand.

```json
{
  "HeadMotion": {
    "Type": "Aim",
    "Spread": 1.0,
    "HitProbability": 0.33,
    "Deflection": true,
    "RelativeTurnSpeed": 1.0
  }
}
```

**Requires:** Sensor that provides target position

---

### Observe
Look around in various ways.

```json
{
  "HeadMotion": {
    "Type": "Observe",
    "AngleRange": [-45, 45],
    "PauseTimeRange": [2, 4],
    "PickRandomAngle": false,
    "ViewSegments": 3,
    "RelativeTurnSpeed": 1.0
  }
}
```

---

### Timer (HeadMotion)
Execute head motion for specific time.

```json
{
  "HeadMotion": {
    "Type": "Timer",
    "Time": [2, 5],
    "Motion": {
      "Type": "Watch"
    }
  }
}
```

---

### Sequence (HeadMotion)
Sequence of head motions.

```json
{
  "HeadMotion": {
    "Type": "Sequence",
    "Looped": true,
    "Motions": [
      { "Type": "Watch" },
      { "Type": "Observe", "AngleRange": [-30, 30] }
    ]
  }
}
```

## Action Types

Actions are executed when instructions match. All actions have an optional `Once` attribute (default: false) to execute only once.

### State Actions

#### State
Set NPC state.

```json
{
  "Type": "State",
  "State": "Combat",
  "ClearState": true
}
```

---

#### ParentState
Set main state from within a component.

```json
{
  "Type": "ParentState",
  "State": "Idle"
}
```

---

### Animation & Audio Actions

#### PlayAnimation
Play an animation.

```json
{
  "Type": "PlayAnimation",
  "Slot": "Status",
  "Animation": "Wave"
}
```

**Slots:** `Status`, `Action`, `Face`

---

#### PlaySound
Play a sound.

```json
{
  "Type": "PlaySound",
  "SoundEventId": "NPC_Alert"
}
```

---

#### SpawnParticles
Spawn particle system.

```json
{
  "Type": "SpawnParticles",
  "ParticleSystem": "Smoke_Puff",
  "Range": 75,
  "Offset": [0, 1, 0]
}
```

---

### Target Actions

#### ReleaseTarget
Clear locked target.

```json
{
  "Type": "ReleaseTarget",
  "TargetSlot": "LockedTarget"
}
```

---

#### SetMarkedTarget
Set target in slot from sensor.

```json
{
  "Type": "SetMarkedTarget",
  "TargetSlot": "LockedTarget"
}
```

**Requires:** Sensor that provides target

---

#### LockOnInteractionTarget
Lock onto interaction player.

```json
{
  "Type": "LockOnInteractionTarget",
  "TargetSlot": "LockedTarget"
}
```

---

#### AddToHostileTargetMemory
Add target to combat memory.

```json
{
  "Type": "AddToHostileTargetMemory"
}
```

**Requires:** Sensor that provides target

---

### Combat Actions

#### Attack
Start an attack.

```json
{
  "Type": "Attack",
  "Attack": "Melee_Sword",
  "AttackType": "Primary",
  "ChargeFor": 0,
  "AttackPauseRange": [0.5, 1.0],
  "AimingTimeRange": [0, 0.5],
  "LineOfSight": false,
  "AvoidFriendlyFire": true,
  "SkipAiming": false,
  "ChargeDistance": 0
}
```

**AttackType:** `Primary`, `Secondary`, `Ability1`, `Ability2`, `Ability3`

---

#### CombatAbility
Start combat ability from evaluator.

```json
{
  "Type": "CombatAbility"
}
```

---

#### ApplyEntityEffect
Apply effect to target or self.

```json
{
  "Type": "ApplyEntityEffect",
  "EntityEffect": "Poison",
  "UseTarget": true
}
```

---

#### OverrideAttitude
Override attitude towards target.

```json
{
  "Type": "OverrideAttitude",
  "Attitude": "HOSTILE",
  "Duration": 10
}
```

**Requires:** Sensor that provides target

---

### Inventory Actions

#### Inventory
Modify inventory.

```json
{
  "Type": "Inventory",
  "Operation": "EquipHotbar",
  "Slot": 0,
  "Item": null,
  "Count": 1,
  "UseTarget": true
}
```

**Operations:** `Add`, `Remove`, `Equip`, `EquipHotbar`, `EquipOffHand`, `SetHotbar`, `SetOffHand`, `RemoveHeldItem`, `ClearHeldItem`

---

#### DropItem
Drop an item.

```json
{
  "Type": "DropItem",
  "Delay": [0, 0],
  "Item": "Gold_Coin",
  "DropList": null,
  "ThrowSpeed": 1,
  "Distance": [1, 3],
  "DropSector": [-45, 45]
}
```

---

#### PickUpItem
Pick up an item.

```json
{
  "Type": "PickUpItem",
  "Range": 1,
  "StorageTarget": "Hotbar",
  "Hoover": false,
  "Items": ["*"]
}
```

---

### Communication Actions

#### Beacon
Send message to nearby NPCs.

```json
{
  "Type": "Beacon",
  "Message": "EnemySighted",
  "Range": 64,
  "TargetGroups": ["Guards"],
  "SendTargetSlot": null,
  "ExpirationTime": 1,
  "SendCount": -1
}
```

---

#### FlockBeacon
Send message to flock members.

```json
{
  "Type": "FlockBeacon",
  "Message": "Regroup",
  "SendTargetSlot": null,
  "ExpirationTime": 1,
  "SendToSelf": true,
  "SendToLeaderOnly": false
}
```

---

#### Notify
Directly notify target NPC.

```json
{
  "Type": "Notify",
  "Message": "TargetAcquired",
  "ExpirationTime": 1,
  "UseTargetSlot": null
}
```

---

### Interaction Actions

#### SetInteractable
Set interaction availability.

```json
{
  "Type": "SetInteractable",
  "Interactable": true,
  "Hint": "server.interactionHints.trade",
  "ShowPrompt": true
}
```

---

#### OpenShop
Open shop UI.

```json
{
  "Type": "OpenShop",
  "Shop": "General_Store"
}
```

---

#### OpenBarterShop
Open barter shop UI.

```json
{
  "Type": "OpenBarterShop",
  "Shop": "Kweebec_Merchant"
}
```

---

#### StartObjective
Start objective for player.

```json
{
  "Type": "StartObjective",
  "Objective": "Quest_CollectItems"
}
```

---

#### CompleteTask
Complete a task.

```json
{
  "Type": "CompleteTask",
  "Slot": "Action",
  "Animation": null,
  "PlayAnimation": true
}
```

---

### Timer/Flag Actions

#### SetAlarm
Set a named alarm.

```json
{
  "Type": "SetAlarm",
  "Name": "AlertCooldown",
  "DurationRange": ["PT30S", "PT60S"]
}
```

---

#### SetFlag
Set a boolean flag.

```json
{
  "Type": "SetFlag",
  "Name": "HasAttacked",
  "SetTo": true
}
```

---

#### TimerStart
Start a timer.

```json
{
  "Type": "TimerStart",
  "Name": "AttackCooldown",
  "StartValueRange": [2, 3],
  "RestartValueRange": [2, 3],
  "Rate": 1,
  "Repeating": false
}
```

---

#### TimerStop / TimerPause / TimerContinue / TimerRestart

```json
{ "Type": "TimerStop", "Name": "MyTimer" }
{ "Type": "TimerPause", "Name": "MyTimer" }
{ "Type": "TimerContinue", "Name": "MyTimer" }
{ "Type": "TimerRestart", "Name": "MyTimer" }
```

---

### Lifecycle Actions

#### Spawn
Spawn NPCs.

```json
{
  "Type": "Spawn",
  "Kind": "Skeleton_Archer",
  "CountRange": [2, 4],
  "DistanceRange": [3, 6],
  "DelayRange": [0.25, 0.5],
  "SpawnAngle": 120,
  "SpawnDirection": 0,
  "FanOut": false,
  "JoinFlock": false,
  "SpawnState": null
}
```

---

#### Die
Kill the NPC.

```json
{
  "Type": "Die"
}
```

---

#### Despawn
Trigger despawn cycle.

```json
{
  "Type": "Despawn",
  "Force": false
}
```

---

#### Remove
Erase entity (no death animation).

```json
{
  "Type": "Remove",
  "UseTarget": true
}
```

---

#### Role
Change NPC role.

```json
{
  "Type": "Role",
  "Role": "Transformed_Form",
  "ChangeAppearance": true,
  "State": null
}
```

---

### Utility Actions

#### Timeout
Delay action execution.

```json
{
  "Type": "Timeout",
  "Delay": [1, 2],
  "DelayAfter": false,
  "Action": { ... }
}
```

---

#### Sequence
Execute actions in order.

```json
{
  "Type": "Sequence",
  "Blocking": false,
  "Atomic": false,
  "Actions": [ ... ]
}
```

---

#### Random
Execute random weighted action.

```json
{
  "Type": "Random",
  "Actions": [
    { "Weight": 2, "Action": { ... } },
    { "Weight": 1, "Action": { ... } }
  ]
}
```

---

#### Nothing
Do nothing (placeholder).

```json
{
  "Type": "Nothing"
}
```

---

#### Log
Log message to console (debugging).

```json
{
  "Type": "Log",
  "Message": "NPC reached target"
}
```

---

#### ResetInstructions
Reset instruction states.

```json
{
  "Type": "ResetInstructions",
  "Instructions": null
}
```

---

### Position & Movement Actions

#### SetStat
Set an NPC stat value directly.

```json
{
  "Type": "SetStat",
  "Stat": "Health",
  "Value": 100
}
```

**Common Stats:** `"Health"`, `"MaxHealth"`, `"Speed"`

---

#### SetLeashPosition
Set the NPC's leash/spawn position.

```json
{
  "Type": "SetLeashPosition",
  "ToCurrent": true,
  "ToTarget": false
}
```

**Attributes:**
- `ToCurrent` (Boolean) - Set leash to current position
- `ToTarget` (Boolean) - Set leash to current target's position

---

#### StorePosition
Store current position to a named slot for later retrieval.

```json
{
  "Type": "StorePosition",
  "Slot": "PatrolPosition"
}
```

**Attributes:**
- `Slot` (String, Required) - Name of the slot to store position

Used with `ReadPosition` sensor to return to stored locations.

---

#### ResetBlockSensors
Reset cached block sensor results.

```json
{
  "Type": "ResetBlockSensors"
}
```

Useful when block state may have changed and sensors need to re-evaluate.

---

#### Crouch
Set the NPC's crouch state.

```json
{
  "Type": "Crouch",
  "Crouch": true
}
```

**Attributes:**
- `Crouch` (Boolean) - Whether to crouch or stand

---

### Mount Actions

#### Mount
Mount the interacting player on this NPC.

```json
{
  "Type": "Mount",
  "AnchorX": 0,
  "AnchorY": 1.2,
  "AnchorZ": 0,
  "MovementConfig": "Mount_Horse"
}
```

**Attributes:**
- `AnchorX`, `AnchorY`, `AnchorZ` (Number) - Mount position offset
- `MovementConfig` (String) - Movement configuration to apply when mounted

---

### Flock Actions

#### JoinFlock
Join a flock of similar NPCs.

```json
{
  "Type": "JoinFlock",
  "ForceJoin": false
}
```

**Attributes:**
- `ForceJoin` (Boolean, Optional) - Force joining even if flock is full

---

#### FlockBeacon
Send a beacon message to flock members.

```json
{
  "Type": "FlockBeacon",
  "Message": "Danger",
  "SendTargetSlot": "LockedTarget",
  "ExpirationTime": 5,
  "SendToSelf": false,
  "SendToLeaderOnly": false
}
```

**Attributes:**
- `Message` (String) - Message to send
- `SendTargetSlot` (String) - Target slot to send with message
- `ExpirationTime` (Number) - Seconds until message expires
- `SendToSelf` (Boolean) - Also send to self
- `SendToLeaderOnly` (Boolean) - Only send to flock leader

**Usage:** Used for alerting flock members of threats or coordinating group behavior.

---

## Instruction References

```json
{
  "Instructions": [
    {
      "Reference": "Component_Instruction_Intelligent_Idle_Motion_Follow_Path",
      "Modify": {
        "FollowPathRange": {
          "Value": 50
        }
      }
    }
  ]
}
```

Reference reusable instruction components with optional parameter overrides.

---

## Continue Flag

```json
{
  "Continue": true,
  "Instructions": [ ... ]
}
```

When `true`, continues checking sibling instructions even after this one matches. Use for parallel behaviors (e.g., track player AND check health).

---

## TreeMode Flag

```json
{
  "TreeMode": true,
  "Instructions": [ ... ]
}
```

Treats instruction as behavior tree - continues evaluation even if child instructions fail. Cannot be combined with `Continue: true`.

---

## Tips for NPC Instructions

1. **Nested structure** - Instructions form behavior trees
2. **Sensor priority** - First matching sensor executes (unless Continue: true)
3. **Continue flag** - Use for parallel behaviors that should all run
4. **TreeMode** - Use for behavior tree patterns where failure should continue
5. **Component reuse** - Reference instruction components for DRY code
6. **State-based** - Organize instructions by state for clarity
7. **ActionsBlocking** - Use when action sequence must complete uninterrupted
8. **ActionsAtomic** - Use when all actions must succeed or none execute
9. **Weight** - Use with Random instructions for weighted selection
10. **Computable values** - Use `{ "Compute": "Param" }` for configurable values

---

## Instruction Quick Reference

| BodyMotion | Purpose |
|------------|---------|
| `Nothing` | Stand still |
| `Wander` | Random movement |
| `WanderInCircle` | Wander in circle area |
| `Seek` | Move to target |
| `Flee` | Move away from target |
| `Path` | Follow patrol path |
| `MaintainDistance` | Keep distance from target |
| `Teleport` | Teleport to position |
| `TakeOff` / `Land` | Flying transitions |

| HeadMotion | Purpose |
|------------|---------|
| `Nothing` | No tracking |
| `Watch` | Track target |
| `Aim` | Aim weapon at target |
| `Observe` | Look around |

| Action Category | Examples |
|-----------------|----------|
| State | `State`, `ParentState` |
| Animation | `PlayAnimation`, `PlaySound`, `SpawnParticles` |
| Target | `ReleaseTarget`, `SetMarkedTarget`, `LockOnInteractionTarget` |
| Combat | `Attack`, `CombatAbility`, `ApplyEntityEffect` |
| Inventory | `Inventory`, `DropItem`, `PickUpItem` |
| Communication | `Beacon`, `FlockBeacon`, `Notify` |
| Interaction | `SetInteractable`, `OpenShop`, `OpenBarterShop` |
| Timer/Flag | `SetAlarm`, `SetFlag`, `TimerStart`, etc. |
| Lifecycle | `Spawn`, `Die`, `Despawn`, `Remove`, `Role` |
| Utility | `Timeout`, `Sequence`, `Random`, `Nothing` |

---

**Previous:** [NPC Sensors](81_NPC_Sensors.md) | **Next:** [NPC Motion Controllers](83_NPC_Motion_Controllers.md)
