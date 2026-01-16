# Reverb

Learn how to configure audio reverb settings to simulate different acoustic spaces like caves, temples, and open areas.

## Overview

Reverb (reverberation) settings control how sounds echo and reflect in different environments. Different reverb presets simulate various spaces:
- Caves and underground areas
- Temples and large halls
- Forests and open plains
- Swamps and reflective surfaces
- Villages and enclosed spaces

## Location
`Server/Audio/Reverb/`

## Basic Reverb Structure

Create `Server/Audio/Reverb/Rev_MyCustom.json`:

```json
{
  "DryGain": 0,
  "ModalDensity": 1,
  "Diffusion": 1,
  "Gain": -20,
  "HighFrequencyGain": -1,
  "DecayTime": 1.49,
  "HighFrequencyDecayRatio": 0.83,
  "ReflectionGain": -26,
  "ReflectionDelay": 0.007,
  "LateReverbGain": 2,
  "LateReverbDelay": 0.011,
  "RoomRolloffFactor": 0,
  "AirAbsorbptionHighFrequencyGain": -0.05,
  "LimitDecayHighFrequency": true
}
```

## Reverb Properties

### DryGain

```json
{
  "DryGain": 0
}
```

Volume of the original (dry) signal. Usually `0` for standard reverb.

### ModalDensity

```json
{
  "ModalDensity": 1
}
```

Density of the reverb tail. Higher = denser, more realistic reverb.

### Diffusion

```json
{
  "Diffusion": 0.21  // Open spaces (Plains)
  "Diffusion": 1.0   // Enclosed spaces (Cave, Temple)
}
```

How scattered the reflections are:
- **Low (0.21-0.5)** - Open spaces, less echo
- **High (0.87-1.0)** - Enclosed spaces, more echo

### Gain

```json
{
  "Gain": -20  // Standard
  "Gain": -4.5 // Strong reverb (Village)
}
```

Overall reverb volume in dB. Typically `-15` to `-30`.

### HighFrequencyGain

```json
{
  "HighFrequencyGain": -50  // Open spaces (Plains)
  "HighFrequencyGain": -11  // Enclosed spaces (Temple)
}
```

High-frequency attenuation in dB. Lower = more muffled (like distance).

### DecayTime

```json
{
  "DecayTime": 1.49  // Small room
  "DecayTime": 11    // Large cave
}
```

How long reverb lasts in seconds. Higher = larger space.

### HighFrequencyDecayRatio

```json
{
  "HighFrequencyDecayRatio": 0.19  // Fast high-frequency decay (Temple)
  "HighFrequencyDecayRatio": 0.83  // Slower decay (Default)
}
```

How quickly high frequencies fade. Lower = faster high-frequency decay.

### ReflectionGain / ReflectionDelay

```json
{
  "ReflectionGain": -26,
  "ReflectionDelay": 0.007
}
```

Early reflection settings:
- **`ReflectionGain`** - Volume of early reflections (dB)
- **`ReflectionDelay`** - Delay before reflections (seconds)

### LateReverbGain / LateReverbDelay

```json
{
  "LateReverbGain": 2,
  "LateReverbDelay": 0.011
}
```

Late reverb tail:
- **`LateReverbGain`** - Volume of late reverb (dB)
- **`LateReverbDelay`** - Delay before late reverb (seconds)

### RoomRolloffFactor

```json
{
  "RoomRolloffFactor": 0
}
```

Distance-based volume rolloff. Usually `0` for standard reverb.

## Common Reverb Presets

### Default

`Server/Audio/Reverb/Rev_Default.json`:

```json
{
  "DryGain": 0,
  "ModalDensity": 1,
  "Diffusion": 1,
  "Gain": -20,
  "HighFrequencyGain": -1,
  "DecayTime": 1.49,
  "HighFrequencyDecayRatio": 0.83,
  "ReflectionGain": -26,
  "ReflectionDelay": 0.007,
  "LateReverbGain": 2,
  "LateReverbDelay": 0.011,
  "RoomRolloffFactor": 0,
  "AirAbsorbptionHighFrequencyGain": -0.05,
  "LimitDecayHighFrequency": true
}
```

Standard indoor reverb.

### Cave

`Server/Audio/Reverb/Rev_Cave.json`:

```json
{
  "DryGain": 0,
  "ModalDensity": 1,
  "Diffusion": 1,
  "Gain": -15,
  "HighFrequencyGain": -39,
  "DecayTime": 11,
  "HighFrequencyDecayRatio": 1.3,
  "ReflectionGain": -10.4,
  "ReflectionDelay": 0.2,
  "LateReverbGain": 0,
  "LateReverbDelay": 0.02,
  "RoomRolloffFactor": 0,
  "AirAbsorbptionHighFrequencyGain": -0.05,
  "LimitDecayHighFrequency": false
}
```

Large, echoey cave with long decay.

### Temple

`Server/Audio/Reverb/Rev_Temple.json`:

```json
{
  "DryGain": 0,
  "ModalDensity": 1,
  "Diffusion": 0.87,
  "Gain": -25,
  "HighFrequencyGain": -11,
  "DecayTime": 4,
  "HighFrequencyDecayRatio": 0.19,
  "ReflectionGain": -15,
  "ReflectionDelay": 0.09,
  "LateReverbGain": 2,
  "LateReverbDelay": 0.042,
  "RoomRolloffFactor": 0,
  "AirAbsorbptionHighFrequencyGain": -0.05,
  "LimitDecayHighFrequency": true
}
```

Grand temple hall with medium decay.

### Plains (Open Space)

`Server/Audio/Reverb/Rev_Plains.json`:

```json
{
  "DryGain": 0,
  "ModalDensity": 1,
  "Diffusion": 0.21,
  "Gain": -20,
  "HighFrequencyGain": -50,
  "DecayTime": 1.49,
  "HighFrequencyDecayRatio": 0.5,
  "ReflectionGain": -40,
  "ReflectionDelay": 0.17,
  "LateReverbGain": -20,
  "LateReverbDelay": 0.1,
  "RoomRolloffFactor": 0,
  "AirAbsorbptionHighFrequencyGain": -0.06,
  "LimitDecayHighFrequency": false
}
```

Open plains with minimal reverb.

### Swamp

`Server/Audio/Reverb/Rev_Swamp.json`:

```json
{
  "DryGain": 0,
  "ModalDensity": 1,
  "Diffusion": 1,
  "Gain": -26,
  "HighFrequencyGain": -26,
  "DecayTime": 1.5,
  "HighFrequencyDecayRatio": 0.54,
  "ReflectionGain": -10.5,
  "ReflectionDelay": 0.162,
  "LateReverbGain": 0,
  "LateReverbDelay": 0.088,
  "RoomRolloffFactor": 0,
  "AirAbsorbptionHighFrequencyGain": -0.05,
  "LimitDecayHighFrequency": true
}
```

Damp, muffled swamp environment.

## Using Reverb in AmbienceFX

Reverb is typically applied via ReverbZones in AmbienceFX:

```json
{
  "Conditions": {
    "EnvironmentIds": ["Cave"]
  },
  "ReverbZone": "Rev_Cave",
  "AmbientBed": {
    "Track": "Sounds/Environments/Cave_Ambient.ogg"
  }
}
```

## Tips for Creating Reverb

1. **Match space size** - Larger spaces = higher `DecayTime`
2. **Diffusion for openness** - Low diffusion = open spaces, high = enclosed
3. **High-frequency gain** - Lower = more muffled/distant sound
4. **Test in-game** - Reverb effects are subtle, test with real audio
5. **Compare presets** - Use existing presets as reference
6. **Layer with AmbienceFX** - Reverb works best with ambient beds

---

**Previous:** [AmbienceFX](54_AmbienceFX.md) | **Next:** [EQ (Audio Equalization)](56_EQ.md)
