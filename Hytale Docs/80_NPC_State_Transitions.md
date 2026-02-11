# NPC State Transitions

Learn how to configure NPC state transitions that trigger actions when NPCs change behavioral states.

## Overview

NPC state transitions trigger actions when NPCs move between states (e.g., Idle → Combat, Sleep → Awake). They allow NPCs to react to state changes with animations, inventory changes, and other behaviors.

## Location
State transitions are configured in `StateTransitions` array in NPC Role definitions.

## Example from Game Files

### NPC State Transition from Template

State transitions live in a top-level `StateTransitions` array on the NPC role (alongside fields like `StartState`, `Instructions`, etc.).

From `Server/NPC/Roles/Intelligent/Aggressive/Goblin/Templates/Template_Goblin_Ogre.json` (sleep + wake transitions):

```132:170:Server/NPC/Roles/Intelligent/Aggressive/Goblin/Templates/Template_Goblin_Ogre.json
  "StateTransitions": [
    {
      "States": [
        {
          "From": [ "Idle" ],
          "To": [ "Sleep" ]
        }
      ],
      "Actions": [
        {
          "Type": "PlayAnimation",
          "Slot": "Status",
          "Animation": "Laydown"
        },
        {
          "Type": "Timeout",
          "Delay": [ 1, 1 ]
        }
      ]
    },
    {
      "States": [
        {
          "From": [ "Sleep" ],
          "To": [ ]
        }
      ],
      "Actions": [
        {
          "Type": "PlayAnimation",
          "Slot": "Status",
          "Animation": "Wake"
        },
        {
          "Type": "Timeout",
          "Delay": [ 1, 1 ]
        }
      ]
    },
```

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

## State Transition Properties

### States

```json
{
  "States": [
    {
      "From": ["Idle"],
      "To": ["Combat", "Alerted"]
    }
  ]
}
```

- **`From`** - Array of source states (empty array = any state)
- **`To`** - Array of target states (empty array = any state)

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

Actions executed when transition occurs.

### Action References

```json
{
  "Actions": {
    "Reference": "Component_ActionList_Wake"
  }
}
```

Reference reusable action list components.

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

### Equipping items on transition (Eat / CallRat → Eat)

From `Template_Goblin_Ogre.json`:

```171:215:Server/NPC/Roles/Intelligent/Aggressive/Goblin/Templates/Template_Goblin_Ogre.json
    {
      "States": [
        {
          "From": [ "Idle" ],
          "To": [ "Eat" ]
        }
      ],
      "Actions": [
        {
          "Type": "Inventory",
          "Operation": "SetHotbar",
          "Item": { "Compute": "EatItem" },
          "Slot": 2,
          "UseTarget": false
        },
        {
          "Type": "Inventory",
          "Operation": "EquipHotbar",
          "Slot": 2,
          "UseTarget": false
        }
      ]
    },
    {
      "States": [
        {
          "From": [ "CallRat" ],
          "To": [ "Eat" ]
        }
      ],
      "Actions": [
        {
          "Type": "Inventory",
          "Operation": "SetHotbar",
          "Item": { "Compute": "FoodNPCItem" },
          "Slot": 2,
          "UseTarget": false
        },
        {
          "Type": "Inventory",
          "Operation": "EquipHotbar",
          "Slot": 2,
          "UseTarget": false
        }
      ]
    },
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

## Official Resources

- **Generated NPC Documentation:** https://hytalemodding.dev/en/docs/official-documentation/npc-doc
- **NPC Tutorial:** https://hytalemodding.dev/en/docs/official-documentation/npc/1-know-your-enemy

---

## Tips for State Transitions

1. **Empty arrays** - `From: []` matches any state, `To: []` matches any destination
2. **Multiple states** - Arrays allow matching multiple source/destination states
3. **Action sequences** - Actions execute in order
4. **Component references** - Use action list components for reusability
5. **Priority** - Some transitions may have priority settings

---

**Previous:** [Item Categories](79_Item_Categories.md) | **Next:** [NPC Sensors](81_NPC_Sensors.md)
