# NPC Motion Controllers

Learn how to configure NPC movement physics and locomotion behavior.

## Overview

Motion controllers define how NPCs move through the world, including walking speed, jumping, climbing, falling, and physics properties.

## Location
Motion controllers are configured in `MotionControllerList` array in NPC Role definitions.

## Basic Motion Controller Structure

```json
{
  "MotionControllerList": [
    {
      "Type": "Walk",
      "MaxWalkSpeed": 3,
      "Gravity": 10,
      "MaxFallSpeed": 20,
      "Acceleration": 10
    }
  ]
}
```

## Motion Controller Properties

### Basic Movement

```json
{
  "Type": "Walk",
  "MaxWalkSpeed": 5,
  "Gravity": 10,
  "MaxFallSpeed": 20,
  "Acceleration": 10
}
```

- **`MaxWalkSpeed`** - Maximum walking speed
- **`Gravity`** - Gravity strength
- **`MaxFallSpeed`** - Maximum fall speed
- **`Acceleration`** - Movement acceleration

### Running

```json
{
  "RunThreshold": 0.7,
  "RunThresholdRange": 0.1
}
```

Thresholds for switching to running.

### Climbing

```json
{
  "MaxClimbHeight": 1,
  "DescendFlatness": 0.7,
  "DescendSpeedCompensation": 0.5
}
```

Climbing and descending properties.

### Jumping

```json
{
  "JumpHeight": 1.5,
  "MinJumpHeight": 1,
  "JumpForce": 1.5,
  "JumpDescentSteepness": 1
}
```

Jump physics properties.

### Falling

```json
{
  "DescentAnimationType": "Fall",
  "DescentSteepness": 2.5,
  "DescentBlending": 1
}
```

Falling animation and physics.

## Common Motion Controller Types

### Walk

Standard ground movement controller.

### Fly

Flying movement (for flying NPCs).

## Tips for Motion Controllers

1. **Speed balance** - Match speeds to NPC behavior
2. **Gravity** - Standard gravity is typically 10
3. **Climbing** - `MaxClimbHeight` controls obstacle navigation
4. **Jumping** - Adjust jump properties for navigation

---

**Previous:** [NPC Instructions](82_NPC_Instructions.md) | **Next:** [NPC Components](84_NPC_Components.md)
