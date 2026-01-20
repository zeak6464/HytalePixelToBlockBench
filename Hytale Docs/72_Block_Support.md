# Block Support

Learn how to configure block support requirements that determine what blocks can be placed on top of or attached to other blocks.

## Overview

Block support defines placement rules:
- What blocks can be placed on (Support.Down)
- What blocks can support other blocks (Supporting.Up)
- Horizontal connections
- Support distance limits
- Tag-based support rules

## Location
Block support is configured in `BlockType.Support` and `BlockType.Supporting` properties.

## Example from Game Files

### Crop Block Support

From `Server/Item/Items/Plant/Crop/_Template/Template_Crop_Block.json`:

```163:172:Server/Item/Items/Plant/Crop/_Template/Template_Crop_Block.json
    "Support": {
      "Down": [
        {
          "TagId": "Type=Soil"
        },
        {
          "TagId": "SubType=Planter"
        }
      ]
    },
```

This shows block support configuration that defines which blocks (Soil or Planter) can support crops from below.

## Basic Support Structure

```json
{
  "BlockType": {
    "Support": {
      "Down": [
        {
          "FaceType": "Full"
        }
      ]
    },
    "Supporting": {
      "Up": [
        {
          "FaceType": "Full"
        }
      ]
    }
  }
}
```

## Support Properties

### Down Support

```json
{
  "Support": {
    "Down": [
      {
        "FaceType": "Full"
      }
    ]
  }
}
```

What this block can be placed on top of.

### Tag-Based Support

```json
{
  "Support": {
    "Down": [
      {
        "TagId": "Type=Soil",
        "FaceType": "Full"
      },
      {
        "TagId": "SubType=Tilled",
        "Support": "Disallowed"
      }
    ]
  }
}
```

Support rules based on block tags:
- **`TagId`** - Tag pattern to match
- **`Support: "Disallowed"`** - Explicitly prevents placement

### Face Types

```json
{
  "FaceType": "Full"
}
```

- **`Full`** - Full face support
- **`Bushes`** - Specific face type for bushes/plants

### Supporting

```json
{
  "Supporting": {
    "Up": [
      {
        "FaceType": "Bushes"
      }
    ]
  }
}
```

What blocks this block can support above it.

### Horizontal Support

```json
{
  "Support": {
    "Horizontal": [
      {
        "TagId": "Type=Trunk",
        "Rotate": false,
        "Support": "Ignored"
      }
    ]
  }
}
```

Side connections (for trees, poles, etc.).

### MaxSupportDistance

```json
{
  "MaxSupportDistance": 5
}
```

Maximum distance blocks can extend without support (for tree trunks, etc.).

### Support Propagation

```json
{
  "Support": {
    "Down": [
      {
        "AllowSupportPropagation": false
      }
    ]
  }
}
```

Whether support requirements propagate upward.

## Complete Example: Plant Bush

```json
{
  "Support": {
    "Down": [
      {
        "TagId": "Type=Soil",
        "FaceType": "Full"
      },
      {
        "FaceType": "Bushes"
      },
      {
        "TagId": "SubType=Tilled",
        "Support": "Disallowed"
      }
    ]
  },
  "Supporting": {
    "Up": [
      {
        "FaceType": "Bushes"
      }
    ]
  }
}
```

Can be placed on soil or other bushes, but not on tilled soil.

## Tips for Block Support

1. **Tag matching** - Use tags for flexible support rules
2. **Disallowed** - Explicitly prevent unwanted placements
3. **Face types** - Use specific face types for special cases
4. **MaxSupportDistance** - Set for blocks that extend vertically
5. **Support propagation** - Control whether support chains upward

---

**Previous:** [Deployables](71_Deployables.md) | **Next:** Back to [README](../README.md)
