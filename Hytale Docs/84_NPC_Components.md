# NPC Components

Learn how to create reusable NPC behavior components for sensors, instructions, and actions.

## Overview

NPC components are reusable behavior definitions that can be referenced by multiple NPCs. Components include sensors, instructions, and action lists that promote code reuse and consistency.

## Location
`Server/NPC/Roles/_Core/Components/`

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
