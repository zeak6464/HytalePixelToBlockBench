# NPC Sensors

Learn how NPC sensors work to detect players, NPCs, and environmental conditions.

## Overview

NPC sensors detect entities, conditions, and triggers in the world. Sensors are the core detection mechanism in Hytale's NPC AI system. They return `true` when conditions are met and can provide targets (players, NPCs, positions) to actions and motions.

**Sensors can detect:**
- Players and NPCs (proximity, line-of-sight, attitudes)
- Combat events (damage, kills, inflicted damage)
- Environmental conditions (blocks, weather, light, time)
- NPC states and timers
- Paths and positions
- Beacon messages from other NPCs
- Inventory and items

## Location
Sensors are configured in NPC Instructions and can be defined as reusable components in `Server/NPC/Roles/_Core/Components/Sensors/`.

## Official Documentation Reference
See [Hytale NPC Documentation](https://hytalemodding.dev/en/docs/official-documentation/npc-doc) for complete sensor specifications.

## Example from Game Files

### NPC Sensors from Template

From `Server/NPC/Roles/_Core/Templates/Template_Intelligent.json`:

```17:32:Server/NPC/Roles/_Core/Templates/Template_Intelligent.json
    "ViewRange": {
      "Value": 15,
      "Description": "The range from which the target will be seen. If zero, sight will be disabled."
    },
    "ViewSector": {
      "Value": 180,
      "Description": "The view sector within which the target needs to be to be seen."
    },
    "HearingRange": {
      "Value": 8,
      "Description": "The range from which the target will be heard. If zero, hearing will be disabled."
    },
    "AbsoluteDetectionRange": {
      "Value": 2,
      "Description": "The range at which a target is guaranteed to be detected. If zero, abosulte detection will be disabled."
    },
```

This shows NPC sensor parameters for sight (ViewRange, ViewSector) and hearing (HearingRange), along with absolute detection range.

## Basic Sensor Structure

```json
{
  "Sensor": {
    "Type": "Mob",
    "Range": 10,
    "GetPlayers": true,
    "GetNPCs": true
  }
}
```

## Sensor Types

### Entity Detection Sensors

#### Player Sensor
Detects players matching specific attributes and filters within range.

```json
{
  "Type": "Player",
  "Range": 15,
  "MinRange": 0,
  "LockOnTarget": true,
  "LockedTargetSlot": "LockedTarget",
  "AutoUnlockTarget": false,
  "OnlyLockedTarget": false,
  "UseProjectedDistance": false,
  "Filters": []
}
```

**Attributes:**
- `Range` (Required) - Maximum range to test entities
- `MinRange` (Default: 0) - Minimum range to test entities
- `LockOnTarget` (Default: false) - Matched target becomes locked target
- `LockedTargetSlot` (Default: "LockedTarget") - Target slot to use
- `AutoUnlockTarget` (Default: false) - Unlock when sensor stops matching
- `OnlyLockedTarget` (Default: false) - Test only locked target
- `Prioritiser` - Prioritize targets by criteria
- `Collector` - Process matched entities
- `Filters` - Entity filter conditions

**Provides:** Player target, NPC target

---

#### Mob Sensor
Detects NPCs matching specific attributes and filters within range.

```json
{
  "Type": "Mob",
  "Range": 15,
  "GetPlayers": false,
  "GetNPCs": true,
  "ExcludeOwnType": true,
  "LockOnTarget": true,
  "Filters": [
    {
      "Type": "LineOfSight"
    }
  ]
}
```

**Attributes:**
- `Range` (Required) - Maximum detection range
- `MinRange` (Default: 0) - Minimum detection range
- `GetPlayers` (Default: false) - Include players in detection
- `GetNPCs` (Default: true) - Include NPCs in detection
- `ExcludeOwnType` (Default: true) - Exclude NPCs of same type
- `LockOnTarget`, `LockedTargetSlot`, `AutoUnlockTarget`, `OnlyLockedTarget` - Target locking options
- `Filters` - Entity filter conditions

**Provides:** Player target, NPC target

---

#### Target Sensor
Tests if a given locked target matches criteria and filters.

```json
{
  "Type": "Target",
  "TargetSlot": "LockedTarget",
  "Range": 10,
  "AutoUnlockTarget": false,
  "Filters": []
}
```

**Attributes:**
- `TargetSlot` (Default: "LockedTarget") - Target slot to check
- `Range` (Default: infinity) - Maximum range of locked target
- `AutoUnlockTarget` (Default: false) - Unlock if match fails
- `Filters` - Entity filter conditions

**Provides:** Player target, NPC target

---

### Combat Sensors

#### Damage Sensor
Triggers when NPC suffers damage.

```json
{
  "Type": "Damage",
  "Combat": true,
  "Friendly": false,
  "Drowning": false,
  "Environment": false,
  "Other": false,
  "TargetSlot": "LockedTarget"
}
```

**Attributes:**
- `Combat` (Default: true) - Test for combat damage
- `Friendly` (Default: false) - Test for damage from usually disabled groups
- `Drowning` (Default: false) - Test for drowning damage
- `Environment` (Default: false) - Test for environmental damage
- `Other` (Default: false) - Test for other damage types
- `TargetSlot` - Slot to lock damage source (optional)

**Provides:** Player target, NPC target, Dropped item target

---

#### Kill Sensor
Triggers when NPC makes a kill.

```json
{
  "Type": "Kill",
  "TargetSlot": null
}
```

**Attributes:**
- `TargetSlot` - Target slot to check if killed (optional, accepts any kill if omitted)

**Provides:** Vector position

---

#### InflictedDamage Sensor
Tests if NPC or its flock inflicted combat damage.

```json
{
  "Type": "InflictedDamage",
  "Target": "Self",
  "FriendlyFire": false
}
```

**Attributes:**
- `Target` - Who to check: `Self`, `Flock`, `FlockLeader`
- `FriendlyFire` (Default: false) - Consider friendly fire

**Provides:** Player target, NPC target

---

#### IsBackingAway Sensor
Tests if NPC is currently backing away from something.

```json
{
  "Type": "IsBackingAway"
}
```

---

### State & Timer Sensors

#### State Sensor
Tests if NPC is in a specific state.

```json
{
  "Type": "State",
  "State": "Combat",
  "IgnoreMissingSetState": false
}
```

**Attributes:**
- `State` (Required) - State to compare (format: "Main.Sub" or ".Sub")
- `IgnoreMissingSetState` (Default: false) - Override validation checks

---

#### Timer Sensor
Tests if a timer exists and value is within range.

```json
{
  "Type": "Timer",
  "Name": "AttackCooldown",
  "State": "ANY",
  "TimeRemainingRange": [0, 999999]
}
```

**Attributes:**
- `Name` (Required) - Timer name
- `State` - Timer state: `ANY`, `RUNNING`, `PAUSED`, `STOPPED`
- `TimeRemainingRange` - Acceptable remaining time range

---

#### Alarm Sensor
Checks state of a named alarm.

```json
{
  "Type": "Alarm",
  "Name": "AlertCooldown",
  "State": "PASSED",
  "Clear": true
}
```

**Attributes:**
- `Name` (Required) - Alarm name
- `State` (Required) - State to check: `SET`, `UNSET`, `PASSED`
- `Clear` (Default: false) - Clear alarm if passed

---

#### Age Sensor
Triggers when NPC age falls within range (using ISO-8601 duration format).

```json
{
  "Type": "Age",
  "AgeRange": ["P0D", "P1D"]
}
```

**Attributes:**
- `AgeRange` (Required) - Age range using Period (P1Y2M3D) or Duration (PT3H4M)

---

### Environment Sensors

#### Path Sensor
Finds paths based on criteria.

```json
{
  "Type": "Path",
  "Path": null,
  "Range": 10,
  "PathType": "AnyPrefabPath"
}
```

**Attributes:**
- `Path` - Path name (null = nearest path)
- `Range` (Default: 10) - Range to nearest waypoint
- `PathType` - `AnyPrefabPath`, `CurrentPrefabPath`, `TransientPath`, `WorldPath`

**Provides:** Vector position, Path

---

#### Block Sensor
Checks for blocks in nearby area (cached until reset).

```json
{
  "Type": "Block",
  "Range": 10,
  "MaxHeight": 4,
  "Blocks": "PlantableBlocks",
  "Random": false,
  "Reserve": false
}
```

**Attributes:**
- `Range` (Required) - Search range
- `MaxHeight` (Default: 4) - Vertical search range
- `Blocks` (Required) - BlockSet asset to search for
- `Random` (Default: false) - Pick random vs closest
- `Reserve` (Default: false) - Reserve block from other NPCs

**Provides:** Vector position

---

#### BlockChange Sensor
Triggers when a block within range is changed or interacted with.

```json
{
  "Type": "BlockChange",
  "Range": 15,
  "BlockSet": "Breakable",
  "EventType": "DAMAGE",
  "SearchType": "PlayerOnly",
  "TargetSlot": null
}
```

**Attributes:**
- `Range` (Required) - Max range to listen
- `BlockSet` (Required) - Block set to listen for
- `EventType` - `DAMAGE`, `DESTRUCTION`, `INTERACTION`
- `SearchType` - `PlayerOnly`, `NpcOnly`, `PlayerFirst`, `NpcFirst`

**Provides:** Player target, NPC target

---

#### Weather Sensor
Matches current weather at NPC position.

```json
{
  "Type": "Weather",
  "Weathers": ["Rain", "Storm"]
}
```

**Attributes:**
- `Weathers` (Required) - Weather glob patterns to match

---

#### Light Sensor
Checks light levels at entity position.

```json
{
  "Type": "Light",
  "LightRange": [0, 100],
  "SkyLightRange": [0, 100],
  "SunlightRange": [0, 100],
  "RedLightRange": [0, 100],
  "GreenLightRange": [0, 100],
  "BlueLightRange": [0, 100],
  "UseTargetSlot": null
}
```

**Attributes:**
- `LightRange` - Light intensity percentage [0-100]
- `SkyLightRange`, `SunlightRange` - Sky/sun light ranges
- `RedLightRange`, `GreenLightRange`, `BlueLightRange` - RGB channel ranges
- `UseTargetSlot` - Check target instead of self

---

#### Time Sensor
Checks if day/year time is within specified range.

```json
{
  "Type": "Time",
  "Period": [6, 18],
  "CheckDay": true,
  "CheckYear": false,
  "ScaleDayTimeRange": true
}
```

**Attributes:**
- `Period` (Required) - Time range [0-24]
- `CheckDay` (Default: true) - Check day time
- `CheckYear` (Default: false) - Check year time
- `ScaleDayTimeRange` (Default: true) - Use relative scale (sunrise=6, noon=12, sunset=18)

---

#### Leash Sensor
Triggers when NPC is outside specified range from leash/spawn point.

```json
{
  "Type": "Leash",
  "Range": 30
}
```

**Attributes:**
- `Range` (Required) - Maximum distance from leash point

**Provides:** Vector position

---

#### InWater Sensor
Checks if NPC is currently in water.

```json
{
  "Type": "InWater"
}
```

---

#### OnGround Sensor
Tests if NPC is on ground.

```json
{
  "Type": "OnGround"
}
```

---

#### InAir Sensor
Tests if NPC is not on ground.

```json
{
  "Type": "InAir"
}
```

---

### Communication Sensors

#### Beacon Sensor
Checks for beacon messages from nearby NPCs.

```json
{
  "Type": "Beacon",
  "Message": "EnemySighted",
  "Range": 64,
  "TargetSlot": null,
  "ConsumeMessage": true
}
```

**Attributes:**
- `Message` (Required) - Message to listen for
- `Range` (Default: 64) - Max distance for beacons
- `TargetSlot` - Slot to store sender as target
- `ConsumeMessage` (Default: true) - Consume message after receiving

**Provides:** Player target, NPC target, Dropped item target

---

### Interaction Sensors

#### CanInteract Sensor
Checks if player can interact with this NPC.

```json
{
  "Type": "CanInteract",
  "ViewSector": 180,
  "Attitudes": ["NEUTRAL", "FRIENDLY", "REVERED"]
}
```

**Attributes:**
- `ViewSector` (Default: 0) - View sector to test player in [0-360]
- `Attitudes` - Attitudes to match

---

#### HasInteracted Sensor
Checks if player pressed interact button.

```json
{
  "Type": "HasInteracted"
}
```

---

#### HasTask Sensor
Checks if player has any of the given tasks.

```json
{
  "Type": "HasTask",
  "TasksById": ["DeliverItem", "CollectResources"]
}
```

**Attributes:**
- `TasksById` (Required) - Task names to match

---

### Item Sensors

#### DroppedItem Sensor
Triggers if item is within range.

```json
{
  "Type": "DroppedItem",
  "Range": 10,
  "ViewSector": 0,
  "LineOfSight": false,
  "Items": ["*"],
  "Attitudes": []
}
```

**Attributes:**
- `Range` (Required) - Detection range
- `ViewSector` (Default: 0) - View sector [0-360]
- `LineOfSight` (Default: false) - Require line of sight
- `Items` - Item glob patterns to match
- `Attitudes` - Item attitudes: `Neutral`, `Ignore`, `Like`, `Love`, `Dislike`

**Provides:** Dropped item target

---

### Navigation Sensors

#### Nav Sensor
Queries navigation/pathfinding state.

```json
{
  "Type": "Nav",
  "NavStates": ["BLOCKED", "ABORTED"],
  "ThrottleDuration": 0,
  "TargetDelta": 0
}
```

**Attributes:**
- `NavStates` - States to match: `INIT`, `PROGRESSING`, `AT_GOAL`, `BLOCKED`, `ABORTED`, `DEFER`
- `ThrottleDuration` (Default: 0) - Min time pathfinder can't reach target
- `TargetDelta` (Default: 0) - Min distance target moved since path computed

---

#### MotionController Sensor
Tests if specific motion controller is active.

```json
{
  "Type": "MotionController",
  "MotionController": "Walk"
}
```

**Attributes:**
- `MotionController` (Required) - Motion controller name

---

### Utility Sensors

#### Any Sensor
Always returns true (no target).

```json
{
  "Type": "Any"
}
```

---

#### Flag Sensor
Tests if a named flag is set.

```json
{
  "Type": "Flag",
  "Name": "HasAttacked",
  "Set": true
}
```

**Attributes:**
- `Name` (Required) - Flag name
- `Set` (Default: true) - Whether flag should be set or not

---

#### Animation Sensor
Checks if animation is being played.

```json
{
  "Type": "Animation",
  "Slot": "Action",
  "Animation": "Attack"
}
```

**Attributes:**
- `Slot` (Required) - Animation slot: `Status`, `Action`, `Face`
- `Animation` (Required) - Animation ID

---

#### Self Sensor
Tests if NPC itself matches entity filters.

```json
{
  "Type": "Self",
  "Filters": [
    {
      "Type": "Stat",
      "Stat": "Health",
      "StatTarget": "Value",
      "RelativeTo": "Health",
      "RelativeToTarget": "Max",
      "ValueRange": [0, 0.5]
    }
  ]
}
```

**Provides:** Vector position

---

#### Random Sensor
Alternates between true/false for random durations.

```json
{
  "Type": "Random",
  "TrueDurationRange": [2, 5],
  "FalseDurationRange": [3, 8]
}
```

**Attributes:**
- `TrueDurationRange` (Required) - Duration range to return true
- `FalseDurationRange` (Required) - Duration range to return false

---

#### Switch Sensor
Checks if a computed boolean is true.

```json
{
  "Type": "Switch",
  "Switch": { "Compute": "IsAggressive" }
}
```

**Attributes:**
- `Switch` (Required) - Boolean value to check

---

### Advanced Sensors

#### CombatActionEvaluator Sensor
Used with the Combat Action Evaluator system to control advanced combat AI.

```json
{
  "Type": "CombatActionEvaluator",
  "TargetInRange": true
}
```

**Attributes:**
- `TargetInRange` (Boolean) - Check if target is within combat range

**Provides:** Combat evaluation state

---

#### HasHostileTargetMemory Sensor
Checks if the NPC has hostile targets in memory.

```json
{
  "Type": "HasHostileTargetMemory"
}
```

**Usage:** Used in combat detection to check if NPC remembers hostile entities.

---

#### InteractionContext Sensor
Checks if the player is using a specific interaction context (e.g., harvesting, mounting).

```json
{
  "Type": "InteractionContext",
  "Context": "Harvest"
}
```

**Attributes:**
- `Context` (String) - The interaction context to check

**Common Contexts:** `"Harvest"`, `"Mount"`, `"Shear"`, `"Milk"`

---

#### ReadPosition Sensor
Reads a stored position from a slot and checks if within range.

```json
{
  "Type": "ReadPosition",
  "Slot": "PatrolPosition",
  "Range": 20,
  "MinRange": 0
}
```

**Attributes:**
- `Slot` (String, Required) - Name of the position slot to read
- `Range` (Number) - Maximum range to check
- `MinRange` (Number) - Minimum range to check

**Provides:** Position target (stored position)

---

#### Eval Sensor
Evaluates a condition expression.

```json
{
  "Type": "Eval",
  "Eval": "blocked"
}
```

**Attributes:**
- `Eval` (String) - The expression to evaluate

**Common Expressions:** `"blocked"` (NPC is blocked from moving)

---

#### FlockLeader Sensor
Detects the flock leader.

```json
{
  "Type": "FlockLeader",
  "Range": 15
}
```

**Attributes:**
- `Range` (Number) - Range to detect flock leader

**Provides:** NPC target (the flock leader)

## Sensor Modifiers (Logical Operators)

### And Sensor
All sensors must return true.

```json
{
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
  ],
  "AutoUnlockTargetSlot": null
}
```

**Attributes:**
- `Sensors` (Required) - Array of sensors (all must match)
- `AutoUnlockTargetSlot` - Target slot to unlock when not matching

---

### Or Sensor
At least one sensor must return true.

```json
{
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
  ],
  "AutoUnlockTargetSlot": null
}
```

**Attributes:**
- `Sensors` (Required) - Array of sensors (any can match)
- `AutoUnlockTargetSlot` - Target slot to unlock when not matching

---

### Not Sensor
Inverts sensor result.

```json
{
  "Type": "Not",
  "Sensor": {
    "Type": "Player",
    "Range": 10
  },
  "UseTargetSlot": null,
  "AutoUnlockTargetSlot": null
}
```

**Attributes:**
- `Sensor` (Required) - Sensor to invert
- `UseTargetSlot` - Locked target slot to feed to action
- `AutoUnlockTargetSlot` - Target slot to unlock when not matching

---

## Entity Filters (IEntityFilter)

Entity filters are used within sensors to refine detection. They test conditions against detected entities.

### LineOfSight Filter
Matches if line of sight exists to target.

```json
{
  "Type": "LineOfSight"
}
```

---

### ViewSector Filter
Matches entities within view cone.

```json
{
  "Type": "ViewSector",
  "ViewSector": 180
}
```

**Attributes:**
- `ViewSector` (Default: 0) - View sector angle [0-360]

---

### Altitude Filter
Matches targets within defined height range above ground.

```json
{
  "Type": "Altitude",
  "AltitudeRange": [0, 10]
}
```

**Attributes:**
- `AltitudeRange` (Required) - Height range above ground

---

### HeightDifference Filter
Matches entities within height range relative to NPC.

```json
{
  "Type": "HeightDifference",
  "HeightDifference": [-5, 5],
  "UseEyePosition": true
}
```

**Attributes:**
- `HeightDifference` - Height range (negative = below NPC)
- `UseEyePosition` (Default: true) - Use eye position for checks

---

### Attitude Filter
Matches NPC's attitude towards target.

```json
{
  "Type": "Attitude",
  "Attitudes": ["HOSTILE", "NEUTRAL"]
}
```

**Attitudes:** `HOSTILE`, `FRIENDLY`, `NEUTRAL`, `REVERED`, `IGNORE`

---

### NPCGroup Filter
Matches entities in specific NPC groups.

```json
{
  "Type": "NPCGroup",
  "IncludeGroups": ["Hostile_Creatures"],
  "ExcludeGroups": null
}
```

**Attributes:**
- `IncludeGroups` - Groups to include (or)
- `ExcludeGroups` - Groups to exclude

---

### Combat Filter
Checks target's combat state.

```json
{
  "Type": "Combat",
  "Mode": "Attacking",
  "Sequence": null,
  "TimeElapsedRange": [0, 999999]
}
```

**Modes:** `Any`, `None`, `Attacking`, `Blocking`, `Melee`, `Ranged`, `Charging`, `Sequence`

---

### MovementState Filter
Checks entity movement state.

```json
{
  "Type": "MovementState",
  "State": "RUNNING"
}
```

**States:** `ANY`, `IDLE`, `WALKING`, `RUNNING`, `SPRINTING`, `CROUCHING`, `JUMPING`, `FALLING`, `CLIMBING`, `FLYING`

---

### Flock Filter
Tests flock membership and properties.

```json
{
  "Type": "Flock",
  "FlockStatus": "Member",
  "FlockPlayerStatus": "Any",
  "Size": null,
  "CheckCanJoin": false
}
```

**FlockStatus:** `Any`, `Leader`, `Follower`, `Member`, `NotMember`

---

### Inventory Filter
Tests entity inventory conditions.

```json
{
  "Type": "Inventory",
  "Items": ["*"],
  "CountRange": [1, 2147483647],
  "FreeSlotRange": [0, 2147483647]
}
```

**Attributes:**
- `Items` - Item glob patterns
- `CountRange` - Required item count range
- `FreeSlotRange` - Required free slots range

---

### ItemInHand Filter
Checks if entity is holding an item.

```json
{
  "Type": "ItemInHand",
  "Items": ["Weapon_Sword_*"],
  "Hand": "Both"
}
```

**Attributes:**
- `Items` (Required) - Item glob patterns
- `Hand` - `Main`, `OffHand`, `Both`

---

### SpotsMe Filter
Checks if entity can see the NPC.

```json
{
  "Type": "SpotsMe",
  "ViewAngle": 90,
  "ViewTest": "VIEW_SECTOR",
  "TestLineOfSight": true
}
```

**ViewTest:** `NONE`, `VIEW_SECTOR`, `VIEW_CONE`

---

### Stat Filter
Matches entity stat values.

```json
{
  "Type": "Stat",
  "Stat": "Health",
  "StatTarget": "Value",
  "RelativeTo": "Health",
  "RelativeToTarget": "Max",
  "ValueRange": [0, 0.3]
}
```

**StatTarget/RelativeToTarget:** `Value`, `Min`, `Max`

---

### StandingOnBlock Filter
Matches block beneath entity.

```json
{
  "Type": "StandingOnBlock",
  "BlockSet": "Flammable"
}
```

---

### InsideBlock Filter
Matches if entity is inside blocks in BlockSet.

```json
{
  "Type": "InsideBlock",
  "BlockSet": "Water"
}
```

---

### And/Or/Not Filters
Logical operators for combining filters.

```json
{
  "Type": "And",
  "Filters": [
    { "Type": "LineOfSight" },
    { "Type": "ViewSector", "ViewSector": 120 }
  ]
}
```

---

## Sensor Prioritiser

Prioritises returned entities when multiple match.

### Attitude Prioritiser

```json
{
  "Prioritiser": {
    "Type": "Attitude",
    "AttitudesByPriority": ["HOSTILE", "NEUTRAL", "FRIENDLY"]
  }
}
```

---

## Sensor Collector

Processes all matched entities (e.g., for combat memory).

### CombatTargets Collector

```json
{
  "Collector": {
    "Type": "CombatTargets"
  }
}
```

Processes matched friendly/hostile targets and adds them to NPC's short-term combat memory.

## Common Sensor Attributes

All sensors share these common attributes:

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `Once` | Boolean | false | Sensor only triggers once |
| `Enabled` | Boolean/Computable | true | Whether sensor is enabled |

---

## Standard Detection Component

`Server/NPC/Roles/_Core/Components/Sensors/Component_Sensor_Standard_Detection.json`:

Combines absolute detection, sight, and sound sensors for intelligent NPCs.

---

## Tips for NPC Sensors

1. **Combined sensors** - Use `Type: "And"` / `Type: "Or"` to combine multiple sensors
2. **Filters** - Use entity filters to refine detection (line-of-sight, groups, attitudes)
3. **Range balance** - Too large = too sensitive, too small = misses targets
4. **View sector** - Control vision cone angle with ViewSector filter
5. **Component reuse** - Create sensor components for reuse across NPCs
6. **Target locking** - Use `LockOnTarget` to persist detected targets
7. **Auto unlock** - Use `AutoUnlockTarget` to release targets when they escape
8. **Once flag** - Use `Once: true` for one-time triggers
9. **Prioritiser** - Use to select best target when multiple match
10. **Computable values** - Use `{ "Compute": "ParameterName" }` for configurable values

---

## Sensor Quick Reference

| Sensor | Purpose | Provides |
|--------|---------|----------|
| `Player` | Detect players | Player/NPC target |
| `Mob` | Detect NPCs | Player/NPC target |
| `Target` | Check locked target | Player/NPC target |
| `Damage` | Detect incoming damage | Player/NPC/Item target |
| `State` | Check NPC state | - |
| `Path` | Find patrol paths | Vector position, Path |
| `Block` | Find blocks | Vector position |
| `Beacon` | Receive NPC messages | Player/NPC/Item target |
| `Timer` | Check timer state | - |
| `Alarm` | Check alarm state | - |
| `Leash` | Check distance from spawn | Vector position |
| `DroppedItem` | Find dropped items | Dropped item target |
| `CanInteract` | Check interaction availability | - |
| `HasInteracted` | Check player interaction | - |
| `Any` | Always true | - |
| `Flag` | Check NPC flags | - |

---

**Previous:** [NPC State Transitions](80_NPC_State_Transitions.md) | **Next:** [NPC Instructions](82_NPC_Instructions.md)
