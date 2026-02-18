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
- **Fire** - Spreading fire (Update 3)

---

## Fire Spread System (Update 3)

Fire uses a special ticker type that spreads to flammable blocks.

From `Server/Item/Block/Fluids/Fire.json`:

```json
{
  "MaxFluidLevel": 8,
  "Effect": ["Lava"],
  "DrawType": "None",
  "Light": { "Color": "#e90" },
  "Particles": [
    { "SystemId": "Fluid_Fire", "Scale": 1, "PositionOffset": { "Y": 0.5 } }
  ],
  "Ticker": {
    "Type": "Fire",
    "CanDemote": false,
    "SpreadFluid": "Fire",
    "FlowRate": 2.0,
    "SupportedBy": "Fire",
    "Flammability": [
      {
        "TagPattern": { "Op": "Equals", "Tag": "Plant" },
        "Priority": 0,
        "BurnLevel": 3,
        "BurnChance": 0.9
      },
      {
        "TagPattern": {
          "Op": "And",
          "Patterns": [
            { "Op": "Equals", "Tag": "Type=Wood" },
            { "Op": "Not", "Pattern": { "Op": "Equals", "Tag": "Family=Burnt" } }
          ]
        },
        "Priority": 0,
        "BurnLevel": 6,
        "BurnChance": 0.5,
        "ResultingBlock": "Wood_Burnt_Trunk"
      }
    ]
  }
}
```

### Fire Ticker Properties

| Property | Type | Description |
|----------|------|-------------|
| `Type` | String | Set to `"Fire"` for fire spread behavior |
| `SpreadFluid` | String | Fluid ID that spreads (usually `"Fire"`) |
| `FlowRate` | Number | Speed of fire spreading |
| `SupportedBy` | String | Block/fluid that sustains the fire |
| `Flammability` | Array | Rules for what blocks catch fire |

### Flammability Rules

| Property | Type | Description |
|----------|------|-------------|
| `TagPattern` | Object | Tag matching (Op: `Equals`, `And`, `Or`, `Not`) |
| `Priority` | Number | Higher priority rules override lower |
| `BurnLevel` | Number | Fire intensity when burning |
| `BurnChance` | Number | 0-1 probability of catching fire |
| `ResultingBlock` | String | Block to replace with (e.g., burnt wood) |

---

## Tips for Creating Fluids

1. **Flow mechanics** - Configure ticker for spreading
2. **Source blocks** - Separate source from flowing variants
3. **Interactions** - Add collision effects (damage, status effects)
4. **Visual effects** - Use FluidFX for appearance
5. **Sound sets** - Assign appropriate fluid sounds

---

**Previous:** [Block Spawners](62_Block_Spawners.md) | **Next:** [Breaking Decals](64_Breaking_Decals.md)
