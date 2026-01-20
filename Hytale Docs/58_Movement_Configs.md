# Movement Configs

Learn how to configure entity movement parameters like speed, jumping, air control, and physics.

## Overview

Movement configs define how entities move through the world, including walking, running, jumping, swimming, climbing, and air control. Different configs can be used for players, mounts, and NPCs.

## Location
`Server/Entity/MovementConfig/`

## Example from Game Files

### Default Movement Config

From `Server/Entity/MovementConfig/Default.json`:

```1:50:Server/Entity/MovementConfig/Default.json
{
  "VelocityResistance": 0.242,
  "JumpForce": 11.8,
  "SwimJumpForce": 10,
  "JumpBufferDuration": 0.3,
  "JumpBufferMaxYVelocity": 3,
  "Acceleration": 0.1,
  "AirDragMin": 0.96,
  "AirDragMax": 0.995,
  "AirDragMinSpeed": 6,
  "AirDragMaxSpeed": 10,
  "AirFrictionMin": 0.02,
  "AirFrictionMax": 0.045,
  "AirFrictionMinSpeed": 6,
  "AirFrictionMaxSpeed": 10,
  "AirSpeedMultiplier": 1,
  "AirControlMinSpeed": 0,
  "AirControlMaxSpeed": 3,
  "AirControlMinMultiplier": 0,
  "AirControlMaxMultiplier": 3.13,
  "ComboAirSpeedMultiplier": 1.05,
  "BaseSpeed": 5.5,
  "ClimbSpeed": 0.035,
  "ClimbSpeedLateral": 0.035,
  "ClimbUpSprintSpeed": 0.050,
  "ClimbDownSprintSpeed": 0.065,
  "HorizontalFlySpeed": 10.32,
  "VerticalFlySpeed": 10.32,
  "MaxSpeedMultiplier": 15,
  "MinSpeedMultiplier": 0.01,
  "WishDirectionGravityX": 0.5,
  "WishDirectionGravityY": 0.5,
  "WishDirectionWeightX": 0.5,
  "WishDirectionWeightY": 0.5,
  "CollisionExpulsionForce": 0.02,
  "ForwardWalkSpeedMultiplier": 0.3,
  "BackwardWalkSpeedMultiplier": 0.3,
  "StrafeWalkSpeedMultiplier": 0.3,
  "ForwardRunSpeedMultiplier": 1,
  "BackwardRunSpeedMultiplier": 0.65,
  "StrafeRunSpeedMultiplier": 0.8,
  "ForwardCrouchSpeedMultiplier": 0.55,
  "BackwardCrouchSpeedMultiplier": 0.4,
  "StrafeCrouchSpeedMultiplier": 0.45,
  "ForwardSprintSpeedMultiplier": 1.273,
  "VariableJumpFallForce": 35,
  "FallEffectDuration": 0.0,
  "FallJumpForce": 7,
  "FallMomentumLoss": 0.1,
  "AutoJumpObstacleSpeedLoss": 0.95,
```

This shows a complete movement configuration with walking, running, jumping, climbing, air control, and speed multipliers.

## Basic Movement Config Structure

Create `Server/Entity/MovementConfig/MyCustom.json`:

```json
{
  "BaseSpeed": 5.5,
  "JumpForce": 11.8,
  "Acceleration": 0.1,
  "ForwardRunSpeedMultiplier": 1,
  "ForwardSprintSpeedMultiplier": 1.273
}
```

## Core Movement Properties

### Base Speed

```json
{
  "BaseSpeed": 5.5
}
```

Base movement speed in blocks per second.

### Jump Force

```json
{
  "JumpForce": 11.8,
  "SwimJumpForce": 10
}
```

- **`JumpForce`** - Jump strength on land
- **`SwimJumpForce`** - Jump strength in water

### Acceleration

```json
{
  "Acceleration": 0.1
}
```

How quickly entity reaches max speed.

## Speed Multipliers

### Walk/Run/Sprint

```json
{
  "ForwardWalkSpeedMultiplier": 0.3,
  "BackwardWalkSpeedMultiplier": 0.3,
  "StrafeWalkSpeedMultiplier": 0.3,
  "ForwardRunSpeedMultiplier": 1,
  "BackwardRunSpeedMultiplier": 0.65,
  "StrafeRunSpeedMultiplier": 0.8,
  "ForwardSprintSpeedMultiplier": 1.273,
  "BackwardRunSpeedMultiplier": 0.65
}
```

Multipliers for different movement states and directions.

### Crouch

```json
{
  "ForwardCrouchSpeedMultiplier": 0.55,
  "BackwardCrouchSpeedMultiplier": 0.4,
  "StrafeCrouchSpeedMultiplier": 0.45
}
```

Crouching speed multipliers.

## Air Control

### Air Drag and Friction

```json
{
  "AirDragMin": 0.96,
  "AirDragMax": 0.995,
  "AirDragMinSpeed": 6,
  "AirDragMaxSpeed": 10,
  "AirFrictionMin": 0.02,
  "AirFrictionMax": 0.045,
  "AirFrictionMinSpeed": 6,
  "AirFrictionMaxSpeed": 10
}
```

Air resistance and friction based on speed.

### Air Control Multiplier

```json
{
  "AirControlMinSpeed": 0,
  "AirControlMaxSpeed": 3,
  "AirControlMinMultiplier": 0,
  "AirControlMaxMultiplier": 3.13,
  "AirSpeedMultiplier": 1
}
```

Control in air based on speed.

## Climbing

```json
{
  "ClimbSpeed": 0.035,
  "ClimbSpeedLateral": 0.035,
  "ClimbUpSprintSpeed": 0.050,
  "ClimbDownSprintSpeed": 0.065
}
```

Climbing movement speeds.

## Flying

```json
{
  "HorizontalFlySpeed": 10.32,
  "VerticalFlySpeed": 10.32
}
```

Flying speeds (for creative mode or abilities).

## Fall and Roll

```json
{
  "MinFallSpeedToEngageRoll": 21.0,
  "MaxFallSpeedToEngageRoll": 31.0,
  "FallDamagePartialMitigationPercent": 33.0,
  "MaxFallSpeedRollFullMitigation": 25.0,
  "RollStartSpeedModifier": 3.5,
  "RollExitSpeedModifier": 2.2,
  "RollTimeToComplete": 0.2
}
```

Automatic roll on landing to mitigate fall damage.

## Common Presets

### Default

`Server/Entity/MovementConfig/Default.json`:

Standard player movement with full features.

### Mount

`Server/Entity/MovementConfig/Mount.json`:

```json
{
  "BaseSpeed": 8,
  "JumpForce": 14,
  "ForwardSprintSpeedMultiplier": 1.65,
  "FallEffectDuration": 0.6
}
```

Faster movement for mounts.

## Using Movement Configs

Reference in gameplay configs:

```json
{
  "Player": {
    "MovementConfig": "Default"
  }
}
```

## Tips for Creating Movement Configs

1. **Test thoroughly** - Movement feel is crucial for gameplay
2. **Balance speeds** - Too fast/slow affects game feel
3. **Air control** - Affects combat and platforming
4. **Crouch ratios** - Should feel slower but still usable
5. **Mount configs** - Typically faster with higher jump

---

**Previous:** [Entity Repulsion](57_Entity_Repulsion.md) | **Next:** [Hitbox Collision](59_Hitbox_Collision.md)
