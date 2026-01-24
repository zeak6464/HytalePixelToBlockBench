# Builder Tools Guide

A comprehensive guide to using builder/creative mode tools for world building and editing in Hytale.

## Overview

Builder Tools in Hytale provide powerful world editing capabilities similar to WorldEdit/VoxelSniper in Minecraft. These tools allow you to quickly build, modify, and create large structures and terraforming.

**Update 2 (Jan 2026):** Scripted brush QoL improvements (decoration/cave brushes, filter fixes, **JumpIfBlockType** single offset). Hotbar active-slot visibility improved (diamond indicator). See [Scripted Brushes](45_Scripted_Brushes.md) and [Patch Notes Update 2](Patch_Notes_Update_2.md).

## Accessing Builder Tools

Builder tools are available in **Creative Mode** and **Builder Mode**. Access them through:
- Creative inventory categories: `Builder Tool`, `Scripted Brushes`
- Builder Tools menu hotkeys (see commands below)

## Selection Tools

### Creating Selections

**Set first position:**
```
/pos1
```

Sets the first corner of your selection to your current position.

**Set second position:**
```
/pos2
```

Sets the second corner, completing a cuboid selection.

**Select entire chunk:**
```
/selectchunk
```

Selects the chunk you're standing in.

**Select chunk section:**
```
/selectchunksection
```

Selects a vertical section of the chunk.

**Deselect:**
```
/deselect
```

Clears your current selection.

### Modifying Selections

**Expand selection:**
```
/expand <amount> [direction]
```

Examples:
```
/expand 10 up
/expand 5 north
/expand 3
```

Expands selection in specified direction or all directions.

**Contract selection:**
```
/contract <amount> [direction]
```

Examples:
```
/contract 5 down
/contract 2
```

Makes selection smaller in specified direction.

**Shift selection:**
```
/shift <amount> <direction>
```

Example:
```
/shift 10 east
```

Moves the entire selection (not contents) in specified direction.

## Editing Commands

### Basic Block Operations

**Set all blocks:**
```
/set <blockType>
```

Example:
```
/set Rock_Stone
```

Sets all blocks in selection to specified type.

**Replace blocks:**
```
/replace <fromBlock> <toBlock>
```

Example:
```
/replace Soil_Dirt Rock_Stone
```

Replaces specific block type with another.

**Fill air blocks:**
```
/fill <blockType>
```

Example:
```
/fill Rock_Stone
```

Fills only air blocks within selection, leaving existing blocks untouched.

**Clear selection:**
```
/clear
```

Sets all blocks in selection to Empty (air).

**Clear entities:**
```
/clearEntities
```

Removes all entities within selection.

### Advanced Editing

**Create walls:**
```
/walls <blockType> [thickness]
```

Example:
```
/walls Rock_Stone 1
/walls Wood_Planks_Oak 2
```

Creates walls around the perimeter of selection.

**Make hollow:**
```
/hollow <blockType> [thickness]
```

Example:
```
/hollow Rock_Stone 1
```

Creates a hollow shell, leaving specified wall thickness.

**Stack selection:**
```
/stack <count> [direction]
```

Examples:
```
/stack 5 up
/stack 3 north
```

Duplicates and stacks the selection multiple times.

**Move selection contents:**
```
/move <amount> [direction]
```

Example:
```
/move 10 east
```

Moves all blocks in selection by specified amount.

### Drawing Tools

**Draw line between points:**
```
/editline <blockType>
```

Example:
```
/editline Rock_Stone
```

Draws a line of blocks between your last two positions.

**Extend face:**
```
/extendface [amount]
```

Extends the face you're looking at.

## Clipboard Operations

### Copy and Paste

**Copy selection:**
```
/copy
```

Copies selected blocks to clipboard.

**Cut selection:**
```
/cut
```

Cuts (copies and removes) selected blocks to clipboard.

**Paste clipboard:**
```
/paste
```

Pastes clipboard contents at your current position.

### Clipboard Transformations

**Rotate clipboard:**
```
/rotate <degrees>
```

Examples:
```
/rotate 90
/rotate 180
/rotate -90
```

Rotates clipboard contents by specified angle.

**Flip clipboard:**
```
/flip [direction]
```

Examples:
```
/flip horizontal
/flip vertical
/flip north
```

Flips clipboard contents along specified axis.

**Edit clipboard:**
```
/edit
```

Opens clipboard editor interface.

**Clear clipboard history:**
```
/clearhistory
```

Clears all clipboard history.

## Special Operations

### Fluids

**Submerge selection:**
```
/submerge <fluidType>
```

Examples:
```
/submerge Water_Normal
/submerge Lava
/submerge Empty    # Removes fluid
```

Fills selection with specified fluid type.

### Coloring

**Tint selection:**
```
/tint <color>
```

Example:
```
/tint #FF0000
```

Tints selected blocks with specified color (hex format).

**Tint chunk:**
```
/chunk tint <color> [--radius=<n>] [--sigma=<n>] [--blur]
```

Example:
```
/chunk tint #00FF00 --blur --radius=5
```

Tints entire chunk with color blending.

### Environment

**Set environment:**
```
/environment <environmentId>
```

Example:
```
/environment Env_Zone1_Plains
```

Sets the environment/biome for selected area.

## Undo and Redo

**Undo last change:**
```
/undo
```

Reverts the most recent edit.

**Redo undone change:**
```
/redo
```

Re-applies the most recently undone edit.

**Enable/disable selection history:**
```
/selectionHistory <true|false>
```

Controls whether selection changes are recorded in history.

**Set history size:**
```
/settoolhistorysize <size>
```

Example:
```
/settoolhistorysize 100
```

Sets maximum number of undoable actions stored.

## Masks and Filters

**Set global block mask:**
```
/globalmask <pattern>
```

Sets a mask pattern that filters which blocks can be edited.

## Advanced Tools

### Import Tools

**Import image as blocks:**
```
/importimage
```

Opens the image-to-blocks converter for creating pixel art.

**Import OBJ model:**
```
/importobj
```

Opens the OBJ-to-voxel converter for importing 3D models.

### Update Tools

**Repair filler blocks:**
```
/repairfillers
```

Fixes and regenerates filler blocks in selection.

**Update selection:**
```
/updateselection
```

Forces update/refresh of current selection.

**Edit selection properties:**
```
/editselection
```

Opens selection property editor.

## Builder Tools Hotbar

**Set hotbar:**
```
/hotbar <slot>
```

Sets your hotbar to a saved configuration.

**Toggle builder tools legend:**
```
/builderToolsLegend
```

Shows/hides the builder tools keyboard shortcut legend.

## Builder Status Messages

The game provides feedback during editing operations:

- **Blocks Edited:** `[key] {count} Blocks Edited`
- **Editing Progress:** `[key] Editing {percent}% completed ({count}/{total})`
- **Done Editing:** `[key] Done Editing`
- **Blocks Changed:** `{total} Blocks Changed`
- **Copy Success:** `Copied {blockCount} blocks`
- **Copy with Entities:** `Copied {blockCount} blocks and {entityCount} entities`
- **Clipboard Rotated:** `Clipboard rotated by {angle} on {axis} axis`

## Common Use Cases

### Building a Structure

1. Set both corners with `/pos1` and `/pos2`
2. Use `/set` to fill with material
3. Use `/hollow` to make it hollow
4. Use `/copy` to save for reuse

### Terraforming

1. Select large area with `/pos1`, `/pos2`, `/expand`
2. Use `/replace` to change terrain type
3. Use `/fill` to fill caves/holes
4. Use `/submerge` for water features

### Creating Walls/Fences

1. Select perimeter with `/pos1`, `/pos2`
2. Use `/walls <blockType> 1` to create walls
3. Stack upward with `/stack 5 up`

### Copying Buildings

1. Select entire building with `/pos1`, `/pos2`
2. Use `/copy` to save to clipboard
3. Move to new location
4. Use `/paste` to place copy
5. Use `/rotate` if orientation needs adjustment

### Mass Replacement

1. Select large area
2. Use `/replace Soil_Grass Rock_Stone` to convert terrain
3. Or use `/set` to completely replace all blocks

## Tips for Efficient Building

1. **Use chunks for large areas** - `/selectchunk` is faster than manual selection
2. **Expand carefully** - Use `/expand` to quickly grow selections
3. **Copy before modifying** - Always `/copy` before destructive edits
4. **Stack for repetition** - Use `/stack` instead of manual copying
5. **Undo safety net** - Keep `/undo` in mind for quick reverts
6. **History size** - Increase history size for complex projects
7. **Clear entities separately** - Use `/clearEntities` to remove NPCs/items from areas
8. **Masks for precision** - Use `/globalmask` to protect certain blocks
9. **Clipboard rotation** - Rotate before pasting for easier alignment
10. **Test on small areas** - Test complex operations on small selections first

## Advanced Techniques

### Creating Terrain Layers

```
/pos1
/pos2
/set Rock_Stone        # Base layer
/expand 10 up
/replace Rock_Stone Soil_Dirt    # Middle layer
/expand 2 up
/replace Soil_Dirt Soil_Grass     # Top layer
```

### Hollowing Buildings

```
/pos1
/pos2
/set Wood_Planks_Oak   # Solid building
/contract 1            # Shrink selection
/set Empty             # Clear interior
```

### Creating Spheres (Approximate)

Use `/editline` with strategic positioning and `/rotate` + `/paste` to create circular structures.

## Block Placement Override

**Toggle block placement rules:**
```
/toggleBlockPlacementOverride
```

Allows breaking normal block placement rules for advanced building.

## Related Guides

- [Creating Prefabs](22_Dungeons_and_Structures.md) - Save your builds as reusable prefabs
- [Block Sets](39_Block_Sets.md) - Understanding block categorization
- [Block Variants](75_Block_Variants.md) - Block rotation and placement
- [Using Commands In-Game](171_Using_Commands_In_Game.md) - All available commands

---

**Previous:** [Using Commands In-Game](171_Using_Commands_In_Game.md) | **Next:** [Asset Types Reference](173_Asset_Types_Reference.md)
