# Item Groups

Learn how item groups organize related blocks for world generation and creative library organization.

## Overview

Item groups are collections of related blocks (typically variants of the same material). They're used for organizing blocks in creative libraries and world generation systems.

## Location
`Server/Item/Groups/`

## Example from Game Files

### Stone Full Blocks Group

From `Server/Item/Groups/FullBlocks_Stone.json`:

```1:6:Server/Item/Groups/FullBlocks_Stone.json
{
  "Blocks": [
    "Rock_Stone",
    "Rock_Stone_Cobble"
  ]
}
```

This shows an item group that organizes related stone blocks together for easier reference in recipes and world generation.

## Basic Item Group Structure

Create `Server/Item/Groups/Group_MyCustom.json`:

```json
{
  "Blocks": [
    "Block_MyCustom_Full",
    "Block_MyCustom_Slab",
    "Block_MyCustom_Stairs",
    "Block_MyCustom_Corner"
  ]
}
```

## Group Properties

### Blocks Array

```json
{
  "Blocks": [
    "Block_ID_1",
    "Block_ID_2",
    "Block_ID_3"
  ]
}
```

Array of block/item IDs that belong to this group.

## Common Item Groups

### Material-Based Groups

Groups organize blocks by material type:

- **`FullBlocks_Stone`** - All stone block variants
- **`FullBlocks_Wood`** - All wood block variants
- **`FullBlocks_Brick`** - All brick variants

### Example: Stone Blocks

`Server/Item/Groups/FullBlocks_Stone.json`:

```json
{
  "Blocks": [
    "Rock_Stone",
    "Rock_Stone_Slab",
    "Rock_Stone_Stairs",
    "Rock_Stone_Corner"
  ]
}
```

## Using Item Groups

Item groups are referenced in:
- Creative library categories
- World generation systems
- Block organization

## Tips for Item Groups

1. **Material organization** - Group blocks by material/family
2. **Complete sets** - Include all variants (full, slab, stairs, etc.)
3. **Consistent naming** - Use clear, descriptive group names
4. **Creative libraries** - Groups help organize creative mode blocks

---

**Previous:** [Block Migrations](77_Block_Migrations.md) | **Next:** [Item Categories](79_Item_Categories.md)
