# NPC Motion Controllers

Learn how to configure NPC movement physics and locomotion behavior.

## Overview

Motion controllers define how NPCs move through the world, including walking speed, jumping, climbing, falling, and physics properties. Each NPC can have multiple motion controllers (e.g., Walk and Fly) and switch between them.

**Motion Controller Types:**
- **Walk** - Ground movement (walking, running, jumping, climbing)
- **Fly** - Flight movement (flying NPCs, birds, dragons)
- **Dive** - Swimming/diving movement (aquatic NPCs)

## Location
Motion controllers are configured in `MotionControllerList` array in NPC Role definitions.

## Official Documentation Reference
See [Hytale NPC Documentation](https://hytalemodding.dev/en/docs/official-documentation/npc-doc) for complete motion controller specifications.

## Example from Game Files

### Walk Motion Controller

From `Server/NPC/Roles/Intelligent/Passive/Quest_Master.json`:

```6:13:Server/NPC/Roles/Intelligent/Passive/Quest_Master.json
  "MotionControllerList": [
    {
      "Type": "Walk",
      "MaxWalkSpeed": 10,
      "Gravity": 10,
      "MaxFallSpeed": 8,
      "Acceleration": 10
    }
  ],
```

This shows a Walk motion controller with speed, gravity, fall speed, and acceleration settings.

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

## Common Attributes (All Motion Controllers)

All motion controllers share these base attributes:

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `Type` | String | Required | Motion controller type (`Walk`, `Fly`, `Dive`) |
| `EpsilonSpeed` | Double | 0.00001 | Minimum speed considered non-zero |
| `EpsilonAngle` | Double | 3.0 | Minimum angle difference in degrees |
| `MaxHeadRotationSpeed` | Double | 360 | Max head rotation speed (degrees/sec) |
| `ForceVelocityDamping` | Double | 0.5 | Damping of external force/velocity |
| `RunThreshold` | Double | 0.7 | Relative threshold for running animation |
| `RunThresholdRange` | Double | 0.15 | Threshold range for walk/run switching |

---

## Walk Motion Controller

Standard ground movement for walking NPCs.

```json
{
  "Type": "Walk",
  "MaxWalkSpeed": 3,
  "MinWalkSpeed": 0.1,
  "MaxFallSpeed": 8,
  "MaxSinkSpeedFluid": 4,
  "Gravity": 10,
  "Acceleration": 3,
  "MaxRotationSpeed": 360,
  "MaxWalkTurnAngle": 90,
  "MaxClimbHeight": 1.3,
  "MaxDropHeight": 3,
  "JumpHeight": 0.5,
  "MinJumpHeight": 0.6,
  "MinJumpDistance": 0.2,
  "JumpForce": 1.5,
  "JumpRange": [0, 0],
  "FenceBlockSet": "Fence"
}
```

### Walk - Movement Attributes

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `MaxWalkSpeed` | Double | 3.0 | Maximum horizontal speed |
| `MinWalkSpeed` | Double | 0.1 | Minimum horizontal speed |
| `MaxFallSpeed` | Double | 8.0 | Maximum fall speed |
| `MaxSinkSpeedFluid` | Double | 4.0 | Maximum sink speed in fluids |
| `Gravity` | Double | 10.0 | Gravity strength |
| `Acceleration` | Double | 3.0 | Movement acceleration |
| `MaxRotationSpeed` | Double | 360.0 | Maximum turn speed (degrees/sec) |
| `MaxWalkTurnAngle` | Double | 90.0 | Max angle NPC can walk without turning |
| `BlendRestTurnAngle` | Double | 60.0 | Angle where speed reduction starts when blending |
| `BlendRestRelativeSpeed` | Double | 0.2 | Relative speed when reducing for turn |

### Walk - Climbing Attributes

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `MaxClimbHeight` | Double | 1.3 | Maximum height NPC can climb |
| `MaxDropHeight` | Double | 3.0 | Maximum safe drop height |
| `ClimbSpeedMult` | Double | 0.0 | Climb speed multiplier |
| `ClimbSpeedPow` | Double | 1.0 | Climb speed power |
| `ClimbSpeedConst` | Double | 5.0 | Climb speed constant |
| `AscentAnimationType` | Flag | Walk | Animation when walking up: `Walk`, `Fly`, `Idle`, `Climb`, `Jump` |
| `DescendFlatness` | Double | 0.7 | Relative forward speed while descending |
| `DescendSpeedCompensation` | Double | 0.9 | Forward speed compensation when descending |
| `MinDescentAnimationHeight` | Double | 1.0 | Min drop to use descent animation |
| `DescentAnimationType` | Flag | Fall | Animation when descending: `Walk`, `Idle`, `Fall` |
| `DescentSteepness` | Double | 1.4 | Relative steepness of descent |
| `DescentBlending` | Double | 1.8 | Descent pattern blending (0=linear, higher=curved) |

### Walk - Jumping Attributes

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `JumpHeight` | Double | 0.5 | How high NPC jumps above climb height |
| `MinJumpHeight` | Double | 0.6 | Minimum height for jump attempt |
| `MinJumpDistance` | Double | 0.2 | Minimum distance for jump execution |
| `JumpForce` | Double | 1.5 | Force multiplier for upward motion |
| `JumpBlending` | Double | 1.0 | Upward jump blending (0=curved, 1=linear) |
| `JumpDescentBlending` | Double | 1.0 | Jump descent blending |
| `JumpDescentSteepness` | Double | 1.0 | Steepness of jump descent |
| `JumpRange` | Double[] | [0, 0] | Jump distance range |
| `FenceBlockSet` | Asset | Fence | Unclimbable blocks |

### Walk - Hover Attributes (for hovering NPCs)

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `MinHover` | Double | 0.0 | Minimum hover height over ground |
| `MaxHover` | Double | 0.0 | Maximum hover height over ground |
| `MinHoverClimb` | Double | 0.0 | Min hover when climbing |
| `MinHoverDrop` | Double | 0.0 | Min hover when dropping |
| `HoverFreq` | Double | 0.0 | Hover oscillation frequency |
| `FloatsDown` | Boolean | true | Float down when hovering (vs gravity) |

### Walk - Combat Attribute

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `MinHitSlowdown` | Double | 0.1 | Min percentage slowdown when hit from behind |

---

## Fly Motion Controller

Flight movement for flying NPCs (birds, dragons, etc.).

```json
{
  "Type": "Fly",
  "MinAirSpeed": 0.1,
  "MaxHorizontalSpeed": 8,
  "MaxClimbSpeed": 6,
  "MaxSinkSpeed": 10,
  "MaxFallSpeed": 40,
  "MaxSinkSpeedFluid": 4,
  "MaxClimbAngle": 45,
  "MaxSinkAngle": 85,
  "Acceleration": 4,
  "Deceleration": 4,
  "Gravity": 40,
  "MaxTurnSpeed": 180,
  "MaxRollAngle": 45,
  "MaxRollSpeed": 180,
  "RollDamping": 0.9,
  "MinHeightOverGround": 1,
  "MaxHeightOverGround": 20,
  "FastFlyThreshold": 0.6,
  "AutoLevel": true,
  "DesiredAltitudeWeight": 0.0
}
```

### Fly - Movement Attributes

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `MinAirSpeed` | Double | 0.1 | Minimum in-air speed |
| `MaxHorizontalSpeed` | Double | 8.0 | Maximum horizontal speed |
| `MaxClimbSpeed` | Double | 6.0 | Maximum climbing speed |
| `MaxSinkSpeed` | Double | 10.0 | Maximum sink/drop speed |
| `MaxFallSpeed` | Double | 40.0 | Maximum fall speed |
| `MaxSinkSpeedFluid` | Double | 4.0 | Maximum sink speed in fluids |
| `MaxClimbAngle` | Double | 45.0 | Maximum climb angle |
| `MaxSinkAngle` | Double | 85.0 | Maximum sink angle |
| `Acceleration` | Double | 4.0 | Maximum acceleration |
| `Deceleration` | Double | 4.0 | Maximum deceleration |
| `Gravity` | Double | 40.0 | Gravity strength |

### Fly - Rotation Attributes

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `MaxTurnSpeed` | Double | 180.0 | Maximum turn speed (degrees/sec) |
| `MaxRollAngle` | Double | 45.0 | Maximum roll angle (degrees) |
| `MaxRollSpeed` | Double | 180.0 | Maximum roll speed (degrees/sec) |
| `RollDamping` | Double | 0.9 | Roll damping [0-1] |
| `AutoLevel` | Boolean | true | Set pitch to 0 when no steering |

### Fly - Altitude Attributes

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `MinHeightOverGround` | Double | 1.0 | Minimum height over ground |
| `MaxHeightOverGround` | Double | 20.0 | Maximum height over ground |
| `DesiredAltitudeWeight` | Double | 0.0 | How much NPC prefers desired altitude [0-1] |

### Fly - Animation Attribute

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `FastFlyThreshold` | Double | 0.6 | Relative threshold for fast flying animation |

---

## Dive Motion Controller

Swimming/diving movement for aquatic NPCs.

```json
{
  "Type": "Dive",
  "MaxSwimSpeed": 3,
  "MaxDiveSpeed": 8,
  "MaxFallSpeed": 10,
  "MaxSinkSpeed": 4,
  "Gravity": 10,
  "Acceleration": 3,
  "MaxRotationSpeed": 360,
  "MaxSwimTurnAngle": 90,
  "FastSwimThreshold": 0.6,
  "SwimDepth": 0.4,
  "SinkRatio": 1.0,
  "MinDiveDepth": 0,
  "MaxDiveDepth": 999999,
  "MinDepthAboveGround": 1,
  "MinDepthBelowSurface": 1,
  "MinWaterDepth": 1,
  "MaxWaterDepth": 0,
  "DesiredDepthWeight": 0.0
}
```

### Dive - Movement Attributes

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `MaxSwimSpeed` | Double | 3.0 | Maximum horizontal swim speed |
| `MaxDiveSpeed` | Double | 8.0 | Maximum vertical dive speed |
| `MaxFallSpeed` | Double | 10.0 | Terminal velocity in air |
| `MaxSinkSpeed` | Double | 4.0 | Terminal velocity sinking in water |
| `Gravity` | Double | 10.0 | Gravity strength |
| `Acceleration` | Double | 3.0 | Movement acceleration |
| `MaxRotationSpeed` | Double | 360.0 | Maximum turn speed (degrees/sec) |
| `MaxSwimTurnAngle` | Double | 90.0 | Max angle to swim without turning |

### Dive - Depth Attributes

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `SwimDepth` | Double | 0.4 | Swim depth [-1 to +1] relative to bounding box |
| `SinkRatio` | Double | 1.0 | Relative sink/climb speed while wandering |
| `MinDiveDepth` | Double | 0.0 | Minimum dive depth below surface |
| `MaxDiveDepth` | Double | ∞ | Maximum dive depth below surface |
| `MinDepthAboveGround` | Double | 1.0 | Minimum distance from ground |
| `MinDepthBelowSurface` | Double | 1.0 | Minimum distance from water surface |
| `MinWaterDepth` | Double | 1.0 | Minimum required water depth |
| `MaxWaterDepth` | Double | 0.0 | Maximum water depth |
| `DesiredDepthWeight` | Double | 0.0 | How much NPC prefers desired depth [0-1] |

### Dive - Animation Attribute

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `FastSwimThreshold` | Double | 0.6 | Relative threshold for fast swimming animation |

---

## Motion Controller List Example

NPCs can have multiple motion controllers:

```json
{
  "MotionControllerList": [
    {
      "Type": "Walk",
      "MaxWalkSpeed": 5,
      "Gravity": 10,
      "MaxClimbHeight": 1.5
    },
    {
      "Type": "Fly",
      "MaxHorizontalSpeed": 12,
      "MinHeightOverGround": 3,
      "MaxHeightOverGround": 30
    }
  ]
}
```

Use `TakeOff` and `Land` body motions to switch between controllers.

---

## Tips for Motion Controllers

1. **Speed balance** - Match speeds to NPC behavior and size
2. **Gravity** - Standard gravity is typically 10 (walk) or 40 (fly)
3. **Climbing** - `MaxClimbHeight` controls obstacle navigation (1-1.5 for humanoids)
4. **Jump tuning** - Adjust `JumpHeight`, `JumpForce` for natural movement
5. **Flight altitude** - Use `MinHeightOverGround`/`MaxHeightOverGround` for flight ceiling
6. **Computable values** - Use `{ "Compute": "ParameterName" }` for templating
7. **Run threshold** - `RunThreshold` at 0.7 means run animation at 70%+ speed
8. **Turn angle** - Lower `MaxWalkTurnAngle` for more realistic turning
9. **Hover** - Use hover attributes for floating/hovering NPCs
10. **Altitude weight** - Increase `DesiredAltitudeWeight` for NPCs that should stay at altitude

---

## Motion Controller Quick Reference

| Controller | Use Case | Key Attributes |
|------------|----------|----------------|
| `Walk` | Ground NPCs | `MaxWalkSpeed`, `MaxClimbHeight`, `JumpHeight` |
| `Fly` | Flying NPCs | `MaxHorizontalSpeed`, `MinHeightOverGround`, `MaxHeightOverGround` |
| `Dive` | Aquatic NPCs | `MaxSwimSpeed`, `MaxDiveDepth`, `MinDepthBelowSurface` |

---

**Previous:** [NPC Instructions](82_NPC_Instructions.md) | **Next:** [NPC Components](84_NPC_Components.md)
