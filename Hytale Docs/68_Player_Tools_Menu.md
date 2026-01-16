# Player Tools Menu

Learn how to configure the player tools menu filter for creative/builder tools.

## Overview

The player tools menu filter defines which items appear in the builder/editor tools menu. Used to organize creative tools like paint, sculpt, selection, and paste tools.

## Location
`Server/Item/PlayerToolsMenuConfig/`

## Basic Tools Menu Structure

Create `Server/Item/PlayerToolsMenuConfig/MyCustom_Filter.json`:

```json
{
  "BuilderToolItems": [
    "EditorTool_Paint",
    "EditorTool_Sculpt",
    "EditorTool_Selection",
    "EditorTool_Paste",
    "EditorTool_Line",
    "EditorTool_Layers"
  ]
}
```

## Tools Menu Properties

### BuilderToolItems

```json
{
  "BuilderToolItems": [
    "ItemId_1",
    "ItemId_2",
    "ItemId_3"
  ]
}
```

Array of item IDs that appear in the tools menu.

## Example: Default Builder Tools

`Server/Item/PlayerToolsMenuConfig/PlayerToolsMenuDropdownFilter.json`:

```json
{
  "BuilderToolItems": [
    "EditorTool_Paint",
    "EditorTool_Sculpt",
    "EditorTool_Selection",
    "EditorTool_Paste",
    "EditorTool_Line",
    "EditorTool_Layers"
  ]
}
```

Lists standard editor tools.

## Tips for Tools Menu

1. **Tool organization** - Group related tools together
2. **Item IDs** - Ensure all referenced items exist
3. **Creative mode** - Typically used in Creative mode contexts

---

**Previous:** [Reticles](67_Reticles.md) | **Next:** [Unarmed Gathering](69_Unarmed_Gathering.md)
