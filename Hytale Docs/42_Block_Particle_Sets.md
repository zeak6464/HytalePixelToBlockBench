# Block Particle Sets

Learn how to configure block particle effects for breaking, hitting, landing, and other block interactions.

## Overview

Block particle sets define particle effects that play when interacting with blocks (breaking, hitting, landing, sprinting, building). They're defined in `Server/Item/Block/Particles/` and referenced by blocks via `BlockParticleSetId`.

## Location
- Block particle sets: `Server/Item/Block/Particles/`
- Particle systems: `Server/Particles/Block/`

## Example from Game Files

### Wood Block Particle Set

From `Server/Item/Block/Particles/Wood.json`:

```1:9:Server/Item/Block/Particles/Wood.json
{
  "Particles": {
    "Sprint": "Block_Sprint_Wood",
    "Hit": "Block_Hit_Wood",
    "Break": "Block_Break_Wood",
    "SoftLand": "Block_Land_Soft_Wood",
    "HardLand": "Block_Land_Hard_Wood",
    "Physics": "Block_Break_Wood_Light"
  }
}
```

This shows a block particle set with particles for sprinting, hitting, breaking, landing, and physics on wood blocks.

## Basic Block Particle Set Structure

Create `Server/Item/Block/Particles/MyCustom_Material.json`:

```json
{
  "Particles": {
    "Hit": "Block_Hit_MyCustom",
    "Break": "Block_Break_MyCustom",
    "Sprint": "Block_Sprint_MyCustom",
    "SoftLand": "Block_Land_Soft_MyCustom",
    "HardLand": "Block_Land_Hard_MyCustom",
    "Build": "Block_Build_Generic_Dust"
  }
}
```

## Block Particle Set Properties

### Hit

```json
{
  "Hit": "Block_Hit_Stone"
}
```

Particle effect when hitting the block (punching, tool impact).

### Break

```json
{
  "Break": "Block_Break_Stone"
}
```

Particle effect when the block is broken.

### Sprint

```json
{
  "Sprint": "Block_Sprint_Stone"
}
```

Particle effect when sprinting on the block.

### SoftLand

```json
{
  "SoftLand": "Block_Land_Soft_Stone"
}
```

Particle effect when landing softly on the block.

### HardLand

```json
{
  "HardLand": "Block_Land_Hard_Stone"
}
```

Particle effect when landing hard on the block (high fall).

### Build

```json
{
  "Build": "Block_Build_Generic_Dust"
}
```

Particle effect when placing/building the block.

## Common Block Particle Sets

### Stone

`Server/Item/Block/Particles/Stone.json`:

```json
{
  "Particles": {
    "Hit": "Block_Hit_Stone",
    "Break": "Block_Break_Stone",
    "Sprint": "Block_Sprint_Stone",
    "SoftLand": "Block_Land_Soft_Stone",
    "HardLand": "Block_Land_Hard_Stone",
    "Build": "Block_Build_Generic_Dust"
  }
}
```

### Wood

`Server/Item/Block/Particles/Wood.json`:

```json
{
  "Particles": {
    "Sprint": "Block_Sprint_Wood",
    "Hit": "Block_Hit_Wood",
    "Break": "Block_Break_Wood",
    "SoftLand": "Block_Land_Soft_Wood",
    "HardLand": "Block_Land_Hard_Wood",
    "Physics": "Block_Break_Wood_Light"
  }
}
```

### Dirt

`Server/Item/Block/Particles/Dirt.json`:

```json
{
  "Particles": {
    "Sprint": "Block_Sprint_Dirt",
    "Run": "Block_Run_Dirt",
    "Hit": "Block_Hit_Dirt",
    "Break": "Block_Break_Dirt",
    "SoftLand": "Block_Land_Soft_Dirt",
    "HardLand": "Block_Land_Hard_Dirt"
  }
}
```

### Sand

`Server/Item/Block/Particles/Sand.json`:

```json
{
  "Particles": {
    "Hit": "Block_Hit_Sand",
    "Break": "Block_Break_Sand",
    "Run": "Block_Run_Sand",
    "Sprint": "Block_Sprint_Sand",
    "SoftLand": "Block_Land_Sand_Soft",
    "HardLand": "Block_Land_Sand_Hard"
  }
}
```

### Metal

`Server/Item/Block/Particles/Metal.json`:

```json
{
  "Particles": {
    "Hit": "Block_Hit_Metal",
    "Break": "Block_Break_Metal",
    "HardLand": "Block_Land_Hard_Metal"
  }
}
```

> **Note:** Metal only defines Hit, Break, and HardLand - no Sprint or SoftLand.

### Grass

`Server/Item/Block/Particles/Grass.json`:

```json
{
  "Particles": {
    "Sprint": "Block_Sprint_Grass",
    "Hit": "Block_Break_Grass",
    "HardLand": "Block_Land_Hard_Grass",
    "Break": "Block_Break_Grass"
  }
}
```

> **Note:** Grass uses the same particle (`Block_Break_Grass`) for both Hit and Break.

## Using Block Particle Sets

### In Block Definition

Reference particle sets in your block items:

```json
{
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Cube",
    "Textures": [
      {
        "All": "BlockTextures/Rock_Stone.png",
        "Weight": 1
      }
    ],
    "BlockParticleSetId": "Stone",
    "BlockSoundSetId": "Stone"
  }
}
```

### Complete Example: Custom Material

#### 1. Particle Set Definition

`Server/Item/Block/Particles/MyCustom_Magical.json`:

```json
{
  "Particles": {
    "Hit": "Block_Hit_Magical",
    "Break": "Block_Break_Magical",
    "Sprint": "Block_Sprint_Magical",
    "SoftLand": "Block_Land_Soft_Magical",
    "HardLand": "Block_Land_Hard_Magical",
    "Build": "Block_Build_Magical_Sparkles"
  }
}
```

#### 2. Using in Block

`Server/Item/Items/Rock/MyCustom/Magic_Stone.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Magic_Stone.name"
  },
  "ItemLevel": 10,
  "MaxStack": 100,
  "Icon": "Icons/ItemsGenerated/Magic_Stone.png",
  "Categories": ["Blocks.Custom"],
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Cube",
    "Textures": [
      {
        "All": "BlockTextures/Magic_Stone.png",
        "Weight": 1
      }
    ],
    "BlockParticleSetId": "MyCustom_Magical",
    "BlockSoundSetId": "Stone",
    "ParticleColor": "#ff6b9d"
  },
  "Tags": {
    "Type": ["Rock"]
  }
}
```

## Particle System Creation

Particle systems for blocks are defined in `Server/Particles/Block/`:

### Example Particle System

`Server/Particles/Block/Stone/Block_Break_Stone.particlesystem`:

```json
{
  "Spawners": [
    {
      "SpawnerId": "Block_Break_Dust",
      "PositionOffset": {
        "X": 0,
        "Y": 0,
        "Z": 0
      }
    },
    {
      "SpawnerId": "Block_Break_Stone_Parts",
      "PositionOffset": {
        "X": 0,
        "Y": 0,
        "Z": 0
      }
    },
    {
      "SpawnerId": "Block_Break_Stone_Sparks",
      "FixedRotation": true,
      "PositionOffset": {
        "X": 0,
        "Y": 0,
        "Z": 0
      }
    }
  ]
}
```

See the [Particle Systems](32_Particle_Systems.md) guide for detailed particle creation.

## Tips for Creating Block Particle Sets

1. **Match material** - Use particle effects that match the block material
2. **Create particle systems** - Define particle systems for each interaction type
3. **Use consistent naming** - Follow naming conventions (e.g., `Block_Break_Material`)
4. **Reference in blocks** - Set `BlockParticleSetId` in your block definitions
5. **Test interactions** - Verify particles play correctly for all interaction types
6. **Match sound sets** - Use corresponding particle and sound sets together
7. **Consider physics** - Some materials may have physics particle effects

---

**Previous:** [Commands](41_Commands.md) | **Next:** [NPC Groups](43_NPC_Groups.md)
