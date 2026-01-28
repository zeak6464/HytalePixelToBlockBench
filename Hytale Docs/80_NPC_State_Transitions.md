# NPC State Transitions

Learn how to configure NPC state transitions that trigger actions when NPCs change behavioral states.

## Overview

NPC state transitions trigger actions when NPCs move between states (e.g., Idle → Combat, Sleep → Awake). They allow NPCs to react to state changes with animations, inventory changes, and other behaviors.

**State transitions are composed of:**
- **StateTransitionController** - Container for all state transitions
- **StateTransition** - Individual transition with states and actions
- **StateTransitionEdges** - Defines from/to state pairs with priority

## Location
State transitions are configured in `StateTransitions` property in NPC Role definitions.

## Official Documentation Reference
See [Hytale NPC Documentation](https://hytalemodding.dev/en/docs/official-documentation/npc-doc) for complete state transition specifications.

## Example from Game Files

### NPC State Transition from Template

From `Server/NPC/Roles/_Core/Templates/Template_Intelligent.json`:

NPC state transitions are configured within the Instructions array, where sensors detect state changes and trigger actions. For example, when an NPC receives damage, it transitions from Idle to Combat state.

## Basic State Transition Structure

```json
{
  "StateTransitions": [
    {
      "States": [
        {
          "From": ["Idle"],
          "To": ["Combat"]
        }
      ],
      "Actions": [
        {
          "Type": "PlayAnimation",
          "Slot": "Status",
          "Animation": "Alerted"
        }
      ]
    }
  ]
}
```

## State Transition Structure

### StateTransitionController
A list of state transitions. This is the top-level container.

```json
{
  "StateTransitions": [
    { /* StateTransition 1 */ },
    { /* StateTransition 2 */ }
  ]
}
```

---

### StateTransition
An individual transition entry with states and actions.

```json
{
  "States": [
    {
      "From": ["Idle"],
      "To": ["Combat", "Alerted"],
      "Priority": 0,
      "Enabled": true
    }
  ],
  "Actions": [ ... ],
  "Enabled": true
}
```

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `States` | Array | Required | List of StateTransitionEdges |
| `Actions` | ObjectRef | Required | Actions to execute on transition |
| `Enabled` | Boolean/Computable | true | Whether transition is enabled |

---

### StateTransitionEdges
Defines from/to state pairs with optional priority.

```json
{
  "From": ["Idle", "Patrol"],
  "To": ["Combat", "Alerted"],
  "Priority": 0,
  "Enabled": true
}
```

**Attributes:**

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `From` | StringList | Required | Source states (empty = any state) |
| `To` | StringList | Required | Target states |
| `Priority` | Integer | 0 | Priority for actions (higher = first) |
| `Enabled` | Boolean/Computable | true | Whether edge is enabled |

---

## State Naming Convention

States use the format `"Main.Sub"`:
- **Main state** - Primary state (e.g., "Idle", "Combat", "Patrol")
- **Sub state** - Secondary state within main (e.g., "Idle.Default", "Combat.Melee")

When nested within a substate context, you can omit the main state: `".Default"`

---

## Priority System

When multiple transitions match, priority determines which actions execute first:

```json
{
  "States": [
    {
      "From": ["*"],
      "To": ["Combat"],
      "Priority": 10
    }
  ],
  "Actions": [ /* High priority actions */ ]
},
{
  "States": [
    {
      "From": ["Idle"],
      "To": ["Combat"],
      "Priority": 0
    }
  ],
  "Actions": [ /* Default priority actions */ ]
}
```

Higher priority transitions execute their actions first.

---

### Actions

```json
{
  "Actions": [
    {
      "Type": "PlayAnimation",
      "Slot": "Status",
      "Animation": "Laydown"
    },
    {
      "Type": "Timeout",
      "Delay": [0.83, 0.83]
    }
  ]
}
```

Actions executed when transition occurs. See [NPC Instructions](82_NPC_Instructions.md) for complete action reference.

### Action References

```json
{
  "Actions": {
    "Reference": "Component_ActionList_Wake"
  }
}
```

Reference reusable action list components for DRY code.

## Common State Transitions

### Sleep Transition

```json
{
  "StateTransitions": [
    {
      "States": [
        {
          "From": ["Idle"],
          "To": ["Sleep"]
        }
      ],
      "Actions": [
        {
          "Type": "Inventory",
          "Operation": "EquipOffHand",
          "Slot": { "Compute": "DefaultOffHandSlot" }
        },
        {
          "Type": "PlayAnimation",
          "Slot": "Status",
          "Animation": "Laydown"
        }
      ]
    }
  ]
}
```

### Wake Transition

```json
{
  "StateTransitions": [
    {
      "States": [
        {
          "From": ["Sleep"],
          "To": ["Alerted", "Idle"]
        }
      ],
      "Actions": {
        "Reference": "Component_ActionList_Wake"
      }
    }
  ]
}
```

### Combat Entry

```json
{
  "StateTransitions": [
    {
      "States": [
        {
          "From": [],
          "To": ["Idle", "Investigate", "Alerted", "Combat"]
        }
      ],
      "Actions": [
        {
          "Type": "Inventory",
          "Operation": "EquipHotbar",
          "Slot": { "Compute": "DefaultHotbarSlot" }
        }
      ]
    }
  ]
}
```

### Return to Idle

```json
{
  "StateTransitions": [
    {
      "States": [
        {
          "To": ["Idle"],
          "From": []
        }
      ],
      "Actions": [
        {
          "Type": "ReleaseTarget"
        },
        {
          "Type": "ResetInstructions"
        }
      ]
    }
  ]
}
```

## Action Types

### PlayAnimation

```json
{
  "Type": "PlayAnimation",
  "Slot": "Status",
  "Animation": "Alerted"
}
```

### Inventory Operations

```json
{
  "Type": "Inventory",
  "Operation": "EquipHotbar",
  "Slot": { "Compute": "DefaultHotbarSlot" }
}
```

### Timeout

```json
{
  "Type": "Timeout",
  "Delay": [2, 3]
}
```

Delay before next action (seconds).

### ReleaseTarget

```json
{
  "Type": "ReleaseTarget"
}
```

Clears NPC's combat target.

### ResetInstructions

```json
{
  "Type": "ResetInstructions"
}
```

Resets NPC behavior instructions.

### SetFlag

```json
{
  "Type": "SetFlag",
  "Name": "AnimationBlock",
  "SetTo": false
}
```

Sets internal NPC flags.

## Tips for State Transitions

1. **Empty arrays** - `From: []` matches any state (wildcard)
2. **Multiple states** - Arrays allow matching multiple source/destination states
3. **Action sequences** - Actions execute in order within a transition
4. **Priority system** - Use `Priority` to control action execution order
5. **Component references** - Use action list components for reusability
6. **Enable/disable** - Use `Enabled` with Computable values for dynamic transitions
7. **State format** - Use "Main.Sub" format for organized state hierarchy
8. **Common patterns** - Sleep/Wake, Combat Entry/Exit, Interaction states

---

## State Transition Quick Reference

| Pattern | From | To | Use Case |
|---------|------|-----|----------|
| Any → Combat | `[]` | `["Combat"]` | Entering combat from any state |
| Combat → Idle | `["Combat"]` | `["Idle"]` | Exiting combat |
| Sleep ↔ Awake | `["Sleep"]` | `["Idle", "Alerted"]` | Waking up |
| Idle → Sleep | `["Idle"]` | `["Sleep"]` | Going to sleep |
| Any → Any | `[]` | `[]` | Global transition (use carefully) |

---

## Common Action Types for Transitions

| Action | Use Case |
|--------|----------|
| `PlayAnimation` | Play transition animation |
| `Inventory` | Equip/unequip weapons |
| `ReleaseTarget` | Clear combat target |
| `ResetInstructions` | Reset behavior state |
| `SetFlag` | Set internal flags |
| `Timeout` | Delay before next action |

---

**Previous:** [Item Categories](79_Item_Categories.md) | **Next:** [NPC Sensors](81_NPC_Sensors.md)
