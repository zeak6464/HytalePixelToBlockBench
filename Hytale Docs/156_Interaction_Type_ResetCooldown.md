# Interaction Type: ResetCooldown

Reset interaction cooldowns for specific abilities or interactions.

## Overview

`ResetCooldown` resets cooldowns for specific interactions. Useful for abilities that can reset other cooldowns, parry systems, or cooldown manipulation mechanics.

## Basic Structure

```json
{
  "Type": "ResetCooldown",
  "Cooldown": {
    "Id": "Debug_Stick_Parry",
    "Cooldown": 0.5
  }
}
```

## Properties

### Cooldown

```json
{
  "Cooldown": {
    "Id": "Debug_Stick_Parry",
    "Cooldown": 0.5
  }
}
```

Cooldown configuration:
- **`Id`** - Cooldown ID to reset
- **`Cooldown`** - Cooldown duration (may be optional, check behavior)

## Complete Examples

### Example 1: Reset Parry Cooldown

```json
{
  "Type": "ResetCooldown",
  "Cooldown": {
    "Id": "Debug_Stick_Parry",
    "Cooldown": 0.5
  }
}
```

Resets parry cooldown to 0.5 seconds.

### Example 2: In Parry System

```json
{
  "Type": "Wielding",
  "BlockedInteractions": {
    "Interactions": [
      {
        "Type": "ResetCooldown",
        "Cooldown": {
          "Id": "Debug_Stick_Parry",
          "Cooldown": 0.5
        }
      }
    ]
  }
}
```

Resets cooldown when attack is blocked (parried).

## Tips

1. **Cooldown IDs** - Must match cooldown IDs defined in interaction cooldowns
2. **Parry systems** - Use in Wielding.BlockedInteractions for parry rewards
3. **Cooldown duration** - May set new cooldown or reset to 0 (check behavior)
4. **Cooldown manipulation** - Use for abilities that affect cooldown timers

---

**Previous:** [UseCoop](155_Interaction_Type_UseCoop.md) | **Next:** Back to [Interaction Types List](109_Interaction_Types_List.md)
