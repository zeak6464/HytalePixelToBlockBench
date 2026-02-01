# Sound Effects & Particle Effects

Learn how to add sound effects and visual effects to your mods.

## Sound Effects & Audio

### Overview

Sound events play audio for various game actions. You reference them in items, interactions, and effects.

### Location
`Server/Audio/SoundEvents/`

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

This shows a sound event with layered audio, random volume/pitch variation, and audio category configuration.

### Basic Sound Event

Create `Server/Audio/SoundEvents/SFX/Custom/SFX_MyCustom_Sound.json`:

```json
{
  "Layers": [
    {
      "Files": [
        "Sounds/Custom/MySound.ogg"
      ],
      "Volume": 0,
      "RandomSettings": {
        "MinVolume": 0,
        "MinPitch": -1,
        "MaxPitch": 1
      }
    }
  ],
  "Volume": 0,
  "Parent": "SFX_Attn_Moderate"
}
```

### Multiple Sound Variants

Use multiple files in the `Files` array - one will be randomly selected:

```json
{
  "Layers": [
    {
      "Files": [
        "Sounds/Weapons/Sword/Impact_01.ogg",
        "Sounds/Weapons/Sword/Impact_02.ogg",
        "Sounds/Weapons/Sword/Impact_03.ogg"
      ],
      "Volume": -2.0,
      "RandomSettings": {
        "MinVolume": -1,
        "MinPitch": -2,
        "MaxPitch": 2
      }
    }
  ],
  "Volume": 0,
  "AudioCategory": "AudioCat_Sword"
}
```

### Sound Event Types

| Type | Location | Use Case |
|------|----------|----------|
| `WorldSoundEventId` | Heard by everyone nearby | Block breaking, explosions |
| `LocalSoundEventId` | Heard only by player | UI interactions, personal effects |
| `PlayerSoundEventId` | Player-specific sounds | Taking damage, eating |

### Using Sounds in Items

```json
{
  "ItemSoundSetId": "ISS_Weapons_Blade_Large",
  "InteractionVars": {
    "Swing_Left_Damage": {
      "Interactions": [
        {
          "DamageEffects": {
            "WorldSoundEventId": "SFX_Sword_Impact",
            "LocalSoundEventId": "SFX_Sword_Impact"
          }
        }
      ]
    }
  }
}
```

### Audio Categories

Audio categories in `Server/Audio/AudioCategories/` control volume groups and can inherit from parent categories.

```json
{
  "Volume": 0,
  "Parent": "AudioCat_NPC"
}
```

Categories support inheritance via `Parent` - child categories inherit the parent's volume settings.

**Organization:**
- Root categories: `AudioCat_NPC`, `AudioCat_Weapons`, `AudioCat_Footsteps`, `AudioCat_Music`, etc.
- NPC-specific: `Server/Audio/AudioCategories/NPC/AudioCat_NPC_Wolf.json`
- Weapon-specific: `Server/Audio/AudioCategories/Weapons/AudioCat_Sword.json`

### Attention Presets (Parent)

Sound events inherit attenuation from parent presets:

| Parent | Usage |
|--------|-------|
| `SFX_Attn_ExtremelyQuiet` | Very subtle sounds |
| `SFX_Attn_VeryQuiet` | Quiet sounds |
| `SFX_Attn_Quiet` | Normal quiet sounds |
| `SFX_Attn_Moderate` | Standard sounds |
| `SFX_Attn_Loud` | Loud sounds |
| `SFX_Attn_VeryLoud` | Very loud sounds |

### Sound Sets (Creative Play)

`Server/Audio/SoundSets/CreativePlayDefaults.json` defines UI sounds for creative mode:

```json
{
  "SoundEvents": {
    "Error": "SFX_Creative_Play_Error",
    "Rotate_Yaw": "SFX_Rotate_Yaw_Default",
    "Eyedropper_Select": "SFX_Creative_Play_Eyedropper_Select",
    "Brush_Paint": "SFX_Creative_Play_Brush_Paint_Base",
    "Paste": "SFX_Creative_Play_Paste"
  },
  "Category": "UI"
}
```

Maps action types to sound event IDs for creative mode interactions.

---

## AmbienceFX System

Environmental audio including music, ambient sounds, and reverb zones.

### Location
`Server/Audio/AmbienceFX/`

### Music System

Zone-based music with conditions:

`Server/Audio/AmbienceFX/Music/Zone1/AMB_Z1_Day_Music.json`:

```json
{
  "Conditions": {
    "DayTime": { "Min": 5, "Max": 19 },
    "EnvironmentTagPattern": {
      "Op": "Equals",
      "Tag": "Zone=Zone1"
    }
  },
  "Music": {
    "Tracks": [
      "Music/Zone1/Exploration_Day_01.ogg",
      "Music/Zone1/Exploration_Day_02.ogg"
    ]
  },
  "AudioCategory": "AudioCat_Music",
  "Priority": 10
}
```

### Music Conditions

| Condition | Description |
|-----------|-------------|
| `DayTime` | Hour range (5-19 = day, 19-5 = night) |
| `EnvironmentTagPattern` | Tag matching with `Op` and `Tag`/`Patterns` |
| `Altitude` | `Min`/`Max` altitude range |
| `SunLightLevel` | Light level 0-15 |
| `WeatherTagPattern` | Weather conditions |

### Tag Pattern Operations

```json
{
  "EnvironmentTagPattern": {
    "Op": "And",
    "Patterns": [
      { "Op": "Equals", "Tag": "Zone=Zone1" },
      { "Op": "Not", "Tag": "Biome=Cave" }
    ]
  }
}
```

- `Equals` - Match single tag
- `And` - All patterns must match
- `Or` - Any pattern must match
- `Not` - Invert match

### Ambient Bed

`Server/Audio/AmbienceFX/Ambience/Zone1/AMB_Z1_Forest_Day.json`:

```json
{
  "Conditions": {
    "DayTime": { "Min": 5, "Max": 19 },
    "EnvironmentTagPattern": {
      "Op": "Equals",
      "Tag": "Biome=Forest"
    }
  },
  "AmbientBed": {
    "Track": "Sounds/Environments/Zone1/Forest_Day_Ambient.ogg",
    "Volume": -11.0
  }
}
```

### Reverb Zones

`Server/Audio/AmbienceFX/ReverbZones/Cave/AMB_Cave_Reverb.json`:

```json
{
  "Conditions": {
    "EnvironmentIds": ["Cave", "Mineshaft"],
    "Roof": true,
    "SunLightLevel": { "Min": 0, "Max": 4 }
  },
  "SoundEffect": {
    "ReverbEffectId": "Rev_Cave",
    "IsInstant": false
  }
}
```

---

## Reverb Presets

`Server/Audio/Reverb/` - 21 reverb presets for different environments.

### Reverb Properties

```json
{
  "DryGain": 0,
  "ModalDensity": 1,
  "Diffusion": 0.87,
  "Gain": -15,
  "HighFrequencyGain": -39,
  "DecayTime": 11,
  "HighFrequencyDecayRatio": 0.19,
  "ReflectionGain": -26,
  "ReflectionDelay": 0.2,
  "LateReverbGain": -7,
  "LateReverbDelay": 0.1,
  "RoomRolloffFactor": 0,
  "AirAbsorbptionHighFrequencyGain": -0.05,
  "LimitDecayHighFrequency": true
}
```

### Common Reverb Presets

| Preset | DecayTime | Description |
|--------|-----------|-------------|
| `Rev_Cave` | 11s | Long decay, delayed reflections |
| `Rev_Forest` | 5s | Moderate decay, natural HF rolloff |
| `Rev_Temple` | 4s | Lower diffusion, controlled |
| `Rev_Mineshaft` | 3s | Tight space acoustics |
| `Rev_Swamp` | 2s | Dampened, murky |

---

## EQ System

`Server/Audio/EQ/` - Equalization for special conditions.

### EQ Properties

```json
{
  "LowGain": 0,
  "LowCutOff": 200,
  "LowMidGain": -17.19,
  "LowMidCenter": 500,
  "LowMidWidth": 1,
  "HighMidGain": -17.9,
  "HighMidCenter": 1500,
  "HighMidWidth": 1,
  "HighGain": -17.9,
  "HighCutOff": 4000
}
```

### Available EQ Presets

- `EQ_Default` - Normal audio
- `EQ_Underwater` - Muffled underwater effect (reduces mid/high frequencies)

---

## Item Sound Sets

`Server/Audio/ItemSounds/` - 36 presets for inventory interactions.

### Structure

```json
{
  "SoundEvents": {
    "Drag": "SFX_Drag_Item_Metal",
    "Drop": "SFX_Drop_Item_Metal"
  }
}
```

### Item Sound Categories

| Category | Examples |
|----------|----------|
| **Armor** | `ISS_Armor_Cloth`, `ISS_Armor_Heavy`, `ISS_Armor_Leather` |
| **Blocks** | `ISS_Blocks_Gravel`, `ISS_Blocks_Stone`, `ISS_Blocks_Wood` |
| **Items** | `ISS_Items_Bones`, `ISS_Items_Gems`, `ISS_Items_Metal`, `ISS_Items_Potion` |
| **Weapons** | `ISS_Weapons_Blade_Large`, `ISS_Weapons_Blunt_Small`, `ISS_Weapons_Shield_Metal` |

### Using in Items

```json
{
  "ItemSoundSetId": "ISS_Weapons_Blade_Large"
}
```

---

## Particle Effects & VFX

### Overview

Particle effects add visual flair to items, abilities, and status effects. They're referenced by `SystemId`.

### Using Particles

**In Items:**
```json
{
  "Particles": [
    {
      "SystemId": "Weapon_Frost_Mist",
      "TargetEntityPart": "PrimaryItem",
      "TargetNodeName": "Crystal1",
      "PositionOffset": {
        "Y": 0
      },
      "Scale": 1
    }
  ]
}
```

**In Status Effects:**
```json
{
  "ApplicationEffects": {
    "Particles": [
      {
        "SystemId": "Effect_Fire"
      },
      {
        "SystemId": "Effect_Poison"
      }
    ]
  }
}
```

**In Interactions:**
```json
{
  "Effects": {
    "Particles": [
      {
        "SystemId": "Explosion_Medium",
        "TargetNodeName": "Handle"
      }
    ]
  }
}
```

### Particle Properties

| Property | Description |
|----------|-------------|
| `SystemId` | Particle system ID (defined in game) |
| `TargetEntityPart` | Where to attach (`PrimaryItem`, `Entity`) |
| `TargetNodeName` | Specific node/bone to attach to |
| `PositionOffset` | Offset from target position |
| `RotationOffset` | Rotation offset |
| `Scale` | Size multiplier |

### Common Particle Systems

- `Effect_Fire` - Fire effects
- `Effect_Poison` - Poison cloud
- `Explosion_Medium` - Explosion
- `MagicPortal` - Portal effects
- `Weapon_Frost_Mist` - Frost weapon effect
- `Impact_Feathers_Black` - Feather particles

### Model VFX (Visual Overrides)

```json
{
  "ApplicationEffects": {
    "ModelOverride": {
      "Model": "VFX/Spells/Roots/Model.blockymodel",
      "Texture": "VFX/Spells/Roots/Model.png",
      "AnimationSets": {
        "Spawn": {
          "Animations": [
            {
              "Animation": "VFX/Spells/Roots/Spawn.blockyanim",
              "Looping": false
            }
          ]
        }
      }
    }
  }
}
```

---

**Previous:** [Creating UI](09_UI.md) | **Next:** [Creating Accessories](11_Creating_Accessories.md)
