# Hytale Patch Notes — Update 3 (February 17, 2026)

Modding-relevant changes from [Hytale Update 3](https://hytale.com/news/2026/2/hytale-patch-notes-update-3). Use this alongside the official patch notes when updating mods and guides.

---

## Fire Spread System

New fluid ticker system for fire propagation.

From `Server/Item/Block/Fluids/Fire.json`:

```json
{
  "MaxFluidLevel": 8,
  "Effect": ["Lava"],
  "Opacity": "Transparent",
  "DrawType": "None",
  "Light": {
    "Color": "#e90"
  },
  "Particles": [
    {
      "SystemId": "Fluid_Fire",
      "Scale": 1,
      "PositionOffset": { "Y": 0.5 }
    }
  ],
  "Ticker": {
    "Type": "Fire",
    "CanDemote": false,
    "SpreadFluid": "Fire",
    "FlowRate": 2.0,
    "SupportedBy": "Fire",
    "Flammability": [
      {
        "TagPattern": { "Op": "Equals", "Tag": "Plant" },
        "Priority": 0,
        "BurnLevel": 3,
        "BurnChance": 0.9
      },
      {
        "TagPattern": {
          "Op": "And",
          "Patterns": [
            { "Op": "Equals", "Tag": "Type=Wood" },
            { "Op": "Not", "Pattern": { "Op": "Equals", "Tag": "Family=Burnt" } }
          ]
        },
        "Priority": 0,
        "BurnLevel": 6,
        "BurnChance": 0.5
      }
    ]
  }
}
```

### Fire Ticker Properties

| Property | Type | Description |
|----------|------|-------------|
| `Type` | String | Ticker type - `"Fire"` for fire spread |
| `SpreadFluid` | String | Fluid ID that spreads from fire |
| `FlowRate` | Number | Speed of fire spreading |
| `SupportedBy` | String | Block/fluid that sustains the fire |
| `Flammability` | Array | Rules for what blocks catch fire |

### Flammability Rules

| Property | Type | Description |
|----------|------|-------------|
| `TagPattern` | Object | Tag matching pattern (Op: Equals, And, Or, Not) |
| `Priority` | Number | Higher priority rules override lower |
| `BurnLevel` | Number | Fire intensity when burning |
| `BurnChance` | Number | 0-1 probability of catching fire |
| `ResultingBlock` | String | Block to replace with when burned (e.g., burnt wood) |

---

## Grass Spreading (Random Block Ticking)

New `RandomTickProcedure` system allows blocks to spread over time.

From `Server/Item/Items/Soil/Grass/Soil_Grass.json`:

```json
{
  "BlockType": {
    "Group": "Grass",
    "RandomTickProcedure": {
      "Type": "SpreadTo",
      "AllowedTag": "Spreadable=Grass",
      "SpreadDirections": [
        { "X": -1, "Z": 0 },
        { "X": 1, "Z": 0 },
        { "X": 0, "Z": -1 },
        { "X": 0, "Z": 1 },
        { "X": -1, "Z": -1 },
        { "X": 1, "Z": -1 },
        { "X": -1, "Z": 1 },
        { "X": 1, "Z": 1 }
      ],
      "MaxY": 1,
      "MinY": -1,
      "RevertBlock": "Soil_Dirt"
    }
  }
}
```

### RandomTickProcedure Properties

| Property | Type | Description |
|----------|------|-------------|
| `Type` | String | Procedure type - `"SpreadTo"` for spreading |
| `AllowedTag` | String | Tag that target blocks must have to be converted |
| `SpreadDirections` | Array | X/Z offsets defining spread pattern |
| `MaxY` | Number | Maximum vertical offset for spreading |
| `MinY` | Number | Minimum vertical offset for spreading |
| `RevertBlock` | String | Block to revert to if conditions fail |

**Note:** Grass spreading currently only works in Creative mode.

---

## Sickles (New Tool Type)

New farming tool that harvests multiple crops in a single swing.

From `Server/Item/Items/Tool/Sickle/Tool_Sickle_Crude.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Tool_Sickle_Crude.name",
    "Description": "server.items.Tool_Sickle.description"
  },
  "Categories": ["Items.Tools"],
  "Set": "Tool_Stone",
  "PlayerAnimationsId": "Sickle",
  "Quality": "Common",
  "ItemLevel": 5,
  "Recipe": {
    "TimeSeconds": 1.5,
    "Input": [
      { "ResourceTypeId": "Wood_Trunk", "Quantity": 2 },
      { "ResourceTypeId": "Rock", "Quantity": 1 }
    ],
    "BenchRequirement": [
      {
        "Type": "Crafting",
        "Categories": ["Farming"],
        "Id": "Farmingbench",
        "RequiredTierLevel": 2
      }
    ]
  },
  "Utility": { "Compatible": true },
  "Weapon": {},
  "Interactions": { "Primary": "Sickle_Attack" },
  "MaxDurability": 100,
  "Tags": { "Type": ["Tool"] }
}
```

### Sickle Interaction: HarvestCrop

From `Server/Item/Interactions/Weapons/Sickle/Attacks/Swing_Right/Sickle_Swing_Right_Selector.json`:

```json
{
  "Type": "Selector",
  "Selector": {
    "Id": "Horizontal",
    "Direction": "ToRight",
    "TestLineOfSight": true,
    "StartDistance": 0.1,
    "EndDistance": 2.5,
    "Length": 70,
    "YawStartOffset": -20
  },
  "HitBlock": {
    "Interactions": [
      {
        "Type": "HarvestCrop",
        "RequireNotBroken": true,
        "Next": {
          "Type": "ModifyInventory",
          "AdjustHeldItemDurability": -1
        },
        "Failed": "Block_Break_Adventure"
      }
    ]
  }
}
```

The `HarvestCrop` interaction type harvests crops within the selector arc.

---

## Animal Taming System

New taming mechanics for friendly animals.

From `Server/NPC/Roles/_Core/Templates/Template_Animal_Neutral.json`:

```json
{
  "Parameters": {
    "IsTameable": {
      "Value": false,
      "Description": "Whether this NPC can be tamed."
    },
    "TameRequiredAttitudes": {
      "Value": ["Friendly", "Neutral"],
      "Description": "The attitude groups of players that can tame the NPC."
    },
    "TameRoleChange": {
      "Value": "Tamed_Sheep",
      "Description": "The role the NPC will change into when it's tamed."
    },
    "AttractiveItemSet": {
      "Value": [],
      "TypeHint": "String",
      "Description": "The list of items that are deemed attractive."
    },
    "WeightFollowItem": {
      "Value": 100,
      "Description": "The probability the NPC will follow an attractive item."
    }
  }
}
```

### Taming Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `IsTameable` | Boolean | Whether the NPC can be tamed |
| `TameRequiredAttitudes` | Array | Player attitude required to tame |
| `TameRoleChange` | String | Role NPC changes to when tamed |
| `AttractiveItemSet` | Array | Items that attract the NPC (favorite food) |
| `WeightFollowItem` | Number | % chance to follow attractive item |

---

## Petting System (Tamed Animals)

From `Server/NPC/Roles/_Core/Templates/Template_Livestock.json`:

```json
{
  "Parameters": {
    "IsPettable": {
      "Value": true,
      "Description": "Whether players can pet the NPC."
    },
    "PetAnimation": {
      "Value": "Alerted",
      "Description": "The animation the NPC will play when petted."
    },
    "PetRequiredAttitudes": {
      "Value": ["Friendly", "Revered"],
      "Description": "The attitude groups of players that can pet the NPC."
    },
    "PetTimeout": {
      "Value": ["PT15M", "PT15M"],
      "Description": "The timeout between consecutive pets."
    },
    "IsHarvestable": {
      "Value": false,
      "Description": "Whether this NPC can be 'harvested'. This includes shearing, milking, etc."
    },
    "HarvestRequiredAttitudes": {
      "Value": ["Friendly", "Revered", "Ignore"],
      "Description": "The attitude groups of players that can harvest the NPC."
    }
  }
}
```

### Livestock Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `IsPettable` | Boolean | Whether players can pet the NPC |
| `PetAnimation` | String | Animation played when petted |
| `PetRequiredAttitudes` | Array | Attitudes that can pet |
| `PetTimeout` | Array | ISO-8601 duration range between pets |
| `IsHarvestable` | Boolean | Whether NPC can be sheared/milked |
| `HarvestInteractionContext` | String | Tool context for harvesting |
| `HarvestDropList` | String | Drop list when harvested |

---

## T2 Furnace

Furnace now has an upgradeable tier system.

From `Server/Item/Items/Bench/Bench_Furnace.json`:

```json
{
  "BlockType": {
    "Bench": {
      "Id": "Furnace",
      "TierLevels": [
        {
          "UpgradeRequirement": {
            "Material": [
              { "ItemId": "Ingredient_Bar_Copper", "Quantity": 5 },
              { "ItemId": "Ingredient_Bar_Iron", "Quantity": 5 },
              { "ItemId": "Ingredient_Bar_Thorium", "Quantity": 5 },
              { "ItemId": "Ingredient_Bar_Cobalt", "Quantity": 5 }
            ],
            "TimeSeconds": 3
          },
          "CraftingTimeReductionModifier": 0.0
        },
        {
          "CraftingTimeReductionModifier": 0.3,
          "ExtraInputSlot": 1
        }
      ]
    },
    "State": {
      "Definitions": {
        "Tier2": {
          "CustomModel": "Blocks/Benches/Furnace2.blockymodel",
          "CustomModelTexture": [
            { "Texture": "Blocks/Benches/Furnace2_Texture_Off.png" }
          ]
        }
      }
    }
  }
}
```

### Tier 2 Furnace Benefits

- **30% faster crafting** (`CraftingTimeReductionModifier: 0.3`)
- **Extra input slot** (`ExtraInputSlot: 1`)
- New model and textures

---

## Goblin Flamethrower

New enemy weapon that can be dropped/used by players.

From `Server/Item/Items/Weapon/Flamethrower/Flamethrower_Goblin.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Flamethrower_Goblin.name"
  },
  "Categories": ["Items.Weapons"],
  "ItemLevel": 40,
  "PlayerAnimationsId": "Rifle",
  "Interactions": {
    "Primary": "Flamethrower_Goblin_Primary"
  },
  "InteractionVars": {
    "Flamethrower_Damage": {
      "Interactions": [
        {
          "DamageCalculator": {
            "BaseDamage": { "Fire": 12 }
          },
          "DamageEffects": {
            "Knockback": {
              "Direction": { "X": 0, "Y": 2.0, "Z": -3.0 },
              "Force": 8.0,
              "VelocityType": "Add"
            }
          },
          "Next": {
            "Type": "ApplyEffect",
            "EffectId": "Flame_Staff_Burn",
            "Entity": "Target"
          }
        }
      ]
    }
  },
  "Light": {
    "Radius": 1,
    "Color": "#a73"
  },
  "Tags": {
    "Family": ["Gun"],
    "Type": ["Weapon"]
  }
}
```

---

## Lime Block Variants

New limestone block family added:

| Block | Location |
|-------|----------|
| Rock_Lime | `Server/Item/Items/Rock/Lime/Rock_Lime.json` |
| Rock_Lime_Stalactite_Large | `Server/Item/Items/Rock/Lime/Rock_Lime_Stalactite_Large.json` |
| Rock_Lime_Stalactite_Small | `Server/Item/Items/Rock/Lime/Rock_Lime_Stalactite_Small.json` |
| Rubble_Lime | `Server/Item/Items/Rubble/Rubble_Lime.json` |
| Rubble_Lime_Medium | `Server/Item/Items/Rubble/Rubble_Lime_Medium.json` |
| Soil_Gravel_Lime | `Server/Item/Items/Soil/Gravel/Soil_Gravel_Lime.json` |
| Rock_Lime_Cobble | Plus walls, stairs, pillars, beams, half blocks |
| Rock_Lime_Brick | Plus smooth, ornate, decorative variants |

---

## Climbing Overhaul

New climbing sounds indicate enhanced climbing mechanics:

- `SFX_Player_Climb_Up.json` - Climbing upward
- `SFX_Player_Climb_Side.json` - **New:** Side-stepping on ropes/ladders
- `SFX_Player_Climb_Down.json` - **New:** Dropping down

Location: `Server/Audio/SoundEvents/SFX/Player/Movement/`

---

## Debug Commands

New and updated debug flags:

| Flag | Description |
|------|-------------|
| `VisFlock` | **New:** Visualizes flock membership and structure |
| `VisLeashPosition` | **New:** Visualizes NPC leashing position |
| `VisAiming` | Shows what NPCs are aiming at |
| `VisSensorRanges` | Shows sensor detection ranges |
| `VisMarkedTargets` | Displays locked targets |

Usage:
```
/npc debug set VisFlock
/npc debug set VisLeashPosition
```

---

## Crafting Changes

| Item | Change |
|------|--------|
| Crude ammo, weapons, tools | Now craft **instantly** |
| Chicken Coop | Craft time reduced **4s → 2s** |
| Builders Bench | Craft time reduced **3s → 2s** |
| Tankards | Now crafted via **Farming Bench** |
| Dough | Now produced using **refillable Tankards** |
| Farmers Bench Tier 4 | Now requires **Drywood Log** (was Redwood) |
| Farmers Bench Tier 6 | Now requires **Redwood Log** (was Drywood) |
| Chicken Coop | Farming Bench tier reduced **5 → 3** |

---

## Localization

New languages added:
- Brazilian Portuguese (`pt-BR`)
- Russian (`ru-RU`)

Location: `Server/Languages/`

---

## Other Notable Changes

| Feature | Details |
|---------|---------|
| Anchor UI System | Server plugins can add UI elements to client pages |
| Avatar Presets | Up to 5 customization presets |
| Map Markers | User-placed with custom names, icons, colors |
| Ophidiophobia Mode | Hatworm now replaced with Larva_Silk model |
| HideFromTooltip | New EntityStatType property |
| Light Property | `Radius` renamed to `BrightRadius`, default = 1 |
| Spawn Markers | Deactivation range standardized to 150 |

---

## Guides to Update for Update 3

These docs should be revised for Update 3:

- **[Block Fluids](63_Block_Fluids.md)** - Fire spread ticker system
- **[Plants & Farming](15_Plants_and_Farming.md)** - Sickles, grass spreading
- **[Tools](14_Tools.md)** - Sickle tool type
- **[NPCs](03_NPCs.md)** - Taming system, petting
- **[Advanced AI & NPC Behavior](180_Advanced_AI_NPC_Behavior.md)** - Debug flags
- **[Blocks & Portals](06_Blocks_and_Portals.md)** - Lime blocks, RandomTickProcedure

---

## Official Patch Notes

Full notes: [Hytale Patch Notes - Update 3](https://hytale.com/news/2026/2/hytale-patch-notes-update-3)

**See also:** [README](../README.md) · [Getting Started](01_Getting_Started.md) · [Patch Notes Update 2](Patch_Notes_Update_2.md)
