# Objective System

Learn how to create quests, objectives, and task tracking using `Server/Objective/`.

## Overview

The Objective System defines quests and tasks for players. It supports various objective types like killing NPCs, gathering items, crafting, reaching locations, treasure maps, and bounties. Objectives use task sets, location markers, and completion rewards.

## Example from Game Files

The Objective System defines quests and tasks for players. It supports various objective types like killing NPCs, gathering items, crafting, reaching locations, treasure maps, and bounties. Objectives use task sets, location markers, and completion rewards.

## Location

`Server/Objective/`

Structure:
- **`Objectives/`** - Objective definitions (tasks, completions)
- **`ObjectiveLines/`** - Objective sequences/chains
- **`ObjectiveLocationMarkers/`** - Area-triggered objectives
- **`ReachLocationMarkers/`** - Destination markers for reach objectives

## Basic Objective Structure

`Server/Objective/Objectives/Objective_Kill.json`:

```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "KillNPC",
          "Count": 3,
          "NPCGroupId": "Trork_Warrior"
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

## Objective Properties

### TaskSets

Array of task sets - each set must be completed in order:

```json
{
  "TaskSets": [
    {
      "Tasks": [
        { "Type": "KillNPC", "Count": 3, "NPCGroupId": "Goblin" }
      ]
    },
    {
      "Tasks": [
        { "Type": "ReachLocation", "TargetLocation": "ObjectiveReachMarker_Example" }
      ]
    }
  ]
}
```

### Completions

Rewards and actions on objective completion:

```json
{
  "Completions": [
    {
      "Type": "GiveItems",
      "DropList": "Reward_DropList"
    }
  ]
}
```

**Completion Types:**
- **`GiveItems`** - Give items from drop list
- **`ClearObjectiveItems`** - Remove objective items from inventory

## Task Types

### KillNPC

Kill a specific number of NPCs:

```json
{
  "Type": "KillNPC",
  "Count": 3,
  "NPCGroupId": "Trork_Warrior"
}
```

**Properties:**
- **`Count`** - Number of NPCs to kill
- **`NPCGroupId`** - NPC group ID to target

### Gather

Gather items or blocks:

```json
{
  "Type": "Gather",
  "BlockTagOrItemId": {
    "ItemId": "Soil_Dirt"
  },
  "Count": 3
}
```

**Properties:**
- **`Count`** - Number to gather
- **`BlockTagOrItemId`** - Item ID or block tag to gather

### Craft

Craft specific items:

```json
{
  "Type": "Craft",
  "Count": 1,
  "ItemId": "Furniture_Crude_Bed"
}
```

**Properties:**
- **`Count`** - Number to craft
- **`ItemId`** - Item ID to craft

### ReachLocation

Reach a specific location:

```json
{
  "Type": "ReachLocation",
  "TargetLocation": "ObjectiveReachMarker_Example"
}
```

**Properties:**
- **`TargetLocation`** - Reference to ReachLocationMarker

### UseBlock

Interact with specific blocks:

```json
{
  "Type": "UseBlock",
  "BlockTagOrItemId": {
    "ItemId": "Furniture_Crude_Bed"
  },
  "Count": 1,
  "TaskConditions": [
    {
      "Type": "SoloInventory",
      "BlockTagOrItemId": {
        "ItemId": "Soil_Dirt"
      },
      "Quantity": 2
    }
  ]
}
```

**Properties:**
- **`Count`** - Number of interactions
- **`BlockTagOrItemId`** - Block to interact with
- **`TaskConditions`** - Optional conditions to complete task

### UseEntity

Interact with entities:

```json
{
  "Type": "UseEntity",
  "TaskId": "Trork_Interact",
  "Count": 1,
  "AnimationIdToPlay": "Greet"
}
```

**Properties:**
- **`TaskId`** - Task identifier
- **`Count`** - Number of interactions
- **`AnimationIdToPlay`** - Animation to play on interaction

### TreasureMap

Find treasure chests:

```json
{
  "Type": "TreasureMap",
  "Chests": [
    {
      "MinRadius": 10,
      "MaxRadius": 20,
      "DropList": "Zone1_Encounters_Tier3",
      "WorldLocationCondition": {
        "Type": "LookBlocksBelow",
        "BlockTags": ["Stone", "Soil"],
        "Count": 3,
        "MinRange": 0,
        "MaxRange": 5
      },
      "ChestBlockTypeKey": "Furniture_Ancient_Chest_Large"
    }
  ]
}
```

**Chest Properties:**
- **`MinRadius`** / **`MaxRadius`** - Spawn distance range
- **`DropList`** - Chest contents
- **`WorldLocationCondition`** - Conditions for chest placement
- **`ChestBlockTypeKey`** - Chest block type

### Bounty

Hunt a specific NPC:

```json
{
  "Type": "Bounty",
  "NpcId": "Trork_Warrior",
  "WorldLocationCondition": {
    "Type": "LocationRadius",
    "MinRadius": 10,
    "MaxRadius": 30
  }
}
```

**Properties:**
- **`NpcId`** - Target NPC ID
- **`WorldLocationCondition`** - Where NPC spawns

## Objective Lines

Objective lines chain multiple objectives together:

`Server/Objective/ObjectiveLines/ObjectiveLine_Test.json`:

```json
{
  "ObjectiveIds": [
    "Objective_Gather",
    "Objective_Craft"
  ]
}
```

Player completes objectives in sequence.

## Location Markers

### Objective Location Markers

Triggers objectives when player enters area:

`Server/Objective/ObjectiveLocationMarkers/ObjectiveLocationMarker_Test.json`:

```json
{
  "Setup": {
    "Type": "Objective",
    "ObjectiveId": "Objective_Kill"
  },
  "Area": {
    "Type": "Box",
    "EntryBox": {
      "Min": { "X": -2, "Y": -2, "Z": -2 },
      "Max": { "X": 2, "Y": 2, "Z": 2 }
    },
    "ExitBox": {
      "Min": { "X": -5, "Y": -5, "Z": -5 },
      "Max": { "X": 5, "Y": 5, "Z": 5 }
    }
  }
}
```

**Properties:**
- **`Setup.Type`** - Setup type (`"Objective"`)
- **`Setup.ObjectiveId`** - Objective to trigger
- **`Area.Type`** - Area type (`"Box"`)
- **`Area.EntryBox`** - Box to enter to trigger
- **`Area.ExitBox`** - Box to exit to cancel

### Reach Location Markers

Destination markers for ReachLocation tasks:

`Server/Objective/ReachLocationMarkers/ObjectiveReachMarker_Example.json`:

```json
{
  "Radius": 5,
  "Name": "Test Location"
}
```

**Properties:**
- **`Radius`** - Detection radius (blocks)
- **`Name`** - Display name

## World Location Conditions

### LookBlocksBelow

Check blocks below location:

```json
{
  "Type": "LookBlocksBelow",
  "BlockTags": ["Stone", "Soil"],
  "Count": 3,
  "MinRange": 0,
  "MaxRange": 5
}
```

### LocationRadius

Spawn within radius:

```json
{
  "Type": "LocationRadius",
  "MinRadius": 10,
  "MaxRadius": 30
}
```

## Task Conditions

Conditions that must be met to complete a task:

### SoloInventory

Requires items in inventory:

```json
{
  "Type": "SoloInventory",
  "BlockTagOrItemId": {
    "ItemId": "Soil_Dirt"
  },
  "Quantity": 2
}
```

## Complete Examples

### Kill Quest

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
      "DropList": "Quest_Reward_Tier1"
    }
  ]
}
```

### Multi-Step Quest

```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "Gather",
          "BlockTagOrItemId": { "ItemId": "Ingredient_Bar_Iron" },
          "Count": 5
        }
      ]
    },
    {
      "Tasks": [
        {
          "Type": "Craft",
          "ItemId": "Weapon_Sword_Iron",
          "Count": 1
        }
      ]
    },
    {
      "Tasks": [
        {
          "Type": "KillNPC",
          "NPCGroupId": "Trork_Warrior",
          "Count": 3
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "GiveItems",
      "DropList": "Quest_Reward_Epic"
    }
  ]
}
```

### Treasure Hunt

```json
{
  "TaskSets": [
    {
      "Tasks": [
        {
          "Type": "TreasureMap",
          "Chests": [
            {
              "MinRadius": 50,
              "MaxRadius": 100,
              "DropList": "Treasure_Rare",
              "WorldLocationCondition": {
                "Type": "LookBlocksBelow",
                "BlockTags": ["Stone"],
                "Count": 5,
                "MinRange": 0,
                "MaxRange": 10
              },
              "ChestBlockTypeKey": "Furniture_Ancient_Chest_Large"
            }
          ]
        }
      ]
    }
  ],
  "Completions": [
    {
      "Type": "ClearObjectiveItems"
    }
  ],
  "RemoveOnItemDrop": true
}
```

---

**Previous:** [Macro Commands](167_Macro_Commands.md) | **Next:** [NPC Models](169_NPC_Models.md)
