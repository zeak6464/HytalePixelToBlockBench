# Farming Modifiers

Learn how to create and configure farming modifiers that affect crop growth rates and farming mechanics.

## Overview

Farming modifiers are configurations that affect crop growth rates based on various conditions (water, light, fertilizer, darkness). They're defined in `Server/Farming/Modifiers/` and applied to farming blocks to modify growth speed. Multiple modifiers can stack to significantly increase or decrease growth rates.

**Update 2 (Jan 2026):** Tilled soil lifetime was increased; it now lasts **1.2–1.5 days** between tillings. Soil decay under fully grown crops was fixed. See [Patch Notes Update 2](Patch_Notes_Update_2.md).

## Location
`Server/Farming/Modifiers/`

## Example from Game Files

### Fertilizer Modifier

From `Server/Farming/Modifiers/Fertilizer.json`:

```1:4:Server/Farming/Modifiers/Fertilizer.json
{
  "Type": "Fertilizer",
  "Modifier": 2
}
```

This shows a fertilizer modifier that doubles crop growth rate (2x).

### Water Modifier

From `Server/Farming/Modifiers/Water.json`:

```1:14:Server/Farming/Modifiers/Water.json
{
  "Type": "Water",
  "Modifier": 2.5,
  "Fluids": [
    "Water_Source",
    "Water"
  ],
  "Weathers": [
    "Zone1_Rain",
    "Zone1_Rain_Light",
    "Zone1_Storm",
    "Zone3_Rain"
  ]
}
```

This shows a water modifier that increases growth 2.5x when water is present or during rainy weather.

### Light Level Modifier

From `Server/Farming/Modifiers/LightLevel.json`:

```1:23:Server/Farming/Modifiers/LightLevel.json
{
  "Type": "LightLevel",
  "Modifier": 2,
  "ArtificialLight": {
    "Red": {
      "Min": 5,
      "Max": 127
    },
    "Green": {
      "Min": 5,
      "Max": 127
    },
    "Blue": {
      "Min": 5,
      "Max": 127
    }
  },
  "Sunlight": {
    "Min": 5.0,
    "Max": 15.0
  },
  "RequireBoth": false
}
```

This shows a light level modifier that doubles growth when adequate sunlight or artificial light is present.

### Using Farming Modifiers in Crop Blocks

From `Server/Item/Items/Plant/Crop/_Template/Template_Crop_Block.json`:

```150:157:Server/Item/Items/Plant/Crop/_Template/Template_Crop_Block.json
      "StartingStageSet": "Default",
      "ActiveGrowthModifiers": [
        "Fertilizer",
        "Water",
        "LightLevel"
      ],
      "StageSetAfterHarvest": "Harvested"
```

This shows a crop block with `ActiveGrowthModifiers` applied (`Fertilizer`, `Water`, `LightLevel`) that affect the growth duration of each stage.

> **Note:** The property is `ActiveGrowthModifiers`, not `Modifiers`.

## Basic Farming Modifier Structure

Create `Server/Farming/Modifiers/MyCustom_Modifier.json`:

```json
{
  "Type": "MyCustom",
  "Modifier": 2.0
}
```

## Farming Modifier Types

### Fertilizer

Simple multiplier that increases growth rate:

`Server/Farming/Modifiers/Fertilizer.json`:

```json
{
  "Type": "Fertilizer",
  "Modifier": 2
}
```

**Properties:**
- **`Modifier`** - Growth rate multiplier (2.0 = 2x faster)

### Water

Modifier based on water source or weather:

`Server/Farming/Modifiers/Water.json`:

```json
{
  "Type": "Water",
  "Modifier": 2.5,
  "Fluids": [
    "Water_Source",
    "Water"
  ],
  "Weathers": [
    "Zone1_Rain",
    "Zone1_Rain_Light",
    "Zone1_Storm",
    "Zone3_Rain"
  ]
}
```

**Properties:**
- **`Modifier`** - Growth rate multiplier when condition is met
- **`Fluids`** - Fluid block types that trigger the modifier
- **`Weathers`** - Weather IDs that trigger the modifier (rain, storms)

### LightLevel

Modifier based on light levels (artificial or sunlight):

`Server/Farming/Modifiers/LightLevel.json`:

```json
{
  "Type": "LightLevel",
  "Modifier": 2,
  "ArtificialLight": {
    "Red": {
      "Min": 5,
      "Max": 127
    },
    "Green": {
      "Min": 5,
      "Max": 127
    },
    "Blue": {
      "Min": 5,
      "Max": 127
    }
  },
  "Sunlight": {
    "Min": 5.0,
    "Max": 15.0
  },
  "RequireBoth": false
}
```

**Properties:**
- **`Modifier`** - Growth rate multiplier when light conditions are met
- **`ArtificialLight`** - RGB light level ranges for artificial light
- **`Sunlight`** - Light level range for sunlight
- **`RequireBoth`** - Whether both artificial and sunlight are required

### Darkness

Negative modifier for low light conditions:

`Server/Farming/Modifiers/Darkness.json`:

```json
{
  "Type": "LightLevel",
  "Modifier": 2,
  "ArtificialLight": {
    "Red": {
      "Min": 0,
      "Max": 4
    },
    "Green": {
      "Min": 0,
      "Max": 4
    },
    "Blue": {
      "Min": 0,
      "Max": 4
    }
  },
  "Sunlight": {
    "Min": 0,
    "Max": 5
  },
  "RequireBoth": true
}
```

Creates a modifier that applies when light is very low (slows growth).

## Using Farming Modifiers

### In Crop Block Definitions

Crops can specify which modifiers they respond to:

```json
{
  "Farming": {
    "Stages": [
      {
        "BlockTagPattern": "Soil_Or_Grass",
        "DurationMinutes": 30,
        "Modifiers": [
          "Fertilizer",
          "Water",
          "LightLevel"
        ]
      }
    ]
  }
}
```

### In Farming Block States

Farming blocks can refresh modifiers:

```json
{
  "BlockType": {
    "State": {
      "Definitions": {
        "Fertilized": {
          "Modifiers": ["Fertilizer"]
        }
      }
    }
  }
}
```

### With Fertilizer Tools

Fertilizer tools apply modifiers:

`Server/Item/RootInteractions/Tools/Fertilizer_Use.json`:

```json
{
  "Type": "FertilizeSoil",
  "RefreshModifiers": [
    "Fertilizer"
  ],
  "Effects": {
    "ItemAnimationId": "Till"
  },
  "Next": {
    "Type": "ModifyInventory",
    "AdjustHeldItemDurability": -1,
    "BrokenItem": "Empty"
  },
  "RunTime": 0.15
}
```

## Complete Example: Custom Growth Boost

### 1. Modifier Definition

`Server/Farming/Modifiers/MyCustom_GrowthBoost.json`:

```json
{
  "Type": "MyCustom_GrowthBoost",
  "Modifier": 3.0
}
```

### 2. Using in Crop

`Server/Item/Items/Plant/Crop/Plant_Crop_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Plant_Crop_MyCustom.name"
  },
  "Categories": ["Blocks.Plants"],
  "BlockType": {
    "Material": "Plant",
    "DrawType": "Model",
    "CustomModel": "Blocks/Farming/MyCustom/Stage_0.blockymodel"
  },
  "Farming": {
    "Stages": [
      {
        "BlockTagPattern": "Soil_Or_Grass",
        "DurationMinutes": 20,
        "Modifiers": [
          "Fertilizer",
          "Water",
          "MyCustom_GrowthBoost"
        ],
        "BlockChange": {
          "BlockType": "Plant_Crop_MyCustom_Stage_1"
        }
      }
    ]
  }
}
```

### 3. Creating Modifier Tool

`Server/Item/Items/Tools/Tool_MyCustom_Fertilizer.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Tool_MyCustom_Fertilizer.name"
  },
  "Categories": ["Tools.Farming"],
  "Model": "Items/Tools/Fertilizer/Fertilizer.blockymodel",
  "Texture": "Items/Tools/Fertilizer/Fertilizer.png",
  "MaxStack": 64,
  "Interactions": {
    "Primary": "Fertilizer_Use_Custom"
  }
}
```

`Server/Item/RootInteractions/Tools/Fertilizer_Use_Custom.json`:

```json
{
  "Type": "FertilizeSoil",
  "RefreshModifiers": [
    "Fertilizer",
    "MyCustom_GrowthBoost"
  ],
  "Effects": {
    "ItemAnimationId": "Till"
  },
  "Next": {
    "Type": "ModifyInventory",
    "AdjustHeldItemDurability": -1,
    "BrokenItem": "Empty"
  },
  "RunTime": 0.15
}
```

## Modifier Stacking

Multiple modifiers stack multiplicatively:

- Base growth: 1.0x
- With Fertilizer (2.0x): 2.0x
- With Water (2.5x): 5.0x (2.0 × 2.5)
- With LightLevel (2.0x): 10.0x (5.0 × 2.0)

## Tips for Creating Farming Modifiers

1. **Balance carefully** - High modifiers can make crops grow too fast
2. **Test combinations** - Modifiers stack, so test combined effects
3. **Use realistic values** - Modifier values around 1.5-3.0 are typical
4. **Match conditions** - Use appropriate conditions for each modifier type
5. **Consider negative modifiers** - Darkness/low light can slow growth
6. **Document requirements** - Note what triggers each modifier
7. **Test in-game** - Verify modifiers work correctly in farming scenarios

---

**Previous:** [Model VFX](51_Model_VFX.md) | **Next:** Back to [README](../README.md)
