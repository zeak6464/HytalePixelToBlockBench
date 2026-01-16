# Dungeons & Structures

Learn how to create prefabs for dungeons, buildings, and world structures.

## Overview

Prefabs are JSON files that define multi-block structures that can be placed in the world. They're used for dungeons, buildings, trees, and other complex structures.

## Location
`Server/Prefabs/`

## Basic Prefab Structure

Create `Server/Prefabs/MyCustom/MyCustom_Structure.prefab.json`:

```json
{
  "version": 8,
  "blockIdVersion": 0,
  "anchorX": 0,
  "anchorY": 0,
  "anchorZ": 0,
  "blocks": [
    {
      "x": 0,
      "y": 0,
      "z": 0,
      "name": "Rock_Stone",
      "rotation": 0
    },
    {
      "x": 0,
      "y": 1,
      "z": 0,
      "name": "Rock_Stone",
      "rotation": 0
    },
    {
      "x": 1,
      "y": 0,
      "z": 0,
      "name": "Rock_Stone",
      "rotation": 0
    }
  ]
}
```

## Prefab Properties

### Header

```json
{
  "version": 8,
  "blockIdVersion": 0,
  "anchorX": 0,
  "anchorY": 0,
  "anchorZ": 0
}
```

- **`version`** - Prefab format version (typically 8)
- **`blockIdVersion`** - Block ID version (typically 0)
- **`anchorX/Y/Z`** - Anchor point (origin) for the prefab (usually 0, 0, 0)

### Block Entries

Each block in the prefab:

```json
{
  "x": 0,        // X coordinate (relative to anchor)
  "y": 1,        // Y coordinate (relative to anchor)
  "z": 0,        // Z coordinate (relative to anchor)
  "name": "Rock_Stone",  // Block/item ID
  "rotation": 0,         // Rotation (0-3 for NESW)
  "components": {        // Optional: block components
    "Components": {
      "container": {
        "Droplist": "Drop_MyCustom_Chest"
      }
    }
  }
}
```

## Block Components

Blocks in prefabs can have components for special functionality:

### Container Component (Chests)

```json
{
  "x": 0,
  "y": 0,
  "z": 0,
  "name": "Furniture_Crude_Chest_Small",
  "components": {
    "Components": {
      "container": {
        "Position": {
          "X": 0,
          "Y": 0,
          "Z": 0
        },
        "Custom": false,
        "AllowViewing": true,
        "Droplist": "Drop_MyCustom_Chest",
        "ItemContainer": {
          "Capacity": 18,
          "Items": {}
        }
      }
    }
  },
  "rotation": 1
}
```

See [Special Items](08_Special_Items.md) for more on chests in prefabs.

### NPC Spawner Component

```json
{
  "components": {
    "Components": {
      "npcSpawner": {
        "RoleId": "Goblin_Scrapper",
        "SpawnDelay": 0
      }
    }
  }
}
```

## Complete Example: Simple Dungeon Room

```json
{
  "version": 8,
  "blockIdVersion": 0,
  "anchorX": 0,
  "anchorY": 0,
  "anchorZ": 0,
  "blocks": [
    // Floor (Y=0)
    { "x": 0, "y": 0, "z": 0, "name": "Rock_Stone", "rotation": 0 },
    { "x": 1, "y": 0, "z": 0, "name": "Rock_Stone", "rotation": 0 },
    { "x": 2, "y": 0, "z": 0, "name": "Rock_Stone", "rotation": 0 },
    { "x": 0, "y": 0, "z": 1, "name": "Rock_Stone", "rotation": 0 },
    { "x": 1, "y": 0, "z": 1, "name": "Rock_Stone", "rotation": 0 },
    { "x": 2, "y": 0, "z": 1, "name": "Rock_Stone", "rotation": 0 },
    { "x": 0, "y": 0, "z": 2, "name": "Rock_Stone", "rotation": 0 },
    { "x": 1, "y": 0, "z": 2, "name": "Rock_Stone", "rotation": 0 },
    { "x": 2, "y": 0, "z": 2, "name": "Rock_Stone", "rotation": 0 },
    
    // Walls (Y=1, Y=2)
    { "x": 0, "y": 1, "z": 0, "name": "Rock_Stone", "rotation": 0 },
    { "x": 0, "y": 2, "z": 0, "name": "Rock_Stone", "rotation": 0 },
    { "x": 2, "y": 1, "z": 0, "name": "Rock_Stone", "rotation": 0 },
    { "x": 2, "y": 2, "z": 0, "name": "Rock_Stone", "rotation": 0 },
    { "x": 0, "y": 1, "z": 2, "name": "Rock_Stone", "rotation": 0 },
    { "x": 0, "y": 2, "z": 2, "name": "Rock_Stone", "rotation": 0 },
    { "x": 2, "y": 1, "z": 2, "name": "Rock_Stone", "rotation": 0 },
    { "x": 2, "y": 2, "z": 2, "name": "Rock_Stone", "rotation": 0 },
    
    // Chest with loot
    {
      "x": 1,
      "y": 0,
      "z": 1,
      "name": "Furniture_Crude_Chest_Small",
      "components": {
        "Components": {
          "container": {
            "Position": { "X": 1, "Y": 0, "Z": 1 },
            "Droplist": "Drop_Dungeon_Chest",
            "ItemContainer": {
              "Capacity": 18,
              "Items": {}
            }
          }
        }
      },
      "rotation": 1
    }
  ]
}
```

## Example: Prefab with Multiple Components

```json
{
  "version": 8,
  "blockIdVersion": 0,
  "anchorX": 0,
  "anchorY": 0,
  "anchorZ": 0,
  "blocks": [
    {
      "x": 0,
      "y": 0,
      "z": 0,
      "name": "Furniture_Crude_Chest_Small",
      "components": {
        "Components": {
          "container": {
            "Droplist": "Drop_Treasure_Chest",
            "ItemContainer": {
              "Capacity": 18,
              "Items": {}
            }
          }
        }
      },
      "rotation": 0
    },
    {
      "x": 2,
      "y": 0,
      "z": 2,
      "name": "Rock_Stone",
      "components": {
        "Components": {
          "npcSpawner": {
            "RoleId": "Goblin_Scrapper",
            "SpawnDelay": 5
          }
        }
      }
    }
  ]
}
```

## Prefab Organization

Organize prefabs by category:

```
Server/Prefabs/
├── Dungeon/
│   ├── Dungeon_Room_01.prefab.json
│   ├── Dungeon_Corridor.prefab.json
│   └── Dungeon_Boss_Room.prefab.json
├── Buildings/
│   ├── House_Village.prefab.json
│   └── Tower.prefab.json
└── Trees/
    ├── Oak_Stage3_001.prefab.json
    └── Oak_Stage3_002.prefab.json
```

## Coordinate System

- **X** - East/West (positive = east)
- **Y** - Up/Down (positive = up)
- **Z** - North/South (positive = south)

All coordinates are relative to the anchor point.

## Block Rotation

Blocks can be rotated when placed:
- `0` - North
- `1` - East
- `2` - South
- `3` - West

## Tips for Creating Prefabs

1. **Start small** - Create simple structures first
2. **Use anchor point** - Set (0, 0, 0) as the base point
3. **Organize by category** - Group related prefabs in folders
4. **Add components** - Use containers and spawners for functionality
5. **Test placement** - Verify prefabs place correctly in game
6. **Use existing blocks** - Reference existing block/item IDs
7. **Plan structure** - Sketch out structure before creating prefab

---

**Previous:** [Advanced Item Interactions](21_Advanced_Item_Interactions.md)

---

*This completes the modding guide series! For more information, refer to the specific guides for each topic.*
