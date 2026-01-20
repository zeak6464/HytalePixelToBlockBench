# Creating Containers

Learn how to create containers like buckets, bags, and other items that can hold fluids or items.

## Overview

Containers are items that can store fluids (like water) or change state when filled/empty. Examples include buckets, bottles, and bags.

## Location
`Server/Item/Items/Container/`

## Example from Game Files

### Bucket Container

From `Server/Item/Items/Container/Container_Bucket.json`:

```47:90:Server/Item/Items/Container/Container_Bucket.json
  "Interactions": {
    "Secondary": {
      "Interactions": [
        {
          "Type": "Condition",
          "Crouching": false,
          "Next": {
            "$Comment": "Not crouching: try water, then milk, then nothing",
            "Type": "RefillContainer",
            "Effects": {
              "ClearSoundEventOnFinish": true,
              "LocalSoundEventId": "SFX_Water_MoveIn"
            },
            "States": {
              "Filled_Water": {
                "AllowedFluids": [
                  "Water_Source"
                ],
                "TransformFluid": "Empty"
              }
            },
            "Failed": "Bucket_Milk_Cow"
          },
          "Failed": {
            "$Comment": "Crouching: try water, then milk, then place bucket",
            "Type": "RefillContainer",
            "Effects": {
              "ClearSoundEventOnFinish": true,
              "LocalSoundEventId": "SFX_Water_MoveIn"
            },
            "States": {
              "Filled_Water": {
                "AllowedFluids": [
                  "Water_Source"
                ],
                "TransformFluid": "Empty"
              }
            },
            "Failed": "Bucket_Milk_Cow_Crouching"
          }
        }
      ]
    }
  },
```

This shows a container with fluid filling mechanics, state changes, and conditional logic based on crouching.

### Village Chest Container

From `Server/Item/Items/Furniture/Village/Furniture_Village_Chest_Small.json`:

```30:73:Server/Item/Items/Furniture/Village/Furniture_Village_Chest_Small.json
    "State": {
      "Id": "container",
      "Capacity": 18,
      "Definitions": {
        "CloseWindow": {
          "InteractionSoundEventId": "SFX_Chest_Wooden_Close",
          "CustomModelAnimation": "Blocks/Animations/Chest/Chest_Close.blockyanim"
        },
        "OpenWindow": {
          "InteractionSoundEventId": "SFX_Chest_Wooden_Open",
          "CustomModelAnimation": "Blocks/Animations/Chest/Chest_Open.blockyanim"
        }
      }
    },
    "Supporting": {
      "Up": [
        {
          "FaceType": "Full"
        }
      ]
    },
    "Support": {
      "Down": [
        {
          "FaceType": "Full"
        }
      ]
    },
    "VariantRotation": "NESW",
    "Interactions": {
      "Primary": "Break_Container",
      "Use": "Open_Container"
    },
```

This shows a container block with storage capacity, open/close animations, and interactions.

## Basic Container Structure

Create `Server/Item/Items/Container/Container_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Container_MyCustom.name",
    "Description": "server.items.Container_MyCustom.description"
  },
  "Icon": "Icons/ItemsGenerated/Container_MyCustom.png",
  "ItemLevel": 5,
  "MaxStack": 1,
  "Categories": ["Furniture.Containers"],
  "Recipe": {
    "TimeSeconds": 1,
    "Input": [
      {
        "ResourceTypeId": "Wood_All",
        "Quantity": 3
      },
      {
        "ItemId": "Ingredient_Bar_Iron",
        "Quantity": 1
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Farmingbench",
        "Type": "Crafting",
        "Categories": ["Farming"]
      }
    ]
  },
  "Interactions": {
    "Secondary": {
      "Interactions": [
        {
          "Type": "RefillContainer",
          "Effects": {
            "ClearSoundEventOnFinish": true,
            "LocalSoundEventId": "SFX_Water_MoveIn"
          },
          "States": {
            "Filled_Water": {
              "AllowedFluids": ["Water_Source"],
              "TransformFluid": "Empty"
            }
          },
          "Failed": {
            "Type": "PlaceFluid",
            "FluidToPlace": "Water_Source",
            "RemoveItemInHand": false,
            "Next": {
              "Type": "ModifyInventory",
              "AdjustHeldItemDurability": -1,
              "BrokenItem": "Container_MyCustom"
            }
          }
        }
      ]
    }
  },
  "State": {
    "Filled_Water": {
      "Variant": true,
      "TranslationProperties": {
        "Name": "server.items.Container_MyCustom_Water.name"
      },
      "Icon": "Icons/ItemsGenerated/Container_MyCustom_Water.png",
      "Interactions": {
        "Secondary": {
          "Interactions": [
            {
              "Type": "PlaceFluid",
              "RemoveItemInHand": false,
              "Next": {
                "Type": "ModifyInventory",
                "AdjustHeldItemDurability": -1,
                "BrokenItem": "Container_MyCustom"
              },
              "FluidToPlace": "Water_Source",
              "Effects": {
                "ClearSoundEventOnFinish": true,
                "LocalSoundEventId": "SFX_WATER_MoveOut"
              }
            }
          ]
        }
      },
      "Consumable": true,
      "MaxDurability": 1,
      "MaxStack": 1
    }
  }
}
```

## Container States

Containers use a state system to represent filled/empty states:

### Empty State (Base)

The base item definition represents the empty container.

### Filled State

```json
"State": {
  "Filled_Water": {
    "Variant": true,
    "TranslationProperties": {
      "Name": "server.items.Container_MyCustom_Water.name"
    },
    "Icon": "Icons/ItemsGenerated/Container_MyCustom_Water.png",
    "Interactions": {
      "Secondary": {
        "Interactions": [
          {
            "Type": "PlaceFluid",
            "FluidToPlace": "Water_Source",
            "RemoveItemInHand": false,
            "Next": {
              "Type": "ModifyInventory",
              "AdjustHeldItemDurability": -1,
              "BrokenItem": "Container_MyCustom"
            }
          }
        ]
      }
    },
    "Consumable": true,
    "MaxDurability": 1
  }
}
```

- **`Variant: true`** - This is a state variant of the base item
- **`Consumable: true`** - Filled container can be consumed (poured out)
- **`MaxDurability: 1`** - One use before returning to empty state

## Refilling Container (Empty → Filled)

```json
"Interactions": {
  "Secondary": {
    "Interactions": [
      {
        "Type": "RefillContainer",
        "Effects": {
          "LocalSoundEventId": "SFX_Water_MoveIn"
        },
        "States": {
          "Filled_Water": {
            "AllowedFluids": ["Water_Source"],
            "TransformFluid": "Empty"
          }
        }
      }
    ]
  }
}
```

- **`AllowedFluids`** - Types of fluid that can be collected
- **`TransformFluid`** - What happens to the fluid source (usually `"Empty"`)

## Placing Fluid (Filled → Empty)

```json
{
  "Type": "PlaceFluid",
  "FluidToPlace": "Water_Source",
  "RemoveItemInHand": false,
  "Next": {
    "Type": "ModifyInventory",
    "AdjustHeldItemDurability": -1,
    "BrokenItem": "Container_MyCustom"
  }
}
```

- **`FluidToPlace`** - Type of fluid to place (must match `AllowedFluids`)
- **`AdjustHeldItemDurability`** - Consumes one durability (returns to empty state)
- **`BrokenItem`** - Item to return when durability reaches 0 (the empty container)

## Container Interactions Flow

### Empty Container Flow

1. Player holds empty container
2. Right-clicks on water source
3. `RefillContainer` interaction triggers
4. Container state changes to `Filled_Water`
5. Water source becomes empty

### Filled Container Flow

1. Player holds filled container
2. Right-clicks on ground/empty space
3. `PlaceFluid` interaction triggers
4. Water is placed
5. Container durability decreases by 1
6. Returns to empty state when durability = 0

## Complex Container: Bucket Example

```json
{
  "TranslationProperties": {
    "Name": "server.items.Container_Bucket.name"
  },
  "Interactions": {
    "Secondary": {
      "Interactions": [
        {
          "Type": "Condition",
          "Crouching": false,
          "Next": {
            "Type": "RefillContainer",
            "States": {
              "Filled_Water": {
                "AllowedFluids": ["Water_Source"],
                "TransformFluid": "Empty"
              }
            },
            "Failed": "Bucket_Milk_Cow"
          },
          "Failed": {
            "Type": "RefillContainer",
            "States": {
              "Filled_Water": {
                "AllowedFluids": ["Water_Source"],
                "TransformFluid": "Empty"
              }
            },
            "Failed": "Bucket_Milk_Cow_Crouching"
          }
        }
      ]
    }
  }
}
```

This bucket can:
- Fill with water (standing)
- Fill with water (crouching)
- Milk a cow (when water fill fails)

## Container Block Placement

Containers can also be placed as blocks:

```json
"BlockType": {
  "Material": "Empty",
  "DrawType": "Model",
  "Opacity": "Transparent",
  "CustomModel": "Blocks/Decorative_Sets/Village/Bucket_Full.blockymodel",
  "CustomModelTexture": [
    {
      "Texture": "Blocks/Decorative_Sets/Village/Bucket_Texture_Water.png",
      "Weight": 1
    }
  ],
  "CustomModelScale": 0.75,
  "RandomRotation": "YawStep1",
  "Gathering": {
    "Harvest": {},
    "Soft": {}
  }
}
```

## Fluid Types

Common fluid types:
- `Water_Source` - Water
- `Lava` - Lava

## Tips for Creating Containers

1. **Define states clearly** - Empty and filled states need separate definitions
2. **Use RefillContainer** - For collecting fluids
3. **Use PlaceFluid** - For placing fluids
4. **Set MaxDurability** - Filled containers should have durability = 1 for single use
5. **Handle failed interactions** - Chain failed interactions for complex behavior
6. **Match fluid types** - Ensure `AllowedFluids` matches `FluidToPlace`
7. **Add sound effects** - Use appropriate sounds for filling/emptying

---

**Previous:** [Traps](17_Traps.md) | **Next:** [Materials & Resources](19_Materials_and_Resources.md)
