# Farming Coops

Learn how to create coops for animal farming, including resident management, produce drops, and wild animal capture.

## Overview

Coops are block entities that house NPCs (animals) and automatically produce items over time. They manage resident animals, define which NPCs can live in them, handle produce drops, and can automatically capture wild animals in range. Coops are used for animal farming systems like chicken coops, pig pens, and other livestock housing.

## Location
- Coop configurations: `Server/Farming/Coops/`
- Coop items: `Server/Item/Items/Coops/`
- Produce drops: `Server/Drops/NPCs/Livestock/Produce/`

## Coop Configuration

### Basic Coop Structure

`Server/Farming/Coops/Coop_Chicken.json`:

```json
{
  "MaxResidents": 6,
  "ProduceDrops": {
    "Chicken": "Drop_Chicken_Produce",
    "Chicken_Desert": "Drop_Chicken_Produce",
    "Skrill": "Drop_Chicken_Produce"
  },
  "ResidentRoamTime": [6, 18],
  "ResidentSpawnOffset": {
    "X": 0,
    "Y": 0,
    "Z": 3
  },
  "AcceptedNpcGroups": [
    "Chicken",
    "Chicken_Desert",
    "Skrill"
  ],
  "CaptureWildNPCsInRange": true,
  "WildCaptureRadius": 10
}
```

## Coop Properties

### MaxResidents

```json
{
  "MaxResidents": 6
}
```

Maximum number of NPCs that can live in the coop simultaneously.

### ProduceDrops

```json
{
  "ProduceDrops": {
    "Chicken": "Drop_Chicken_Produce",
    "Chicken_Desert": "Drop_Chicken_Produce",
    "Skrill": "Drop_Chicken_Produce"
  }
}
```

Maps NPC IDs to their produce drop tables. When an NPC produces an item, it uses the drop table specified here.

**Properties:**
- Key: NPC ID (must match NPC role ID)
- Value: Drop table ID (file name without `.json`)

**Drop Table Location:** `Server/Drops/NPCs/Livestock/Produce/`

### ResidentRoamTime

```json
{
  "ResidentRoamTime": [6, 18]
}
```

Time range when residents can roam outside the coop (in-game hours).

**Format:** `[StartHour, EndHour]`
- `[6, 18]` - Roam from 6 AM to 6 PM (daytime)
- `[18, 6]` - Roam from 6 PM to 6 AM (nighttime)

### ResidentSpawnOffset

```json
{
  "ResidentSpawnOffset": {
    "X": 0,
    "Y": 0,
    "Z": 3
  }
}
```

Offset from coop center where residents spawn.

**Properties:**
- **`X`** - Horizontal offset (blocks, negative = left, positive = right)
- **`Y`** - Vertical offset (blocks, negative = down, positive = up)
- **`Z`** - Depth offset (blocks, negative = forward, positive = backward)

### AcceptedNpcGroups

```json
{
  "AcceptedNpcGroups": [
    "Chicken",
    "Chicken_Desert",
    "Skrill"
  ]
}
```

List of NPC group IDs that can live in this coop. Only NPCs from these groups will be accepted as residents.

**NPC Groups:** Defined in `Server/NPC/Groups/`

### CaptureWildNPCsInRange

```json
{
  "CaptureWildNPCsInRange": true
}
```

Whether the coop automatically captures wild NPCs within range.

- **`true`** - Automatically captures wild NPCs
- **`false`** - Must manually place NPCs in coop

### WildCaptureRadius

```json
{
  "WildCaptureRadius": 10
}
```

Range in blocks within which wild NPCs will be captured (only if `CaptureWildNPCsInRange: true`).

## Produce Drop Tables

### Basic Produce Drop

`Server/Drops/NPCs/Livestock/Produce/Drop_Chicken_Produce.json`:

```json
{
  "Container": {
    "Type": "Single",
    "Item": {
      "ItemId": "Food_Egg",
      "QuantityMin": 1,
      "QuantityMax": 1
    }
  }
}
```

This produces 1 egg per production cycle.

### Multiple Produce

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Food_Egg",
          "QuantityMin": 1,
          "QuantityMax": 2
        }
      },
      {
        "Type": "Single",
        "Weight": 50.0,
        "Item": {
          "ItemId": "Food_Egg_Golden",
          "QuantityMin": 1,
          "QuantityMax": 1
        }
      }
    ]
  }
}
```

Always produces 1-2 eggs, 50% chance for golden egg.

### Weighted Produce

```json
{
  "Container": {
    "Type": "Choice",
    "Containers": [
      {
        "Type": "Single",
        "Weight": 80,
        "Item": {
          "ItemId": "Food_Egg",
          "QuantityMin": 1,
          "QuantityMax": 1
        }
      },
      {
        "Type": "Single",
        "Weight": 15,
        "Item": {
          "ItemId": "Food_Egg_Golden",
          "QuantityMin": 1,
          "QuantityMax": 1
        }
      },
      {
        "Type": "Single",
        "Weight": 5,
        "Item": {
          "ItemId": "Food_Egg_Diamond",
          "QuantityMin": 1,
          "QuantityMax": 1
        }
      }
    ]
  }
}
```

80% normal egg, 15% golden egg, 5% diamond egg.

See [Drop Tables](105_Drop_Tables.md) for complete drop table documentation.

## Coop Item Configuration

### Basic Coop Item

`Server/Item/Items/Coops/Coop_Chicken.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Coop_Chicken.name",
    "Description": "server.items.Coop_Chicken.description"
  },
  "Categories": ["Items.Farming", "Items.Structures"],
  "Icon": "Icons/ItemsGenerated/Coop_Chicken.png",
  "BlockType": {
    "CustomModel": "Blocks/Farming/ChickenCoop.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/Farming/ChickenCoop_Texture.png"
      }
    ],
    "State": {
      "Id": "coop",
      "Definitions": {
        "default": {
          "CustomModel": "Blocks/Farming/ChickenCoop.blockymodel"
        },
        "withProduce": {
          "CustomModel": "Blocks/Farming/ChickenCoop_WithProduce.blockymodel"
        }
      }
    },
    "BlockEntity": {
      "Components": [
        {
          "Coop": {
            "FarmingCoopId": "Coop_Chicken"
          }
        }
      ]
    }
  }
}
```

### Linking Coop Configuration

The item links to the coop configuration via `BlockEntity.Components`:

```json
{
  "BlockEntity": {
    "Components": [
      {
        "Coop": {
          "FarmingCoopId": "Coop_Chicken"
        }
      }
    ]
  }
}
```

**Properties:**
- **`Coop`** - Coop component type
- **`FarmingCoopId`** - ID of coop configuration (file name without `.json`)

The coop configuration must exist at `Server/Farming/Coops/Coop_Chicken.json`.

### Coop States

Coops can have multiple states for visual feedback:

```json
{
  "State": {
    "Id": "coop",
    "Definitions": {
      "default": {
        "CustomModel": "Blocks/Farming/ChickenCoop.blockymodel"
      },
      "withProduce": {
        "CustomModel": "Blocks/Farming/ChickenCoop_WithProduce.blockymodel"
      }
    }
  }
}
```

**States:**
- **`default`** - Empty or normal state
- **`withProduce`** - Has produce ready to collect

The coop automatically switches states when produce is ready.

## Complete Examples

### Example 1: Simple Chicken Coop

**Coop Configuration:** `Server/Farming/Coops/Coop_Chicken.json`

```json
{
  "MaxResidents": 6,
  "ProduceDrops": {
    "Chicken": "Drop_Chicken_Produce"
  },
  "ResidentRoamTime": [6, 18],
  "ResidentSpawnOffset": {
    "X": 0,
    "Y": 0,
    "Z": 3
  },
  "AcceptedNpcGroups": [
    "Chicken"
  ],
  "CaptureWildNPCsInRange": true,
  "WildCaptureRadius": 10
}
```

**Produce Drop:** `Server/Drops/NPCs/Livestock/Produce/Drop_Chicken_Produce.json`

```json
{
  "Container": {
    "Type": "Single",
    "Item": {
      "ItemId": "Food_Egg",
      "QuantityMin": 1,
      "QuantityMax": 1
    }
  }
}
```

### Example 2: Multi-Species Coop

```json
{
  "MaxResidents": 8,
  "ProduceDrops": {
    "Chicken": "Drop_Chicken_Produce",
    "Chicken_Desert": "Drop_Chicken_Produce",
    "Skrill": "Drop_Chicken_Produce",
    "Duck": "Drop_Duck_Produce"
  },
  "ResidentRoamTime": [6, 18],
  "ResidentSpawnOffset": {
    "X": 0,
    "Y": 0,
    "Z": 3
  },
  "AcceptedNpcGroups": [
    "Chicken",
    "Chicken_Desert",
    "Skrill",
    "Duck"
  ],
  "CaptureWildNPCsInRange": true,
  "WildCaptureRadius": 15
}
```

Accepts multiple bird types, each with their own produce.

### Example 3: Pig Pen

```json
{
  "MaxResidents": 4,
  "ProduceDrops": {
    "Pig": "Drop_Pig_Produce"
  },
  "ResidentRoamTime": [6, 20],
  "ResidentSpawnOffset": {
    "X": 0,
    "Y": 0,
    "Z": 2
  },
  "AcceptedNpcGroups": [
    "Pig"
  ],
  "CaptureWildNPCsInRange": false,
  "WildCaptureRadius": 5
}
```

Pig pen with manual placement only, smaller spawn area.

### Example 4: Advanced Produce System

**Produce Drop:** `Server/Drops/NPCs/Livestock/Produce/Drop_Chicken_Produce.json`

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Food_Egg",
          "QuantityMin": 1,
          "QuantityMax": 2
        }
      },
      {
        "Type": "Choice",
        "Weight": 100,
        "Containers": [
          {
            "Type": "Single",
            "Weight": 70,
            "Item": {
              "ItemId": "Ingredient_Feather",
              "QuantityMin": 1,
              "QuantityMax": 2
            }
          },
          {
            "Type": "Single",
            "Weight": 25,
            "Item": {
              "ItemId": "Food_Egg_Golden",
              "QuantityMin": 1,
              "QuantityMax": 1
            }
          },
          {
            "Type": "Single",
            "Weight": 5,
            "Item": {
              "ItemId": "Food_Egg_Diamond",
              "QuantityMin": 1,
              "QuantityMax": 1
            }
          }
        ]
      }
    ]
  }
}
```

Always produces 1-2 eggs, plus 70% feathers (1-2), 25% golden egg, or 5% diamond egg.

## Resident Management

### How Residents Work

1. **Placement:** NPCs can be placed manually or captured automatically (if enabled)
2. **Roaming:** Residents roam during `ResidentRoamTime` hours
3. **Production:** Each resident produces items based on their `ProduceDrops` entry
4. **Capacity:** Maximum residents limited by `MaxResidents`
5. **Spawn:** New residents spawn at `ResidentSpawnOffset` relative to coop center

### Capture Behavior

If `CaptureWildNPCsInRange: true`:
- Coop automatically captures wild NPCs within `WildCaptureRadius`
- Only NPCs from `AcceptedNpcGroups` are captured
- Captured NPCs become residents

If `CaptureWildNPCsInRange: false`:
- Must manually place NPCs in coop
- Only NPCs from `AcceptedNpcGroups` can be placed

## Tips for Farming Coops

1. **MaxResidents** - Balance capacity with coop size (4-8 typical)
2. **ProduceDrops** - Create separate drop tables for each NPC type
3. **ResidentRoamTime** - Match to NPC behavior (daytime animals [6, 18])
4. **ResidentSpawnOffset** - Position inside coop bounds, adjust Z for spacing
5. **AcceptedNpcGroups** - Match to NPC groups in `Server/NPC/Groups/`
6. **CaptureWildNPCsInRange** - Enable for automatic farming, disable for manual control
7. **WildCaptureRadius** - Larger radius = easier capture, but may catch unwanted NPCs
8. **Produce variety** - Use weighted drop tables for rare produce items
9. **State management** - Use block states to show produce ready state
10. **Multiple species** - Support multiple NPC types with different produce drops

## Coop States and Interactions

Coops can have interactions for collecting produce:

```json
{
  "Interactions": {
    "Use": {
      "Interactions": [
        {
          "Type": "Condition",
          "RequiredState": "withProduce",
          "Next": {
            "Interactions": [
              {
                "Type": "ChangeState",
                "Changes": {
                  "withProduce": "default"
                }
              },
              {
                "Type": "SpawnDrops",
                "DropList": "Drop_Chicken_Produce"
              }
            ]
          }
        }
      ]
    }
  }
}
```

When player interacts:
1. Checks if state is `withProduce`
2. Changes state back to `default`
3. Spawns produce drops

## Linking Everything Together

1. **Create Coop Configuration** - `Server/Farming/Coops/Coop_Chicken.json`
2. **Create Produce Drop Tables** - `Server/Drops/NPCs/Livestock/Produce/Drop_Chicken_Produce.json`
3. **Create Coop Item** - `Server/Item/Items/Coops/Coop_Chicken.json`
4. **Link in Item** - Set `BlockEntity.Components[].Coop.FarmingCoopId` to coop ID
5. **Define NPC Groups** - Ensure NPCs are in groups listed in `AcceptedNpcGroups`

---

**Previous:** [Entity UI Configuration](106_Entity_UI_Configuration.md) | **Next:** [Item Sound Sets](108_Item_Sound_Sets.md)
