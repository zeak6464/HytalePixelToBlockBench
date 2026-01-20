# Creating NPCs

Learn how to create NPCs, configure their spawning, and set up drop tables.

## Location
`Server/NPC/Roles/`

## Example from Game Files

### Goblin Scrapper NPC

From `Server/NPC/Roles/Intelligent/Aggressive/Goblin/Goblin_Scrapper.json`:

```1:20:Server/NPC/Roles/Intelligent/Aggressive/Goblin/Goblin_Scrapper.json
{
  "Type": "Variant",
  "Reference": "Template_Goblin_Scrapper",
  "Modify": {
    "_CombatConfig": "CAE_Goblin_Scrapper",
    "MaxHealth": 38,
    "IsMemory": true,
    "MemoriesCategory": "Goblin",
    "MemoriesNameOverride": "Goblin_Scrapper",
    "NameTranslationKey": {
      "Compute": "NameTranslationKey"
    }
  },
  "Parameters": {
    "NameTranslationKey": {
      "Value": "server.npcRoles.Goblin_Scrapper.name",
      "Description": "Translation key for NPC name display"
    }
  }
}
```

This shows a complete NPC variant that inherits from a template and modifies specific properties.

## NPC Structure Overview

NPCs require two main components:
1. **Role** (`Server/NPC/Roles/`) - Behavior and stats
2. **Model** (`Server/Models/`) - Visual appearance

## Creating a Variant NPC

Create `Server/NPC/Roles/Intelligent/Aggressive/MyCustom/Custom_Goblin.json`:

```json
{
  "Type": "Variant",
  "Reference": "Template_Goblin_Scrapper",
  "Modify": {
    "_CombatConfig": "CAE_Goblin_Scrapper",
    "MaxHealth": 75,
    "IsMemory": true,
    "MemoriesCategory": "Goblin",
    "MemoriesNameOverride": "Elite_Goblin",
    "NameTranslationKey": {
      "Compute": "NameTranslationKey"
    }
  },
  "Parameters": {
    "NameTranslationKey": {
      "Value": "server.npcRoles.Custom_Goblin.name",
      "Description": "Translation key for NPC name display"
    }
  }
}
```

## Creating an Abstract Template

```json
{
  "Type": "Abstract",
  "StartState": "Idle",
  "Parameters": {
    "Appearance": {
      "Value": "Goblin_Scrapper",
      "Description": "Model to be used"
    },
    "ViewRange": {
      "Value": 15,
      "Description": "View range"
    },
    "HearingRange": {
      "Value": 8,
      "Description": "Hearing range"
    },
    "MaxHealth": {
      "Value": 100,
      "Description": "Max health for the NPC"
    },
    "Weapons": {
      "Value": ["Weapon_Club_Scrap"],
      "Description": "NPC Weapons"
    },
    "DefaultPlayerAttitude": {
      "Value": "Hostile",
      "Description": "Default attitude towards players"
    }
  },
  "Appearance": { "Compute": "Appearance" },
  "MaxHealth": { "Compute": "MaxHealth" },
  "DefaultPlayerAttitude": { "Compute": "DefaultPlayerAttitude" }
}
```

## NPC Model Configuration

Create `Server/Models/Intelligent/MyCustom/Custom_Goblin.json`:

```json
{
  "Parent": "Goblin",
  "Model": "NPC/Intelligent/Goblin/Models/Model.blockymodel",
  "Texture": "NPC/Intelligent/Goblin/Models/Model_Textures/Moldy.png",
  "DefaultAttachments": [
    {
      "Model": "NPC/Intelligent/Goblin/Models/Attachments/Cosmetics/Rag/Rag_Chest.blockymodel",
      "Texture": "NPC/Intelligent/Goblin/Models/Attachments/Cosmetics/Rag/Rag_Chest_Brown.png"
    }
  ]
}
```

### NPC Hitbox Configuration

NPC hitboxes define collision detection and can be set to override a parent's hitbox.

**Basic Hitbox:**
```json
{
  "HitBox": {
    "Max": {
      "X": 0.3,
      "Y": 1.5,
      "Z": 0.3
    },
    "Min": {
      "X": -0.3,
      "Y": 0,
      "Z": -0.3
    }
  }
}
```

**Hitbox Properties:**
- **`Min`** - Minimum corner of the collision box (bottom-left-front)
- **`Max`** - Maximum corner of the collision box (top-right-back)
- **Coordinates** - Relative to NPC's origin/feet (blocks)

**Example: Humanoid NPC Hitbox (Klops)**
```json
{
  "HitBox": {
    "Max": {
      "X": 0.3,   // Width (0.6 blocks total)
      "Y": 1.5,   // Height
      "Z": 0.3    // Depth (0.6 blocks total)
    },
    "Min": {
      "X": -0.3,
      "Y": 0,     // Starts at feet
      "Z": -0.3
    }
  }
}
```

**Example: Small NPC Hitbox (Cat)**
```json
{
  "HitBox": {
    "Max": {
      "X": 0.3,
      "Y": 0.9,   // Shorter height
      "Z": 0.3
    },
    "Min": {
      "X": -0.3,
      "Y": 0,
      "Z": -0.3
    }
  }
}
```

**Hitbox Size Guidelines:**
- **Humanoid NPCs**: Width 0.3-0.325, Height 1.5-1.85
- **Small NPCs**: Width 0.2-0.3, Height 0.7-1.0
- **Large NPCs**: Width 0.5-1.0+, Height 2.0+
- **Width/Depth**: Usually same value (square cross-section)

**Inheriting Hitboxes:**
```json
{
  "Parent": "Player"  // Inherits hitbox from Player model
}
```

When using `Parent`, the NPC inherits the parent's hitbox unless you override it with a `HitBox` property.

**Overriding Parent Hitbox:**
```json
{
  "Parent": "Goblin",
  "HitBox": {
    "Max": {
      "X": 0.35,
      "Y": 1.6,
      "Z": 0.35
    },
    "Min": {
      "X": -0.35,
      "Y": 0,
      "Z": -0.35
    }
  }
}
```

This overrides the parent Goblin's hitbox with a custom size.

## NPC Immunities and Resistance

**Update 1 Note:** NPCs can have damage immunities and knockback resistance.

### Damage Immunities

Certain NPCs are immune to specific damage types:

- **Fire-themed NPCs** - Immune to fire damage (still catch fire visually for visual effect)
- **Kweebecs** - Immune to environmental damage from cactus/brambles
- **Skeletons** - No longer take drowning damage

**Example: Fire Immunity**
```json
{
  "Type": "Variant",
  "Reference": "Template_Fire_Elemental",
  "Modify": {
    "Immunities": {
      "Fire": true  // Immune to fire damage
    }
  }
}
```

### Knockback Resistance

**Update 1 Note:** NPCs now have knockback resistance. NPCs with knockback resistance will be pushed less by knockback effects from damage.

This affects how much entities are pushed by damage effects:
```json
{
  "DamageEffects": {
    "Knockback": {
      "Force": 5.5,
      "VelocityY": 5
    }
  }
}
```

NPCs with knockback resistance will experience reduced knockback from the same force.

### Environmental Damage Changes

**Update 1 Note:** Cactus and brambles now deal **Environmental** damage type. NPCs that are immune to environmental damage (like Kweebecs) will not take damage from these sources.

## Available NPC Categories

| Folder | Examples |
|--------|----------|
| `Intelligent/Aggressive/` | Goblins, Trorks, Skeletons |
| `Intelligent/Neutral/` | Kweebecs, Feran |
| `Intelligent/Passive/` | Villagers |
| `Creature/` | Bears, Wolves, Deer |
| `Undead/` | Zombies, Skeletons |
| `Elemental/` | Golems |

---

## Creating Random World Spawns

### Location
`Server/NPC/Spawn/World/`

### Overview

Random world spawns make NPCs appear naturally in specific environments based on:
- **Environment types** (biomes like Forests, Plains, Deserts, etc.)
- **Block types** they can spawn on (Soil, Rock, Sand, etc.)
- **Time of day** restrictions
- **Weight values** for spawn probability

### Basic Spawn Configuration

Create a new spawn file (e.g., `Server/NPC/Spawn/World/Zone1/Spawns_Zone1_MyCustom_NPC.json`):

```json
{
  "Environments": [
    "Env_Zone1_Plains",
    "Env_Zone1_Forests"
  ],
  "NPCs": [
    {
      "Weight": 10,
      "SpawnBlockSet": "Soil",
      "Id": "Goblin_Scrapper",
      "Flock": "Group_Small"
    }
  ],
  "DayTimeRange": [6, 18]
}
```

### Field Explanations

#### `Environments`
Array of environment/biome IDs where the NPCs can spawn:
- `Env_Zone1_Plains` - Plains biome in Zone 1
- `Env_Zone1_Forests` - Forest biome in Zone 1
- `Env_Zone1_Mountains` - Mountain biome in Zone 1
- `Env_Zone1_Swamps` - Swamp biome in Zone 1
- `Env_Zone2_Deserts` - Desert biome in Zone 2
- `Env_Zone2_Savanna` - Savanna biome in Zone 2
- `Env_Zone3_Tundra` - Tundra biome in Zone 3
- `Env_Zone4_Jungles` - Jungle biome in Zone 4

#### `NPCs` Array

Each entry defines one spawnable NPC:

| Field | Type | Description |
|-------|------|-------------|
| `Weight` | Number | Spawn probability (higher = more common) |
| `SpawnBlockSet` | String | Block types the NPC can spawn on |
| `Id` | String | NPC Role ID (from `Server/NPC/Roles/`) |
| `Flock` | String/Object | Optional: Group size configuration |

#### `SpawnBlockSet` Values

| Value | Description |
|-------|-------------|
| `Soil` | Grass, dirt, and soil blocks |
| `Sand` | Sand blocks |
| `Rock` | Stone and rock blocks |
| `StoneAndSoil` | Both stone and soil blocks |
| `Birds` | Special spawn set for flying creatures |

#### `Flock` Configuration

**Using predefined flock types:**
```json
{
  "Id": "Goblin_Scrapper",
  "Flock": "Group_Small"
}
```

Available flock types:
- `One_Or_Two` - Spawns 1-2 NPCs
- `Group_Tiny` - Small group (1-3)
- `Group_Small` - Small group (2-4)
- `Group_Medium` - Medium group (3-5)
- `Group_Large` - Large group (4-8)

**Custom flock size:**
```json
{
  "Id": "Skeleton_Fighter",
  "Flock": {
    "Size": [1, 3]
  }
}
```

#### `DayTimeRange`

Controls when NPCs can spawn (24-hour format):
- `[6, 18]` - Only spawn during day (6 AM to 6 PM)
- `[0, 24]` - Spawn at any time
- `[18, 6]` - Only spawn at night (6 PM to 6 AM)

### Complete Example: Custom Goblin Spawn

```json
{
  "Environments": [
    "Env_Zone1_Plains",
    "Env_Zone1_Forests",
    "Env_Zone1_Swamps"
  ],
  "NPCs": [
    {
      "Weight": 5,
      "SpawnBlockSet": "Soil",
      "Id": "Goblin_Scrapper",
      "Flock": {
        "Size": [2, 4]
      }
    },
    {
      "Weight": 2,
      "SpawnBlockSet": "Soil",
      "Id": "Goblin_Lobber",
      "Flock": "Group_Medium"
    }
  ],
  "DayTimeRange": [18, 6]
}
```

This configuration:
- Spawns goblins in Plains, Forests, and Swamps biomes
- Only spawns at night (6 PM to 6 AM)
- Spawns on soil blocks
- `Goblin_Scrapper` has weight 5 (more common)
- `Goblin_Lobber` has weight 2 (rarer)
- Both spawn in groups of varying sizes

### Multiple NPC Types in One File

You can include multiple NPCs with different spawn conditions:

```json
{
  "Environments": [
    "Env_Zone1_Forests"
  ],
  "NPCs": [
    {
      "Weight": 10,
      "SpawnBlockSet": "Soil",
      "Id": "Bear_Grizzly"
    },
    {
      "Weight": 15,
      "SpawnBlockSet": "Soil",
      "Id": "Deer_Stag",
      "Flock": "Group_Small"
    },
    {
      "Weight": 5,
      "SpawnBlockSet": "Birds",
      "Id": "Hawk"
    }
  ],
  "DayTimeRange": [6, 18]
}
```

### Weight System Explained

Weights are relative. Higher numbers spawn more frequently:

- Weight `1` = Very rare
- Weight `5` = Uncommon
- Weight `10` = Common
- Weight `40` = Very common

In a file with NPCs of weight 10, 5, and 1:
- The weight 10 NPC spawns 10/16 times (~62%)
- The weight 5 NPC spawns 5/16 times (~31%)
- The weight 1 NPC spawns 1/16 times (~6%)

### Zone Organization

Spawn files are organized by zone:

```
Server/NPC/Spawn/World/
├── Zone1/              # Zone 1 spawns
│   ├── Spawns_Zone1_Plains_Animal.json
│   ├── Spawns_Zone1_Forests_Predator.json
│   └── Wander_Zone1_Tier1.json
├── Zone2/              # Zone 2 spawns
├── Zone3/              # Zone 3 spawns
└── Zone4/              # Zone 4 spawns
```

### File Naming Convention

Common naming patterns:
- `Spawns_Zone{Number}_{Biome}_{Category}.json`
  - Example: `Spawns_Zone1_Plains_Animal.json`
- `Wander_Zone{Number}_Tier{Tier}.json`
  - Example: `Wander_Zone1_Tier1.json`

### Tips for Random Spawning

1. **Start with existing files** - Copy a similar biome spawn file and modify it
2. **Test weights** - Adjust weights until you get desired spawn frequency
3. **Match environments** - Use NPCs that make sense for the biome
4. **Time restrictions** - Use `DayTimeRange` to create day/night spawn variations
5. **Block sets** - Ensure your `SpawnBlockSet` matches the biome (sand for deserts, soil for plains, etc.)
6. **NPC ID must exist** - The `Id` field must reference an existing NPC Role in `Server/NPC/Roles/`

---

## Creating Drops

### Location
`Server/Drops/`

### Drop Table Structure

Create `Server/Drops/NPCs/Intelligent/MyCustom/Drop_Custom_Goblin.json`:

```json
{
  "Container": {
    "Type": "Multiple",
    "Containers": [
      {
        "Type": "Choice",
        "Weight": 25,
        "Containers": [
          {
            "Type": "Single",
            "Item": {
              "ItemId": "Weapon_Sword_Iron"
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
              "QuantityMax": 5
            }
          }
        ]
      }
    ]
  }
}
```

### Container Types

| Type | Description |
|------|-------------|
| `Single` | Drops a single item |
| `Choice` | Random selection based on weight |
| `Multiple` | Contains multiple sub-containers |

### Linking Drops to NPCs

In your NPC role file, set the `DropList` parameter:

```json
"Parameters": {
  "DropList": {
    "Value": "Drop_Custom_Goblin",
    "Description": "Drop Items"
  }
}
```

---

## Creating Pets & Companions

### Overview

Pets are NPCs that follow and accompany players. There are two main approaches:
1. **Tameable animals** - NPCs that follow players when they hold specific items (using `LovedItems`/`AttractiveItems`)
2. **Interactive pets** - NPCs that become pets when interacted with (using custom states and instructions)

### Method 1: Tameable Animals (Simple)

Animals can follow players when they hold specific items. This uses the `LovedItems` and `AttractiveItems` properties.

#### Basic Tameable Pet

Create `Server/NPC/Roles/Creature/MyCustom/MyCustom_Pet.json`:

```json
{
  "Type": "Variant",
  "Reference": "Template_Animal_Neutral",
  "Modify": {
    "Appearance": "MyCustom_Animal",
    "LovedItems": [
      "Food_Apple"
    ],
    "ChanceToTurnFriendly": 30,
    "ChanceToTurnFriendlyWithAttractiveItem": 60,
    "WeightGreet": 1,
    "WeightFollowItem": 20,
    "WeightFollow": 40,
    "WeightIgnore": 10,
    "MaxHealth": 50,
    "NameTranslationKey": {
      "Compute": "NameTranslationKey"
    }
  },
  "Parameters": {
    "NameTranslationKey": {
      "Value": "server.npcRoles.MyCustom_Pet.name",
      "Description": "Translation key for NPC name display"
    }
  }
}
```

#### Key Properties for Tameable Pets

**LovedItems:**
```json
"LovedItems": [
  "Plant_Crop_Carrot_Item",
  "Food_Apple"
]
```
- Items that **guarantee** the NPC will follow the player
- NPC becomes friendly when player holds these items
- Spawns heart particles when following

**AttractiveItems:**
```json
"AttractiveItems": [
  "Tool_Feedbag"
]
```
- Items that have a **chance** to make NPC follow
- Less reliable than LovedItems
- NPC will be interested but may lose interest after timeout

**Friendliness Properties:**
```json
"ChanceToTurnFriendly": 30,  // % chance to become friendly normally
"ChanceToTurnFriendlyWithAttractiveItem": 60,  // % chance with attractive item
```

**Behavior Weights:**
```json
"WeightGreet": 1,        // How often to greet players
"WeightFollowItem": 20,  // How often to follow item holders
"WeightFollow": 40,      // How often to follow friendly players
"WeightIgnore": 10       // How often to ignore players
```

Higher weights = more likely behavior.

#### Complete Example: Tameable Rabbit

```json
{
  "Type": "Variant",
  "Reference": "Template_Animal_Neutral",
  "Modify": {
    "Appearance": "Rabbit",
    "FlockArray": ["Rabbit", "Bunny"],
    "LovedItems": [
      "Plant_Crop_Carrot_Item"
    ],
    "DropList": "Drop_Rabbit",
    "MaxHealth": 49,
    "MaxSpeed": 8,
    "AlertedActionRange": 16,
    "AbsoluteDetectionRange": 4,
    "ChanceToTurnFriendly": 30,
    "ChanceToTurnFriendlyWithAttractiveItem": 60,
    "WeightGreet": 1,
    "WeightFollowItem": 20,
    "WeightFollow": 40,
    "WeightIgnore": 10,
    "Timid": true,
    "IsMemory": true,
    "NameTranslationKey": {
      "Compute": "NameTranslationKey"
    }
  },
  "Parameters": {
    "NameTranslationKey": {
      "Value": "server.npcRoles.Rabbit.name",
      "Description": "Translation key for NPC name display"
    }
  }
}
```

### Method 2: Interactive Pets (Advanced)

Pets that become companions when interacted with. They use custom states and instructions.

#### Basic Interactive Pet Structure

Create `Server/NPC/Roles/Pets/MyCustom_Pet.json`:

```json
{
  "Type": "Generic",
  "Appearance": "Corgi",
  "StartState": "Idle",
  "MaxHealth": 50,
  "MotionControllerList": [
    {
      "Type": "Walk",
      "MaxWalkSpeed": 3,
      "Gravity": 10,
      "MaxFallSpeed": 20,
      "Acceleration": 10
    }
  ],
  "Instructions": [
    {
      "Instructions": [
        {
          "Sensor": {
            "Type": "State",
            "State": "Idle"
          },
          "Instructions": [
            {
              "Sensor": {
                "Type": "Player",
                "Range": 15,
                "LockOnTarget": true
              },
              "BodyMotion": {
                "Type": "Seek",
                "SlowDownDistance": 5,
                "StopDistance": 3,
                "RelativeSpeed": 1
              }
            }
          ]
        },
        {
          "Sensor": {
            "Type": "State",
            "State": "Pet"
          },
          "Instructions": [
            {
              "Sensor": {
                "Type": "Target"
              },
              "BodyMotion": {
                "Type": "MaintainDistance",
                "DesiredDistanceRange": [2, 3],
                "RelativeForwardsSpeed": 1
              }
            }
          ]
        }
      ]
    }
  ],
  "InteractionInstruction": {
    "Instructions": [
      {
        "Sensor": {
          "Type": "HasInteracted"
        },
        "Instructions": [
          {
            "Sensor": {
              "Type": "Not",
              "Sensor": {
                "Type": "State",
                "State": "Pet"
              }
            },
            "Actions": [
              {
                "Type": "LockOnInteractionTarget"
              },
              {
                "Type": "State",
                "State": "Pet"
              }
            ]
          }
        ]
      }
    ]
  },
  "NameTranslationKey": "server.npcRoles.MyCustom_Pet.name"
}
```

#### Key Components for Interactive Pets

**Pet State:**
- When interacted with, the NPC enters the "Pet" state
- In this state, the pet follows the player who interacted

**InteractionInstruction:**
```json
"InteractionInstruction": {
  "Instructions": [
    {
      "Sensor": {
        "Type": "HasInteracted"
      },
      "Actions": [
        {
          "Type": "LockOnInteractionTarget"  // Lock onto the player
        },
        {
          "Type": "State",
          "State": "Pet"  // Enter pet state
        }
      ]
    }
  ]
}
```

**Following Behavior:**
```json
{
  "Sensor": {
    "Type": "State",
    "State": "Pet"
  },
  "Instructions": [
    {
      "Sensor": {
        "Type": "Target"  // The locked-on target
      },
      "BodyMotion": {
        "Type": "MaintainDistance",
        "DesiredDistanceRange": [2, 3],
        "RelativeForwardsSpeed": 1
      }
    }
  ]
}
```

### Pet Model Configuration

Create `Server/Models/Pets/MyCustom_Pet.json`:

```json
{
  "Model": "NPC/Pets/MyCustom_Pet/Models/Model.blockymodel",
  "Texture": "NPC/Pets/MyCustom_Pet/Models/Texture.png",
  "EyeHeight": 0.7,
  "HitBox": {
    "Max": {
      "X": 0.3,
      "Y": 0.9,
      "Z": 0.3
    },
    "Min": {
      "X": -0.3,
      "Y": 0,
      "Z": -0.3
    }
  },
  "AnimationSets": {
    "Idle": {
      "Animations": [
        {
          "Animation": "NPC/Pets/MyCustom_Pet/Animations/Idle.blockyanim"
        }
      ]
    },
    "Walk": {
      "Animations": [
        {
          "Animation": "NPC/Pets/MyCustom_Pet/Animations/Walk.blockyanim"
        }
      ]
    },
    "Run": {
      "Animations": [
        {
          "Animation": "NPC/Pets/MyCustom_Pet/Animations/Run.blockyanim"
        }
      ]
    },
    "Jump": {
      "Animations": [
        {
          "Animation": "NPC/Pets/MyCustom_Pet/Animations/Jump.blockyanim",
          "BlendingDuration": 0.1,
          "Looping": false
        }
      ]
    }
  }
}
```

### Comparison: Tameable vs Interactive Pets

| Feature | Tameable Animals | Interactive Pets |
|---------|------------------|------------------|
| **Activation** | Hold specific item | Interact with NPC |
| **Persistence** | Temporary (while item held) | Permanent (until state changes) |
| **Complexity** | Simple (uses template) | Complex (custom instructions) |
| **Best For** | Livestock, wild animals | True companions, pets |

### Tips for Creating Pets

1. **Start with Template_Animal_Neutral** - For simple tameable pets
2. **Use LovedItems** - Guaranteed following behavior
3. **Set appropriate weights** - Balance follow vs ignore behaviors
4. **Pet state management** - Interactive pets need careful state handling
5. **Follow distance** - Adjust `DesiredDistanceRange` for comfortable following
6. **Speed matching** - Set `RelativeSpeed` to match player movement
7. **Animations** - Ensure pet models have all necessary animations (Idle, Walk, Run, Jump)
8. **Hitbox sizing** - Size hitboxes appropriately for the pet model

### Example Pet Items

**Items commonly used for taming:**
- `Plant_Crop_Carrot_Item` - For rabbits
- `Plant_Crop_Corn_Item` - For turkeys/chickens
- `Tool_Feedbag` - General attractive item
- Custom food items for your pets

---

**Previous:** [Creating Items](02_Items.md) | **Next:** [Potions & Effects](04_Potions_and_Effects.md)
