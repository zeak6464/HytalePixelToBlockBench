# Interaction Type: ApplyForce

Apply physics force to entities for movement, knockback, or dashes.

## Overview

`ApplyForce` applies physics force to entities, useful for dashes, jumps, knockback, or any forced movement. Supports directional forces, velocity configs, and ground checks.

## Example from Game Files

### Apply Force Interaction

Apply force interactions apply physics forces to entities. These are used for knockback, pushing, pulling, launching entities, and other physics-based interactions.

## Basic Structure

```json
{
  "Type": "ApplyForce",
  "Direction": {
    "X": 1,
    "Y": 0,
    "Z": 0
  },
  "AdjustVertical": false,
  "WaitForGround": false,
  "Force": 13,
  "ChangeVelocityType": "Set",
  "VelocityConfig": {
    "AirResistance": 0.97,
    "AirResistanceMax": 0.96,
    "GroundResistance": 0.94,
    "GroundResistanceMax": 0.82,
    "Threshold": 5.0,
    "Style": "Exp"
  }
}
```

## Properties

### Direction

```json
{
  "Direction": {
    "X": 1,
    "Y": 0,
    "Z": 0
  }
}
```

Direction vector:
- **`X`** - Left/right (negative = left, positive = right)
- **`Y`** - Up/down (negative = down, positive = up)
- **`Z`** - Forward/back (negative = forward, positive = back)

### Force

```json
{
  "Force": 13
}
```

Force magnitude. Higher = stronger push.

### AdjustVertical

```json
{
  "AdjustVertical": false
}
```

If `true`, adjusts vertical component based on ground slope. If `false`, uses exact Y direction.

### WaitForGround

```json
{
  "WaitForGround": false
}
```

If `true`, waits for entity to be on ground before applying force.

### ChangeVelocityType

```json
{
  "ChangeVelocityType": "Set"
}
```

How to apply velocity:
- **`"Set"`** - Set velocity directly
- **`"Add"`** - Add to existing velocity
- **`"Multiply"`** - Multiply existing velocity

### VelocityConfig

```json
{
  "VelocityConfig": {
    "AirResistance": 0.97,
    "AirResistanceMax": 0.96,
    "GroundResistance": 0.94,
    "GroundResistanceMax": 0.82,
    "Threshold": 5.0,
    "Style": "Exp"
  }
}
```

Resistance/decay configuration:
- **`AirResistance`** - Air resistance (0-1, 1 = no resistance)
- **`AirResistanceMax`** - Max air resistance
- **`GroundResistance`** - Ground resistance
- **`GroundResistanceMax`** - Max ground resistance
- **`Threshold`** - Velocity threshold for resistance changes
- **`Style`** - "Exp" (exponential) or "Linear"

## Complete Examples

### Example 1: Dodge Right

```json
{
  "Type": "ApplyForce",
  "Direction": {
    "X": 1,
    "Y": 0,
    "Z": 0
  },
  "AdjustVertical": false,
  "WaitForGround": false,
  "Force": 13,
  "ChangeVelocityType": "Set",
  "VelocityConfig": {
    "AirResistance": 0.97,
    "AirResistanceMax": 0.96,
    "GroundResistance": 0.94,
    "GroundResistanceMax": 0.82,
    "Threshold": 5.0,
    "Style": "Exp"
  }
}
```

Dodge right with 13 force.

### Example 2: Dash Forward

```json
{
  "Type": "ApplyForce",
  "Direction": {
    "X": 0,
    "Y": 0,
    "Z": -5
  },
  "Force": 30,
  "ChangeVelocityType": "Set",
  "VelocityConfig": {
    "AirResistance": 0.9,
    "GroundResistance": 0.9
  }
}
```

Dash forward with 30 force.

### Example 3: Jump

```json
{
  "Type": "ApplyForce",
  "Direction": {
    "X": 0,
    "Y": 1,
    "Z": 0
  },
  "AdjustVertical": true,
  "WaitForGround": true,
  "Force": 15,
  "ChangeVelocityType": "Add"
}
```

Jump upward, waits for ground, adds to existing velocity.

## Tips

1. **Direction vectors** - Normalize directions for consistent force (or use relative values)
2. **Force values** - 10-15 for dashes, 20-30 for strong pushes
3. **AdjustVertical** - Use `true` for ground-based movement, `false` for precise control
4. **WaitForGround** - Use `true` for ground-only abilities
5. **VelocityConfig** - Adjust resistance for different movement feels (dash vs float)

---

**Previous:** [ChangeStatWithModifier](123_Interaction_Type_ChangeStatWithModifier.md) | **Next:** [SpawnNPC](125_Interaction_Type_SpawnNPC.md)
