# Trading & Quests

Learn how to create barter shops and quest objectives.

## Creating Trading/Barter Shops

### Overview

Barter shops allow NPCs to trade items with players. You can create fixed trades or randomized trade pools.

### Location
`Server/BarterShops/`

## Example from Game Files

### Klops Merchant Shop

From `Server/BarterShops/Klops_Merchant.json`:

```1:17:Server/BarterShops/Klops_Merchant.json
{
  "DisplayNameKey": "server.barter.klops_merchant.title",
  "RefreshInterval": {
    "Days": 1
  },
  "RestockHour": 6,
  "TradeSlots": [
    {
      "Type": "Fixed",
      "Trade": {
        "Output": { "ItemId": "Furniture_Construction_Sign", "Quantity": 1 },
        "Input": [{ "ItemId": "Furniture_Construction_Sign", "Quantity": 1 }],
        "Stock": 1
      }
    }
  ]
}
```

This shows a basic barter shop with fixed trades, refresh interval, and restock hour configuration.

### Basic Shop Structure

Create `Server/BarterShops/MyCustom_Shop.json`:

```json
{
  "DisplayNameKey": "server.barter.myCustom_shop.title",
  "RefreshInterval": {
    "Days": 3
  },
  "RestockHour": 6,
  "TradeSlots": [
    {
      "Type": "Fixed",
      "Trade": {
        "Output": { "ItemId": "Weapon_Sword_Iron", "Quantity": 1 },
        "Input": [{ "ItemId": "Ingredient_Bar_Copper", "Quantity": 10 }],
        "Stock": 5
      }
    }
  ]
}
```

### Fixed Trade (Always Available)

```json
{
  "Type": "Fixed",
  "Trade": {
    "Output": {
      "ItemId": "Weapon_Sword_Iron",
      "Quantity": 1
    },
    "Input": [
      {
        "ItemId": "Ingredient_Bar_Iron",
        "Quantity": 5
      },
      {
        "ItemId": "Ingredient_Leather_Light",
        "Quantity": 2
      }
    ],
    "Stock": 10
  }
}
```

### Randomized Trade Pool

```json
{
  "Type": "Pool",
  "SlotCount": 3,
  "Trades": [
    {
      "Weight": 50,
      "Output": {
        "ItemId": "Weapon_Sword_Iron",
        "Quantity": 1
      },
      "Input": [
        {
          "ItemId": "Ingredient_Life_Essence",
          "Quantity": 30
        }
      ],
      "Stock": [5, 10]
    },
    {
      "Weight": 30,
      "Output": {
        "ItemId": "Weapon_Sword_Steel",
        "Quantity": 1
      },
      "Input": [
        {
          "ItemId": "Ingredient_Life_Essence",
          "Quantity": 50
        }
      ],
      "Stock": [3, 7]
    }
  ]
}
```

### Shop Refresh System

```json
{
  "RefreshInterval": {
    "Days": 3,
    "Hours": 12
  },
  "RestockHour": 6
}
```

**Refresh Settings:**
- `Days` - Days until refresh
- `Hours` - Additional hours
- `RestockHour` - Hour of day to restock (0-23)

### Complete Example: Merchant Shop

```json
{
  "DisplayNameKey": "server.barter.merchant.title",
  "RefreshInterval": {
    "Days": 1
  },
  "RestockHour": 6,
  "TradeSlots": [
    {
      "Type": "Fixed",
      "Trade": {
        "Output": { "ItemId": "Food_Bread", "Quantity": 5 },
        "Input": [{ "ItemId": "Ingredient_Life_Essence", "Quantity": 10 }],
        "Stock": 20
      }
    },
    {
      "Type": "Pool",
      "SlotCount": 3,
      "Trades": [
        {
          "Weight": 50,
          "Output": { "ItemId": "Weapon_Sword_Iron", "Quantity": 1 },
          "Input": [{ "ItemId": "Ingredient_Life_Essence", "Quantity": 30 }],
          "Stock": [5, 10]
        },
        {
          "Weight": 30,
          "Output": { "ItemId": "Armor_Iron_Chest", "Quantity": 1 },
          "Input": [{ "ItemId": "Ingredient_Life_Essence", "Quantity": 40 }],
          "Stock": [3, 7]
        }
      ]
    }
  ]
}
```

---

## Creating Objectives/Quests

### Overview

Objectives are tasks players can complete. They include kill, gather, craft, and location objectives with rewards.

### Location
`Server/Objective/Objectives/`

### Kill Objective

Create `Server/Objective/Objectives/Objective_Kill_Goblins.json`:

```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "KillNPC",
          "Count": 5,
          "NPCGroupId": "Goblin_Scrapper"
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "GiveItems",
      "DropList": "Reward_Goblin_Hunter"
    }
  ]
}
```

### Gather Objective

```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "Gather",
          "BlockTagOrItemId": {
            "ItemId": "Rock_Stone"
          },
          "Count": 10
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "GiveItems",
      "DropList": "Reward_Gather_Stone"
    }
  ]
}
```

### Craft Objective

```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "Craft",
          "Count": 1,
          "ItemId": "Furniture_Crude_Chest_Small"
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "GiveItems",
      "DropList": "Reward_Crafting"
    }
  ]
}
```

### Location Objective

```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "ReachLocation",
          "TargetLocation": "ObjectiveReachMarker_Treasure"
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "GiveItems",
      "DropList": "Reward_Treasure"
    }
  ]
}
```

### Multiple Task Objective

```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "KillNPC",
          "Count": 3,
          "NPCGroupId": "Goblin_Scrapper"
        },
        {
          "Type": "Gather",
          "BlockTagOrItemId": {
            "ItemId": "Ore_Iron"
          },
          "Count": 5
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "GiveItems",
      "DropList": "Reward_Complete"
    }
  ]
}
```

### Objective Types

| Type | Parameters | Use Case |
|------|------------|----------|
| `KillNPC` | `Count`, `NPCGroupId` | Kill X NPCs |
| `Gather` | `Count`, `BlockTagOrItemId` | Collect items/blocks |
| `Craft` | `Count`, `ItemId` | Craft items |
| `ReachLocation` | `TargetLocation` | Go to a location |

---

**Previous:** [Potions & Effects](04_Potions_and_Effects.md) | **Next:** [Blocks & Portals](06_Blocks_and_Portals.md)
