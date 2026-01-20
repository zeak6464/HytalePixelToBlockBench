# Block Fluids

Learn how to create fluid blocks like water, lava, and custom liquids with flow mechanics.

## Overview

Fluid blocks simulate liquids that flow and spread. They have special properties like:
- Flow mechanics
- Source blocks
- Finite/infinite variants
- Collision interactions
- Block interactions (e.g., water + lava = stone)

## Location
`Server/Item/Block/Fluids/`

## Example from Game Files

### Water Fluid

From `Server/Item/Block/Fluids/Water.json`:

```1:8:Server/Item/Block/Fluids/Water.json
{
  "Parent": "Water_Source",
  "MaxFluidLevel": 8,
  "Ticker": {
    "CanDemote": true,
    "SupportedBy": "Water_Source"
  }
}
```

This shows a fluid block configuration for water with flow mechanics and source properties.

## Basic Fluid Structure

Create `Server/Item/Block/Fluids/Water_Source.json`:

```json
{
  "MaxFluidLevel": 1,
  "Effect": ["Water"],
  "Opacity": "Transparent",
  "Textures": [
    {
      "Weight": 1,
      "All": "BlockTextures/Fluid_Water.png"
    }
  ],
  "BlockParticleSetId": "Water",
  "BlockSoundSetId": "Water",
  "FluidFXId": "Water"
}
```

## Fluid Properties

### MaxFluidLevel

```json
{
  "MaxFluidLevel": 1
}
```

Maximum fluid level (affects depth/height).

### Effect

```json
{
  "Effect": ["Water"]
}
```

Fluid effect type. Used for interactions.

### FluidFXId

```json
{
  "FluidFXId": "Water"
}
```

References `Server/Item/Block/FluidFX/` for visual effects.

## Fluid Ticker

Flow and spread mechanics:

```json
{
  "Ticker": {
    "CanDemote": false,
    "SpreadFluid": "Water",
    "Collisions": {
      "Lava": {
        "BlockToPlace": "Rock_Stone_Cobble",
        "SoundEvent": "SFX_Flame_Break"
      }
    }
  }
}
```

- **`SpreadFluid`** - Fluid ID that spreads from this
- **`Collisions`** - Block transformations on contact (e.g., water + lava)

## Fluid Interactions

```json
{
  "Interactions": {
    "Collision": {
      "Cooldown": {
        "Id": "ClearBurn",
        "Cooldown": 1
      },
      "Interactions": [
        {
          "Type": "ClearEntityEffect",
          "EntityEffectId": "Burn"
        }
      ]
    }
  }
}
```

Collision interactions (e.g., water extinguishes fire).

## Fluid Types

### Source Blocks

```json
{
  "MaxFluidLevel": 1,
  "Ticker": {
    "SpreadFluid": "Water"
  }
}
```

Infinite source blocks that generate flowing fluid.

### Flowing Blocks

Finite fluid that flows from sources.

### Finite Blocks

```json
{
  "BlockId": "Water_Finite"
}
```

Limited fluid that doesn't regenerate.

## Common Fluids

- **Water** - Standard water
- **Lava** - Magma with damage
- **Poison** - Toxic fluid
- **Slime** - Sticky/viscous fluid
- **Tar** - Slow-moving dark fluid

## Tips for Creating Fluids

1. **Flow mechanics** - Configure ticker for spreading
2. **Source blocks** - Separate source from flowing variants
3. **Interactions** - Add collision effects (damage, status effects)
4. **Visual effects** - Use FluidFX for appearance
5. **Sound sets** - Assign appropriate fluid sounds

---

**Previous:** [Block Spawners](62_Block_Spawners.md) | **Next:** [Breaking Decals](64_Breaking_Decals.md)
