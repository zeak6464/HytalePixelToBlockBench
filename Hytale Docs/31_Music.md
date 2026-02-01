# Music & Audio

Learn how to configure background music and audio categories in Hytale.

## Overview

Music in Hytale is configured through audio categories and sound events. Music files are stored as `.ogg` files in `Common/Music/` and configured in `Server/Audio/`.

## Location
- Music files: `Common/Music/` (`.ogg` format)
- Audio categories: `Server/Audio/AudioCategories/`
- Sound events: `Server/Audio/SoundEvents/`

## Example from Game Files

### Music Audio Category

From `Server/Audio/AudioCategories/AudioCat_Music.json`:

```1:3:Server/Audio/AudioCategories/AudioCat_Music.json
{
  "Volume": -14.0
}
```

This shows an audio category configuration for music with volume settings.

## Audio Categories

Audio categories define groups of sounds and their volume settings.

### Basic Audio Category

Create `Server/Audio/AudioCategories/AudioCat_MyCustom.json`:

```json
{
  "Volume": -14.0
}
```

### Music Category

`Server/Audio/AudioCategories/AudioCat_Music.json`:

```json
{
  "Volume": -14.0
}
```

### Volume Settings

- **`-14.0`** - Default music volume
- Negative values reduce volume
- Positive values increase volume
- Range typically `-60.0` to `0.0`

## Sound Events

Sound events reference audio files and define how they're played. All sound events use the `Layers` array with `Files`.

### Basic Sound Event

Create `Server/Audio/SoundEvents/Music/SFX_MyCustom_Music.json`:

```json
{
  "Layers": [
    {
      "Files": [
        "Music/MyCustom/Track_01.ogg"
      ],
      "Volume": 0
    }
  ],
  "AudioCategory": "AudioCat_Music",
  "Parent": "SFX_Attn_Moderate"
}
```

### Ambient Sound Event Structure

```json
{
  "Layers": [
    {
      "Files": [
        "Sounds/Environments/Zone1/Ambient_01.ogg",
        "Sounds/Environments/Zone1/Ambient_02.ogg"
      ],
      "Volume": 0,
      "RandomSettings": {
        "MinVolume": -10,
        "MinPitch": -1,
        "MaxPitch": 1
      }
    }
  ],
  "PreventSoundInterruption": true,
  "Volume": 1.0,
  "AudioCategory": "AudioCat_Music",
  "Parent": "SFX_Attn_Moderate"
}
```

### Sound Event Properties

#### AudioCategory

```json
{
  "AudioCategory": "AudioCat_Music"
}
```

References an audio category for volume control.

#### Layers Array

```json
{
  "Layers": [
    {
      "Files": [
        "Music/MyCustom/Track.ogg"
      ],
      "Volume": 0
    }
  ]
}
```

- **`Files`** - Array of paths to `.ogg` files (relative to `Common/`)
- **`Volume`** - Relative volume in decibels (0 = full, negative = quieter)
- **`RandomSettings`** - Optional pitch/volume variation

#### Multiple Sound Variants

```json
{
  "Layers": [
    {
      "Files": [
        "Music/Zone1/Ambient_01.ogg",
        "Music/Zone1/Ambient_02.ogg",
        "Music/Zone1/Battle_01.ogg"
      ],
      "Volume": 0,
      "RandomSettings": {
        "MinVolume": -2,
        "MinPitch": -1,
        "MaxPitch": 1
      }
    }
  ],
  "Parent": "SFX_Attn_Moderate"
}
```

One file is randomly selected from the Files array.

## Music File Organization

Organize music files by zone/area:

```
Common/Music/
├── Zone1/
│   ├── Ambient_01.ogg
│   ├── Ambient_02.ogg
│   └── Battle_01.ogg
├── Zone2/
│   ├── Ambient_01.ogg
│   └── Battle_01.ogg
└── MyCustom/
    ├── Track_01.ogg
    └── Track_02.ogg
```

## Using Music in Environments

Music can be configured in environment files (see [Biomes & Environments](24_Biomes_and_Environments.md)):

```json
{
  "MusicSoundEventId": "SFX_Zone1_Ambient",
  "CombatMusicSoundEventId": "SFX_Zone1_Battle"
}
```

## Sound Event Ducking

Music can be ducked (volume reduced) during dialogue or other audio:

```json
{
  "Category": "AudioCat_Music",
  "Ducking": {
    "Volume": -2.0
  }
}
```

### Ducking Example

`Server/Audio/SoundEvents/SFX/SFX_Music_Ducking_2db.json`:

```json
{
  "Volume": -2.0
}
```

## Complete Example: Custom Music System

### 1. Audio Category

`Server/Audio/AudioCategories/AudioCat_MyCustom_Music.json`:

```json
{
  "Volume": -14.0
}
```

### 2. Ambient Music

`Server/Audio/SoundEvents/Music/SFX_MyCustom_Ambient.json`:

```json
{
  "Layers": [
    {
      "Files": [
        "Music/MyCustom/Ambient_01.ogg",
        "Music/MyCustom/Ambient_02.ogg"
      ],
      "Volume": 0,
      "RandomSettings": {
        "MinVolume": -2,
        "MinPitch": -1,
        "MaxPitch": 1
      }
    }
  ],
  "AudioCategory": "AudioCat_Music",
  "Parent": "SFX_Attn_Moderate"
}
```

### 3. Battle Music

`Server/Audio/SoundEvents/Music/SFX_MyCustom_Battle.json`:

```json
{
  "Layers": [
    {
      "Files": [
        "Music/MyCustom/Battle_01.ogg"
      ],
      "Volume": 0
    }
  ],
  "AudioCategory": "AudioCat_Music",
  "Parent": "SFX_Attn_Moderate"
}
```

### 4. Environment Configuration

`Server/Environments/Zone1/Env_MyCustom_Biome.json`:

```json
{
  "Parent": "Env_Zone1",
  "MusicSoundEventId": "SFX_MyCustom_Ambient",
  "CombatMusicSoundEventId": "SFX_MyCustom_Battle"
}
```

## Music File Requirements

### Format

- **Format:** `.ogg` (Ogg Vorbis)
- **Location:** `Common/Music/`
- **Naming:** Use descriptive names (e.g., `Ambient_01.ogg`)

### Quality Recommendations

- **Sample Rate:** 44.1 kHz or 48 kHz
- **Bitrate:** 128-192 kbps for ambient, 192-320 kbps for battle
- **Channels:** Stereo (2 channels)

## Tips for Music Configuration

1. **Organize by zone** - Group music files by area/biome
2. **Use appropriate volumes** - Balance music with sound effects
3. **Set looping correctly** - Ambient music should loop, one-shots shouldn't
4. **Create separate battle music** - Use `CombatMusicSoundEventId` for combat
5. **Test volume levels** - Ensure music doesn't overpower gameplay
6. **Use multiple tracks** - Random selection adds variety
7. **Configure ducking** - Reduce music volume during dialogue

---

**Previous:** [Tags System](30_Tags_System.md) | **Next:** [Particle Systems](32_Particle_Systems.md)
