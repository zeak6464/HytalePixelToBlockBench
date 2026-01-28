# NPC Components

Learn how to create reusable NPC behavior components for sensors, instructions, and actions.

## Overview

NPC components are reusable behavior definitions that can be referenced by multiple NPCs. Components include sensors, instructions, and action lists that promote code reuse and consistency. The component system uses a **Reference/Modify** pattern for inheritance-like behavior.

**Component Types:**
- **Instruction Components** - Reusable behavior trees
- **Sensor Components** - Reusable detection logic
- **ActionList Components** - Reusable action sequences

## Location
`Server/NPC/Roles/_Core/Components/`

## Official Documentation Reference
See [Hytale NPC Documentation](https://hytalemodding.dev/en/docs/official-documentation/npc-doc) for component specifications.

## Example from Game Files

### Path Following Component

From `Server/NPC/Roles/_Core/Components/Steps/Component_Instruction_Intelligent_Idle_Motion_Follow_Path.json`:

```1:33:Server/NPC/Roles/_Core/Components/Steps/Component_Instruction_Intelligent_Idle_Motion_Follow_Path.json
{
  "Class": "Instruction",
  "Interface": "Hytale.Instruction.Intelligent.Idle.Motion",
  "Parameters": {
    "FollowPathRange": {
      "Value": 30,
      "Description": "The search radius when looking for a patrol path"
    },
    "PathShape": {
      "Value": "Line",
      "Description": "The shape of the path the NPC should follow. 'Chain' will result in chained paths"
    }
  },
  "Content": {
    "Continue": true,
    "Sensor": {
      "Type": "Path",
      "Range": { "Compute": "FollowPathRange" }
    },
    "Actions": [
      {
        "Type": "SetLeashPosition",
        "ToCurrent": true
      }
    ],
    "BodyMotion": {
      "Type": "Path",
      "UseNodeViewDirection": true,
      "Shape": { "Compute": "PathShape" }
    }
  },
  "Type": "Component"
}
```

This shows a reusable NPC component for following paths that can be referenced by multiple NPCs, with configurable parameters and sensor-based path detection.

Components are organized into folders:
- **`Sensors/`** - Detection components
- **`Steps/`** - Instruction components
- **`Selectors/`** - Targeting/selection components
- **`ActionLists/`** - Action sequence components
- **`Flock/`** - Flocking behavior components
- **`Paths/`** - Path-related components

## Basic Component Structure

Create `Server/NPC/Roles/_Core/Components/Steps/Component_MyCustom.json`:

```json
{
  "Class": "Instruction",
  "Interface": "Hytale.Instruction.Intelligent.Idle.Motion",
  "Type": "Component",
  "Parameters": {
    "FollowPathRange": {
      "Value": 30,
      "Description": "Search radius for patrol paths"
    }
  },
  "Content": {
    "Sensor": {
      "Type": "Path",
      "Range": { "Compute": "FollowPathRange" }
    },
    "BodyMotion": {
      "Type": "Path",
      "UseNodeViewDirection": true
    }
  }
}
```

## Component Properties

### Class

```json
{
  "Class": "Instruction"  // or "Sensor", "ActionList"
}
```

Component class type.

### Interface

```json
{
  "Interface": "Hytale.Instruction.Intelligent.Idle.Motion"
}
```

Interface/category for component organization.

### Type

```json
{
  "Type": "Component"
}
```

Must be `"Component"` for component definitions.

### Parameters

```json
{
  "Parameters": {
    "ParameterName": {
      "Value": 30,
      "Description": "Parameter description"
    }
  }
}
```

Configurable parameters for the component.

### Content

```json
{
  "Content": {
    "Sensor": { ... },
    "BodyMotion": { ... }
  }
}
```

The actual behavior definition.

## Using Components

Reference components in NPC definitions:

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

## Component Types

### Sensor Components

`Server/NPC/Roles/_Core/Components/Sensors/Component_Sensor_Standard_Detection.json`:

Reusable sensor definitions.

### Instruction Components

`Server/NPC/Roles/_Core/Components/Steps/Component_Instruction_Intelligent_Idle_Motion_Follow_Path.json`:

Reusable instruction behaviors.

### Action List Components

`Server/NPC/Roles/_Core/Components/ActionLists/Component_ActionList_Wake.json`:

Reusable action sequences.

## NPC Role Types

From the official documentation, NPC Roles define the core behavior configuration:

### Generic Role
Standard role for NPCs with full behavior configuration.

```json
{
  "Type": "Generic",
  "MaxHealth": 100,
  "Appearance": "Goblin_Scrapper",
  "NameTranslationKey": "npc.goblin.name",
  "StartState": "Idle",
  "DefaultSubState": "Default",
  "BusyStates": ["Chase", "Combat"],
  "DefaultPlayerAttitude": "HOSTILE",
  "DefaultNPCAttitude": "NEUTRAL",
  "MotionControllerList": [ ... ],
  "Instructions": [ ... ],
  "StateTransitions": [ ... ]
}
```

### Abstract Role
Generic role used as a base template (not instantiated directly).

### Variant Role
Creates a variant from an existing NPC JSON file.

```json
{
  "Type": "Variant",
  "Reference": "Template_Trork_Melee",
  "Modify": {
    "Appearance": "Trork_Warrior",
    "MaxHealth": 80
  }
}
```

---

## Key Role Attributes

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `MaxHealth` | Integer | Required | Maximum health points |
| `Appearance` | Asset | Required | Model to use for rendering |
| `NameTranslationKey` | String | Required | Translation key for NPC name |
| `StartState` | String | "start" | Initial state |
| `DefaultSubState` | String | "Default" | Default sub-state when transitioning |
| `BusyStates` | StringList | Required | States where NPC can't be interrupted |
| `DefaultPlayerAttitude` | Flag | HOSTILE | Attitude towards players |
| `DefaultNPCAttitude` | Flag | NEUTRAL | Attitude towards other NPCs |
| `AttitudeGroup` | Asset | null | Attitude group for NPC relations |
| `Invulnerable` | Boolean | false | Ignore damage |
| `KnockbackScale` | Double | 1.0 | Knockback multiplier |
| `DeathAnimationTime` | Double | 5.0 | Death animation duration |
| `DespawnAnimationTime` | Double | 0.8 | Despawn animation duration |
| `SpawnLockTime` | Double | 1.5 | Time NPC is locked after spawn |

**Attitudes:** `HOSTILE`, `FRIENDLY`, `NEUTRAL`, `REVERED`, `IGNORE`

---

## Inventory Configuration

```json
{
  "InventorySize": 9,
  "HotbarSize": 3,
  "OffHandSlots": 1,
  "HotbarItems": ["Weapon_Sword_Iron"],
  "OffHandItems": ["Shield_Wooden"],
  "DefaultOffHandSlot": 0,
  "Armor": ["Armor_Chest_Iron"],
  "DropList": "Goblin_Drops"
}
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `InventorySize` | Integer | 0 | Number of inventory slots [0-36] |
| `HotbarSize` | Integer | 3 | Number of hotbar slots [3-8] |
| `OffHandSlots` | Integer | 0 | Off-hand slots [0-4] |
| `HotbarItems` | AssetArray | null | Hotbar items to spawn with |
| `OffHandItems` | AssetArray | null | Off-hand items |
| `DefaultOffHandSlot` | Integer | -1 | Default off-hand slot |
| `Armor` | AssetArray | null | Armor items |
| `DropList` | Asset | null | Drop list when killed |

---

## Collision & Avoidance

```json
{
  "CollisionDistance": 5,
  "CollisionRadius": -1,
  "CollisionViewAngle": 320,
  "SeparationDistance": 3,
  "SeparationWeight": 1,
  "ApplyAvoidance": false,
  "ApplySeparation": false,
  "AvoidanceMode": "Any",
  "EntityAvoidanceStrength": 1
}
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `CollisionDistance` | Double | 5.0 | Collision lookahead distance |
| `SeparationDistance` | Double | 3.0 | Desired separation from others |
| `ApplyAvoidance` | Boolean | false | Apply avoidance steering |
| `ApplySeparation` | Boolean | false | Apply separation steering |
| `AvoidanceMode` | Flag | Any | `Evade`, `Slowdown`, `Any` |

---

## Flock Configuration

```json
{
  "FlockSpawnTypes": ["Sheep", "Lamb"],
  "FlockSpawnTypesRandom": false,
  "FlockAllowedNPC": ["Sheep"],
  "FlockCanLead": true,
  "FlockInfluenceRange": 10,
  "DisableDamageFlock": true
}
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `FlockSpawnTypes` | StringArray | null | NPC types for flock |
| `FlockSpawnTypesRandom` | Boolean | false | Randomize flock composition |
| `FlockAllowedNPC` | StringArray | null | NPCs allowed to join flock |
| `FlockCanLead` | Boolean | false | Can be flock leader |
| `FlockInfluenceRange` | Double | 10.0 | Flock influence radius |
| `DisableDamageFlock` | Boolean | true | Disable damage from flock members |

---

## Tips for NPC Components

1. **Reusability** - Create components for common behaviors across NPC types
2. **Parameters** - Use parameters for configurability with descriptions
3. **Modify** - Override parameters when referencing to create variants
4. **Organization** - Organize by folder (Sensors, Steps, ActionLists, Flock, Paths)
5. **Interface** - Use Interface for component categorization
6. **Templates** - Create abstract templates for NPC families
7. **Variants** - Use Variant type for minor NPC variations
8. **BusyStates** - Always define states where NPC shouldn't be interrupted
9. **Attitudes** - Configure appropriate attitudes for NPC behavior
10. **Flock** - Use flock configuration for group behaviors

---

## Component Organization

```
Server/NPC/Roles/_Core/Components/
├── Sensors/           # Detection components
├── Steps/             # Instruction components
├── Selectors/         # Targeting components
├── ActionLists/       # Action sequence components
├── Flock/             # Flocking behavior components
└── Paths/             # Path-related components
```

---

**Previous:** [NPC Motion Controllers](83_NPC_Motion_Controllers.md) | **Next:** [NPC Appearance](85_NPC_Appearance.md)
