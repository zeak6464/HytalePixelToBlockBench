# NPC Groups

Learn how to define NPC groups for faction relationships, combat targeting, and AI behavior.

## Overview

NPC groups define relationships between NPCs, allowing them to recognize allies, enemies, and neutral entities. They're used for combat AI, flocking behavior, and spawning logic. NPC groups are defined in `Server/NPC/Groups/`.

## Location
`Server/NPC/Groups/`

## Example from Game Files

### Livestock NPC Group

From `Server/NPC/Groups/Livestock.json`:

```1:16:Server/NPC/Groups/Livestock.json
{
  "IncludeGroups": [
    "Bison",
    "Chicken",
    "Cow",
    "Pig",
    "Sheep",
    "Mouflon",
    "Turkey",
    "Chicken_Desert",
    "Goat",
    "Rabbit",
    "Camel",
    "Deer"
  ]
}
```

This shows an NPC group that includes multiple livestock creature groups.

### “Edible” NPC group (used by AI filters)

From `Server/NPC/Groups/Creature/Vermin/Edible_Rat.json`:

```1:5:Server/NPC/Groups/Creature/Vermin/Edible_Rat.json
{
  "IncludeRoles": [
    "Edible_Rat"
  ]
}
```

NPC groups are frequently used as **filters** in sensors. For example, `Template_Goblin_Ogre` searches for nearby edible NPCs using an `NPCGroup` filter:

```501:513:Server/NPC/Roles/Intelligent/Aggressive/Goblin/Templates/Template_Goblin_Ogre.json
            {
              "Sensor": {
                "Type": "Mob",
                "Range": 2.5,
                "Filters": [
                  {
                    "Type": "NPCGroup",
                    "IncludeGroups": { "Compute": "FoodNPCGroups" }
                  },
                  {
                    "Type": "LineOfSight"
                  }
                ]
              },
```

## Basic NPC Group Structure

Create `Server/NPC/Groups/MyCustom_Faction.json`:

```json
{
  "IncludeRoles": [
    "MyCustom_*",
    "Custom_NPC_*"
  ],
  "ExcludeRoles": [
    "*Enemy*"
  ]
}
```

## NPC Group Properties

### IncludeRoles

```json
{
  "IncludeRoles": [
    "Goblin_*",
    "Trork_*"
  ]
}
```

Wildcard patterns for NPC roles to include in the group:
- **`"Goblin_*"`** - All NPCs starting with `Goblin_`
- **`"Trork_Chieftain"`** - Specific NPC role
- **`"*Aggressive"`** - All NPCs ending with `Aggressive`

### ExcludeRoles

```json
{
  "ExcludeRoles": [
    "*Boss*",
    "*Elite"
  ]
}
```

Wildcard patterns for NPC roles to exclude from the group.

### IncludeGroups

```json
{
  "IncludeGroups": [
    "Goblin",
    "Predators"
  ]
}
```

References to other NPC groups to include (nesting groups).

### ExcludeGroups

```json
{
  "ExcludeGroups": [
    "Livestock",
    "Critters"
  ]
}
```

References to other NPC groups to exclude.

## Common NPC Groups

### Predators

`Server/NPC/Groups/Predators.json`:

```json
{
  "IncludeRoles": [
    "Fox*",
    "Hyena*",
    "Fen_Stalker",
    "Spark*",
    "Toad*"
  ]
}
```

Predatory animals that hunt other creatures.

### Livestock

`Server/NPC/Groups/Livestock.json`:

```json
{
  "IncludeGroups": [
    "Bison",
    "Chicken",
    "Cow",
    "Pig",
    "Sheep",
    "Mouflon",
    "Turkey",
    "Chicken_Desert",
    "Goat",
    "Rabbit",
    "Camel",
    "Deer"
  ]
}
```

Tameable farm animals.

### Prey

`Server/NPC/Groups/Prey.json`:

```json
{
  "IncludeGroups": [
    "Livestock",
    "Deer",
    "Rabbit"
  ]
}
```

Animals that are hunted by predators.

### Undead

`Server/NPC/Groups/Undead.json`:

```json
{
  "IncludeGroups": [
    "Skeleton",
    "Zombie"
  ]
}
```

Undead NPCs.

### Intelligent Aggressive

`Server/NPC/Groups/Intelligent/Aggressive/Goblin/Goblin.json`:

```json
{
  "IncludeRoles": [
    "Goblin_*"
  ]
}
```

Aggressive intelligent NPCs (goblins, trorks).

### Self

`Server/NPC/Groups/Self.json`:

```json
{
  "IncludeRoles": [
    "*"
  ]
}
```

Same NPC type (used for flocking and avoiding same type).

### Player

`Server/NPC/Groups/Player.json`:

```json
{
  "IncludeRoles": [
    "Player"
  ]
}
```

Player characters.

## Using NPC Groups

### In NPC Role Definition

Reference groups in NPC behavior:

```json
{
  "Instructions": [
    {
      "Sensor": {
        "Type": "Mob",
        "Range": 15,
        "LockOnTarget": true,
        "Filters": [
          {
            "Type": "NPCGroup",
            "IncludeGroups": ["Prey"]
          }
        ]
      },
      "Actions": [
        {
          "Type": "Attack"
        }
      ]
    }
  ]
}
```

### In Combat AI

NPCs can target specific groups:

```json
{
  "CombatConfig": {
    "TargetGroups": ["Prey", "Player"],
    "AvoidGroups": ["Predators", "Self"]
  }
}
```

### In Flocking Behavior

Flock members use groups to identify flock mates:

```json
{
  "FlockAllowedNPC": ["Goblin_*"],
  "FlockCanLead": true,
  "FlockInfluenceRange": 4
}
```

### In NPC Spawning

Groups control which NPCs can spawn together:

```json
{
  "NPCs": [
    {
      "Id": "Goblin_Scrapper",
      "Flock": "Group_Small",
      "ExcludeGroups": ["Predators"]
    }
  ]
}
```

## Complete Example: Custom Faction

### 1. Group Definition

`Server/NPC/Groups/MyCustom_Faction.json`:

```json
{
  "IncludeRoles": [
    "MyCustom_Knight*",
    "MyCustom_Guard*",
    "MyCustom_Soldier*"
  ],
  "ExcludeRoles": [
    "*Boss*",
    "*Elite"
  ]
}
```

### 2. Using in NPC Role

`Server/NPC/Roles/Intelligent/Aggressive/MyCustom/MyCustom_Knight.json`:

```json
{
  "Type": "Variant",
  "Reference": "Template_Fighter",
  "Modify": {
    "DefaultPlayerAttitude": "Hostile",
    "Instructions": [
      {
        "Sensor": {
          "Type": "Mob",
          "Range": 20,
          "LockOnTarget": true,
          "Filters": [
            {
              "Type": "NPCGroup",
              "IncludeGroups": ["Enemy_Faction"]
            }
          ]
        },
        "Actions": [
          {
            "Type": "Attack"
          }
        ]
      },
      {
        "Sensor": {
          "Type": "Mob",
          "Range": 10,
          "Filters": [
            {
              "Type": "NPCGroup",
              "IncludeGroups": ["MyCustom_Faction"]
            },
            {
              "Type": "Health",
              "Max": 0.5
            }
          ]
        },
        "Actions": [
          {
            "Type": "Defend"
          }
        ]
      }
    ]
  }
}
```

### 3. Enemy Faction Definition

`Server/NPC/Groups/Enemy_Faction.json`:

```json
{
  "IncludeRoles": [
    "Enemy_Knight*",
    "Enemy_Guard*"
  ]
}
```

## Group Hierarchy

Groups can include other groups:

```json
{
  "IncludeGroups": [
    "Goblin",
    "Trork",
    "Predators"
  ]
}
```

This creates a hierarchy where the parent group contains all members of child groups.

## Tips for Creating NPC Groups

1. **Use wildcards** - Pattern matching is flexible for grouping similar NPCs
2. **Create hierarchies** - Use `IncludeGroups` to nest groups logically
3. **Exclude carefully** - Remove NPCs that shouldn't be in the group
4. **Name consistently** - Use clear, descriptive names for groups
5. **Use in AI** - Reference groups in combat, flocking, and behavior logic
6. **Test relationships** - Verify NPCs interact correctly with group members
7. **Document groups** - Keep track of which NPCs belong to which groups

---

**Previous:** [Block Particle Sets](42_Block_Particle_Sets.md) | **Next:** [Response Curves](44_Response_Curves.md)
