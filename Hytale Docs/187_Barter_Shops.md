# Barter Shops & Trading

Create NPC merchants with trading inventories.

## Overview

Barter Shops define merchant NPCs that can trade items with players. They support fixed trades, random pools, restocking, and refresh intervals.

## Location
`Server/BarterShops/`

## Example from Game Files

### Kweebec Merchant

From `Server/BarterShops/Kweebec_Merchant.json`:

```json
{
  "DisplayNameKey": "server.barter.kweebec_merchant.title",
  "RefreshInterval": {
    "Days": 3
  },
  "RestockHour": 6,
  "TradeSlots": [
    {
      "Type": "Fixed",
      "Trade": {
        "Output": { "ItemId": "Ingredient_Spices", "Quantity": 3 },
        "Input": [{ "ItemId": "Ingredient_Life_Essence", "Quantity": 20 }],
        "Stock": 10
      }
    },
    {
      "Type": "Pool",
      "SlotCount": 3,
      "Trades": [
        {
          "Weight": 50,
          "Output": { "ItemId": "Plant_Crop_Berry_Block", "Quantity": 1 },
          "Input": [{ "ItemId": "Ingredient_Life_Essence", "Quantity": 30 }],
          "Stock": [10, 20]
        },
        {
          "Weight": 30,
          "Output": { "ItemId": "Plant_Crop_Berry_Winter_Block", "Quantity": 1 },
          "Input": [{ "ItemId": "Ingredient_Life_Essence", "Quantity": 50 }],
          "Stock": [10, 20]
        }
      ]
    }
  ]
}
```

## Shop Properties

| Property | Type | Description |
|----------|------|-------------|
| `DisplayNameKey` | String | Localization key for shop name |
| `RefreshInterval` | Object | How often stock resets |
| `RestockHour` | Int | Hour of day stock restocks (0-23) |
| `TradeSlots` | Array | List of trade slot configurations |

### RefreshInterval

```json
{
  "RefreshInterval": {
    "Days": 3
  }
}
```

Stock refreshes every 3 in-game days.

---

## Trade Slot Types

### Fixed Trades

Always available, same items every time:

```json
{
  "Type": "Fixed",
  "Trade": {
    "Output": { "ItemId": "Ingredient_Spices", "Quantity": 3 },
    "Input": [{ "ItemId": "Ingredient_Life_Essence", "Quantity": 20 }],
    "Stock": 10
  }
}
```

### Pool Trades

Random selection from a weighted pool:

```json
{
  "Type": "Pool",
  "SlotCount": 3,
  "Trades": [
    {
      "Weight": 50,
      "Output": { "ItemId": "Food_Kebab_Meat", "Quantity": 1 },
      "Input": [{ "ItemId": "Ingredient_Life_Essence", "Quantity": 15 }],
      "Stock": [4, 8]
    },
    {
      "Weight": 20,
      "Output": { "ItemId": "Food_Kebab_Mushroom", "Quantity": 1 },
      "Input": [{ "ItemId": "Ingredient_Life_Essence", "Quantity": 15 }],
      "Stock": [4, 8]
    }
  ]
}
```

| Property | Description |
|----------|-------------|
| `SlotCount` | Number of trades to pick from pool |
| `Weight` | Chance of being selected (relative) |

---

## Trade Properties

### Output (What player receives)

```json
{
  "Output": { "ItemId": "Ingredient_Spices", "Quantity": 3 }
}
```

### Input (What player pays)

```json
{
  "Input": [
    { "ItemId": "Ingredient_Life_Essence", "Quantity": 20 }
  ]
}
```

Supports multiple input items (player must have all).

### Stock

Fixed stock:
```json
{
  "Stock": 10
}
```

Random stock range:
```json
{
  "Stock": [4, 8]
}
```

Stock depletes as players buy, restocks at `RestockHour`.

---

## Complete Example: Custom Merchant

`Server/BarterShops/MyCustom_Merchant.json`:

```json
{
  "DisplayNameKey": "server.barter.mycustom_merchant.title",
  "RefreshInterval": {
    "Days": 1
  },
  "RestockHour": 6,
  "TradeSlots": [
    {
      "Type": "Fixed",
      "Trade": {
        "Output": { "ItemId": "Weapon_Sword_Iron", "Quantity": 1 },
        "Input": [
          { "ItemId": "Ingredient_Bar_Iron", "Quantity": 5 },
          { "ItemId": "Ingredient_Life_Essence", "Quantity": 50 }
        ],
        "Stock": 3
      }
    },
    {
      "Type": "Pool",
      "SlotCount": 2,
      "Trades": [
        {
          "Weight": 40,
          "Output": { "ItemId": "Potion_Health", "Quantity": 1 },
          "Input": [{ "ItemId": "Ingredient_Life_Essence", "Quantity": 25 }],
          "Stock": [5, 10]
        },
        {
          "Weight": 30,
          "Output": { "ItemId": "Potion_Stamina", "Quantity": 1 },
          "Input": [{ "ItemId": "Ingredient_Life_Essence", "Quantity": 25 }],
          "Stock": [5, 10]
        },
        {
          "Weight": 10,
          "Output": { "ItemId": "Recipe_Potion_Health_Greater", "Quantity": 1 },
          "Input": [{ "ItemId": "Ingredient_Life_Essence_Concentrated", "Quantity": 3 }],
          "Stock": [1]
        }
      ]
    }
  ]
}
```

---

## Linking to NPC

Reference the barter shop in NPC role:

```json
{
  "BarterShopId": "MyCustom_Merchant",
  "Interactions": {
    "Use": {
      "Interactions": [
        {
          "Type": "OpenBarterShop"
        }
      ]
    }
  }
}
```

---

## Tips

1. **Balance prices** - Consider Life Essence economy
2. **Stock limits** - Prevent infinite farming
3. **Pool weights** - Higher weight = more common
4. **Multiple inputs** - Require multiple currencies
5. **Refresh interval** - Longer for rare items
6. **Recipe trades** - Sell recipes with limited stock

---

**Related:** [NPCs](03_NPCs.md) | [Items](02_Items.md)
