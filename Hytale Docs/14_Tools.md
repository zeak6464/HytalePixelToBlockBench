# Creating Tools

Learn how to create tools like pickaxes, axes, hoes, shovels, and other utility items.

## Overview

Tools are items used for gathering resources, breaking blocks, and performing specific actions. Each tool has power ratings for different gather types (Rocks, Woods, Soils, etc.).

## Location
`Server/Item/Items/Tool/{ToolType}/`

## Example from Game Files

### Iron Pickaxe

From `Server/Item/Items/Tool/Pickaxe/Tool_Pickaxe_Iron.json`:

```1:50:Server/Item/Items/Tool/Pickaxe/Tool_Pickaxe_Iron.json
{
  "TranslationProperties": {
    "Name": "server.items.Tool_Pickaxe_Iron.name",
    "Description": "server.items.Tool_Pickaxe_Crude.description"
  },
  "Parent": "Tool_Pickaxe_Crude",
  "Icon": "Icons/ItemsGenerated/Tool_Pickaxe_Iron.png",
  "Quality": "Uncommon",
  "ItemLevel": 20,
  "Model": "Items/Tools/Pickaxe/Iron.blockymodel",
  "Texture": "Items/Tools/Pickaxe/Iron_Texture.png",
  "Recipe": {
    "TimeSeconds": 3.5,
    "Input": [
      {
        "ItemId": "Ingredient_Bar_Iron",
        "Quantity": 5
      },
      {
        "ItemId": "Ingredient_Leather_Light",
        "Quantity": 2
      },
      {
        "ItemId": "Ingredient_Fabric_Scrap_Linen",
        "Quantity": 2
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Workbench",
        "Type": "Crafting",
        "Categories": [
          "Workbench_Tools"
        ]
      }
    ]
  },
  "Tool": {
    "Specs": [
      {
        "Power": 1,
        "GatherType": "SoftBlocks"
      },
      {
        "Power": 0.5,
        "GatherType": "Soils"
      },
      {
        "Power": 0.05,
        "GatherType": "Woods"
```

This shows a complete tool with gathering specifications for different block types.

## Tool Types

| Tool Type | Use Case | Animation ID |
|-----------|----------|--------------|
| **Pickaxe** | Mining rocks, ores | `"Pickaxe"` |
| **Hatchet** | Chopping wood | `"Hatchet"` |
| **Hoe** | Tilling soil, farming | `"Hoe"` |
| **Shovel** | Digging soil, sand | `"Shovel"` |
| **Hammer** | Breaking structures | `"Hammer"` |
| **Sickle** | Harvesting crops | `"Sickle"` |
| **Shears** | Gathering wool, plants | `"Shears"` |

## Basic Tool Structure

Create `Server/Item/Items/Tool/Pickaxe/Tool_Pickaxe_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Tool_Pickaxe_MyCustom.name",
    "Description": "server.items.Tool_Pickaxe_MyCustom.description"
  },
  "Categories": ["Items.Tools"],
  "Quality": "Common",
  "ItemLevel": 10,
  "Icon": "Icons/ItemsGenerated/Tool_Pickaxe_MyCustom.png",
  "Model": "Items/Tools/Pickaxe/Crude.blockymodel",
  "Texture": "Items/Tools/Pickaxe/Crude_Texture.png",
  "Recipe": {
    "TimeSeconds": 1,
    "Input": [
      {
        "ResourceTypeId": "Rubble",
        "Quantity": 2
      },
      {
        "ItemId": "Ingredient_Fibre",
        "Quantity": 2
      },
      {
        "ItemId": "Ingredient_Stick",
        "Quantity": 2
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Workbench",
        "Type": "Crafting",
        "Categories": ["Workbench_Tools"]
      }
    ]
  },
  "Tool": {
    "Specs": [
      {
        "Power": 1,
        "GatherType": "SoftBlocks"
      },
      {
        "Power": 0.25,
        "GatherType": "Rocks"
      }
    ],
    "DurabilityLossBlockTypes": [
      {
        "BlockSets": ["Stone", "Rock", "Ores", "Soil"],
        "DurabilityLossOnHit": 0.25
      }
    ]
  },
  "PlayerAnimationsId": "Pickaxe",
  "Interactions": {
    "Primary": "Pickaxe_Attack",
    "Secondary": "Pickaxe_Attack"
  },
  "MaxDurability": 150,
  "Tags": {
    "Type": ["Tool"]
  },
  "ItemSoundSetId": "ISS_Weapons_Wood"
}
```

## Tool Power System

The `Tool.Specs` array defines how effective the tool is at gathering different block types:

```json
"Tool": {
  "Specs": [
    {
      "Power": 1.0,           // 100% effective
      "GatherType": "SoftBlocks"
    },
    {
      "Power": 0.25,          // 25% effective
      "GatherType": "Rocks"
    },
    {
      "Power": 0.05,          // 5% effective (very slow)
      "GatherType": "Woods"
    }
  ]
}
```

### Gather Types

| GatherType | Description | Best Tool |
|------------|-------------|-----------|
| `SoftBlocks` | All tools can break | Any tool |
| `Rocks` | Stone, ore blocks | Pickaxe |
| `Woods` | Wood blocks, trees | Hatchet/Axe |
| `Soils` | Dirt, grass, sand | Shovel |
| `Benches` | Crafting benches | Pickaxe/Hatchet |
| `VolcanicRocks` | Volcanic stone | Pickaxe (low power) |

### Power Values

- **`1.0`** - Full effectiveness (optimal tool for this type)
- **`0.5`** - Half effectiveness
- **`0.25`** - Quarter effectiveness
- **`0.05`** - Very slow (wrong tool type)
- **`0.01`** - Extremely slow (nearly useless)

## Durability Loss Configuration

Tools lose durability based on what they're breaking:

```json
"DurabilityLossBlockTypes": [
  {
    "BlockSets": ["Stone", "Rock", "Ores", "Soil"],
    "DurabilityLossOnHit": 0.25
  },
  {
    "BlockSets": ["Wood"],
    "DurabilityLossOnHit": 0.1
  }
]
```

- **`DurabilityLossOnHit`** - Durability lost per hit (0.1 = lose 10% per 10 hits)

## Tool Interactions

### Pickaxe Interactions

```json
"Interactions": {
  "Primary": "Pickaxe_Attack",
  "Secondary": "Pickaxe_Attack"
},
"InteractionVars": {
  "Pickaxe_Mine_Effect": {
    "Interactions": [
      {
        "Parent": "Pickaxe_Mine_Effect",
        "Effects": {
          "WorldSoundEventId": "SFX_Tool_T1_Swing",
          "LocalSoundEventId": "SFX_Pickaxe_T1_Swing_Down_Local"
        }
      }
    ]
  },
  "Pickaxe_Mine_Damage": {
    "Interactions": [
      {
        "Parent": "Pickaxe_Mine_Damage",
        "DamageCalculator": {
          "Type": "Absolute",
          "BaseDamage": {
            "Physical": 1
          }
        }
      }
    ]
  }
}
```

### Hatchet Interactions

```json
"Interactions": {
  "Primary": "Hatchet_Attack",
  "Secondary": "Hatchet_Attack"
}
```

### Hoe Interactions

Hoes have special tilling capability:

```json
"Interactions": {
  "Primary": "Hoe_Attack",
  "Secondary": "Hoe_Till"  // Special tilling interaction
}
```

### Shovel Interactions

```json
"Interactions": {
  "Primary": {
    "Interactions": ["Shovel_Attack"]
  }
}
```

## Tool Tier Examples

**Update 2 (Jan 2026):** Mining and pickaxe progression were adjusted. Lower-tier pickaxes deal much less damage to higher-tier ores; upgrading significantly reduces hits required. **Adamantite** now requires **Thorium or Cobalt** pickaxe. **Hoes** unlock at different farming bench tiers; recipe costs were adjusted.

### Crude Pickaxe (Tier 1)

```json
{
  "Quality": "Common",
  "ItemLevel": 10,
  "Tool": {
    "Specs": [
      { "Power": 1, "GatherType": "SoftBlocks" },
      { "Power": 0.35, "GatherType": "Soils" },
      { "Power": 0.25, "GatherType": "Rocks" },
      { "Power": 0.05, "GatherType": "Woods" }
    ]
  },
  "MaxDurability": 150
}
```

### Copper Pickaxe (Tier 2) — Update 2

```json
{
  "Quality": "Common",
  "ItemLevel": 10,
  "Tool": {
    "Specs": [
      { "Power": 1, "GatherType": "SoftBlocks" },
      { "Power": 0.5, "GatherType": "Rocks" },
      { "Power": 0.4, "GatherType": "Soils" },
      { "Power": 0.05, "GatherType": "Woods" }
    ]
  },
  "MaxDurability": 120
}
```

### Iron Pickaxe (Tier 4) — Update 2: was Tier 2

```json
{
  "Quality": "Uncommon",
  "ItemLevel": 20,
  "Tool": {
    "Specs": [
      { "Power": 1, "GatherType": "SoftBlocks" },
      { "Power": 0.6, "GatherType": "Rocks" },
      { "Power": 0.4, "GatherType": "Soils" },
      { "Power": 0.05, "GatherType": "Woods" }
    ]
  },
  "MaxDurability": 300
}
```

### Thorium Pickaxe (Tier 6) — Update 2: new tier

```json
{
  "Quality": "Rare",
  "ItemLevel": 40,
  "Tool": {
    "Specs": [
      { "Power": 1, "GatherType": "SoftBlocks" },
      { "Power": 1, "GatherType": "Rocks" },
      { "Power": 0.8, "GatherType": "VolcanicRocks" },
      { "Power": 0.5, "GatherType": "Soils" }
    ]
  },
  "MaxDurability": 500
}
```

**Tier order:** Crude → Copper (T2) → … → Iron (T4) → … → Thorium (T6) → Cobalt. **Adamantite:** Thorium or Cobalt required.

## Special Tools

### Repair Kit

```json
{
  "TranslationProperties": {
    "Name": "server.items.Tool_Repair_Kit_Iron.name"
  },
  "Categories": ["Items.Tools"],
  "Interactions": {
    "Primary": "Repair_Kit_Use"
  },
  "MaxStack": 10
}
```

### Watering Can

```json
{
  "Interactions": {
    "Primary": "Watering_Can_Use"
  },
  "MaxDurability": 100
}
```

### Feedbag / Fertilizer

```json
{
  "Interactions": {
    "Primary": "Feedbag_Use"
  }
}
```

## Tool Crafting

Tools are typically crafted at a `Workbench` or `Fieldcraft` bench:

```json
"Recipe": {
  "TimeSeconds": 1,
  "Input": [
    { "ResourceTypeId": "Rubble", "Quantity": 2 },
    { "ItemId": "Ingredient_Fibre", "Quantity": 2 },
    { "ItemId": "Ingredient_Stick", "Quantity": 2 }
  ],
  "BenchRequirement": [
    {
      "Id": "Workbench",
      "Type": "Crafting",
      "Categories": ["Workbench_Tools"]
    },
    {
      "Id": "Fieldcraft",
      "Type": "Crafting",
      "Categories": ["Tools"]
    }
  ]
}
```

## Player Animation IDs

Each tool type has a specific animation set:

- `"Pickaxe"` - Pickaxe mining animation
- `"Hatchet"` - Hatchet chopping animation
- `"Hoe"` - Hoe tilling animation
- `"Shovel"` - Shovel digging animation
- `"Hammer"` - Hammer swinging animation
- `"Sickle"` - Sickle harvesting animation
- `"Shears"` - Shears cutting animation

## Sound Sets

Tools use specific sound sets:

- `"ISS_Weapons_Wood"` - Wooden tools
- `"ISS_Weapons_Stone"` - Stone tools
- `"ISS_Weapons_Blade_Large"` - Metal tools

## Tips for Creating Tools

1. **Balance Power Ratings** - Higher tier tools should have better power ratings
2. **Set Appropriate Durability** - Higher tier = more durability
3. **Use Correct GatherTypes** - Each tool should excel at its intended purpose
4. **Match Animation IDs** - Use the correct `PlayerAnimationsId` for each tool type
5. **Configure Durability Loss** - Heavier use blocks should cause more durability loss
6. **Tier Progression** - Follow Update 2 tier order (Crude → Copper → … → Iron → … → Thorium → Cobalt). See [Patch Notes Update 2](Patch_Notes_Update_2.md).
7. **Special Interactions** - Some tools (like hoes) have unique secondary interactions; hoes unlock at different farming bench tiers.

---

**Previous:** [Food Items](13_Food_Items.md) | **Next:** [Plants & Farming](15_Plants_and_Farming.md)
