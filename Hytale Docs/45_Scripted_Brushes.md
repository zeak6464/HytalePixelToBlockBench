# Scripted Brushes

Learn how to create custom world editing brushes using scripted brush operations.

## Overview

Scripted brushes are advanced world editing tools that use a sequence of operations to place, modify, or generate blocks. They're defined in `Server/ScriptedBrushes/` and used by builder tools. Scripted brushes can create terrain features, structures, decorations, and complex patterns.

## Location
- Scripted brushes: `Server/ScriptedBrushes/`
- Builder tool items: `Server/Item/Items/EditorTool/ScriptedBrushes/`

## Example from Game Files

### Grass Brush

From `Server/ScriptedBrushes/GrassBrush.json`:

```1:43:Server/ScriptedBrushes/GrassBrush.json
{
  "Operations": [
    {
      "Id": "mask",
      "Mask": "Empty,>Soil_Grass_Full|Soil_Grass|Soil_Leaves|Soil_Grass_Deep|Soil_Grass_Sunny"
    },
    {
      "Id": "density",
      "Density": 90
    },
    {
      "Id": "pattern",
      "BlockPattern": "Plant_Grass_Sharp_Short"
    },
    {
      "Id": "set"
    },
    {
      "Id": "dimensions",
      "Width": {
        "Value": -2,
        "Relative": true
      },
      "Height": {
        "Value": -2,
        "Relative": true
      }
    },
    {
      "Id": "mask",
      "Mask": "Plant_Grass_Sharp_Short"
    },
    {
      "Id": "pattern",
      "BlockPattern": "Plant_Grass_Sharp"
    },
    {
      "Id": "density",
      "Density": 75
    },
    {
      "Id": "set"
    }
```

This shows a scripted brush that places grass patterns with density and masking operations.

## Basic Scripted Brush Structure

Create `Server/ScriptedBrushes/MyCustom_Brush.json`:

```json
{
  "Operations": [
    {
      "Id": "echoonce",
      "Message": "My Custom Brush Instructions"
    },
    {
      "Id": "set",
      "BlockPattern": "Rock_Stone"
    }
  ]
}
```

## Common Brush Operations

### Echo/EchoOnce

Display messages to the user:

```json
{
  "Id": "echoonce",
  "Message": "Instructions for using this brush"
}
```

- **`echoonce`** - Display message once when brush is first used
- **`echo`** - Display message each time operation runs

### Set

Place blocks:

```json
{
  "Id": "set",
  "BlockPattern": "Rock_Stone"
}
```

Places the specified block pattern in the selection.

### Pattern

Set block pattern for subsequent operations:

```json
{
  "Id": "pattern",
  "BlockPattern": "Soil_Grass"
}
```

### Density

Control placement density:

```json
{
  "Id": "density",
  "Density": 75
}
```

Density value (0-100) controls how many blocks are placed.

### Dimensions

Set brush dimensions:

```json
{
  "Id": "dimensions",
  "Width": {
    "Value": 10,
    "Relative": true
  },
  "Height": {
    "Value": 5,
    "Relative": true
  }
}
```

### Jump

Control flow with jumps:

```json
{
  "Id": "jump",
  "StoredIndexName": "target_operation"
}
```

Jumps to a labeled operation.

### SaveIndex/LoadIndex

Store and retrieve operation positions:

```json
{
  "Id": "saveindex",
  "StoredIndexName": "my_label"
}
```

```json
{
  "Id": "loadindex",
  "StoredIndexName": "my_label"
}
```

### JumpIfClickType

Conditional based on click type:

```json
{
  "Id": "jumpifclicktype",
  "ClickType": "Left",
  "StoredIndexName": "left_click_action"
}
```

- **`"Left"`** - Left click
- **`"Right"`** - Right click

### JumpIfToolArg

Conditional based on tool arguments:

```json
{
  "Id": "jumpiftoolarg",
  "ArgName": "EnableFeature",
  "ComparisonType": "Equals",
  "ComparisonValue": "true",
  "StoredIndexName": "feature_enabled"
}
```

### JumpIfBlockType

Conditional jump based on block type at an offset. **Update 2 (Jan 2026):** Uses a **single `Offset`** object; the previous list-of-offsets format is deprecated.

```json
{
  "Id": "jumpifblocktype",
  "Mask": "Filter_Rock|Empty",
  "StoredIndexName": "skip_prefab_paste",
  "Offset": {
    "X": { "Value": 0, "Relative": true },
    "Y": { "Value": 0, "Relative": false },
    "Z": { "Value": 0, "Relative": true }
  }
}
```

- **`Mask`** - Block filter (e.g. `Filter_Rock|Empty`, `Filter_TreeLeaves|Filter_TreeWood|Empty`). Jump occurs when the block at the offset matches.
- **`StoredIndexName`** - Label to jump to when the condition matches.
- **`Offset`** - **Single** offset object (not an array). Use `Relative: true` for brush-relative coords.

**Migration:** If you have `"Offset": [ { "X": ..., "Y": ..., "Z": ... } ]`, replace with `"Offset": { "X": ..., "Y": ..., "Z": ... }`. See [Patch Notes Update 2](Patch_Notes_Update_2.md).

### LoadMaterial

Load block pattern from tool argument:

```json
{
  "Id": "loadmaterial",
  "ArgName": "PrimaryBlocks"
}
```

## Advanced Operations

These operations are used in complex brushes like BoulderBrush and CaveBrush.

### DisableOnHold

Disable brush execution when holding the mouse button:

```json
{
  "Id": "disableonhold"
}
```

Prevents repeated execution while holding click.

### Offset

Offset the brush position:

```json
{
  "Id": "offset",
  "Offset": {
    "X": { "Value": 0, "Relative": true },
    "Y": { "Value": -1, "Relative": true },
    "Z": { "Value": 0, "Relative": true }
  },
  "Negate": false
}
```

- **`Relative`** - If true, offset is relative to current position
- **`Negate`** - Reverse the offset direction

### JumpRandom

Jump to a randomly selected index based on weights:

```json
{
  "Id": "jumprandom",
  "WeightedListOfIndexNames": [
    { "Left": 1, "Right": "layer_pos_x" },
    { "Left": 1, "Right": "layer_neg_x" },
    { "Left": 1, "Right": "layer_pos_z" },
    { "Left": 1, "Right": "layer_neg_z" }
  ]
}
```

- **`Left`** - Weight (higher = more likely)
- **`Right`** - Index name to jump to

### LoadInt

Load an integer from a tool argument into a brush field:

```json
{
  "Id": "loadint",
  "ArgName": "kJitter",
  "TargetField": "OffsetX",
  "Relative": true,
  "Negate": false
}
```

| Property | Description |
|----------|-------------|
| `ArgName` | Tool argument name |
| `TargetField` | Field to set (`OffsetX`, `OffsetY`, `OffsetZ`, `Density`, etc.) |
| `Relative` | Add to current value if true |
| `Negate` | Negate the value |

### Melt

Apply terrain melting/erosion:

```json
{
  "Id": "melt"
}
```

Erodes blocks to create organic cave-like shapes. Often used in loops.

### Smooth

Apply smoothing to terrain:

```json
{
  "Id": "smooth",
  "SmoothStrength": 1
}
```

- **`SmoothStrength`** - Intensity of smoothing (1-5 typical)

### HistoryMask

Control which blocks are affected based on brush history:

```json
{
  "Id": "historymask",
  "HistoryMask": "Only"
}
```

- **`"Only"`** - Only affect blocks modified by this brush
- **`"None"`** - Clear history mask, affect all matching blocks

### RandomDimensions

Randomize brush dimensions within a range:

```json
{
  "Id": "randomdimensions",
  "WidthRange": {
    "Min": { "Value": 2, "Relative": true },
    "Max": { "Value": 4, "Relative": true }
  },
  "HeightRange": {
    "Min": { "Value": 2, "Relative": true },
    "Max": { "Value": 7, "Relative": true }
  }
}
```

### AppendMaskFromToolArg

Add blocks from a tool argument to the current mask:

```json
{
  "Id": "appendmaskfromtoolarg",
  "ArgName": "cCaveFloorBlocks",
  "FilterType": "TargetBlock",
  "Invert": true,
  "AdditionalBlocks": ""
}
```

| Property | Description |
|----------|-------------|
| `ArgName` | Tool argument containing block pattern |
| `FilterType` | `"TargetBlock"`, `"BelowBlock"`, or `"AboveBlock"` |
| `Invert` | Invert the mask if true |
| `AdditionalBlocks` | Extra blocks to include |

### ClearOperationMask

Clear the current operation mask:

```json
{
  "Id": "clearoperationmask"
}
```

Resets the mask for the next operation.

### Material

Set the block material directly (alternative to loadmaterial):

```json
{
  "Id": "material",
  "BlockType": "Empty"
}
```

### LoadLoop

Load a loop count from a tool argument:

```json
{
  "Id": "loadloop",
  "StoredIndexName": "melt_loop",
  "ArgName": "yMeltStrength"
}
```

Repeats from `StoredIndexName` the number of times specified by `ArgName`.

### Exit

Exit the brush operation early:

```json
{
  "Id": "exit"
}
```

Stops all further operations.

### Shape

Set the brush shape:

```json
{
  "Id": "shape",
  "Shape": "Sphere"
}
```

Options: `"Cube"`, `"Sphere"`, `"Cylinder"`

## Comments

Add documentation comments to operations using `$Comment`:

```json
{
  "$Comment": "This operation places the primary blocks",
  "Id": "set"
}
```

Comments are ignored during execution but help document complex brushes.

## Complete Example: Simple Brush

`Server/ScriptedBrushes/MyCustom_SimpleBrush.json`:

```json
{
  "Operations": [
    {
      "Id": "echoonce",
      "Message": "Simple brush that places stone blocks"
    },
    {
      "Id": "pattern",
      "BlockPattern": "Rock_Stone"
    },
    {
      "Id": "set"
    }
  ]
}
```

## Example: Boulder Brush

The boulder brush demonstrates complex operations:

### Brush Structure

`Server/ScriptedBrushes/BoulderBrush.json`:

```json
{
  "Operations": [
    {
      "Id": "echoonce",
      "Message": "Boulder Brush - Right Click to create, Left Click to decorate"
    },
    {
      "Id": "jumpifclicktype",
      "ClickType": "Left",
      "StoredIndexName": "decoration"
    },
    {
      "Id": "saveindex",
      "StoredIndexName": "stack_loop"
    },
    {
      "Id": "loadmaterial",
      "ArgName": "aPrimaryBlocks"
    },
    {
      "Id": "dimensions",
      "Width": {
        "Value": 4,
        "Relative": true
      },
      "Height": {
        "Value": 4,
        "Relative": true
      }
    },
    {
      "Id": "set"
    },
    {
      "Id": "dimensions",
      "Width": {
        "Value": -4,
        "Relative": true
      },
      "Height": {
        "Value": -4,
        "Relative": true
      }
    },
    {
      "Id": "jumpiftoolarg",
      "ArgName": "aScalingHeight",
      "ComparisonType": "GreaterThan",
      "ComparisonValue": "0",
      "StoredIndexName": "continue_stacking"
    },
    {
      "Id": "jump",
      "StoredIndexName": "finished"
    }
  ]
}
```

## Creating Builder Tool Items

Create an item that uses your scripted brush:

`Server/Item/Items/EditorTool/ScriptedBrushes/EditorTool_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.builderTools.tools.MyCustom.name",
    "Description": "server.builderTools.tools.MyCustom.description"
  },
  "Icon": "Icons/Items/EditorTools/Paint.png",
  "Categories": [
    "Tool.ScriptedBrushes"
  ],
  "Model": "Items/Tools/Prototype_Teseract_2.blockymodel",
  "Texture": "Items/Tools/Prototype_Teseract_3.png",
  "MaxStack": 1,
  "PlayerAnimationsId": "Item",
  "BuilderTool": {
    "UI": [],
    "Tools": [
      {
        "Id": "Paint",
        "IsBrush": true,
        "BrushConfigurationCommand": "MyCustom",
        "BrushData": {
          "Width": {
            "Default": 5,
            "Min": 1,
            "Max": 100
          },
          "Height": {
            "Default": 5,
            "Min": 1,
            "Max": 100
          },
          "Shape": {
            "Default": "Cube"
          },
          "Origin": {
            "Default": "Center"
          }
        },
        "Args": {
          "PrimaryBlocks": {
            "Type": "BlockPattern",
            "Default": "Rock_Stone"
          },
          "BrushDensity": {
            "Type": "Int",
            "Default": 100,
            "Min": 1,
            "Max": 100
          }
        }
      }
    ]
  },
  "Interactions": {
    "Primary": "Builder_Tool",
    "Secondary": "Builder_Tool"
  },
  "InteractionConfig": {
    "DebugOutlines": true,
    "UseDistance": {
      "Adventure": 128,
      "Creative": 128
    },
    "AllEntities": true
  },
  "Set": "AAAA10",
  "Quality": "Tool"
}
```

## Brush Data Properties

### Width/Height

```json
{
  "Width": {
    "Default": 10,
    "Min": 1,
    "Max": 100
  }
}
```

### Shape

```json
{
  "Shape": {
    "Default": "Cube"
  }
}
```

Options: `"Cube"`, `"Sphere"`, `"Cylinder"`

### Origin

```json
{
  "Origin": {
    "Default": "Center"
  }
}
```

Options: `"Center"`, `"Bottom"`, `"Top"`

## Tool Arguments

### BlockPattern

```json
{
  "PrimaryBlocks": {
    "Type": "BlockPattern",
    "Default": "Rock_Stone"
  }
}
```

### Integer

```json
{
  "BrushDensity": {
    "Type": "Int",
    "Default": 100,
    "Min": 1,
    "Max": 100
  }
}
```

### Boolean

```json
{
  "EnableFeature": {
    "Type": "Bool",
    "Default": true
  }
}
```

## Tips for Creating Scripted Brushes

1. **Start simple** - Begin with basic set operations
2. **Use echo messages** - Guide users on how to use the brush
3. **Test incrementally** - Add operations one at a time
4. **Use jumps** - Control flow for different click types or conditions
5. **Save indices** - Mark important operation points for jumping
6. **Load materials** - Use tool arguments for flexible block patterns
7. **Test thoroughly** - Try different sizes, shapes, and arguments

---

**Previous:** [Response Curves](44_Response_Curves.md) | **Next:** [Portal Types](46_Portal_Types.md)
