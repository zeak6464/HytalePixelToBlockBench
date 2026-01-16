# Response Curves

Learn how to use mathematical curves for animations, effects, and interpolations.

## Overview

Response curves define mathematical functions used for smooth interpolation in animations, particle effects, camera movements, and stat modifications. They control how values change over time or input. Response curves are defined in `Server/ResponseCurves/`.

## Location
`Server/ResponseCurves/`

## Basic Response Curve Structure

Create `Server/ResponseCurves/Exponential/MyCustom_Curve.json`:

```json
{
  "Type": "Exponential",
  "Slope": 1,
  "Exponent": 2
}
```

## Response Curve Types

### Exponential

Used for gradual acceleration or deceleration.

`Server/ResponseCurves/Exponential/Linear.json`:

```json
{
  "Type": "Exponential",
  "Slope": 1,
  "Exponent": 1
}
```

**Properties:**
- **`Slope`** - Steepness of the curve (1 = linear)
- **`Exponent`** - Power of the exponential function (1 = linear, 2 = quadratic, etc.)

### Quadratic

`Server/ResponseCurves/Exponential/Quadratic.json`:

```json
{
  "Type": "Exponential",
  "Slope": 1,
  "Exponent": 2
}
```

Creates a quadratic curve (faster acceleration).

### Inverse Exponential

`Server/ResponseCurves/Exponential/InverseExponential.json`:

```json
{
  "Type": "Exponential",
  "Slope": -1,
  "Exponent": 2
}
```

Inverted exponential (deceleration).

### Logistic

S-shaped curve for smooth transitions.

`Server/ResponseCurves/Logistic/SimpleLogistic.json`:

```json
{
  "Type": "Logistic",
  "Ceiling": 1,
  "RateOfChange": 1,
  "HorizontalShift": 0.5,
  "VerticalShift": 0
}
```

**Properties:**
- **`Ceiling`** - Maximum value (upper limit)
- **`RateOfChange`** - Speed of the transition (higher = faster)
- **`HorizontalShift`** - Horizontal offset (0-1, midpoint of S-curve)
- **`VerticalShift`** - Vertical offset (vertical translation)

### Late Rise Logistic

`Server/ResponseCurves/Logistic/LateRise.json`:

```json
{
  "Type": "Logistic",
  "Ceiling": 1,
  "RateOfChange": 0.5,
  "HorizontalShift": 0.7,
  "VerticalShift": 0
}
```

Slow start, fast finish.

### Late Falloff Logistic

`Server/ResponseCurves/Logistic/LateFalloff.json`:

```json
{
  "Type": "Logistic",
  "Ceiling": 1,
  "RateOfChange": 0.5,
  "HorizontalShift": 0.3,
  "VerticalShift": 0
}
```

Fast start, slow finish.

### Sine Wave

Oscillating curve for cyclic animations.

`Server/ResponseCurves/SineWave/SimpleParabola.json`:

```json
{
  "Type": "SineWave",
  "Amplitude": 1,
  "Frequency": 1,
  "Phase": 0,
  "VerticalShift": 0
}
```

**Properties:**
- **`Amplitude`** - Height of the wave (peak to trough)
- **`Frequency`** - Number of cycles per unit
- **`Phase`** - Horizontal offset (starting point)
- **`VerticalShift`** - Vertical offset (centerline)

## Using Response Curves

### In Animations

Curves control animation speed over time:

```json
{
  "Animation": "NPC/MyCustom/Walk.blockyanim",
  "Speed": {
    "ResponseCurveId": "Quadratic",
    "Duration": 2.0
  }
}
```

### In Particle Effects

Curves control particle properties:

```json
{
  "ParticleLifeSpan": 5.0,
  "ScaleOverLife": {
    "ResponseCurveId": "LateFalloff"
  },
  "OpacityOverLife": {
    "ResponseCurveId": "SimpleLogistic"
  }
}
```

### In Camera Effects

Curves control camera shake intensity:

```json
{
  "CameraShake": {
    "Intensity": {
      "ResponseCurveId": "InverseExponential",
      "Duration": 1.5
    }
  }
}
```

### In Stat Modifications

Curves control how stats change over time:

```json
{
  "StatModifiers": [
    {
      "EntityStatId": "Health",
      "Value": 50,
      "Duration": 10.0,
      "Curve": {
        "ResponseCurveId": "SimpleLogistic"
      }
    }
  ]
}
```

### In Effects

Curves control effect intensity:

```json
{
  "StatusEffect": {
    "Duration": 30.0,
    "IntensityOverTime": {
      "ResponseCurveId": "LateRise"
    }
  }
}
```

## Complete Example: Custom Animation Curve

### 1. Curve Definition

`Server/ResponseCurves/Exponential/MyCustom_SlowStart.json`:

```json
{
  "Type": "Exponential",
  "Slope": 0.5,
  "Exponent": 3
}
```

Creates a slow start, fast finish curve.

### 2. Using in Animation

`Server/Models/Intelligent/MyCustom/MyCustom_NPC.json`:

```json
{
  "AnimationSets": {
    "Walk": {
      "Animations": [
        {
          "Animation": "NPC/MyCustom/Walk.blockyanim",
          "Speed": {
            "ResponseCurveId": "MyCustom_SlowStart",
            "Duration": 2.0
          },
          "Looping": true
        }
      ]
    }
  }
}
```

## Curve Selection Guide

### When to Use Each Type

| Curve Type | Best For | Effect |
|------------|----------|--------|
| **Linear** | Constant speed | Steady, uniform change |
| **Quadratic** | Acceleration | Starts slow, speeds up |
| **Inverse Exponential** | Deceleration | Starts fast, slows down |
| **Simple Logistic** | Smooth transitions | S-curve, balanced |
| **Late Rise** | Delayed start | Slow start, fast finish |
| **Late Falloff** | Early peak | Fast start, slow finish |
| **Sine Wave** | Oscillations | Cyclic, repeating motion |

## Tips for Creating Response Curves

1. **Start simple** - Use existing curves before creating custom ones
2. **Test visually** - Preview curves to see their shape
3. **Match use case** - Choose curves that fit the desired effect
4. **Adjust parameters** - Fine-tune slope, rate, and shifts
5. **Use in animations** - Curves make animations feel more natural
6. **Combine with duration** - Curves work with time-based properties
7. **Document purpose** - Note what each custom curve is used for

---

**Previous:** [NPC Groups](43_NPC_Groups.md) | **Next:** [Scripted Brushes](45_Scripted_Brushes.md)
