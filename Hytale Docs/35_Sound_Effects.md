# Sound Effects

Learn how to create and configure sound events, item sound sets, and audio systems.

## Overview

Sound effects in Hytale are defined using sound events and item sound sets. Sound events play audio files, and item sound sets group sounds for specific item types.

## Location
- Sound events: `Server/Audio/SoundEvents/`
- Item sound sets: `Server/Audio/ItemSounds/`
- Audio categories: `Server/Audio/AudioCategories/`
- Sound files: `Common/Sounds/` (`.ogg` format)

## Example from Game Files

### Sword Impact Sound Event

From `Server/Audio/SoundEvents/SFX/Weapons/Sword/Impacts/SFX_Sword_T2_Impact.json`:

```1:34:Server/Audio/SoundEvents/SFX/Weapons/Sword/Impacts/SFX_Sword_T2_Impact.json
{
  "Layers": [
    {
      "Files": [
        "Sounds/Weapons/Shared/Impacts/Light_Melee_Impact_Base_03.ogg",
        "Sounds/Weapons/Shared/Impacts/Light_Melee_Impact_Base_04.ogg"
      ],
      "Volume": -2.0,
      "RandomSettings": {
        "MinVolume": -1,
        "MinPitch": -2,
        "MaxPitch": 2
      },
      "StartDelay": 0
    },
    {
      "Files": [
        "Sounds/Weapons/Sword/T2/Impacts/Sword_Common_Impact_02.ogg",
        "Sounds/Weapons/Sword/T2/Impacts/Sword_Common_Impact_03.ogg"
      ],
      "Volume": -8.0,
      "RandomSettings": {
        "MinVolume": -1,
        "MinPitch": -2,
        "MaxPitch": 2
      },
      "StartDelay": 0
    }
  ],
  "Volume": 0,
  "PreventSoundInterruption": true,
  "AudioCategory": "AudioCat_Sword",
  "Parent": "SFX_Attn_Moderate"
}
```

This shows a sound event with multiple layers, random settings, and audio category configuration.

## Basic Sound Event

Create `Server/Audio/SoundEvents/SFX/MyCustom/SFX_MyCustom_Sound.json`:

```json
{
  "Layers": [
    {
      "Files": [
        "Sounds/Custom/MySound.ogg"
      ],
      "Volume": 0
    }
  ],
  "Parent": "SFX_Attn_Moderate"
}
```

## Sound Event Structure

### Simple Sound Event

```json
{
  "Layers": [
    {
      "Files": [
        "Sounds/Weapons/Sword/Impact_01.ogg"
      ],
      "Volume": 0
    }
  ],
  "Parent": "SFX_Attn_Moderate"
}
```

### Multiple Sound Variants

```json
{
  "Layers": [
    {
      "Files": [
        "Sounds/Weapons/Sword/Impact_01.ogg",
        "Sounds/Weapons/Sword/Impact_02.ogg",
        "Sounds/Weapons/Sword/Impact_03.ogg"
      ],
      "Volume": 0,
      "RandomSettings": {
        "MinVolume": -1,
        "MinPitch": -2,
        "MaxPitch": 2
      }
    }
  ],
  "Parent": "SFX_Attn_Moderate"
}
```

One random file is played from the Files array.

> **Note:** The old `"Sounds"` array with `"SoundPath"` format is deprecated. Use `"Layers"` with `"Files"` instead.

### Legacy Format (Deprecated)

```json
{
  "Sounds": [
    {
      "SoundPath": "Sounds/Weapons/Sword/Impact_02.ogg",
      "Volume": 1.0
    },
    {
      "SoundPath": "Sounds/Weapons/Sword/Impact_03.ogg",
      "Volume": 1.0
    }
  ]
}
```

Randomly selects one of the sounds each time.

## Advanced Sound Event (Layered)

```json
{
  "Layers": [
    {
      "Files": [
        "Sounds/Weapons/Sword/T1/Sword_Swing_T1_Down_Stereo_01.ogg",
        "Sounds/Weapons/Sword/T1/Sword_Swing_T1_Down_Stereo_02.ogg",
        "Sounds/Weapons/Sword/T1/Sword_Swing_T1_Down_Stereo_03.ogg"
      ],
      "RandomSettings": {
        "MinPitch": -2,
        "MaxPitch": 2,
        "MinVolume": -3,
        "MaxVolume": 0
      },
      "StartDelay": 0.25,
      "Volume": 1.0
    },
    {
      "Files": [
        "Sounds/PlayerActions/Movement/Player_Clth_Foley_Swing_01.ogg"
      ],
      "RandomSettings": {
        "MinVolume": -5,
        "MinPitch": -2,
        "MaxPitch": 5
      },
      "Volume": -14.0,
      "StartDelay": 0.35
    }
  ],
  "Volume": 0,
  "PreventSoundInterruption": true,
  "AudioCategory": "AudioCat_Sword"
}
```

### Layer Properties

- **`Files`** - Array of sound files (randomly selected)
- **`RandomSettings`** - Pitch and volume variation
- **`StartDelay`** - Delay before playing (seconds)
- **`Volume`** - Volume level (0.0 to 1.0, or dB like `-14.0`)
- **`PreventSoundInterruption`** - Whether sound can be interrupted
- **`AudioCategory`** - Audio category for volume control

## Sound Event Properties

### Sound Path

```json
{
  "SoundPath": "Sounds/Weapons/Sword/Impact_01.ogg"
}
```

Path to sound file (relative to `Common/`).

### Volume

```json
{
  "Volume": 1.0  // 0.0 to 1.0
}
```

or

```json
{
  "Volume": -14.0  // dB scale
}
```

Volume level:
- **`0.0 to 1.0`** - Linear scale (1.0 = 100%)
- **Negative values** - dB scale (e.g., `-14.0` dB)

### Pitch

```json
{
  "Pitch": 1.0
}
```

Pitch multiplier:
- **`1.0`** - Normal pitch
- **`< 1.0`** - Lower pitch
- **`> 1.0`** - Higher pitch

### Random Settings

```json
{
  "RandomSettings": {
    "MinPitch": -2,
    "MaxPitch": 2,
    "MinVolume": -3,
    "MaxVolume": 0
  }
}
```

Random variation:
- **`MinPitch/MaxPitch`** - Pitch variation range
- **`MinVolume/MaxVolume`** - Volume variation range

## Sound Event Types

### World Sound Event

```json
{
  "Effects": {
    "WorldSoundEventId": "SFX_Sword_Impact"
  }
}
```

Heard by all players nearby (explosions, block breaking).

### Local Sound Event

```json
{
  "Effects": {
    "LocalSoundEventId": "SFX_Sword_Swing_Local"
  }
}
```

Heard only by the player performing the action.

### Player Sound Event

```json
{
  "Effects": {
    "PlayerSoundEventId": "SFX_Player_Damage"
  }
}
```

Player-specific sounds (damage, eating).

## Item Sound Sets

Item sound sets group sounds for specific item types.

### Basic Item Sound Set

Create `Server/Audio/ItemSounds/ISS_MyCustom_Items.json`:

```json
{
  "SoundEvents": {
    "Drop": "SFX_Drop_Items_MyCustom",
    "Drag": "SFX_Drag_Items_MyCustom"
  }
}
```

### Using Item Sound Sets

Reference in items:

```json
{
  "ItemSoundSetId": "ISS_Items_Ingots"
}
```

### Common Item Sound Set Types

- **`ISS_Items_Ingots`** - Metal ingots/bars
- **`ISS_Items_Bones`** - Bone items
- **`ISS_Items_Foliage`** - Plant/foliage items
- **`ISS_Weapons_Blade_Large`** - Large bladed weapons
- **`ISS_Blocks_Wood`** - Wooden blocks

## Audio Categories

Audio categories control volume groups.

### Audio Category Structure

`Server/Audio/AudioCategories/AudioCat_MyCustom.json`:

```json
{
  "Volume": -14.0
}
```

### Common Audio Categories

- **`AudioCat_Music`** - Background music
- **`AudioCat_Sword`** - Sword sounds
- **`AudioCat_Weapons`** - Weapon sounds
- **`AudioCat_Blocks`** - Block sounds

## Using Sounds in Items

### Item Sound Set

```json
{
  "ItemSoundSetId": "ISS_Items_Ingots"
}
```

### Interaction Sounds

```json
{
  "InteractionVars": {
    "Swing_Left_Damage": {
      "Interactions": [
        {
          "DamageEffects": {
            "WorldSoundEventId": "SFX_Sword_Impact",
            "LocalSoundEventId": "SFX_Sword_Swing_Local"
          }
        }
      ]
    }
  }
}
```

## Sound File Organization

Organize sound files by category:

```
Common/Sounds/
├── Weapons/
│   ├── Sword/
│   │   ├── Impact_01.ogg
│   │   └── Swing_01.ogg
│   └── Bow/
│       └── Shoot_01.ogg
├── Blocks/
│   ├── Break_Stone.ogg
│   └── Place_Wood.ogg
└── Player/
    ├── Damage.ogg
    └── Footstep.ogg
```

## Complete Example: Custom Sound System

### 1. Sound Event

`Server/Audio/SoundEvents/SFX/Weapons/MyCustom/SFX_MyCustom_Weapon_Swing.json`:

```json
{
  "Layers": [
    {
      "Files": [
        "Sounds/Weapons/MyCustom/Swing_01.ogg",
        "Sounds/Weapons/MyCustom/Swing_02.ogg"
      ],
      "RandomSettings": {
        "MinPitch": -1,
        "MaxPitch": 1,
        "MinVolume": -2,
        "MaxVolume": 0
      },
      "Volume": 1.0
    }
  ],
  "Volume": 0,
  "PreventSoundInterruption": false,
  "AudioCategory": "AudioCat_Weapons"
}
```

### 2. Item Sound Set

`Server/Audio/ItemSounds/ISS_Weapons_MyCustom.json`:

```json
{
  "SoundEvents": {
    "Drop": "SFX_Drop_Items_Weapons",
    "Drag": "SFX_Drag_Items_Weapons"
  }
}
```

### 3. Using in Item

```json
{
  "ItemSoundSetId": "ISS_Weapons_MyCustom",
  "InteractionVars": {
    "Swing_Left_Damage": {
      "Interactions": [
        {
          "Effects": {
            "LocalSoundEventId": "SFX_MyCustom_Weapon_Swing"
          }
        }
      ]
    }
  }
}
```

## Sound File Requirements

### Format

- **Format:** `.ogg` (Ogg Vorbis)
- **Location:** `Common/Sounds/`
- **Naming:** Use descriptive names (e.g., `Swing_01.ogg`)

### Quality Recommendations

- **Sample Rate:** 44.1 kHz or 48 kHz
- **Bitrate:** 128-192 kbps
- **Channels:** Mono or stereo

## Tips for Creating Sound Effects

1. **Use multiple variants** - Random selection adds variety
2. **Add random pitch/volume** - Prevents repetition
3. **Layer sounds** - Combine multiple sound layers for richness
4. **Set appropriate volumes** - Balance with other sounds
5. **Use correct event types** - World vs Local vs Player
6. **Organize by category** - Group related sounds
7. **Test in-game** - Sounds behave differently in engine

---

**Previous:** [Camera Effects](34_Camera_Effects.md) | **Next:** [Resource Types](36_Resource_Types.md)
