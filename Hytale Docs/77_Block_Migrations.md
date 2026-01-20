# Block Migrations

Learn how block migrations handle updates when block IDs or states change between game versions.

## Overview

Block migrations allow the game to update old block IDs to new ones when loading worlds created with older versions. This ensures backward compatibility when block systems are updated.

## Location
`Server/Item/Block/Migrations/`

## Example from Game Files

### Block Migration Version 9

From `Server/Item/Block/Migrations/9.json`:

```1:12:Server/Item/Block/Migrations/9.json
{
  "DirectMigrations": {
    "*Furniture_Temple_Dark_Coffin_State_Definitions_OpenWindow": "*Furniture_Temple_Dark_Coffin_State_Definitions_Open",
    "*Furniture_Temple_Dark_Coffin_State_Definitions_CloseWindow": "Furniture_Temple_Dark_Coffin",
    "*Furniture_Human_Ruins_Coffin_State_Definitions_OpenWindow": "*Furniture_Human_Ruins_Coffin_State_Definitions_Open",
    "*Furniture_Human_Ruins_Coffin_State_Definitions_CloseWindow": "Furniture_Human_Ruins_Coffin",
    "*Furniture_Ancient_Coffin_State_Definitions_OpenWindow": "*Furniture_Ancient_Coffin_State_Definitions_Open",
    "*Furniture_Ancient_Coffin_State_Definitions_CloseWindow": "Furniture_Ancient_Coffin",
    "*Furniture_Village_Coffin_State_Definitions_OpenWindow": "*Furniture_Ancient_Coffin_State_Definitions_Open",
    "*Furniture_Village_Coffin_State_Definitions_CloseWindow": "Furniture_Ancient_Coffin"
  }
}
```

This shows a block migration configuration that maps old block IDs to new ones for version compatibility.

## Migration File Structure

Migration files are numbered (0.json, 1.json, etc.) and processed in order:

```json
{
  "DirectMigrations": {
    "Old_Block_ID": "New_Block_ID",
    "Old_State_ID": "*Block_ID_State_Definitions_StateName"
  }
}
```

## Direct Migrations

### Block ID Migration

```json
{
  "DirectMigrations": {
    "Plant_Crop_Carrot_Stage_0": "Plant_Crop_Carrot_Block",
    "Rock_Crystal_Blue_Small": "Rock_Crystal_Blue"
  }
}
```

Maps old block IDs to new block IDs.

### State Migration

```json
{
  "DirectMigrations": {
    "Plant_Crop_Carrot_Stage_1": "*Plant_Crop_Carrot_Block_State_Definitions_Stage1",
    "Plant_Crop_Carrot_Stage_2": "*Plant_Crop_Carrot_Block_State_Definitions_Stage2"
  }
}
```

Maps old state-based blocks to new state definitions:
- **`*Block_ID_State_Definitions_StateName`** - References a state in the block's state definitions

## Migration Processing

Migrations are processed in numerical order:
- `0.json` processes first
- `1.json` processes next
- And so on...

This allows multi-step migrations where blocks change IDs multiple times.

## Complete Example: Crop Migration

```json
{
  "DirectMigrations": {
    "Plant_Crop_Carrot_Stage_0": "Plant_Crop_Carrot_Block",
    "Plant_Crop_Carrot_Stage_1": "*Plant_Crop_Carrot_Block_State_Definitions_Stage1",
    "Plant_Crop_Carrot_Stage_2": "*Plant_Crop_Carrot_Block_State_Definitions_Stage2"
  }
}
```

Converts old stage-based crop blocks to new state-based system.

## Tips for Block Migrations

1. **Versioning** - Number migrations sequentially
2. **Backward compatibility** - Always migrate old IDs to new ones
3. **State references** - Use `*Block_ID_State_Definitions_StateName` format for states
4. **Testing** - Test migrations with old world saves
5. **Documentation** - Document why migrations are needed

---

**Previous:** [Block States](76_Block_States.md) | **Next:** [Item Groups](78_Item_Groups.md)
