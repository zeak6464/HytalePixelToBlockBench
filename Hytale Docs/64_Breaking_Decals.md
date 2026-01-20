# Breaking Decals

Learn how to create visual decals that appear as blocks take damage and break.

## Overview

Breaking decals show progressive damage on blocks as they're being broken. They display crack textures that become more visible as the block takes more damage.

## Location
`Server/Item/Block/BreakingDecals/`

## Example from Game Files

### Wood Breaking Decals

From `Server/Item/Block/BreakingDecals/Breaking_Decals_Wood.json`:

```1:11:Server/Item/Block/BreakingDecals/Breaking_Decals_Wood.json
{
  "StageTextures": [
    "BlockTextures/Cracks/T_Crack_Wood_01.png",
    "BlockTextures/Cracks/T_Crack_Wood_02.png",
    "BlockTextures/Cracks/T_Crack_Wood_03.png",
    "BlockTextures/Cracks/T_Crack_Wood_04.png",
    "BlockTextures/Cracks/T_Crack_Wood_05.png",
    "BlockTextures/Cracks/T_Crack_Wood_06.png",
    "BlockTextures/Cracks/T_Crack_Wood_07.png"
  ]
}
```

This shows breaking decal configurations for wood blocks with visual damage states.

## Basic Breaking Decal Structure

Create `Server/Item/Block/BreakingDecals/Breaking_Decals_MyCustom.json`:

```json
{
  "StageTextures": [
    "BlockTextures/Cracks/T_Crack_Wood_01.png",
    "BlockTextures/Cracks/T_Crack_Wood_02.png",
    "BlockTextures/Cracks/T_Crack_Wood_03.png",
    "BlockTextures/Cracks/T_Crack_Wood_04.png",
    "BlockTextures/Cracks/T_Crack_Wood_05.png",
    "BlockTextures/Cracks/T_Crack_Wood_06.png",
    "BlockTextures/Cracks/T_Crack_Wood_07.png"
  ]
}
```

## Breaking Decal Properties

### StageTextures

```json
{
  "StageTextures": [
    "BlockTextures/Cracks/Crack_01.png",
    "BlockTextures/Cracks/Crack_02.png",
    "BlockTextures/Cracks/Crack_03.png"
  ]
}
```

Array of crack textures, shown progressively as block takes damage. More damage = later textures in the array.

## Common Breaking Decals

### Wood

`Server/Item/Block/BreakingDecals/Breaking_Decals_Wood.json`:

Wood crack textures.

### Stone/Rock

`Server/Item/Block/BreakingDecals/Breaking_Decals_Rock.json`:

Stone crack textures.

### Soil

`Server/Item/Block/BreakingDecals/Breaking_Decals_Soil.json`:

Dirt/soil crack textures.

## Using Breaking Decals

Breaking decals are automatically applied when blocks take damage. The game selects decals based on block material or breaking decal ID.

## Tips for Creating Breaking Decals

1. **Progressive stages** - Create 5-7 crack textures from light to heavy damage
2. **Material matching** - Match decal style to block material
3. **Texture format** - Use transparent PNGs with crack patterns
4. **Consistency** - Maintain consistent style across stages

---

**Previous:** [Block Fluids](63_Block_Fluids.md) | **Next:** [Fluid FX](65_Fluid_FX.md)
