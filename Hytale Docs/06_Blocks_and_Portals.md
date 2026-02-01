# Creating Blocks & Portals

Learn how to create placeable blocks and fast travel portals.

## Creating Blocks

### Overview

Blocks are placeable world objects. They can be simple cubes or complex models with interactions.

### Location
`Server/Item/Items/Rock/` (or other block categories)

## Example from Game Files

### Stone Block

From `Server/Item/Items/Rock/Stone/Rock_Stone.json`:

```1:61:Server/Item/Items/Rock/Stone/Rock_Stone.json
{
  "TranslationProperties": {
    "Name": "server.items.Rock_Stone.name"
  },
  "ItemLevel": 10,
  "MaxStack": 100,
  "Icon": "Icons/ItemsGenerated/Rock_Stone.png",
  "Categories": [
    "Blocks.Rocks"
  ],
  "PlayerAnimationsId": "Block",
  "Set": "Rock_Stone",
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Cube",
    "Group": "Stone",
    "Flags": {},
    "Gathering": {
      "Breaking": {
        "GatherType": "Rocks",
        "ItemId": "Rock_Stone_Cobble"
      }
    },
    "BlockParticleSetId": "Stone",
    "Textures": [
      {
        "All": "BlockTextures/Rock_Stone.png",
        "Weight": 2
      },
      {
        "All": "BlockTextures/Rock_Stone_2.png",
        "Weight": 1
      },
      {
        "All": "BlockTextures/Rock_Stone_3.png",
        "Weight": 1
      }
    ],
    "ParticleColor": "#737055",
    "BlockSoundSetId": "Stone",
    "Aliases": ["stone", "stone00"],
    "BlockBreakingDecalId": "Breaking_Decals_Rock"
  },
  "ResourceTypes": [
    { "Id": "Rock" },
    { "Id": "Rock_Stone" }
  ],
  "Tags": {
    "Type": ["Rock"]
  },
  "ItemSoundSetId": "ISS_Blocks_Stone"
}
```

This shows a complete block definition with textures, particle sets, sound sets, and gathering properties.

### Portal Type Configuration

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

This shows a portal type configuration with spawn settings and display information.

### Basic Block Structure

Create `Server/Item/Items/Rock/MyCustom/MyCustom_Block.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.MyCustom_Block.name"
  },
  "ItemLevel": 10,
  "MaxStack": 100,
  "Icon": "Icons/ItemsGenerated/MyCustom_Block.png",
  "Categories": ["Blocks.Custom"],
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Cube",
    "Textures": [
      {
        "All": "BlockTextures/MyCustom_Block.png",
        "Weight": 1
      }
    ],
    "BlockParticleSetId": "Stone",
    "BlockSoundSetId": "Stone",
    "ParticleColor": "#737055"
  },
  "Tags": {
    "Type": ["Rock"]
  }
}
```

### Textured Block (Cube)

```json
{
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Cube",
    "Textures": [
      {
        "All": "BlockTextures/Stone.png",
        "Weight": 2
      },
      {
        "All": "BlockTextures/Stone_2.png",
        "Weight": 1
      }
    ],
    "BlockParticleSetId": "Stone",
    "BlockSoundSetId": "Stone"
  }
}
```

### Model Block

```json
{
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Model",
    "CustomModel": "Blocks/MyCustom/Block.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/MyCustom/Block_Texture.png",
        "Weight": 1
      }
    ],
    "BlockParticleSetId": "Wood",
    "BlockSoundSetId": "Wood"
  }
}
```

### Block Breaking Properties

```json
{
  "BlockType": {
    "Gathering": {
      "Breaking": {
        "GatherType": "Rocks",
        "ItemId": "Rock_Stone_Cobble"
      }
    }
  }
}
```

**GatherTypes:**
- `Rocks` - Requires pickaxe
- `Woods` - Requires axe
- `SoftBlocks` - Can break with hands
- `Unbreakable` - Cannot be broken

### Block Variants

```json
{
  "BlockType": {
    "VariantRotation": "NESW",
    "ConnectedBlockRuleSet": {
      "Type": "CustomTemplate",
      "TemplateShapeAssetId": "ChestConnectedBlockTemplate",
      "TemplateShapeBlockPatterns": {
        "Default": "Chest_Small",
        "Double": "Chest_Large"
      }
    }
  }
}
```

---

## Creating Portals

### Overview

Portals allow fast travel between locations. They can be simple return portals or complex configurable portals.

### Location
`Server/Item/Items/Portal/`

### Return Portal (Simple)

Create `Server/Item/Items/Portal/Portal_MyReturn.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Portal_MyReturn.name"
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
            "Type": "PortalReturn",
            "Next": {
              "Type": "Simple",
              "Effects": {
                "LocalSoundEventId": "SFX_Portal_Neutral_Teleport_Local"
              }
            }
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

### Configurable Portal Device

```json
{
  "BlockType": {
    "BlockEntity": {
      "Components": {
        "Portal": {
          "Config": {
            "OnState": "Active",
            "SpawningState": "Spawning",
            "ReturnBlockType": "Portal_Return"
          }
        }
      }
    },
    "State": {
      "Definitions": {
        "Spawning": {
          "Particles": [
            {
              "SystemId": "Portal_Going_Through_Blue",
              "PositionOffset": { "Y": 1 }
            }
          ],
          "CustomModelAnimation": "Blocks/Miscellaneous/Platform_Magic_Activate.blockyanim"
        },
        "Active": {
          "AmbientSoundEventId": "SFX_Portal_Neutral",
          "Particles": [
            {
              "SystemId": "MagicPortal",
              "PositionOffset": { "Y": 2.0 }
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
        }
      }
    },
    "Interactions": {
      "Use": {
        "Interactions": [
          {
            "Type": "OpenCustomUI",
            "Page": {
              "Id": "PortalDevice",
              "Config": {
                "OnState": "Active",
                "SpawningState": "Spawning",
                "ReturnBlockType": "Portal_Return"
              }
            }
          }
        ]
      }
    }
  }
}
```

### Portal Types

- **Return Portal** - Returns to last spawn point (`Type: "PortalReturn"`)
- **Configurable Portal** - Opens UI to set destination (`Type: "Portal"`)
- **Portal Device** - Advanced portal with states and configuration

---

**Previous:** [Trading & Quests](05_Trading_and_Quests.md) | **Next:** [Creating Spells](07_Spells.md)
