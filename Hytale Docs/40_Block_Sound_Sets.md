# Block Sound Sets

Learn how to configure block sounds for footsteps, breaking, building, and other block interactions.

## Overview

Block sound sets define sounds that play when interacting with blocks (walking, breaking, building, etc.). They're defined in `Server/Item/Block/Sounds/` and referenced by blocks via `BlockSoundSetId`.

## Location
- Block sound sets: `Server/Item/Block/Sounds/`
- Sound events: `Server/Audio/SoundEvents/BlockSounds/`

## Example from Game Files

### Wood Block Sound Set

From `Server/Item/Block/Sounds/Wood.json`:

```1:8:Server/Item/Block/Sounds/Wood.json
{
  "SoundEvents": {
    "Walk": "SFX_Wood_Walk",
    "Land": "SFX_Wood_Land",
    "Hit": "SFX_Wood_Hit",
    "Break": "SFX_Wood_Break",
    "Build": "SFX_Default_Build"
  }
}
```

This shows a block sound set with sounds for walking, landing, hitting, breaking, and building on wood blocks.

## Basic Block Sound Set Structure

Create `Server/Item/Block/Sounds/MyCustom_Material.json`:

```json
{
  "SoundEvents": {
    "Walk": "SFX_MyCustom_Walk",
    "Land": "SFX_MyCustom_Land",
    "Hit": "SFX_MyCustom_Hit",
    "Break": "SFX_MyCustom_Break",
    "Build": "SFX_MyCustom_Build",
    "Harvest": "SFX_MyCustom_Harvest"
  }
}
```

## Block Sound Set Properties

### Walk

```json
{
  "Walk": "SFX_Stone_Walk"
}
```

Sound played when walking on the block.

### Land

```json
{
  "Land": "SFX_Stone_Land"
}
```

Sound played when landing/jumping onto the block.

### Hit

```json
{
  "Hit": "SFX_Stone_Hit"
}
```

Sound played when hitting the block (punching, tool impact).

### Break

```json
{
  "Break": "SFX_Stone_Break"
}
```

Sound played when the block is broken.

### Build

```json
{
  "Build": "SFX_Default_Build"
}
```

Sound played when placing/building the block.

### Harvest

```json
{
  "Harvest": "SFX_Stone_Harvest"
}
```

Sound played when harvesting (crops, plants).

## Common Block Sound Sets

### Stone

`Server/Item/Block/Sounds/Stone.json`:

```json
{
  "SoundEvents": {
    "Walk": "SFX_Stone_Walk",
    "Land": "SFX_Stone_Land",
    "Hit": "SFX_Stone_Hit",
    "Break": "SFX_Stone_Break",
    "Build": "SFX_Default_Build",
    "Harvest": "SFX_Stone_Harvest"
  }
}
```

### Wood

`Server/Item/Block/Sounds/Wood.json`:

```json
{
  "SoundEvents": {
    "Walk": "SFX_Wood_Walk",
    "Land": "SFX_Wood_Land",
    "Hit": "SFX_Wood_Hit",
    "Break": "SFX_Wood_Break",
    "Build": "SFX_Default_Build",
    "Harvest": "SFX_Wood_Harvest"
  }
}
```

### Default

`Server/Item/Block/Sounds/Default.json`:

```json
{
  "SoundEvents": {
    "Walk": "SFX_Default_Walk",
    "Land": "SFX_Dirt_Land",
    "Clone": "SFX_Default_Clone",
    "Break": "SFX_Default_Break",
    "Build": "SFX_Default_Build"
  }
}
```

## Using Block Sound Sets in Blocks

### Referencing Sound Set

In block definitions:

```json
{
  "BlockType": {
    "BlockSoundSetId": "Stone",
    "Material": "Solid",
    "DrawType": "Cube"
  }
}
```

## Sound Event Structure

Block sound events are in `Server/Audio/SoundEvents/BlockSounds/`:

### Basic Sound Event

`Server/Audio/SoundEvents/BlockSounds/Default/SFX_Default_Break.json`:

```json
{
  "Layers": [
    {
      "Files": [
        "Sounds/Blocks/Default/Break_01.ogg",
        "Sounds/Blocks/Default/Break_02.ogg"
      ],
      "Volume": 0
    }
  ],
  "Parent": "SFX_Attn_Moderate"
}
```

### Multiple Variants

```json
{
  "Layers": [
    {
      "Files": [
        "Sounds/Blocks/Stone/Break_01.ogg",
        "Sounds/Blocks/Stone/Break_02.ogg",
        "Sounds/Blocks/Stone/Break_03.ogg"
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

Randomly selects one file from the Files array for variety.

## Complete Example: Custom Block Sound Set

### 1. Sound Set Definition

`Server/Item/Block/Sounds/MyCustom_Magical.json`:

```json
{
  "SoundEvents": {
    "Walk": "SFX_Magic_Walk",
    "Land": "SFX_Magic_Land",
    "Hit": "SFX_Magic_Hit",
    "Break": "SFX_Magic_Break",
    "Build": "SFX_Magic_Build"
  }
}
```

### 2. Sound Events

`Server/Audio/SoundEvents/BlockSounds/Magic/SFX_Magic_Break.json`:

```json
{
  "Layers": [
    {
      "Files": [
        "Sounds/Blocks/Magic/Break_01.ogg",
        "Sounds/Blocks/Magic/Break_02.ogg"
      ],
      "Volume": 0
    }
  ],
  "Parent": "SFX_Attn_Moderate"
}
```

### 3. Using in Block

```json
{
  "BlockType": {
    "BlockSoundSetId": "MyCustom_Magical",
    "Material": "Solid",
    "DrawType": "Cube"
  }
}
```

## Common Block Sound Set IDs

- **`"Stone"`** - Stone/rock blocks
- **`"Wood"`** - Wooden blocks
- **`"Dirt"`** - Dirt/soil blocks
- **`"Soft"`** - Soft blocks (wool, etc.)
- **`"Default"`** - Default fallback sounds

## Tips for Creating Block Sound Sets

1. **Use appropriate sounds** - Match sounds to material type
2. **Create multiple variants** - Random selection adds variety
3. **Reference existing sets** - Reuse common sound sets when possible
4. **Test in-game** - Block sounds behave differently in engine
5. **Set all events** - Include Walk, Land, Hit, Break, Build for completeness
6. **Match material** - Sound sets should match block material type
7. **Use consistent naming** - Follow existing naming conventions

---

**Previous:** [Block Sets](39_Block_Sets.md) | **Next:** Back to [README](../README.md)
