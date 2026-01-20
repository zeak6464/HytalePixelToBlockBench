# World Generation

Learn how to configure world generation, zones, biomes, and terrain generation in Hytale.

## Overview

World generation in Hytale is controlled through zone configurations, biome mappings, density settings, and world structure definitions. These systems determine how terrain, biomes, and structures generate in the world.

## Location
- World settings: `Server/HytaleGenerator/Settings/`
- World structures: `Server/HytaleGenerator/WorldStructures/`
- Biomes: `Server/HytaleGenerator/Biomes/`
- Assignments: `Server/HytaleGenerator/Assignments/`
- Density: `Server/HytaleGenerator/Density/`

## Example from Game Files

### World Configuration

From `Server/World/Default/World.json`:

```1:11:Server/World/Default/World.json
{
  "Masks": ["Mask.json"],
  "PrefabStore": "ASSETS",
  "Height": 1,
  "Width": 1,
  "OffsetX": 0,
  "OffsetY": 0,
  "Randomizer": {
    "Generators": []
  }
}
```

This shows a basic world configuration with masks, prefab store settings, and dimensions.

## World Settings

### Basic Settings

`Server/HytaleGenerator/Settings/Settings.json`:

```json
{
  "StatsCheckpoints": [1, 100, 500, 1000],
  "CustomConcurrency": -1,
  "BufferCapacityFactor": 0.1,
  "TargetViewDistance": 512,
  "TargetPlayerCount": 3
}
```

### Settings Properties

- **`StatsCheckpoints`** - Performance monitoring checkpoints
- **`CustomConcurrency`** - Concurrency level (-1 = auto)
- **`BufferCapacityFactor`** - Buffer capacity multiplier
- **`TargetViewDistance`** - Target view distance (blocks)
- **`TargetPlayerCount`** - Expected player count

## World Structures

World structures define how structures generate in the world.

### Basic World Structure

`Server/HytaleGenerator/WorldStructures/Default.json`:

```json
{
  "Type": "NoiseRange",
  "DefaultBiome": "Basic",
  "MaxBiomeEdgeDistance": 32,
  "DefaultTransitionDistance": 32,
  "Density": {
    "Type": "Imported",
    "Name": "Biome-Map"
  },
  "ContentFields": []
}
```

### World Structure Properties

- **`Type`** - Structure type (e.g., `"NoiseRange"`)
- **`DefaultBiome`** - Default biome when none specified
- **`MaxBiomeEdgeDistance`** - Maximum distance for biome edges
- **`DefaultTransitionDistance`** - Default transition distance
- **`Density`** - Density configuration (noise maps, imported maps)
- **`ContentFields`** - Additional content fields

## Zone Configuration

Zones define regions of the world with specific biomes and settings.

### Zone Types

Common zones:
- **`Zone0`** - Starting zone
- **`Zone1`** - First zone (e.g., Plains)
- **`Zone2`** - Second zone (e.g., Desert)
- **`Zone3`** - Third zone (e.g., Taiga)
- **`Zone4`** - Fourth zone (e.g., Volcanic)

## Biome Configuration

Biomes define terrain types, blocks, and generation rules.

### Basic Biome

`Server/HytaleGenerator/Biomes/Basic.json`:

```json
{
  "Name": "Basic",
  "BlockSets": ["Stone", "Dirt"],
  "Environment": "Env_Zone1"
}
```

### Biome Properties

- **`Name`** - Biome identifier
- **`BlockSets`** - Block sets used in this biome
- **`Environment`** - Environment configuration (see [Biomes & Environments](24_Biomes_and_Environments.md))

## Assignments

Assignments define how structures and features are placed in biomes.

### Assignment Structure

Assignments in `Server/HytaleGenerator/Assignments/{Zone}/`:

```json
{
  "Biome": "Plains1",
  "StructureId": "Trees_Plains",
  "Weight": 100,
  "MinDistance": 5,
  "MaxDistance": 20
}
```

### Assignment Properties

- **`Biome`** - Target biome
- **`StructureId`** - Structure/prefab to place
- **`Weight`** - Spawn weight (higher = more common)
- **`MinDistance`** - Minimum distance between instances
- **`MaxDistance`** - Maximum distance between instances

## Density Configuration

Density settings control how densely features spawn.

### Basic Density

`Server/HytaleGenerator/Density/Map_Default.json`:

```json
{
  "Type": "Constant",
  "Value": 1.0
}
```

### Density Types

- **`Constant`** - Fixed density value
- **`Noise`** - Noise-based density
- **`Imported`** - Imported density map

## World Structure Types

### Zone-Based Structures

`Server/HytaleGenerator/WorldStructures/Zone1_Plains1.json`:

```json
{
  "Type": "NoiseRange",
  "DefaultBiome": "Plains1",
  "MaxBiomeEdgeDistance": 64,
  "DefaultTransitionDistance": 32,
  "Density": {
    "Type": "Imported",
    "Name": "Zone1-Plains-Map"
  }
}
```

### Portal Structures

`Server/HytaleGenerator/WorldStructures/Portals_Hedera.json`:

Defines portal placement and portal-related structures.

## Biome Assignments

### Tree Assignments

`Server/HytaleGenerator/Assignments/Forest1/Forest1_River_Trees.json`:

```json
{
  "Biome": "Forest1_River",
  "StructureId": "Tree_Birch",
  "Weight": 50,
  "MinDistance": 3,
  "MaxDistance": 10
}
```

### Plant Assignments

`Server/HytaleGenerator/Assignments/Plains1/Plains1_Grasses.json`:

```json
{
  "Biome": "Plains1",
  "StructureId": "Plant_Grass",
  "Weight": 200,
  "MinDistance": 1,
  "MaxDistance": 3
}
```

## Complete Example: Custom Zone Configuration

### 1. Zone Biome

`Server/HytaleGenerator/Biomes/MyCustom/MyCustom_Plains.json`:

```json
{
  "Name": "MyCustom_Plains",
  "BlockSets": ["Stone", "Dirt", "Grass"],
  "Environment": "Env_MyCustom_Plains"
}
```

### 2. World Structure

`Server/HytaleGenerator/WorldStructures/Zone_MyCustom.json`:

```json
{
  "Type": "NoiseRange",
  "DefaultBiome": "MyCustom_Plains",
  "MaxBiomeEdgeDistance": 48,
  "DefaultTransitionDistance": 24,
  "Density": {
    "Type": "Noise",
    "Scale": 0.1,
    "Amplitude": 0.5
  }
}
```

### 3. Assignment

`Server/HytaleGenerator/Assignments/MyCustom/MyCustom_Trees.json`:

```json
{
  "Biome": "MyCustom_Plains",
  "StructureId": "Tree_MyCustom",
  "Weight": 75,
  "MinDistance": 4,
  "MaxDistance": 12
}
```

## Tips for Configuring World Generation

1. **Start with defaults** - Use existing zone/biome configs as templates
2. **Test density values** - Balance spawn frequency with performance
3. **Configure transitions** - Smooth biome transitions improve visuals
4. **Use assignments** - Place structures consistently across biomes
5. **Reference environments** - Link biomes to environment configs
6. **Test in-game** - World generation looks different in-game
7. **Optimize performance** - Too many structures impact performance

---

**Previous:** [Projectiles](37_Projectiles.md) | **Next:** Back to [README](../README.md)
