# Prefab Catalog

Reference guide for available prefabs organized by category.

## Overview

Prefabs are pre-built structures used in world generation. This catalog lists all prefab categories with file counts to help find appropriate structures for custom content.

## Location
`Server/Prefabs/`

---

## Prefab Categories

### Cave (492 prefabs)
Underground cave formations and structures.

```
Server/Prefabs/Cave/
├── Decorations/      # Cave decorations
├── Entrances/        # Cave entrances
├── Formations/       # Rock formations
├── Rooms/            # Cave rooms
└── Tunnels/          # Tunnel segments
```

**Use for:** Underground exploration, cave systems, mining areas

---

### Dungeon (800+ prefabs)
Dungeon rooms, corridors, and features.

```
Server/Prefabs/Dungeon/
├── Challenge_Gate/   # Challenge gates (4)
├── Cursed_Crypt/     # Crypt dungeon (58)
├── Goblin_Lair/      # Goblin dungeon (97)
├── Labyrinth/        # Maze dungeon (71)
├── Magic_Ruins/      # Magic ruins (100)
├── Outlander_Temple/ # Temple dungeon (76)
├── Rift/             # Rift dimension (10)
├── Sandstone/        # Desert dungeon (61)
├── Sewer/            # Sewer system (148)
├── Shale/            # Shale dungeon (135)
├── Slate/            # Slate dungeon
└── Stone/            # Stone dungeon
    ├── Entrance/     # Dungeon entrances
    ├── Level_1/      # First level rooms
    ├── Level_2/      # Second level
    ├── Level_3/      # Boss rooms
    └── Walls/        # Wall segments
```

**Use for:** Dungeon generation, boss arenas, loot rooms

---

### Mineshaft (1,160 prefabs)
Mine corridors, rooms, and infrastructure.

```
Server/Prefabs/Mineshaft/
├── Corridors/
├── Crossings/
├── Elevators/
├── Entrances/
├── Rooms/
├── Shafts/
├── Stairs/
└── Supports/
```

**Use for:** Underground mines, resource areas, abandoned structures

---

### Mineshaft_Drift (117 prefabs)
Mineshaft drift tunnels and variations.

---

### Monuments (778 prefabs)
Surface structures and points of interest.

```
Server/Prefabs/Monuments/
├── Camps/            # NPC camps
├── Ruins/            # Ancient ruins
├── Shrines/          # Small shrines
├── Statues/          # Decorative statues
├── Temples/          # Temple structures
├── Towers/           # Tower buildings
├── Villages/         # Village buildings
└── Wells/            # Water wells
```

**Use for:** World landmarks, quest locations, NPC settlements

---

### Npc (856 prefabs)
NPC-specific structures and homes.

```
Server/Prefabs/Npc/
├── Camps/            # Enemy camps
├── Goblin/           # Goblin structures
├── Kweebec/          # Kweebec homes
├── Outlander/        # Outlander buildings
├── Skeleton/         # Undead lairs
├── Slothian/         # Slothian homes
├── Trork/            # Trork camps
└── Villager/         # Generic villager
```

**Use for:** NPC spawns, faction areas, enemy encounters

---

### Plants (555 prefabs)
Flora and vegetation structures.

```
Server/Prefabs/Plants/
├── Bushes/           # Bush clusters
├── Flowers/          # Flower arrangements
├── Grass/            # Grass patches
├── Mushrooms/        # Mushroom clusters
└── Vines/            # Vine decorations
```

**Use for:** Biome decoration, garden areas, natural environments

---

### Rock_Formations (1,676 prefabs)
Natural rock structures and geological features.

```
Server/Prefabs/Rock_Formations/
├── Arches/           # Rock arches (275)
├── Crystal_Floating/ # Floating crystals
├── Crystal_Pattern/  # Crystal patterns
├── Crystal_Pits/     # Crystal caves
├── Crystals/         # Crystal formations
├── Dolmen/           # Stone monuments
├── Fossils/          # Fossil formations (72)
├── Geode_Floating/   # Floating geodes (61)
├── Hotsprings/       # Hot spring areas (30)
├── Ice_Formations/   # Ice structures (17)
├── Mushrooms/        # Giant mushrooms (33)
├── Pillars/          # Rock pillars (215)
├── Rocks/            # Boulder clusters (944)
└── Stalactites/      # Cave formations
    ├── Basalt/
    ├── Shale/
    ├── Stone/
    └── Volcanic/
```

**Use for:** Terrain decoration, biome variety, natural landmarks

---

### Spawn (20 prefabs)
Player spawn and respawn structures.

```
Server/Prefabs/Spawn/
├── Initial_Spawn/
├── Respawn_Point/
└── Safe_Zone/
```

**Use for:** Player spawning, respawn points, tutorial areas

---

### Testing (154 prefabs)
Development and testing prefabs.

```
Server/Prefabs/Testing/
├── Combat_Arenas/
├── Debug_Structures/
└── Test_Rooms/
```

**Use for:** Testing, development, debugging

---

### Trees (1,061 prefabs)
Tree structures organized by type and growth stage.

```
Server/Prefabs/Trees/
├── Acacia/           # Savanna trees
├── Amber/            # Amber trees
├── Birch/            # Birch trees
├── Cedar/            # Cedar trees
├── Cinnamon/         # Cinnamon trees
├── Coconut/          # Palm trees
├── Dead/             # Dead trees
├── Fir/              # Fir trees
├── Joshua/           # Desert trees
├── Jungle/           # Jungle trees
├── Maple/            # Maple trees
├── Oak/              # Oak trees
├── Palm/             # Palm trees
├── Pine/             # Pine trees
├── Redwood/          # Giant redwoods
├── Spruce/           # Spruce trees
├── Swamp/            # Swamp trees
├── Walnut/           # Walnut trees
└── Willow/           # Willow trees
```

Each tree type has growth stages:
```
Trees/{Type}/
├── Stage_0/          # Sapling/small
├── Stage_1/          # Medium
├── Stage_2/          # Large
└── Stage_3/          # Fully grown
```

**Use for:** Forests, biome variety, harvestable resources

---

### Unique (1 prefab)
One-of-a-kind special structures.

---

## Prefab Counts Summary

| Category | Count | Description |
|----------|-------|-------------|
| Trees | 1,061 | Forest vegetation |
| Rock_Formations | 1,676 | Natural terrain |
| Mineshaft | 1,160 | Underground mines |
| Npc | 856 | NPC structures |
| Monuments | 778 | Surface landmarks |
| Plants | 555 | Flora/vegetation |
| Cave | 492 | Cave systems |
| Dungeon | 800+ | Dungeon rooms |
| Testing | 154 | Dev/test |
| Mineshaft_Drift | 117 | Mine tunnels |
| Spawn | 20 | Player spawn |
| **Total** | **7,600+** | |

---

## Finding Prefabs

### By Zone

Many prefabs are organized by zone compatibility:
- `Zone1/` - Temperate zone prefabs
- `Zone2/` - Desert zone prefabs
- `Zone3/` - Cold zone prefabs
- `Zone4/` - Volcanic zone prefabs

### By Size

Check file names for size hints:
- `_Small_` - Small structures
- `_Medium_` - Medium structures
- `_Large_` - Large structures

### By Purpose

- `Encounter_` - Combat encounters
- `Loot_` - Loot containers
- `Boss_` - Boss arenas
- `Entrance_` - Entry points

---

## Using Prefabs

### In World Generation

Reference in PrefabPatterns:
```json
{
  "Prefabs": [
    {
      "Path": "Trees/Oak/Stage_2/Oak_Stage2_001.prefab.json",
      "Weight": 1
    }
  ]
}
```

### In Interactions

Spawn prefab from interaction:
```json
{
  "Type": "SpawnPrefab",
  "PrefabPath": "Monuments/Shrines/Shrine_Small_001.prefab.json"
}
```

---

## Tips

1. **Check existing prefabs** - Often a suitable prefab exists
2. **Stage variants** - Trees have growth stages
3. **Zone compatibility** - Match prefabs to biomes
4. **Weight balance** - Vary weights for natural distribution
5. **Size awareness** - Check prefab dimensions before placing

---

**Related:** [Prefabs](186_Prefabs.md) | [World Generation](38_World_Generation.md) | [Zone Layers](191_Zone_Layers.md)
