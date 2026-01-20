# HytaleGenerator BlockMasks

Learn how to configure block masks for controlling block placement in world generation using `Server/HytaleGenerator/BlockMasks/`.

## Overview

Block masks define which blocks can or cannot be placed or replaced during world generation. They're used to prevent structures from overwriting important blocks (like tree roots) or to restrict placement to specific block types.

## Example from Game Files

Block masks define which blocks can or cannot be placed or replaced during world generation. They're used to prevent structures from overwriting important blocks (like tree roots) or to restrict placement to specific block types.

## Location

`Server/HytaleGenerator/BlockMasks/`

## Basic Block Mask Structure

`Server/HytaleGenerator/BlockMasks/Taiga1_Trees.json`:

```json
{
  "$Title": "[ROOT] BlockMask",
  "ExportAs": "Taiga1_Trees",
  "DontPlace": {
    "Inclusive": true,
    "Materials": [
      {
        "Solid": "Wood_Ash_Roots"
      },
      {
        "Solid": "Wood_Cedar_Roots"
      },
      {
        "Solid": "Wood_Fir_Roots"
      },
      {
        "Solid": "Wood_Redwood_Roots"
      }
    ]
  },
  "DontReplace": {
    "Inclusive": false,
    "Materials": [
      {
        "Solid": "Empty"
      },
      {
        "Solid": "Soil_Grass"
      },
      {
        "Solid": "Soil_Leaves"
      },
      {
        "Solid": "Soil_Needles"
      },
      {
        "Solid": "Soil_Dirt"
      },
      {
        "Solid": "Soil_Pathway"
      },
      {
        "Solid": "Soil_Mud"
      }
    ]
  }
}
```

## Block Mask Properties

### ExportAs

Export name for reference in assignments:

```json
{
  "ExportAs": "Taiga1_Trees"
}
```

### DontPlace

Blocks that cannot be placed on:

```json
{
  "DontPlace": {
    "Inclusive": true,
    "Materials": [
      {
        "Solid": "Wood_Ash_Roots"
      },
      {
        "Solid": "Wood_Cedar_Roots"
      }
    ]
  }
}
```

**Properties:**
- **`Inclusive`** - If `true`, blocks in list cannot be placed on; if `false`, only blocks NOT in list can be placed on
- **`Materials`** - Array of block materials
  - **`Solid`** - Block ID (e.g., `"Wood_Ash_Roots"`)

**Use Case:** Prevents placing structures on tree roots or other protected blocks.

### DontReplace

Blocks that cannot be replaced:

```json
{
  "DontReplace": {
    "Inclusive": false,
    "Materials": [
      {
        "Solid": "Empty"
      },
      {
        "Solid": "Soil_Grass"
      },
      {
        "Solid": "Soil_Leaves"
      },
      {
        "Solid": "Soil_Dirt"
      }
    ]
  }
}
```

**Properties:**
- **`Inclusive`** - If `true`, blocks in list cannot be replaced; if `false`, only blocks NOT in list can be replaced
- **`Materials`** - Array of block materials to protect

**Use Case:** Prevents overwriting important surface blocks (grass, leaves, dirt) when placing structures.

## Inclusive vs Exclusive

### Inclusive: true

When `Inclusive: true`, the mask applies to blocks **in** the list:

```json
{
  "DontPlace": {
    "Inclusive": true,
    "Materials": [
      {"Solid": "Wood_Ash_Roots"},
      {"Solid": "Wood_Cedar_Roots"}
    ]
  }
}
```

**Meaning:** Cannot place on `Wood_Ash_Roots` or `Wood_Cedar_Roots`.

### Inclusive: false

When `Inclusive: false`, the mask applies to blocks **not in** the list:

```json
{
  "DontReplace": {
    "Inclusive": false,
    "Materials": [
      {"Solid": "Empty"},
      {"Solid": "Soil_Grass"}
    ]
  }
}
```

**Meaning:** Can only replace `Empty` or `Soil_Grass` blocks. All other blocks are protected.

## Common Block Mask Patterns

### Tree Root Protection

Protects tree root blocks from being replaced:

```json
{
  "ExportAs": "TreeRoots_Protection",
  "DontPlace": {
    "Inclusive": true,
    "Materials": [
      {"Solid": "Wood_Ash_Roots"},
      {"Solid": "Wood_Cedar_Roots"},
      {"Solid": "Wood_Fir_Roots"},
      {"Solid": "Wood_Redwood_Roots"},
      {"Solid": "Wood_Oak_Roots"}
    ]
  }
}
```

### Surface Block Protection

Protects surface blocks from being replaced:

```json
{
  "ExportAs": "Surface_Protection",
  "DontReplace": {
    "Inclusive": false,
    "Materials": [
      {"Solid": "Empty"},
      {"Solid": "Soil_Grass"},
      {"Solid": "Soil_Leaves"},
      {"Solid": "Soil_Dirt"},
      {"Solid": "Soil_Pathway"}
    ]
  }
}
```

**Meaning:** Can only replace empty space or surface blocks. Bedrock and other important blocks are protected.

### Air-Only Placement

Only allows placement on air/empty blocks:

```json
{
  "ExportAs": "AirOnly_Placement",
  "DontPlace": {
    "Inclusive": false,
    "Materials": [
      {"Solid": "Empty"}
    ]
  }
}
```

**Meaning:** Can only place on `Empty` blocks. All solid blocks are protected.

## Using Block Masks in Assignments

Reference block masks in assignments:

```json
{
  "Type": "Prefab",
  "WeightedPrefabPaths": [
    {
      "Path": "Trees/Oak/Stage_0",
      "Weight": 100
    }
  ],
  "Scanner": {
    "Type": "ColumnLinear",
    "MaxY": 60,
    "MinY": 0
  },
  "BlockMask": "Taiga1_Trees"
}
```

**Note:** Block masks are typically referenced by their `ExportAs` name in assignment properties.

## Creating Custom Block Masks

### Step 1: Create Block Mask File

Create `Server/HytaleGenerator/BlockMasks/MyCustom_Mask.json`:

```json
{
  "$Title": "[ROOT] BlockMask",
  "ExportAs": "MyCustom_Mask",
  "DontPlace": {
    "Inclusive": true,
    "Materials": [
      {
        "Solid": "Rock_Bedrock"
      },
      {
        "Solid": "Rock_Marble"
      }
    ]
  },
  "DontReplace": {
    "Inclusive": false,
    "Materials": [
      {
        "Solid": "Empty"
      },
      {
        "Solid": "Soil_Grass"
      },
      {
        "Solid": "Soil_Dirt"
      }
    ]
  }
}
```

### Step 2: Reference in Assignments

Reference in assignments that need block restrictions:

```json
{
  "Type": "Prefab",
  "BlockMask": "MyCustom_Mask",
  "WeightedPrefabPaths": [...]
}
```

## Tips

1. **Protect Important Blocks** - Use `DontReplace` to protect bedrock, roots, and important structures
2. **Inclusive Logic** - Understand `Inclusive` to get the correct behavior
3. **Surface Blocks** - Protect surface blocks (`Soil_Grass`, `Soil_Leaves`) to preserve terrain
4. **Root Protection** - Always protect tree root blocks when placing structures near trees
5. **Export Names** - Export masks for reuse across multiple assignments
6. **Empty Blocks** - Include `Empty` in `DontReplace` with `Inclusive: false` to allow only air replacement
7. **Material Lists** - List all relevant block types that need protection

---

**Previous:** [HytaleGenerator Graphs](165_HytaleGenerator_Graphs.md) | **Next:** [Macro Commands](167_Macro_Commands.md)
