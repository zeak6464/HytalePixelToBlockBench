# System Integration Guide

Learn how Hytale's systems work together to create complete gameplay experiences: NPCs + shops + quests + items + progression.

## Overview

This guide shows how to integrate multiple systems into cohesive gameplay loops:
- NPC + Shop integration
- Quest + Objective systems
- Item + Crafting progression
- NPC + Memory + Reputation
- Complete faction workflows
- Loot + Drops + Rewards

All examples are from actual game files.

---

## NPC + Shop Integration

### Complete Merchant NPC

From `Kweebec_Merchant.json` + `Kweebec_Merchant.json` (shop file):

#### Step 1: Create the NPC Role

File: `Server/NPC/Roles/Intelligent/Neutral/Kweebec/Kweebec_Merchant.json`

```json
{
  "Type": "Generic",
  "StartState": "Idle",
  "DefaultNPCAttitude": "Ignore",
  "DefaultPlayerAttitude": "Neutral",
  "Appearance": "Kweebec_Rootling",
  "MaxHealth": 50,
  "IsMemory": true,
  "MemoriesCategory": "Kweebec",
  "BusyStates": ["$Interaction"],
  "Instructions": [
    {
      "Sensor": { "Type": "State", "State": "Idle" },
      "Instructions": [
        {
          "Sensor": { "Type": "Player", "Range": 8 },
          "Actions": [
            {
              "Type": "PlayAnimation",
              "Slot": "Status",
              "Animation": "Wave"
            },
            {
              "Type": "State",
              "State": "Watching"
            }
          ]
        }
      ]
    }
  ],
  "InteractionInstruction": {
    "Instructions": [
      {
        "Sensor": { "Type": "HasInteracted" },
        "Instructions": [
          {
            "Actions": [
              {
                "Type": "OpenBarterShop",
                "Shop": "Kweebec_Merchant"
              },
              {
                "Type": "State",
                "State": "$Interaction"
              }
            ]
          }
        ]
      }
    ]
  }
}
```

**Key integration points:**
- `"IsMemory": true` - Players remember this NPC
- `"MemoriesCategory": "Kweebec"` - Groups with other Kweebecs
- `"OpenBarterShop"` action links to shop file
- `"$Interaction"` state prevents NPC wandering while trading

---

#### Step 2: Create the Shop

File: `Server/BarterShops/Kweebec_Merchant.json`

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
          "Weight": 20,
          "Output": { "ItemId": "Recipe_Food_Pie_Apple", "Quantity": 1 },
          "Input": [{ "ItemId": "Ingredient_Life_Essence_Concentrated", "Quantity": 2 }],
          "Stock": [1]
        }
      ]
    }
  ]
}
```

**Shop features:**
- **Fixed trades** - Always available (spices, salt, dough)
- **Pool trades** - Random selection from pool
- **Stock limits** - Items run out until restock
- **Timed refresh** - Restocks every 3 days at 6 AM

---

#### Step 3: Create the Currency Item

File: `Server/Item/Items/Ingredient/Essence/Ingredient_Life_Essence.json`

```json
{
  "TranslationProperties": {
    "Name": "server.items.Ingredient_Life_Essence.name"
  },
  "Categories": ["Items.Ingredients"],
  "Icon": "Icons/ItemsGenerated/Life_Essence.png",
  "Quality": "Common",
  "ItemLevel": 1,
  "MaxStack": 999,
  "Tags": {
    "Type": ["Currency", "Ingredient"]
  }
}
```

**Currency design:**
- High stack size (999) for convenient trading
- Tagged as "Currency" for filtering
- Also used as crafting ingredient (dual purpose)

---

#### Step 4: Add Localization

File: `Server/Languages/en-US/server.lang`

```
npcRoles.Kweebec_Merchant.name = Kweebec Trader
barter.kweebec_merchant.title = Kweebec Goods
interactionHints.trade = Trade
```

---

### Complete Integration Flow

```
Player approaches NPC (8m range)
  ↓
NPC waves (animation)
  ↓
NPC enters "Watching" state
  ↓
Player clicks NPC
  ↓
"OpenBarterShop" action triggers
  ↓
Shop UI opens with:
  - Fixed trades (always same)
  - Pool trades (random from weighted list)
  - Stock amounts (limited quantities)
  ↓
NPC enters "$Interaction" state (prevents movement)
  ↓
Player trades Life Essence for items
  ↓
Shop deducts stock
  ↓
Player closes shop
  ↓
NPC returns to "Watching" state
  ↓
Player leaves (12m range)
  ↓
NPC returns to "Idle" state
```

---

## Quest + Objective System

### Simple Crafting Quest

From `Objective_Craft.json`:

```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "Craft",
          "Count": 1,
          "ItemId": "Furniture_Crude_Bed"
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "GiveItems",
      "DropList": "Trork_Camp_Inventory"
    }
  ]
}
```

**Quest flow:**
1. Player receives objective
2. Player crafts "Furniture_Crude_Bed"
3. Objective tracks craft event
4. On completion, gives items from "Trork_Camp_Inventory" drop list

---

### Multi-Objective Quest Line

File: `Server/Objective/ObjectiveLines/ObjectiveLine_Tutorial.json`

```json
{
  "ObjectiveIds": [
    "Objective_Tutorial"
  ]
}
```

**Objective lines:**
- Group related objectives
- Create quest chains
- Track progression through multiple steps

**Example quest chain:**
```json
{
  "ObjectiveIds": [
    "Objective_Gather_Wood",      // Step 1
    "Objective_Craft_Workbench",  // Step 2
    "Objective_Craft_Pickaxe",    // Step 3
    "Objective_Mine_Stone"        // Step 4
  ]
}
```

---

### Objective Types

All available from game files:

#### Gather Objective

```json
{
  "Type": "Gather",
  "ItemId": "Wood_Oak_Log",
  "Count": 10
}
```

#### Kill Objective

```json
{
  "Type": "Kill",
  "NPCRole": "Trork",
  "Count": 5
}
```

#### Reach Location Objective

```json
{
  "Type": "ReachLocation",
  "MarkerId": "ObjectiveLocationMarker_Village"
}
```

#### Use Block Objective

```json
{
  "Type": "UseBlock",
  "BlockId": "Bench_Workbench",
  "Count": 1
}
```

---

## Item + Crafting Progression

### Tiered Item Progression

From actual pickaxe progression in game files:

#### Tier 1: Crude Pickaxe

```json
{
  "Parent": "Template_Tool_Pickaxe",
  "ItemLevel": 1,
  "MaxDurability": 60,
  "Recipe": {
    "Input": [
      { "ItemId": "Rock_Stone", "Quantity": 3 },
      { "ItemId": "Wood_Oak_Log", "Quantity": 2 }
    ],
    "BenchRequirement": [{ "Id": "Workbench" }]
  }
}
```

**Update 2 (Jan 2026):** Pickaxe tiers changed. **Copper** is Tier 2, **Iron** Tier 4, **Thorium** Tier 6. **Adamantite** requires Thorium or Cobalt. See [Tools](14_Tools.md) and [Patch Notes Update 2](Patch_Notes_Update_2.md).

#### Tier 2: Copper Pickaxe (Update 2)

```json
{
  "Parent": "Template_Tool_Pickaxe",
  "ItemLevel": 10,
  "MaxDurability": 120,
  "Recipe": {
    "Input": [
      { "ItemId": "Ingredient_Bar_Copper", "Quantity": 3 },
      { "ItemId": "Wood_Oak_Plank", "Quantity": 2 }
    ],
    "BenchRequirement": [{ "Id": "Workbench" }]
  }
}
```

#### Tier 4: Iron Pickaxe (Update 2)

```json
{
  "Parent": "Template_Tool_Pickaxe",
  "ItemLevel": 20,
  "MaxDurability": 250,
  "Tool": {
    "Specs": [
      { "Power": 1, "GatherType": "SoftBlocks" },
      { "Power": 0.5, "GatherType": "Soils" },
      { "Power": 0.05, "GatherType": "Woods" }
    ]
  },
  "Recipe": {
    "Input": [
      { "ItemId": "Ingredient_Bar_Iron", "Quantity": 3 },
      { "ItemId": "Wood_Oak_Plank", "Quantity": 2 }
    ],
    "BenchRequirement": [{ "Id": "Workbench" }]
  }
}
```

#### Tier 6: Thorium Pickaxe (Update 2)

```json
{
  "Parent": "Template_Tool_Pickaxe",
  "ItemLevel": 40,
  "MaxDurability": 500,
  "Tool": {
    "Specs": [
      { "Power": 1, "GatherType": "SoftBlocks" },
      { "Power": 1, "GatherType": "Rocks" },
      { "Power": 0.8, "GatherType": "VolcanicRocks" },
      { "Power": 0.5, "GatherType": "Soils" }
    ]
  },
  "Recipe": {
    "Input": [
      { "ItemId": "Ingredient_Bar_Thorium", "Quantity": 5 },
      { "ItemId": "Wood_Oak_Plank", "Quantity": 2 }
    ],
    "BenchRequirement": [{ "Id": "Workbench" }]
  }
}
```

**Progression gates (Update 2):**
- **Crude** (Stone) → Gather basic resources
- **Copper** (T2) → Earlier ore tier; requires smelting
- **Iron** (T4) → Better stats, requires iron ore
- **Thorium** (T6) → Adamantite requires Thorium or Cobalt pickaxe

---

### Recipe Unlocking System

#### Knowledge-Locked Recipes

```json
{
  "Recipe": {
    "KnowledgeRequired": true,
    "Input": [
      { "ItemId": "Ingredient_Magic_Crystal", "Quantity": 1 }
    ]
  }
}
```

**Player must discover/unlock recipe before crafting.**

#### Recipe Items (Unlock Scrolls)

```json
{
  "TranslationProperties": {
    "Name": "server.items.Recipe_Food_Pie_Apple.name"
  },
  "Categories": ["Items.Recipes"],
  "Icon": "Icons/ItemsGenerated/Recipe_Scroll.png",
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "UnlockRecipe",
          "RecipeId": "Food_Pie_Apple"
        }
      ]
    }
  }
}
```

**Integration with shops:**
From `Kweebec_Merchant.json` shop - sells recipe scrolls:

```json
{
  "Weight": 20,
  "Output": { "ItemId": "Recipe_Food_Pie_Apple", "Quantity": 1 },
  "Input": [{ "ItemId": "Ingredient_Life_Essence_Concentrated", "Quantity": 2 }],
  "Stock": [1]
}
```

**Complete flow:**
1. Player gathers Life Essence (kills NPCs, finds in world)
2. Concentrates essence (crafting/alchemy)
3. Trades with merchant for recipe scroll
4. Uses scroll to unlock recipe
5. Can now craft Apple Pie

---

## NPC + Memory + Reputation

### Memory System

From `Kweebec_Merchant.json`:

```json
{
  "IsMemory": true,
  "MemoriesCategory": "Kweebec",
  "MemoriesNameOverride": "Kweebec_Rootling"
}
```

**What memories do:**
- Track player discoveries (first encounter)
- Count kills per NPC type
- Unlock achievements
- Affect reputation

**Example integration:**

```json
{
  "Sensor": {
    "Type": "MemoriesCondition",
    "Memory": "Kweebec",
    "Count": 5
  },
  "Actions": [
    {
      "Type": "SendMessage",
      "Message": "The Kweebecs recognize you as a friend!"
    }
  ]
}
```

**Player who helped 5 Kweebecs gets friendly message.**

---

### NPC Attitude System

```json
{
  "DefaultNPCAttitude": "Ignore",
  "DefaultPlayerAttitude": "Neutral"
}
```

**Attitude options:**
- `Ignore` - NPC doesn't react to player
- `Neutral` - Watches player but doesn't attack
- `Friendly` - Waves, offers help
- `Hostile` - Attacks on sight

**Dynamic attitude changes:**

```json
{
  "Sensor": {
    "Type": "PlayerAttacked"
  },
  "Actions": [
    {
      "Type": "SetPlayerAttitude",
      "Attitude": "Hostile"
    }
  ]
}
```

**If player attacks peaceful NPC, NPC becomes hostile.**

---

## Complete Faction Workflow

### Example: Kweebec Trading Faction

#### 1. Create Faction NPCs

- `Kweebec_Merchant` (trader)
- `Kweebec_Guard` (protects merchant)
- `Kweebec_Villager` (ambient NPCs)

#### 2. Create Faction Shop

`Kweebec_Merchant.json` shop with:
- Food items
- Recipe scrolls
- Farming supplies
- Rare ingredients

#### 3. Create Faction Currency

`Ingredient_Life_Essence` - obtained by:
- Killing hostile NPCs
- Completing quests
- Finding in dungeon chests

#### 4. Create Faction Quests

**Quest 1: Introduction**
```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "ReachLocation",
          "MarkerId": "Kweebec_Village"
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "GiveItems",
      "Items": [
        { "ItemId": "Ingredient_Life_Essence", "Quantity": 50 }
      ]
    }
  ]
}
```

**Quest 2: Gather Resources**
```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "Gather",
          "ItemId": "Plant_Fruit_Berries_Red",
          "Count": 20
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "GiveItems",
      "Items": [
        { "ItemId": "Ingredient_Life_Essence", "Quantity": 100 }
      ]
    }
  ]
}
```

**Quest 3: Protect Village**
```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "Kill",
          "NPCRole": "Trork_Warrior",
          "Count": 10
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "GiveItems",
      "Items": [
        { "ItemId": "Recipe_Armor_Kweebec_Helm", "Quantity": 1 },
        { "ItemId": "Ingredient_Life_Essence", "Quantity": 200 }
      ]
    }
  ]
}
```

#### 5. Create Faction Rewards

Unlock exclusive items:
- Kweebec armor set (recipe scrolls)
- Kweebec weapon skins
- Pet Kweebec (cosmetic follower)

---

## Loot + Drops + Rewards

### NPC Drop Tables

From typical NPC configuration:

```json
{
  "DropList": "Trork_Warrior_Drops"
}
```

#### Create Drop Table

File: `Server/Drops/NPCs/Trork_Warrior_Drops.json`

```json
{
  "Drops": [
    {
      "ItemId": "Ingredient_Life_Essence",
      "Quantity": [5, 10],
      "Chance": 1.0
    },
    {
      "ItemId": "Weapon_Sword_Crude",
      "Quantity": 1,
      "Chance": 0.1
    },
    {
      "ItemId": "Armor_Leather_Chest",
      "Quantity": 1,
      "Chance": 0.05
    }
  ]
}
```

**Drop logic:**
- 100% chance: 5-10 Life Essence (currency)
- 10% chance: Crude Sword
- 5% chance: Leather Chest

---

### Quest Reward Tables

From `Objective_Craft.json`:

```json
{
  "Completions": [
    {
      "Type": "GiveItems",
      "DropList": "Trork_Camp_Inventory"
    }
  ]
}
```

**Rewards can be:**
- Fixed items
- Random from drop table
- Currency
- Recipe unlocks
- Experience (if system exists)

---

### Chest Loot Tables

```json
{
  "BlockType": {
    "Container": {
      "DropList": "Dungeon_Chest_Common"
    }
  }
}
```

#### Create Chest Loot Table

File: `Server/Drops/Chests/Dungeon_Chest_Common.json`

```json
{
  "Drops": [
    {
      "ItemId": "Ingredient_Life_Essence",
      "Quantity": [10, 30],
      "Chance": 0.8
    },
    {
      "ItemId": "Potion_Health_Lesser",
      "Quantity": [1, 3],
      "Chance": 0.6
    },
    {
      "ItemId": "Recipe_Weapon_Sword_Iron",
      "Quantity": 1,
      "Chance": 0.05
    }
  ]
}
```

---

## Complete Gameplay Loop Example

### "Merchant Rescue" Quest Chain

#### Quest 1: Find the Merchant

```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "ReachLocation",
          "MarkerId": "Ambush_Site"
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "StartObjective",
      "ObjectiveId": "Objective_Rescue_Merchant"
    }
  ]
}
```

#### Quest 2: Defeat Bandits

```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "Kill",
          "NPCRole": "Bandit",
          "Count": 5
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "SpawnNPC",
      "NPCRole": "Kweebec_Merchant_Grateful",
      "Location": "Ambush_Site"
    }
  ]
}
```

#### Quest 3: Escort to Village

```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "EscortNPC",
          "NPCId": "Kweebec_Merchant_Grateful",
          "Destination": "Kweebec_Village"
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "GiveItems",
      "Items": [
        { "ItemId": "Ingredient_Life_Essence_Concentrated", "Quantity": 5 },
        { "ItemId": "Recipe_Special_Discount_Token", "Quantity": 1 }
      ]
    },
    {
      "Type": "UnlockShop",
      "ShopId": "Kweebec_Merchant_Grateful"
    }
  ]
}
```

**Result:**
- Player gets concentrated essence (high-value currency)
- Unlocks special discount token (trade for rare items)
- Unlocks new merchant with exclusive items

---

## System Integration Checklist

When creating integrated content, ensure:

### NPC Integration
- ✅ NPC role defined with states
- ✅ Interaction instruction set up
- ✅ Localization keys added
- ✅ Shop/quest references correct
- ✅ Memory category assigned (if trackable)

### Shop Integration
- ✅ Shop file created in `Server/BarterShops/`
- ✅ Currency items exist
- ✅ Trade items exist
- ✅ Stock limits reasonable
- ✅ Refresh timing set
- ✅ Localization for shop title

### Quest Integration
- ✅ Objective file created
- ✅ Task types correct
- ✅ Completion rewards exist
- ✅ Quest chain order logical
- ✅ Markers placed (for location objectives)

### Item Integration
- ✅ Items exist in `Server/Item/Items/`
- ✅ Recipes have correct bench requirements
- ✅ Progression gates make sense
- ✅ Drop tables reference items correctly
- ✅ Icons and models assigned

### Loot Integration
- ✅ Drop tables created in `Server/Drops/`
- ✅ NPCs reference drop tables
- ✅ Chests reference loot tables
- ✅ Reward quantities balanced
- ✅ Rare item chances appropriate

---

## Common Integration Patterns

### Pattern 1: Currency → Shop → Progression

1. Create currency item (high stack size)
2. Add currency to NPC drop tables
3. Create shop that accepts currency
4. Sell progression items (recipes, upgrades)

### Pattern 2: Quest → Unlock → New Content

1. Create quest objective
2. Reward unlocks new NPC/shop
3. New content offers better items
4. Creates progression incentive

### Pattern 3: Memory → Reputation → Discounts

1. NPCs track player interactions
2. Memory count affects dialogue
3. High reputation unlocks discounts
4. Creates repeat engagement

### Pattern 4: Dungeon → Loot → Crafting

1. Dungeons contain chests with loot tables
2. Loot tables drop rare materials
3. Materials used in advanced recipes
4. Creates exploration incentive

---

## Troubleshooting Integration Issues

### Shop Not Opening

**Check:**
- Shop file exists in `Server/BarterShops/`
- Shop ID matches in NPC's `OpenBarterShop` action
- NPC has `InteractionInstruction` configured
- Player can interact (not blocked by state)

### Quest Not Completing

**Check:**
- Task type matches action (Craft vs Gather)
- ItemId/NPCRole spelled correctly
- Count threshold reasonable
- Completion rewards exist

### Items Not Dropping

**Check:**
- Drop table file exists
- NPC references correct drop table
- Item IDs in drop table are valid
- Chance values between 0.0 and 1.0

### Recipe Not Unlocking

**Check:**
- Recipe item has `UnlockRecipe` interaction
- RecipeId matches target item
- Recipe scroll is consumable
- Shop sells recipe scroll (if applicable)

---

## Related Guides

- **[Advanced Techniques](176_Advanced_Techniques.md)** - Complex interaction patterns
- **[Trading & Quests](05_Trading_and_Quests.md)** - Basic quest/shop creation
- **[Creating NPCs](03_NPCs.md)** - NPC role creation
- **[Creating Items](02_Items.md)** - Item and recipe creation
- **[Drop Tables](105_Drop_Tables.md)** - Loot table configuration

---

**Previous:** [Advanced Techniques](176_Advanced_Techniques.md) | **Next:** [Block Flags Reference](178_Block_Flags_Reference.md)
