# Portal Types

Learn how to configure portal destinations and portal instances for fast travel.

## Overview

Portal types define specific portal destinations and their properties. They're separate from portal block items and configure where portals lead, spawn points, and instance settings. Portal types are defined in `Server/PortalTypes/` and referenced by portal blocks.

## Location
`Server/PortalTypes/`

## Example from Game Files

### Henges Portal

From `Server/PortalTypes/Henges.json`:

```1:15:Server/PortalTypes/Henges.json
{
  "InstanceId": "Portals_Henges",
  "Description": {
    "DisplayName": "server.portals.henges",
    "FlavorText": "server.portals.henges.description",
    "ThemeColor": "#81fdedff",
    "SplashImage": "DefaultArtwork.png"
  },
  "PlayerSpawn": {
    "Y": 120,
    "ScanHeight": 20,
    "MinRadius": 250,
    "MaxRadius": 250
  }
}
```

This shows a portal type configuration with instance ID, description, and player spawn settings.

## Basic Portal Type Structure

Create `Server/PortalTypes/MyCustom_Destination.json`:

```json
{
  "InstanceId": "MyCustom_Instance",
  "Description": {
    "DisplayName": "server.portals.myCustom.title",
    "FlavorText": "server.portals.myCustom.description",
    "ThemeColor": "#81fdedff",
    "SplashImage": "DefaultArtwork.png"
  },
  "PlayerSpawn": {
    "Y": 120,
    "ScanHeight": 20,
    "MinRadius": 250,
    "MaxRadius": 250
  }
}
```

## Portal Type Properties

### InstanceId

```json
{
  "InstanceId": "MyCustom_Instance"
}
```

The server instance ID where the portal leads (from `Server/Instances/`).

### Description

```json
{
  "Description": {
    "DisplayName": "server.portals.myCustom.title",
    "FlavorText": "server.portals.myCustom.description",
    "ThemeColor": "#81fdedff",
    "SplashImage": "DefaultArtwork.png"
  }
}
```

**Properties:**
- **`DisplayName`** - Translation key for portal name
- **`FlavorText`** - Translation key for portal description
- **`ThemeColor`** - UI theme color (hex format with alpha)
- **`SplashImage`** - Image shown when entering portal

### PlayerSpawn

```json
{
  "PlayerSpawn": {
    "Y": 120,
    "ScanHeight": 20,
    "MinRadius": 250,
    "MaxRadius": 250
  }
}
```

**Properties:**
- **`Y`** - Target Y coordinate (height)
- **`ScanHeight`** - Vertical range to search for valid spawn (above/below Y)
- **`MinRadius`** - Minimum distance from spawn point
- **`MaxRadius`** - Maximum distance from spawn point

## Common Portal Types

### Henges Portal

`Server/PortalTypes/Henges.json`:

```json
{
  "InstanceId": "Portals_Henges",
  "Description": {
    "DisplayName": "server.portals.henges",
    "FlavorText": "server.portals.henges.description",
    "ThemeColor": "#81fdedff",
    "SplashImage": "DefaultArtwork.png"
  },
  "PlayerSpawn": {
    "Y": 120,
    "ScanHeight": 20,
    "MinRadius": 250,
    "MaxRadius": 250
  }
}
```

### Jungles Portal

`Server/PortalTypes/Jungles.json`:

```json
{
  "InstanceId": "Portals_Jungles",
  "Description": {
    "DisplayName": "server.portals.jungles",
    "FlavorText": "server.portals.jungles.description",
    "ThemeColor": "#4a7c59ff",
    "SplashImage": "JunglesArtwork.png"
  },
  "PlayerSpawn": {
    "Y": 150,
    "ScanHeight": 30,
    "MinRadius": 200,
    "MaxRadius": 300
  }
}
```

### Taiga Portal

`Server/PortalTypes/Taiga.json`:

```json
{
  "InstanceId": "Portals_Taiga",
  "Description": {
    "DisplayName": "server.portals.taiga",
    "FlavorText": "server.portals.taiga.description",
    "ThemeColor": "#a8c5d0ff",
    "SplashImage": "TaigaArtwork.png"
  },
  "PlayerSpawn": {
    "Y": 100,
    "ScanHeight": 25,
    "MinRadius": 250,
    "MaxRadius": 250
  }
}
```

## Using Portal Types

### In Portal Block Items

Portal blocks reference portal types through instance configuration:

```json
{
  "BlockType": {
    "BlockEntity": {
      "Components": {
        "Portal": {
          "Config": {
            "OnState": "Active",
            "SpawningState": "Spawning",
            "ReturnBlockType": "Portal_Return",
            "PortalTypeId": "MyCustom_Destination"
          }
        }
      }
    }
  }
}
```

### In Portal Device UI

Portal devices can display available portal types:

```json
{
  "Interaction": {
    "Type": "OpenCustomUI",
    "Page": {
      "Id": "PortalDevice",
      "Config": {
        "PortalTypeId": "MyCustom_Destination",
        "OnState": "Active"
      }
    }
  }
}
```

## Complete Example: Custom Portal Type

### 1. Portal Type Definition

`Server/PortalTypes/MyCustom_Dungeon.json`:

```json
{
  "InstanceId": "Dungeon_MyCustom",
  "Description": {
    "DisplayName": "server.portals.myCustomDungeon.title",
    "FlavorText": "server.portals.myCustomDungeon.description",
    "ThemeColor": "#8b4513ff",
    "SplashImage": "DungeonArtwork.png"
  },
  "PlayerSpawn": {
    "Y": 80,
    "ScanHeight": 15,
    "MinRadius": 100,
    "MaxRadius": 200
  }
}
```

### 2. Create Instance

`Server/Instances/Dungeon_MyCustom/config.json`:

```json
{
  "Version": 4,
  "Seed": 123456789,
  "SpawnProvider": {
    "Id": "Global",
    "SpawnPoint": {
      "X": 0.0,
      "Y": 80.0,
      "Z": 0.0
    }
  },
  "WorldGen": {
    "Type": "Flat",
    "Layers": [
      {
        "BlockType": "Rock_Stone",
        "Environment": "Env_Zone1_Plains",
        "To": 80,
        "From": 0
      }
    ]
  },
  "GameMode": "Adventure",
  "GameplayConfig": "Default",
  "IsPvpEnabled": false
}
```

### 3. Create Portal Block

`Server/Item/Items/Portal/Portal_MyCustom_Dungeon.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Portal_MyCustom_Dungeon.name"
  },
  "Categories": ["Blocks.Portals"],
  "BlockType": {
    "DrawType": "Model",
    "Material": "Solid",
    "Opacity": "Transparent",
    "CustomModel": "Blocks/Miscellaneous/Platform_Magic_Exit.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/Miscellaneous/Platform_Magic_Blue2.png",
        "Weight": 1
      }
    ],
    "HitboxType": "Pad_Portal",
    "BlockEntity": {
      "Components": {
        "Portal": {
          "Config": {
            "OnState": "Active",
            "SpawningState": "Spawning",
            "ReturnBlockType": "Portal_Return",
            "PortalTypeId": "MyCustom_Dungeon"
          }
        }
      }
    },
    "Particles": [
      {
        "SystemId": "MagicPortal_VoidKeyArt",
        "PositionOffset": {
          "Y": 2.5
        }
      }
    ],
    "Interactions": {
      "CollisionEnter": {
        "Interactions": [
          {
            "Type": "Portal"
          }
        ]
      }
    }
  },
  "Tags": {
    "Type": ["Portal"]
  }
}
```

## Portal Type vs Portal Blocks

- **Portal Types** - Define destinations and spawn settings
- **Portal Blocks** - Placeable blocks that activate portals
- **Instances** - Actual server worlds/instances portals lead to

## Tips for Creating Portal Types

1. **Create instance first** - Portal types reference instances that must exist
2. **Set spawn correctly** - Use appropriate Y, scan height, and radius for the destination
3. **Match theme colors** - Use colors that match the destination biome/environment
4. **Add translations** - Define display names and descriptions in language files
5. **Test spawn points** - Ensure spawn areas are safe and accessible
6. **Use splash images** - Custom artwork enhances portal experience
7. **Link in portal blocks** - Reference portal types in portal block configurations

---

**Previous:** [Scripted Brushes](45_Scripted_Brushes.md) | **Next:** [Word Lists](47_Word_Lists.md)
