# Creating Furniture

Learn how to create decorative furniture items like chairs, tables, beds, and other placeable objects.

## Overview

Furniture items are placeable blocks that provide decoration and sometimes functionality (like seating, storage, or lighting).

## Location
`Server/Item/Items/Furniture/{Style}/`

## Example from Game Files

### Village Chair

From `Server/Item/Items/Furniture/Village/Furniture_Village_Chair.json`:

```1:73:Server/Item/Items/Furniture/Village/Furniture_Village_Chair.json
{
  "TranslationProperties": {
    "Name": "server.items.Furniture_Village_Chair.name"
  },
  "Icon": "Icons/ItemsGenerated/Furniture_Village_Chair.png",
  "Categories": [
    "Furniture.Furniture"
  ],
  "Interactions": {
    "Primary": "Block_Primary",
    "Secondary": "Block_Secondary"
  },
  "Recipe": {
    "Input": [
      {
        "ResourceTypeId": "Wood_Hardwood",
        "Quantity": 2
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Builders",
        "Type": "StructuralCrafting",
        "Categories": [
          "Chair"
        ]
      }
    ]
  },
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Model",
    "Opacity": "Transparent",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/Decorative_Sets/Village/Chair_Texture.png",
        "Weight": 1
      }
    ],
    "Group": "Wood",
    "HitboxType": "Chair",
    "VariantRotation": "NESW",
    "Seats": [
      {
        "Offset": {
          "X": 0,
          "Y": 0,
          "Z": 0.2
        },
        "Yaw": 0
      }
    ],
    "Interactions": {
      "Use": "Block_Seat"
    },
    "Flags": {},
    "Gathering": {
      "Breaking": {
        "GatherType": "Woods"
      }
    },
    "BlockParticleSetId": "Wood",
    "Support": {
      "Down": [
        {
          "FaceType": "Full"
        }
      ]
    },
    "CustomModel": "Blocks/Decorative_Sets/Village/Chair.blockymodel",
    "ParticleColor": "#5b3222",
    "BlockSoundSetId": "Wood"
  },
```

This shows a complete furniture item with seating functionality, recipe, and block configuration.

## Basic Furniture Structure

Create `Server/Item/Items/Furniture/MyCustom/Furniture_MyCustom_Chair.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Furniture_MyCustom_Chair.name"
  },
  "Icon": "Icons/ItemsGenerated/Furniture_MyCustom_Chair.png",
  "Categories": ["Furniture.Furniture"],
  "Interactions": {
    "Primary": "Block_Primary",
    "Secondary": "Block_Secondary"
  },
  "Recipe": {
    "Input": [
      {
        "ResourceTypeId": "Wood_Hardwood",
        "Quantity": 2
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Builders",
        "Type": "StructuralCrafting",
        "Categories": ["Chair"]
      }
    ]
  },
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Model",
    "Opacity": "Transparent",
    "CustomModel": "Blocks/Decorative_Sets/MyCustom/Chair.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/Decorative_Sets/MyCustom/Chair_Texture.png",
        "Weight": 1
      }
    ],
    "Group": "Wood",
    "HitboxType": "Chair",
    "VariantRotation": "NESW",
    "Seats": [
      {
        "Offset": {
          "X": 0,
          "Y": 0,
          "Z": 0.2
        },
        "Yaw": 0
      }
    ],
    "Interactions": {
      "Use": "Block_Seat"
    },
    "Gathering": {
      "Breaking": {
        "GatherType": "Woods"
      }
    },
    "Support": {
      "Down": [
        {
          "FaceType": "Full"
        }
      ]
    },
    "BlockParticleSetId": "Wood",
    "BlockSoundSetId": "Wood",
    "ParticleColor": "#5b3222"
  },
  "PlayerAnimationsId": "Block",
  "Tags": {
    "Type": ["Furniture"],
    "Family": ["MyCustom"]
  },
  "ItemSoundSetId": "ISS_Blocks_Wood"
}
```

## Seating Furniture

Chairs and benches can be sat on by players.

### Seat Configuration

```json
"Seats": [
  {
    "Offset": {
      "X": 0,
      "Y": 0,
      "Z": 0.2
    },
    "Yaw": 0
  }
]
```

- **`Offset`** - Position relative to block center (Y is up)
- **`Yaw`** - Rotation angle (0-360 degrees)

### Seating Interaction

```json
"Interactions": {
  "Use": "Block_Seat"
}
```

Players right-click to sit on the furniture.

## Furniture Types

### Chair

```json
{
  "HitboxType": "Chair",
  "Seats": [
    {
      "Offset": { "X": 0, "Y": 0, "Z": 0.2 },
      "Yaw": 0
    }
  ]
}
```

### Table

```json
{
  "HitboxType": "Table",
  "Categories": ["Furniture.Tables"]
}
```

### Bed

Beds use style-specific HitboxTypes (e.g., `"Bed_Village"`, `"Bed_Crude"`):

```json
{
  "HitboxType": "Bed_Village",
  "Beds": [
    {
      "Offset": { "X": 0, "Y": 0.5, "Z": 0 }
    }
  ],
  "Interactions": {
    "Use": {
      "Type": "Bed"
    }
  }
}
```

### Chest/Storage

Chests are covered in [Special Items](08_Special_Items.md).

### Light Source

Light sources use `Radius` (not Range) and `Color`. From `Furniture_Lumberjack_Lamp.json`:

```json
{
  "BlockType": {
    "Light": {
      "Color": "#edd",
      "Radius": 0
    },
    "Interactions": {
      "Use": {
        "Interactions": [
          {
            "Type": "ChangeState",
            "Changes": {
              "default": "Off",
              "On": "Off",
              "Off": "On"
            }
          }
        ]
      }
    },
    "State": {
      "Definitions": {
        "On": {
          "AmbientSoundEventId": "SFX_Torch_Default_Loop"
        },
        "Off": {
          "Light": null,
          "InteractionHint": "server.interactionHints.turnon"
        }
      }
    }
  }
}
```

## Hitbox Types

| HitboxType | Description |
|------------|-------------|
| `Chair` | Single-seat chair |
| `Table` | Table surface |
| `Bed_Village` | Village-style bed |
| `Bed_Crude` | Crude-style bed |
| `Lamp_Tall_Lumberjack` | Lumberjack lamp |

> **Note:** HitboxTypes are typically style-specific (e.g., `Bed_Village` not just `Bed`).

## Rotation Support

Furniture can be rotated when placed:

```json
"VariantRotation": "NESW"  // 4 directions (North, East, South, West)
```

> **Note:** Only `"NESW"` is used in game files. 8-directional rotation is not currently available.

## Furniture Crafting

Furniture is typically crafted at the `Builders` bench:

```json
"Recipe": {
  "Input": [
    {
      "ResourceTypeId": "Wood_Hardwood",
      "Quantity": 2
    }
  ],
  "BenchRequirement": [
    {
      "Id": "Builders",
      "Type": "StructuralCrafting",
      "Categories": ["Chair"]
    }
  ]
}
```

### Builders Bench Categories

- `Chair` - Chairs and stools
- `Table` - Tables
- `Bed` - Beds
- `Shelf` - Shelves and cabinets
- `Door` - Doors
- `Window` - Windows

## Resource Types

Furniture can provide resource types when broken:

```json
"ResourceTypes": [
  {
    "Id": "Fuel"
  },
  {
    "Id": "Charcoal"
  }
]
```

## Furniture Groups

Group furniture by material for connected block rules:

```json
"Group": "Wood"
```

Groups:
- `Wood` - Wooden furniture
- `Stone` - Stone furniture
- `Metal` - Metal furniture

## Complete Examples

### Simple Chair

```json
{
  "TranslationProperties": {
    "Name": "server.items.Furniture_MyCustom_Chair.name"
  },
  "Icon": "Icons/ItemsGenerated/Furniture_MyCustom_Chair.png",
  "Categories": ["Furniture.Furniture"],
  "BlockType": {
    "CustomModel": "Blocks/Decorative_Sets/MyCustom/Chair.blockymodel",
    "HitboxType": "Chair",
    "VariantRotation": "NESW",
    "Seats": [
      {
        "Offset": { "X": 0, "Y": 0, "Z": 0.2 },
        "Yaw": 0
      }
    ],
    "Interactions": {
      "Use": "Block_Seat"
    },
    "Gathering": {
      "Breaking": {
        "GatherType": "Woods"
      }
    }
  },
  "Tags": {
    "Type": ["Furniture"]
  }
}
```

### Lighting Furniture

```json
{
  "BlockType": {
    "CustomModel": "Blocks/Decorative_Sets/MyCustom/Lantern.blockymodel",
    "Light": {
      "Color": "#ffaa00",
      "Intensity": 0.8,
      "Range": 8
    },
    "Interactions": {
      "Use": "Block_ToggleLight"
    }
  }
}
```

## Tips for Creating Furniture

1. **Define seating positions** - For chairs, set appropriate `Seats` offsets
2. **Use correct hitbox types** - Match hitbox to furniture function
3. **Enable rotation** - Use `VariantRotation` for placement flexibility
4. **Add support requirements** - Furniture needs `Support.Down` configuration
5. **Set appropriate groups** - Use `Group` for connected block systems
6. **Define gathering** - Set what tool is needed to break the furniture
7. **Add light sources** - Use `Light` for lanterns and torches

---

**Previous:** [Plants & Farming](15_Plants_and_Farming.md) | **Next:** [Traps](17_Traps.md)
