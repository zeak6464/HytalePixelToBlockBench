# Fluid FX

Learn how to configure visual effects for fluid blocks like underwater distortion, fog, and particles.

## Overview

Fluid FX (Fluid Effects) define visual properties for fluid blocks, including:
- Fog settings
- Color filters
- Distortion effects
- Underwater particle effects
- Movement settings

## Location
`Server/Item/Block/FluidFX/`

## Example from Game Files

### Water Fluid FX

From `Server/Item/Block/FluidFX/Water.json`:

```1:28:Server/Item/Block/FluidFX/Water.json
{
  "Fog": "EnvironmentTint",
  "FogDistance": [
    -437,
    190
  ],
  "ColorsSaturation": 1.6,
  "ColorsFilter": [
    1,
    1,
    1
  ],
  "DistortionAmplitude": 5,
  "DistortionFrequency": 6,
  "MovementSettings": {
    "SwimUpSpeed": 2.5,
    "SwimDownSpeed": -2.5,
    "HorizontalSpeedMultiplier": 0.6,
    "SinkSpeed": -1.35,
    "FieldOfViewMultiplier": 1,
    "EntryVelocityMultiplier": 1
  },
  "Particle": {
    "SystemId": "Underwater_Effects"
  },
  "FogDepthFalloff": 1.3,
  "FogDepthStart": 95
}
```

This shows fluid FX configuration for water with fog, distortion, movement settings, and particle effects.

## Basic Fluid FX Structure

Create `Server/Item/Block/FluidFX/Water.json`:

```json
{
  "Fog": "EnvironmentTint",
  "FogDistance": [-437, 190],
  "ColorsSaturation": 1.6,
  "ColorsFilter": [1, 1, 1],
  "DistortionAmplitude": 5,
  "DistortionFrequency": 6,
  "MovementSettings": {
    "SwimUpSpeed": 2.5,
    "SwimDownSpeed": -2.5,
    "HorizontalSpeedMultiplier": 0.6,
    "SinkSpeed": -1.35,
    "FieldOfViewMultiplier": 1,
    "EntryVelocityMultiplier": 1
  },
  "Particle": {
    "SystemId": "Underwater_Effects"
  }
}
```

## Fluid FX Properties

### Fog

```json
{
  "Fog": "EnvironmentTint"
}
```

Fog type/color source.

### Fog Distance

```json
{
  "FogDistance": [-437, 190],
  "FogDepthFalloff": 1.3,
  "FogDepthStart": 95
}
```

Fog visibility distance and depth settings.

### Color Effects

```json
{
  "ColorsSaturation": 1.6,
  "ColorsFilter": [1, 1, 1]
}
```

- **`ColorsSaturation`** - Color saturation multiplier
- **`ColorsFilter`** - RGB color tint [Red, Green, Blue] (0-1)

### Distortion

```json
{
  "DistortionAmplitude": 5,
  "DistortionFrequency": 6
}
```

Underwater visual distortion (warping effect).

### Movement Settings

```json
{
  "MovementSettings": {
    "SwimUpSpeed": 2.5,
    "SwimDownSpeed": -2.5,
    "HorizontalSpeedMultiplier": 0.6,
    "SinkSpeed": -1.35,
    "FieldOfViewMultiplier": 1,
    "EntryVelocityMultiplier": 1
  }
}
```

Player movement modifications when in fluid.

### Particles

```json
{
  "Particle": {
    "SystemId": "Underwater_Effects"
  }
}
```

Particle system to show in fluid.

## Common Fluid FX

- **Water** - Clear underwater effects
- **Lava** - Red/orange glow effects
- **Poison** - Green toxic effects
- **Slime** - Viscous bubble effects

## Using Fluid FX

Reference in fluid block definitions:

```json
{
  "FluidFXId": "Water"
}
```

## Tips for Creating Fluid FX

1. **Color filters** - Tint colors to match fluid appearance
2. **Fog distance** - Control visibility/opacity
3. **Distortion** - Add warping for underwater feel
4. **Movement** - Adjust swim speeds for fluid feel
5. **Particles** - Add bubbles/effects for immersion

---

**Previous:** [Breaking Decals](64_Breaking_Decals.md) | **Next:** [Item Animations (Detailed)](66_Item_Animations.md)
