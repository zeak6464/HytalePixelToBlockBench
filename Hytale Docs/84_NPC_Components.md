# NPC Components

Learn how to create reusable NPC behavior components for sensors, instructions, and actions.

## Overview

NPC components are reusable behavior definitions that can be referenced by multiple NPCs. Components include sensors, instructions, and action lists that promote code reuse and consistency.

## Location
`Server/NPC/Roles/_Core/Components/`

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

---

## Component patterns from `Template_Goblin_Ogre`

The `NPC_Tutorial_Draft.pdf` uses the **Goblin Ogre** as a teaching example. In the current game files, the same patterns exist and are implemented via reusable components.

### Pattern: “Wait a bit, then return to parent state”

`Component_Instruction_State_Timeout` is a small reusable building block used heavily across NPCs.

```1:25:Server/NPC/Roles/_Core/Components/Steps/Component_Instruction_State_Timeout.json
{
  "Type": "Component",
  "Class": "Instruction",
  "Parameters": {
    "_ImportStates": [ "Main" ],
    "Delay": {
      "Value": [ 3, 5 ],
      "Description": "The amount of time to wait before transitioning"
    }
  },
  "Content": {
    "Continue": true,
    "ActionsBlocking": true,
    "Actions": [
      {
        "Type": "Timeout",
        "Delay": { "Compute": "Delay" }
      },
      {
        "Type": "ParentState",
        "State": "Main"
      }
    ]
  }
}
```

Key ideas:

- **`_ImportStates`** declares “slots” a component can jump back to (here: `Main`).
- Call-sites typically provide **`_ExportStates`** when referencing the component to define what `Main` should mean in that context.
- **`ParentState`** switches back to that exported state.

### Pattern: “Play an animation while a timeout runs”

`Component_Instruction_Play_Animation_In_State_For_Duration` composes two smaller components:

```1:33:Server/NPC/Roles/_Core/Components/Steps/Component_Instruction_Play_Animation_In_State_For_Duration.json
{
  "Type": "Component",
  "Class": "Instruction",
  "Parameters": {
    "_ImportStates": [ "Main" ],
    "Animation": {
      "Value": "",
      "Description": "The animation to play"
    },
    "Duration": {
      "Value": [ 3, 5 ],
      "Description": "The amount of time to wait before transitioning"
    }
  },
  "Content": {
    "Continue": true,
    "Instructions": [
      {
        "Reference": "Component_Instruction_State_Timeout",
        "Modify": {
          "_ExportStates": [ "Main" ],
          "Delay": { "Compute": "Duration" }
        }
      },
      {
        "Reference": "Component_Instruction_Play_Animation",
        "Modify": {
          "Animation": { "Compute": "Animation" }
        }
      }
    ]
  }
}
```

This is used in `Template_Goblin_Ogre` for sleep/eat behaviors, while still ensuring the NPC eventually transitions to another state.

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

## Tips for NPC Components

1. **Reusability** - Create components for common behaviors
2. **Parameters** - Use parameters for configurability
3. **Modify** - Override parameters when referencing
4. **Organization** - Organize by folder (Sensors, Steps, etc.)

---

**Previous:** [NPC Motion Controllers](83_NPC_Motion_Controllers.md) | **Next:** [NPC Appearance](85_NPC_Appearance.md)
