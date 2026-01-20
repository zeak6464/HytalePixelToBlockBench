# HytaleGenerator Density

Learn how to configure density maps for world generation using `Server/HytaleGenerator/Density/`.

## Overview

Density maps define how terrain density varies across the world. They're complex node-based systems that generate 2D or 3D density fields used for terrain generation, biome placement, and feature distribution.

## Example from Game Files

Density maps define how terrain density varies across the world. They're complex node-based systems that generate 2D or 3D density fields used for terrain generation, biome placement, and feature distribution.

## Location

`Server/HytaleGenerator/Density/`

Examples:
- `Server/HytaleGenerator/Density/Map_Default.json`
- `Server/HytaleGenerator/Density/Map_Portals.json`
- `Server/HytaleGenerator/Density/Map_Tiles.json`
- `Server/HytaleGenerator/Density/Map_Tiles_Rivers.json`

## Basic Density Map Structure

Density maps use a complex node-based graph system:

`Server/HytaleGenerator/Density/Map_Default.json`:

```json
{
  "$Title": "[ROOT] Exported Density",
  "Type": "Exported",
  "ExportAs": "Biome-Map",
  "SingleInstance": true,
  "Skip": false,
  "Inputs": [
    {
      "Type": "YOverride",
      "Value": 0,
      "Inputs": [
        {
          "Type": "Cache",
          "Capacity": 1,
          "Inputs": [
            {
              "Type": "Scale",
              "ScaleX": 1,
              "ScaleY": 1,
              "ScaleZ": 1,
              "Inputs": [
                {
                  "Type": "Mix",
                  "Inputs": [...]
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

## Density Node Types

### Exported

Exports density for use in world structures:

```json
{
  "Type": "Exported",
  "ExportAs": "Biome-Map",
  "SingleInstance": true,
  "Skip": false,
  "Inputs": [...]
}
```

**Properties:**
- **`ExportAs`** - Export name (referenced elsewhere)
- **`SingleInstance`** - If `true`, cached for performance
- **`Skip`** - If `true`, skips this node
- **`Inputs`** - Array of input density nodes

### YOverride

Locks Y coordinate for 2D operations:

```json
{
  "Type": "YOverride",
  "Value": 0,
  "Inputs": [
    {
      "Type": "Cache",
      "Inputs": [...]
    }
  ]
}
```

**Properties:**
- **`Value`** - Y coordinate to lock to (typically 0 for surface)
- **`Inputs`** - Input density nodes

### Cache

Caches density results for performance:

```json
{
  "Type": "Cache",
  "Capacity": 1,
  "Inputs": [...]
}
```

**Properties:**
- **`Capacity`** - Cache size (1-3 recommended)
- **`Inputs`** - Input density nodes to cache

### Scale

Scales density coordinates:

```json
{
  "Type": "Scale",
  "ScaleX": 1,
  "ScaleY": 1,
  "ScaleZ": 1,
  "Inputs": [...]
}
```

**Properties:**
- **`ScaleX`** - X-axis scale
- **`ScaleY`** - Y-axis scale
- **`ScaleZ`** - Z-axis scale
- **`Inputs`** - Input density nodes

### Mix

Blends multiple density inputs:

```json
{
  "Type": "Mix",
  "Inputs": [
    {
      "Type": "Sum",
      "Inputs": [...]
    },
    {
      "Type": "Constant",
      "Value": -0.3
    }
  ]
}
```

**Properties:**
- **`Inputs`** - Array of density nodes to blend

### Sum

Adds multiple density inputs:

```json
{
  "Type": "Sum",
  "Inputs": [
    {
      "Type": "SimplexNoise2D",
      "Scale": 5000,
      "Seed": "World-Continent-Map"
    },
    {
      "Type": "Distance",
      "Curve": {
        "Type": "Manual",
        "Points": [
          {"In": 0, "Out": -1},
          {"In": 1000, "Out": 0}
        ]
      }
    }
  ]
}
```

**Properties:**
- **`Inputs`** - Array of density nodes to sum

### Min/Max

Takes minimum or maximum of inputs:

```json
{
  "Type": "Min",
  "Inputs": [
    {
      "Type": "Imported",
      "Name": "World-Continent-Map"
    },
    {
      "Type": "Mix",
      "Inputs": [...]
    }
  ]
}
```

**Types:**
- **`"Min"`** - Minimum value
- **`"Max"`** - Maximum value

### SimplexNoise2D

Generates 2D simplex noise:

```json
{
  "Type": "SimplexNoise2D",
  "Seed": "World-Continent-Map",
  "Scale": 5000,
  "Octaves": 3,
  "Persistence": 0.3,
  "Lacunarity": 5,
  "ExportAs": ""
}
```

**Properties:**
- **`Seed`** - Random seed string
- **`Scale`** - Noise scale (higher = larger features)
- **`Octaves`** - Detail levels (1-6)
- **`Persistence`** - Detail persistence (0.0-1.0)
- **`Lacunarity`** - Frequency multiplier (2-8)
- **`ExportAs`** - Optional export name

### PositionsCellNoise

Generates cell noise based on positions:

```json
{
  "Type": "PositionsCellNoise",
  "MaxDistance": 460,
  "Positions": {
    "Type": "Mesh2D",
    "ExportAs": "World-Biome-Cells",
    "PointsY": 0,
    "PointGenerator": {
      "Type": "Mesh",
      "Jitter": 0.2,
      "ScaleX": 350,
      "ScaleY": 350,
      "ScaleZ": 350,
      "Seed": "World-Biome-Cells"
    }
  },
  "ReturnType": {
    "Type": "CellValue"
  },
  "DistanceFunction": {
    "Type": "Euclidean"
  }
}
```

**Properties:**
- **`MaxDistance`** - Maximum distance for cell noise
- **`Positions`** - Position generator (Mesh2D)
  - **`PointGenerator`** - Mesh point generator
    - **`Jitter`** - Position jitter (0.0-1.0)
    - **`ScaleX/Y/Z`** - Mesh cell size
    - **`Seed`** - Random seed
- **`ReturnType`** - `"CellValue"` or `"Distance"`
- **`DistanceFunction`** - `"Euclidean"` or other distance types

### Imported

References exported density from other files:

```json
{
  "Type": "Imported",
  "Name": "World-Continent-Map"
}
```

**Properties:**
- **`Name`** - Export name from another density map

### Constant

Returns a constant density value:

```json
{
  "Type": "Constant",
  "Value": -0.3
}
```

**Properties:**
- **`Value`** - Constant density value (typically -1.0 to 1.0)

### Normalizer

Normalizes density range:

```json
{
  "Type": "Normalizer",
  "FromMin": -1,
  "FromMax": 1,
  "ToMin": -0.5,
  "ToMax": 1,
  "Inputs": [...]
}
```

**Properties:**
- **`FromMin`** / **`FromMax`** - Source range
- **`ToMin`** / **`ToMax`** - Target range
- **`Inputs`** - Input density node

### Clamp

Clamps density to a range:

```json
{
  "Type": "Clamp",
  "WallA": -1,
  "WallB": 1,
  "Inputs": [...]
}
```

**Properties:**
- **`WallA`** - Minimum value
- **`WallB`** - Maximum value
- **`Inputs`** - Input density node

### Inverter

Inverts density values:

```json
{
  "Type": "Inverter",
  "Inputs": [
    {
      "Type": "Imported",
      "Name": "World-River-Map"
    }
  ]
}
```

### FastGradientWarp

Warps density using gradients:

```json
{
  "Type": "FastGradientWarp",
  "WarpScale": 100,
  "WarpPersistence": 0.2,
  "WarpLacunarity": 2,
  "WarpOctaves": 2,
  "WarpFactor": 100,
  "Seed": "A",
  "Inputs": [...]
}
```

**Properties:**
- **`WarpScale`** - Warp noise scale
- **`WarpPersistence`** - Detail persistence
- **`WarpLacunarity`** - Frequency multiplier
- **`WarpOctaves`** - Detail levels
- **`WarpFactor`** - Warp strength

## Common Density Maps

### Biome Map

Maps biome placement:

`Server/HytaleGenerator/Density/Map_Default.json`:

Exports `"Biome-Map"` used by world structures for biome distribution.

**Structure:**
- Uses `PositionsCellNoise` for biome cells
- Mixes continent and river maps
- Normalizes and clamps values
- Exports as `"Biome-Map"`

### River Map

Maps river placement:

`Server/HytaleGenerator/Density/Map_Tiles_Rivers.json`:

Exports `"World-River-Map"` for river generation.

**Structure:**
- Uses `Sum` of multiple noise layers
- Inverts for river valleys
- Normalizes to -1.0 to 1.0 range

### Portal Map

Maps portal placement:

`Server/HytaleGenerator/Density/Map_Portals.json`:

Exports `"Biome-Map-Portals"` for portal structures.

### Tile Map

Maps terrain tiles:

`Server/HytaleGenerator/Density/Map_Tiles.json`:

Exports tile density for terrain generation.

## Creating Custom Density Maps

### Step 1: Create Density File

Create `Server/HytaleGenerator/Density/Map_MyCustom.json`:

```json
{
  "$Title": "[ROOT] Exported Density",
  "Type": "Exported",
  "ExportAs": "MyCustom-Map",
  "SingleInstance": true,
  "Skip": false,
  "Inputs": [
    {
      "Type": "YOverride",
      "Value": 0,
      "Inputs": [
        {
          "Type": "Cache",
          "Capacity": 1,
          "Inputs": [
            {
              "Type": "SimplexNoise2D",
              "Seed": "MyCustom-Map",
              "Scale": 1000,
              "Octaves": 3,
              "Persistence": 0.3,
              "Lacunarity": 5
            }
          ]
        }
      ]
    }
  ]
}
```

### Step 2: Reference in World Structure

Reference in world structure:

```json
{
  "Type": "NoiseRange",
  "DefaultBiome": "Basic",
  "Density": {
    "Type": "Imported",
    "Name": "MyCustom-Map"
  }
}
```

## Tips

1. **Export Important Maps** - Export density maps you'll reuse with `ExportAs`
2. **Use Cache** - Cache expensive operations with `Cache` nodes
3. **YOverride for 2D** - Use `YOverride` to lock to surface (Y=0) for 2D maps
4. **Normalize Ranges** - Use `Normalizer` to convert between value ranges
5. **Clamp Values** - Use `Clamp` to ensure valid ranges (-1 to 1)
6. **Mix Layers** - Use `Mix` or `Sum` to combine multiple density layers
7. **Import Reusables** - Import exported maps instead of duplicating
8. **SingleInstance** - Set `SingleInstance: true` for exported maps used once
9. **Scale Appropriately** - Use appropriate noise scales (large = continents, small = details)
10. **Seed Consistency** - Use consistent seed strings for reproducible results

---

**Previous:** [HytaleGenerator Assignments](163_HytaleGenerator_Assignments.md) | **Next:** [HytaleGenerator Graphs](165_HytaleGenerator_Graphs.md)
