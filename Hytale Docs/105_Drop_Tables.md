# Drop Tables

Learn how to create comprehensive drop tables for NPCs, blocks, crops, and prefabs using the container system.

## Overview

Drop tables define what items entities drop when killed, broken, or harvested. They use a flexible container system with weighted choices, multiple items, quantity ranges, and reusable droplists. Drop tables are used for NPCs, blocks, crops, prefabs, objectives, and more.

## Location
`Server/Drops/`

## Example from Game Files

### Goblin Scrapper Drop Table

From `Server/Drops/NPCs/Intelligent/Goblin/Drop_Goblin_Scrapper.json`:

```1:33:Server/Drops/NPCs/Intelligent/Goblin/Drop_Goblin_Scrapper.json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Choice",
        "Weight": 15,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Weapon_Mace_Scrap"
            }
          }
        ]
      },
      {
        "Type": "Choice",
        "Weight": 100,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Ingredient_Fabric_Scrap_Linen",
              "QuantityMin": 1,
              "QuantityMax": 3
            }
          }
        ]
      }
    ]
  }
}
```

This shows a drop table with multiple containers using Choice type with weighted drops, including quantity ranges.

### Goblin Scrapper Drop Table

From `Server/Drops/NPCs/Intelligent/Goblin/Drop_Goblin_Scrapper.json`:

```1:33:Server/Drops/NPCs/Intelligent/Goblin/Drop_Goblin_Scrapper.json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Choice",
        "Weight": 15,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Weapon_Mace_Scrap"
            }
          }
        ]
      },
      {
        "Type": "Choice",
        "Weight": 100,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Ingredient_Fabric_Scrap_Linen",
              "QuantityMin": 1,
              "QuantityMax": 3
            }
          }
        ]
      }
    ]
  }
}
```

This shows a drop table with weighted choices - a weapon with 15% chance and fabric scraps with 100% chance (guaranteed).

Drop tables are organized by category:
- **NPCs**: `Server/Drops/NPCs/`
- **Blocks**: `Server/Drops/Rock/`, `Server/Drops/Wood/`, `Server/Drops/Soil_*.json`
- **Crops**: `Server/Drops/Crop/`
- **Prefabs**: `Server/Drops/Prefabs/`
- **Objectives**: `Server/Drops/Objectives/`
- **Traps**: `Server/Drops/Traps/`

## Basic Drop Table Structure

All drop tables use a `Container` system:

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Ingredient_Bone_Fragment",
          "QuantityMin": 1,
          "QuantityMax": 2
        }
      }
    ]
  }
}
```

## Container Types

### Single

Drops a single item with optional quantity range:

```json
{
  "Type": "Single",
  "Item": {
    "ItemId": "Ingredient_Bone_Fragment",
    "QuantityMin": 1,
    "QuantityMax": 2
  }
}
```

**Properties:**
- **`ItemId`** - Item ID to drop
- **`QuantityMin`** - Minimum quantity (optional, defaults to 1)
- **`QuantityMax`** - Maximum quantity (optional, defaults to 1)

### Multiple

Drops all items in the container (guaranteed):

```json
{
  "Type": "Multiple",
  "Containers": [
    {
      "Type": "Single",
      "Item": {
        "ItemId": "Plant_Crop_Carrot_Item"
      }
    },
    {
      "Type": "Single",
      "Weight": 100.0,
      "Item": {
        "ItemId": "Ingredient_Life_Essence",
        "QuantityMin": 2,
        "QuantityMax": 3
      }
    }
  ]
}
```

All items in a `Multiple` container are dropped together.

**Optional Weight:** Can use `Weight` on items within `Multiple` containers to control probability (only applies if nested in a `Choice` container).

### Choice

Selects one option from weighted choices:

```json
{
  "Type": "Choice",
  "Weight": 100,
  "Containers": [
    {
      "Type": "Multiple",
      "Weight": 50,
      "Containers": [
        {
          "Type": "Single",
          "Item": {
            "ItemId": "Ingredient_Stick",
            "QuantityMin": 0,
            "QuantityMax": 2
          }
        }
      ]
    },
    {
      "Type": "Multiple",
      "Weight": 50,
      "Containers": [
        {
          "Type": "Single",
          "Item": {
            "ItemId": "Ingredient_Stick"
          }
        }
      ]
    }
  ]
}
```

**Weight System:**
- Each option in a `Choice` container has a `Weight`
- Higher weight = more likely to be selected
- Probability = `Weight / Sum of All Weights`

**Example:** With weights 50 and 50, each has a 50% chance. With weights 100, 25, and 25, the first has 66.7% chance, others have 16.7% each.

### Droplist

References another drop table file:

```json
{
  "Type": "Droplist",
  "DroplistId": "Zone3_Encounters_Tier1"
}
```

**Properties:**
- **`DroplistId`** - ID of the drop table to reference (file name without `.json`)

Allows reusing common drop tables across multiple entities.

## NPC Drop Tables

### Basic NPC Drop

`Server/Drops/NPCs/Intelligent/Goblin/Drop_Goblin_Scrapper.json`:

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Choice",
        "Weight": 15,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Weapon_Mace_Scrap"
            }
          }
        ]
      },
      {
        "Type": "Choice",
        "Weight": 100,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Ingredient_Fabric_Scrap_Linen",
              "QuantityMin": 1,
              "QuantityMax": 3
            }
          }
        ]
      }
    ]
  }
}
```

### Multiple Guaranteed Drops

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Ingredient_Hide_Medium",
          "QuantityMin": 2,
          "QuantityMax": 3
        }
      },
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Food_Wildmeat_Raw",
          "QuantityMin": 2,
          "QuantityMax": 3
        }
      }
    ]
  }
}
```

Both items always drop (with quantity ranges).

### Weighted Choice Drops

```json
{
  "Container": {
    "Type": "Choice",
    "Containers": [
      {
        "Type": "Single",
        "Weight": 80,
        "Item": {
          "ItemId": "Ingredient_Fabric_Scrap_Linen",
          "QuantityMin": 1,
          "QuantityMax": 2
        }
      },
      {
        "Type": "Single",
        "Weight": 20,
        "Item": {
          "ItemId": "Weapon_Mace_Scrap"
        }
      }
    ]
  }
}
```

80% chance for fabric (1-2), 20% chance for weapon.

## Block Drop Tables

### Crop Drops

`Server/Drops/Crop/Carrot/Drops_Plant_Crop_Carrot_Eternal_StageFinal.json`:

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Plant_Crop_Carrot_Item"
        }
      },
      {
        "Type": "Single",
        "Weight": 100.0,
        "Item": {
          "ItemId": "Ingredient_Life_Essence",
          "QuantityMin": 2,
          "QuantityMax": 3
        }
      }
    ]
  }
}
```

Always drops carrot, 100% weighted chance for essence.

### Rock Drops

`Server/Drops/Rock/Crystal/Rock_Crystal_Blue_Small.json`:

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Ingredient_Crystal_Cyan",
          "QuantityMin": 0,
          "QuantityMax": 1
        }
      }
    ]
  }
}
```

Can drop 0-1 crystal (50% chance with 0-1 range).

### Wood Drops with Choice

`Server/Drops/Wood/Wood_Branch.json`:

```json
{
  "Container": {
    "Type": "Choice",
    "Containers": [
      {
        "Type": "Multiple",
        "Weight": 50,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Ingredient_Stick",
              "QuantityMin": 0,
              "QuantityMax": 2
            }
          },
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Ingredient_Tree_Sap",
              "QuantityMin": 0,
              "QuantityMax": 1
            }
          }
        ]
      },
      {
        "Type": "Multiple",
        "Weight": 50,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Ingredient_Stick"
            }
          }
        ]
      }
    ]
  }
}
```

50% chance for both stick (0-2) + sap (0-1), 50% chance for single stick.

## Prefab Drop Tables

### Prefab with Droplist Reference

`Server/Drops/Prefabs/Zone3_Trork_Tier1.json`:

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Choice",
        "RollsMin": 1,
        "RollsMax": 2,
        "Containers": [
          { "Type": "Single", "Weight": 8, "Item": { "ItemId": "Food_Beef_Raw", "QuantityMin": 1, "QuantityMax": 2 } },
          { "Type": "Single", "Weight": 8, "Item": { "ItemId": "Food_Pork_Raw", "QuantityMin": 1, "QuantityMax": 2 } },
          { "Type": "Single", "Weight": 1, "Item": { "ItemId": "Armor_Trork_Chest", "QuantityMin": 1, "QuantityMax": 1 } },
          { "Type": "Single", "Weight": 1, "Item": { "ItemId": "Weapon_Mace_Stone_Trork", "QuantityMin": 1, "QuantityMax": 1 } }
        ]
      },
      {
        "Type": "Droplist",
        "DroplistId": "Zone3_Encounters_Tier1"
      }
    ]
  }
}
```

**RollsMin/RollsMax:** How many times to roll the choice container (1-2 rolls, each selecting one item).

## Advanced Features

### RollsMin and RollsMax

Roll a choice container multiple times:

```json
{
  "Type": "Choice",
  "RollsMin": 1,
  "RollsMax": 3,
  "Containers": [
    {
      "Type": "Single",
      "Weight": 50,
      "Item": {
        "ItemId": "Ingredient_Gold_Coin"
      }
    },
    {
      "Type": "Single",
      "Weight": 50,
      "Item": {
        "ItemId": "Ingredient_Silver_Coin"
      }
    }
  ]
}
```

Rolls 1-3 times, each time selecting one option. Can get multiple coins!

### Nested Containers

Complex nested structures:

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Ingredient_Bone_Fragment",
          "QuantityMin": 1,
          "QuantityMax": 3
        }
      },
      {
        "Type": "Choice",
        "Weight": 100,
        "Containers": [
          {
            "Type": "Multiple",
            "Weight": 70,
            "Containers": [
              {
                "Type": "Single",
                "Item": {
                  "ItemId": "Ingredient_Fabric_Scrap_Linen",
                  "QuantityMin": 1,
                  "QuantityMax": 2
                }
              }
            ]
          },
          {
            "Type": "Single",
            "Weight": 30,
            "Item": {
              "ItemId": "Weapon_Sword_Rusty"
            }
          }
        ]
      }
    ]
  }
}
```

Always drops bone fragments (1-3), then 70% chance for fabric (1-2) OR 30% chance for weapon.

### Reusable Droplists

Create common drop tables:

`Server/Drops/Prefabs/Zone3_Encounters_Tier1.json`:

```json
{
  "Container": {
    "Type": "Choice",
    "RollsMin": 1,
    "RollsMax": 1,
    "Containers": [
      { "Type": "Single", "Weight": 20, "Item": { "ItemId": "Potion_Health_Lesser", "QuantityMin": 1, "QuantityMax": 1 } },
      { "Type": "Single", "Weight": 10, "Item": { "ItemId": "Potion_Mana_Lesser", "QuantityMin": 1, "QuantityMax": 1 } },
      { "Type": "Single", "Weight": 5, "Item": { "ItemId": "Weapon_Sword_Iron", "QuantityMin": 1, "QuantityMax": 1 } }
    ]
  }
}
```

Then reference it:

```json
{
  "Type": "Droplist",
  "DroplistId": "Zone3_Encounters_Tier1"
}
```

## Quantity Ranges

### Basic Range

```json
{
  "Item": {
    "ItemId": "Ingredient_Bone_Fragment",
    "QuantityMin": 1,
    "QuantityMax": 3
  }
}
```

Drops 1-3 items.

### Chance with 0-1 Range

```json
{
  "Item": {
    "ItemId": "Ingredient_Crystal_Cyan",
    "QuantityMin": 0,
    "QuantityMax": 1
  }
}
```

50% chance to drop (0 or 1).

### Guaranteed with Variable Amount

```json
{
  "Item": {
    "ItemId": "Ingredient_Gold_Coin",
    "QuantityMin": 5,
    "QuantityMax": 10
  }
}
```

Always drops 5-10 coins.

## Weight System

### Weight Calculation

Weights are relative within a `Choice` container:

```json
{
  "Type": "Choice",
  "Containers": [
    {
      "Type": "Single",
      "Weight": 100,  // 100 / 200 = 50%
      "Item": { "ItemId": "Item_Common" }
    },
    {
      "Type": "Single",
      "Weight": 100,  // 100 / 200 = 50%
      "Item": { "ItemId": "Item_Common" }
    }
  ]
}
```

```json
{
  "Type": "Choice",
  "Containers": [
    {
      "Type": "Single",
      "Weight": 90,   // 90 / 100 = 90%
      "Item": { "ItemId": "Item_Common" }
    },
    {
      "Type": "Single",
      "Weight": 10,   // 10 / 100 = 10%
      "Item": { "ItemId": "Item_Rare" }
    }
  ]
}
```

### Weight Examples

| Weight 1 | Weight 2 | Weight 3 | Probability 1 | Probability 2 | Probability 3 |
|----------|----------|----------|---------------|---------------|---------------|
| 50 | 50 | - | 50% | 50% | - |
| 100 | 25 | 25 | 66.7% | 16.7% | 16.7% |
| 90 | 10 | - | 90% | 10% | - |
| 1 | 1 | 1 | 33.3% | 33.3% | 33.3% |

## Complete Examples

### Example 1: Common NPC Drop

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Ingredient_Bone_Fragment",
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
            "Weight": 80,
            "Item": {
              "ItemId": "Ingredient_Fabric_Scrap_Linen",
              "QuantityMin": 1,
              "QuantityMax": 2
            }
          },
          {
            "Type": "Single",
            "Weight": 20,
            "Item": {
              "ItemId": "Weapon_Mace_Scrap"
            }
          }
        ]
      }
    ]
  }
}
```

Always drops 1-2 bone fragments, then 80% fabric (1-2) or 20% weapon.

### Example 2: Boss Drop Table

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Ingredient_Boss_Essence",
          "QuantityMin": 5,
          "QuantityMax": 10
        }
      },
      {
        "Type": "Choice",
        "RollsMin": 2,
        "RollsMax": 4,
        "Containers": [
          { "Type": "Single", "Weight": 40, "Item": { "ItemId": "Potion_Health_Greater", "QuantityMin": 1, "QuantityMax": 1 } },
          { "Type": "Single", "Weight": 40, "Item": { "ItemId": "Potion_Mana_Greater", "QuantityMin": 1, "QuantityMax": 1 } },
          { "Type": "Single", "Weight": 10, "Item": { "ItemId": "Weapon_Legendary_Sword", "QuantityMin": 1, "QuantityMax": 1 } },
          { "Type": "Single", "Weight": 10, "Item": { "ItemId": "Armor_Legendary_Chest", "QuantityMin": 1, "QuantityMax": 1 } }
        ]
      },
      {
        "Type": "Droplist",
        "DroplistId": "Boss_Common_Drops"
      }
    ]
  }
}
```

Always drops essence (5-10), then rolls 2-4 times for potions/gear, plus common drops from droplist.

### Example 3: Crop Harvest

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Single",
        "Item": {
          "ItemId": "Plant_Crop_Carrot_Item",
          "QuantityMin": 2,
          "QuantityMax": 4
        }
      },
      {
        "Type": "Single",
        "Weight": 100.0,
        "Item": {
          "ItemId": "Ingredient_Life_Essence",
          "QuantityMin": 1,
          "QuantityMax": 2
        }
      }
    ]
  }
}
```

Always drops 2-4 carrots, always drops 1-2 essence.

## Linking Drop Tables to Entities

### NPCs

In NPC role files:

```json
{
  "Modify": {
    "DropList": "Drop_Goblin_Scrapper"
  }
}
```

The drop table file must be at `Server/Drops/NPCs/Intelligent/Goblin/Drop_Goblin_Scrapper.json`.

### Blocks

Blocks reference drop tables through gathering or block properties (check block/item definition).

### Crops

Crop blocks reference drop tables when harvested.

## Tips for Drop Tables

1. **Always drop something** - Use `Multiple` containers for guaranteed drops
2. **Use weights wisely** - Keep weights simple (10, 25, 50, 100) for easy percentages
3. **Quantity ranges** - Use `QuantityMin: 0, QuantityMax: 1` for chance-based drops
4. **Reusable droplists** - Create common drop tables for shared loot pools
5. **RollsMin/RollsMax** - Use for loot crates or multiple-roll scenarios
6. **Nested structures** - Combine `Multiple` and `Choice` for complex drop logic
7. **Test probabilities** - Verify weight distributions feel right in-game
8. **File organization** - Keep drop tables organized by category (NPCs, Blocks, Crops)

## Drop Table Organization

Recommended structure:

```
Server/Drops/
├── NPCs/
│   ├── Intelligent/
│   │   ├── Goblin/
│   │   │   ├── Drop_Goblin_Scrapper.json
│   │   │   └── Drop_Goblin_Lobber.json
│   │   └── Trork/
│   ├── Beast/
│   └── Livestock/
├── Crop/
│   ├── Carrot/
│   │   └── Drops_Plant_Crop_Carrot_Eternal_StageFinal.json
│   └── Wheat/
├── Rock/
│   ├── Crystal/
│   └── Rubble/
├── Wood/
├── Prefabs/
│   └── Zone3_Encounters_Tier1.json
└── Traps/
```

---

**Previous:** [Damage Calculator](104_Damage_Calculator.md) | **Next:** [Entity UI Configuration](106_Entity_UI_Configuration.md)
