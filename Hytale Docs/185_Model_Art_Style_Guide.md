# Model Art Style Guide

Official Hytale art direction guidelines for creating models and textures.

## Overview

This guide is based on the official Hytale blog post ["An Introduction to Making Models for Hytale"](https://hytale.com/news/2025/12/an-introduction-to-making-models-for-hytale) by Thomas Frick (Art Director).

**Hytale Art Style Definition:**
> "A modern, stylized voxel game, with retro pixel-art textures"

Hytale's art style combines modern game engine capabilities with old-school pixel art charm - at the intersection of low-definition pixel art and hand-painted 3D.

## The Art Pillars

Every creation should align with these core visual pillars:

### 1. Immersive
- Make the world feel alive
- Motion and detail everywhere
- World reacts to the player
- Creatures express emotion
- Leave lasting impressions on players

### 2. Fantasy
- Deeply Fantasy, but not limited to Medieval Fantasy
- Consistent across universes, stories, and themes
- Each character has unique personality
- Recognizable Hytale style in any setting

### 3. Stylized
- Iconic proportions and color palette
- Models should be easily identifiable
- Players can read the world regardless of clutter
- **Simplicity is key** - takes many iterations to achieve
- Carefully select which geometry to preserve/discard

### 4. Flexible
- Models composed of primitive shapes (cubes, quads)
- Easy to understand how models are made
- Simple technical structure
- Easy to comprehend, iterate, and modify
- Empowers user creativity

## Geometry Constraints

### Allowed Primitives
Only use these two primitives:
- **Cubes** (6 sides)
- **Quads** (2 sides)

### Not Allowed
- Edge loops
- Special topology
- Triangles
- Pyramids
- Spheres (**No spheres allowed!**)
- Other traditional modeling functions

### Why These Constraints?
- Easy to make and unwrap
- Easy to animate
- No weight painting required
- No rigging expertise needed
- No 3D art degree required to start

## Texture Guidelines

### Texture Sizes
Textures must be:
- Non-square allowed
- **Multiples of 32px** (32, 64, 96, 128, etc.)

### Texel Density

| Type | Density | Use For |
|------|---------|---------|
| Characters/Attachments | 64px per unit | Cosmetics, Tools, Weapons, Food items |
| Props/Blocks | 32px per unit | Furniture, Decorations, World objects |

**Why higher density for characters?**
- Allows details on skin, tattoos, makeup
- Better eye and mouth motion
- More life in faces
- Characters are seen close up (first person, fighting)
- Helps characters detach from environment
- Better readability in cluttered scenes
- More cosmetic personalization options

### Stretching Rules
Geometry stretching is allowed for fine adjustments (avoiding Z-fighting, resizing):

| Stretch Range | Status |
|---------------|--------|
| 0.7x - 1.3x | ✅ Acceptable |
| < 0.7x | ❌ Pixels obviously stretched |
| > 1.3x | ❌ Pixels obviously stretched |

## Color Guidelines

### Colors to Avoid
- **Pure white** - Breaks lighting, adds too much contrast
- **Pure black** - Ruins values, breaks lighting

### Shadow Colors
- Shadows should **never** be purely desaturated
- Always contain color nuances
- **Add a hint of purple** in shadows for vibrancy
- Creates more vibrant, lively models

### Good Practices
- Test colors in-game when possible
- Use color theory principles
- Maintain consistent palette across related models

## Texturing Techniques

### Recommended Brushes
Any painting software works (Photoshop, GIMP, Krita, Blockbench, Procreate):

1. **Pencil Brush** (opacity on) - For details and color-blocking
2. **Round Soft Brush** (opacity pressure on) - For smoothing and shading

### Texturing Philosophy
- Treat each texture as an **illustration**
- Paint/bake shadows, ambient occlusion, and highlights directly into texture
- Simulate complex lighting that doesn't exist in-game
- Create **illusion of detail**

### What to Avoid
- Noise
- Too much grain
- Perfectly flat surfaces

### The Process
1. **Color-blocking** with Pencil brush
2. **Smoothing and shading** with Soft Brush
3. **Polish** with opacity Pencil

## Model Optimization

### Triangle Count
- Work as simply as possible first
- Increase geometry only when needed to improve silhouette
- Many scenes render several thousand blocks = millions of triangles
- Triangles are major contributor to FPS loss

### Guidelines by Model Type

| Model Type | Triangle Target | Notes |
|------------|-----------------|-------|
| Simple props | Very low | Minimal geometry |
| Furniture | Low-Medium | Basic shapes |
| Characters | Medium | Balance detail vs performance |
| Complex models | As needed | Optimize silhouette |

## Shading Modes

Hytale uses a limited set of material types called "shading modes" for performance and style consistency. These can be set per-node in models.

**Note:** Blockbench cannot display shading modes yet, but they can be set and exported.

### Disabling Side Shadows
For characters, disable side shadows to avoid hard edge effects on organic volumes.

## The Hytale Renderer

### Key Points
- Prioritizes speed and performance
- Designed to run on older computers
- Not using industry-standard PBR workflows
- No roughness, normal maps, or displacement

### Lighting Approach
- Paint lights and shadows inside textures
- Use real lights/shadows to bring everything together
- In-house light propagation techniques
- Selective shaders and post-processing (Bloom, DoF, AO)

### Design Goal
> "Even without any effects, our models should look good on their own."

## Tools

### Blockbench + Hytale Plugin
- Download [Blockbench](https://www.blockbench.net/)
- Install [Hytale Blockbench Plugin](https://www.blockbench.net/plugins/hytale_plugin)
- [Plugin Source Code](https://github.com/JannisX11/hytale-blockbench-plugin)

### Plugin Features
- Maintains consistent pixel ratio across textures
- Exports models and animations in correct format
- Quality-of-life improvements
- Keeps everything simple

## Character Bone Naming

Characters require properly named bones for animation compatibility. The animation system automatically animates correctly named bones.

Common bone names:
- `Head`, `Neck`
- `Body`, `Torso`
- `ArmL`, `ArmR`
- `LegL`, `LegR`
- `HandL`, `HandR`
- `FootL`, `FootR`

## Quick Reference

### Do's ✅
- Use cubes and quads only
- Add color to shadows (purple works great)
- Keep textures as multiples of 32px
- Paint lighting into textures
- Keep geometry simple
- Test in-game when possible
- Use soft brush for volume
- Create illusion of depth

### Don'ts ❌
- Use spheres
- Use pure white or pure black
- Create purely desaturated shadows
- Over-complicate geometry
- Add too much noise/grain
- Make perfectly flat surfaces
- Stretch beyond 0.7x-1.3x range

## Related Resources

- [Hytale Blockbench Plugin](https://www.blockbench.net/plugins/hytale_plugin)
- [Plugin Source Code](https://github.com/JannisX11/hytale-blockbench-plugin)
- [Official Blog Post](https://hytale.com/news/2025/12/an-introduction-to-making-models-for-hytale)

---

**Previous:** [Modding Languages & Scripting](184_Modding_Languages_And_Scripting.md) | **Next:** Back to [README](../README.md)
