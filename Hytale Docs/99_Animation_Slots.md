# Animation Slots

Learn how animation slots allow multiple animations to play simultaneously on NPCs and players.

## Overview

Animation slots allow different types of animations to play at the same time. Common slots include Movement (walking/running), Status (idle/alerted), and Combat (attacking/blocking).

## Location
Animation slots are specified in animation actions and NPC behavior definitions.

## Example from Game Files

### Animation Slot

From `Server/NPC/Roles/_Core/Tests/Test_Animation.json`:

```34:37:Server/NPC/Roles/_Core/Tests/Test_Animation.json
              {
                "Type": "PlayAnimation",
                "Slot": "Action",
                "Animation": "Alerted"
              }
```

This shows an animation action that plays the "Alerted" animation in the "Action" slot for an NPC.

## Basic Animation Slot Usage

```json
{
  "Type": "PlayAnimation",
  "Slot": "Status",
  "Animation": "Alerted"
}
```

Plays animation on specific slot.

## Common Animation Slots

### Status

```json
{
  "Slot": "Status"
}
```

Status animations (idle, alerted, sleeping, etc.). Typically loops or plays until interrupted.

### Movement

```json
{
  "Slot": "Movement"
}
```

Movement animations (walk, run, jump, fall). Usually controlled by movement state.

### Combat

```json
{
  "Slot": "Combat"
}
```

Combat animations (attack, block, dodge). Plays on action triggers.

## Multiple Slots Simultaneously

```json
{
  "Actions": [
    {
      "Type": "PlayAnimation",
      "Slot": "Status",
      "Animation": "Alerted"
    },
    {
      "Type": "PlayAnimation",
      "Slot": "Movement",
      "Animation": "Walk"
    }
  ]
}
```

Both animations play at once - NPC walks while in alerted status.

## State-Based Animation Slots

NPCs transition between slots based on state:

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
          "Animation": "CombatStance"
        }
      ]
    }
  ]
}
```

Status slot changes when entering combat.

## Tips for Animation Slots

1. **Layering** - Use multiple slots for layered animations (walk + status + combat)
2. **Status slot** - Common for state-based animations (idle, alerted, sleeping)
3. **Movement slot** - Usually automatic based on movement speed
4. **Combat slot** - Plays attack/block animations during combat

---

**Previous:** [Model Attachments](98_Model_Attachments.md) | **Next:** [Path Definitions](100_Path_Definitions.md)
