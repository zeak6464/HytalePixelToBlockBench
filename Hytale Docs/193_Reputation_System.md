# Reputation System

Track NPC faction relationships and player reputation.

## Overview

The Reputation System tracks player standing with NPC factions. Reputation affects NPC behavior, trade prices, and available quests. It's stored per-instance and can change based on player actions.

## Location
- Instance data: `Server/Instances/{Instance}/resources/ReputationData.json`
- NPC faction configs: Within NPC role definitions

## Instance Reputation Data

From `Server/Instances/Default/resources/ReputationData.json`:

```json
{
  "Stats": {}
}
```

The `Stats` object stores reputation values per faction:

```json
{
  "Stats": {
    "Kweebec": 50,
    "Outlander": 25,
    "Goblin": -100
  }
}
```

---

## Reputation Values

| Range | Status | Description |
|-------|--------|-------------|
| 100+ | Allied | Full access, best prices |
| 50-99 | Friendly | Good relations |
| 0-49 | Neutral | Default state |
| -49 to -1 | Unfriendly | Limited services |
| -100 or below | Hostile | NPCs attack on sight |

---

## NPC Faction Attitudes

NPCs can have attitude conditions based on reputation:

```json
{
  "Attitudes": {
    "Default": "Neutral",
    "Conditions": [
      {
        "Type": "Reputation",
        "Faction": "Kweebec",
        "Threshold": 50,
        "Attitude": "Friendly"
      },
      {
        "Type": "Reputation",
        "Faction": "Kweebec",
        "Threshold": -50,
        "Attitude": "Hostile"
      }
    ]
  }
}
```

---

## Modifying Reputation

### Through Interactions

```json
{
  "InteractionVars": {
    "Complete_Quest": {
      "Interactions": [
        {
          "Type": "ModifyReputation",
          "Faction": "Kweebec",
          "Amount": 10
        }
      ]
    }
  }
}
```

### Through NPC Deaths

Configure reputation loss on NPC kill:

```json
{
  "OnDeath": {
    "ReputationChange": {
      "Faction": "Kweebec",
      "Amount": -5
    }
  }
}
```

---

## Faction Configuration

### Defining a Faction

Factions are defined by NPC groups and shared reputation:

```json
{
  "Faction": "Kweebec",
  "FactionAllies": ["Outlander"],
  "FactionEnemies": ["Goblin", "Trork"]
}
```

### Faction Relationships

| Relationship | Description |
|--------------|-------------|
| Allies | Reputation changes affect both |
| Enemies | Killing enemies may increase reputation |
| Neutral | No automatic reputation effects |

---

## Reputation Effects

### Trade Prices

```json
{
  "BarterShop": {
    "ReputationModifiers": {
      "Friendly": 0.9,
      "Neutral": 1.0,
      "Unfriendly": 1.5
    }
  }
}
```

- Friendly: 10% discount
- Unfriendly: 50% markup

### Quest Availability

```json
{
  "QuestRequirements": {
    "MinReputation": {
      "Faction": "Kweebec",
      "Value": 25
    }
  }
}
```

### NPC Behavior

```json
{
  "BehaviorModifiers": {
    "Hostile": {
      "AttackOnSight": true,
      "TradeEnabled": false
    },
    "Friendly": {
      "FollowPlayer": true,
      "HelpInCombat": true
    }
  }
}
```

---

## Creating Custom Faction

### 1. Define Faction in NPC Role

```json
{
  "Type": "Variant",
  "Faction": "MyFaction",
  "FactionAllies": [],
  "FactionEnemies": ["Goblin"],
  "ReputationConfig": {
    "OnKill": -10,
    "OnHelp": 5
  }
}
```

### 2. Configure Attitude Thresholds

```json
{
  "Attitudes": {
    "Default": "Neutral",
    "Conditions": [
      {
        "Type": "Reputation",
        "Faction": "MyFaction",
        "Threshold": 100,
        "Attitude": "Allied"
      },
      {
        "Type": "Reputation",
        "Faction": "MyFaction",
        "Threshold": 50,
        "Attitude": "Friendly"
      },
      {
        "Type": "Reputation",
        "Faction": "MyFaction",
        "Threshold": -25,
        "Attitude": "Unfriendly"
      },
      {
        "Type": "Reputation",
        "Faction": "MyFaction",
        "Threshold": -75,
        "Attitude": "Hostile"
      }
    ]
  }
}
```

### 3. Add Reputation Interactions

```json
{
  "Interactions": {
    "GiveGift": {
      "Interactions": [
        {
          "Type": "ModifyReputation",
          "Faction": "MyFaction",
          "Amount": 5
        },
        {
          "Type": "ModifyInventory",
          "AdjustHeldItemQuantity": -1
        }
      ]
    }
  }
}
```

---

## Tips

1. **Start neutral** - Default reputation should be 0
2. **Gradual changes** - Small increments for natural progression
3. **Recovery options** - Allow reputation repair through quests
4. **Faction allies** - Consider ripple effects on allied factions
5. **Save frequently** - Reputation persists in instance data
6. **Visual feedback** - Show reputation changes to players

---

**Related:** [NPCs](03_NPCs.md) | [Barter Shops](187_Barter_Shops.md) | [Objective System](168_Objective_System.md)
