# Item Qualities

Learn how item quality tiers work and how to configure quality properties.

## Overview

Item qualities define tiers like Common, Uncommon, Rare, Epic, and Legendary. Qualities affect item properties, appearance, and game balance.

## Location
`Server/Item/Qualities/`

## Example from Game Files

### Common Quality

From `Server/Item/Qualities/Common.json`:

```1:15:Server/Item/Qualities/Common.json
{
  "QualityValue": 1,
  "ItemTooltipTexture": "UI/ItemQualities/Tooltips/ItemTooltipCommon.png",
  "ItemTooltipArrowTexture": "UI/ItemQualities/Tooltips/ItemTooltipCommonArrow.png",
  "SlotTexture": "UI/ItemQualities/Slots/SlotCommon.png",
  "BlockSlotTexture": "UI/ItemQualities/Slots/SlotCommon.png",
  "SpecialSlotTexture": "UI/ItemQualities/Slots/SlotCommon.png",
  "TextColor": "#c9d2dd",
  "LocalizationKey": "server.general.qualities.Common",
  "VisibleQualityLabel": true,
  "RenderSpecialSlot": true,
  "ItemEntityConfig": {
    "ParticleSystemId": "Drop_Common"
  }
}
```

This shows a quality tier definition with textures, colors, and particle effects.

## Quality Properties

### QualityValue

```json
{
  "QualityValue": 3
}
```

Numeric tier value (higher = better quality). Used for sorting and comparison.

### TextColor

```json
{
  "TextColor": "#2770b7"
}
```

Hex color for quality text in UI (item names, tooltips).

### LocalizationKey

```json
{
  "LocalizationKey": "server.general.qualities.Rare"
}
```

Translation key for the quality name.

### Tooltip Textures

```json
{
  "ItemTooltipTexture": "UI/ItemQualities/Tooltips/ItemTooltipRare.png",
  "ItemTooltipArrowTexture": "UI/ItemQualities/Tooltips/ItemTooltipRareArrow.png"
}
```

UI textures for item tooltips (background and arrow).

### Slot Textures

```json
{
  "SlotTexture": "UI/ItemQualities/Slots/SlotRare.png",
  "BlockSlotTexture": "UI/ItemQualities/Slots/SlotRare.png",
  "SpecialSlotTexture": "UI/ItemQualities/Slots/SlotRare.png"
}
```

Inventory slot textures for different slot types.

### Display Options

```json
{
  "VisibleQualityLabel": true,
  "RenderSpecialSlot": true
}
```

- **`VisibleQualityLabel`** - Whether to show the quality label in UI
- **`RenderSpecialSlot`** - Whether to render special slot appearance

### Item Drop Particles

```json
{
  "ItemEntityConfig": {
    "ParticleSystemId": "Drop_Rare"
  }
}
```

Particle effect when item is dropped in world.

### HideFromSearch (Debug/Template)

```json
{
  "HideFromSearch": true
}
```

Hides quality from search (used for Debug and Template qualities).

## Quality Tiers

All quality files from `Server/Item/Qualities/`:

| Quality | QualityValue | TextColor | Drop Particle | Notes |
|---------|--------------|-----------|---------------|-------|
| Junk | 0 | `#c9d2dd` | - | No label shown |
| Common | 1 | `#c9d2dd` | `Drop_Common` | |
| Uncommon | 2 | `#6fb651` | `Drop_Uncommon` | |
| Rare | 3 | `#2770b7` | `Drop_Rare` | |
| Epic | 4 | `#9e3fd7` | `Drop_Epic` | |
| Legendary | 5 | `#bb8a2c` | `Drop_Legendary` | |
| Technical | 8 | `#3b7a8f` | - | Special UI items |
| Tool | 9 | `#269edc` | - | Tools category |
| Debug | 10 | - | - | Hidden from search |
| Developer | 10 | - | - | Hidden from search |
| Template | 0 | - | - | Hidden from search |

### Junk Quality

From `Server/Item/Qualities/Junk.json` - Low-value items with no quality label:

```json
{
  "QualityValue": 0,
  "TextColor": "#c9d2dd",
  "VisibleQualityLabel": false,
  "RenderSpecialSlot": false
}
```

### Tool Quality

From `Server/Item/Qualities/Tool.json` - Special quality for tools:

```json
{
  "QualityValue": 9,
  "TextColor": "#269edc",
  "LocalizationKey": "server.general.qualities.Tool"
}
```

### Technical Quality

From `Server/Item/Qualities/Technical.json` - Technical/UI items:

```json
{
  "QualityValue": 8,
  "TextColor": "#3b7a8f",
  "LocalizationKey": "server.general.qualities.Technical"
}
```

### Special Qualities

- **Debug** - `QualityValue: 10`, hidden from search
- **Developer** - `QualityValue: 10`, hidden from search
- **Template** - `QualityValue: 0`, hidden from search

## Using Qualities

Reference in item definitions:

```json
{
  "Quality": "Common"
}
```

## Tips for Item Qualities

1. **Color coding** - Use distinct colors for each tier
2. **Naming** - Clear suffix conventions help players
3. **Balance** - Higher qualities should have better stats/features

---

**Previous:** [Unarmed Gathering](69_Unarmed_Gathering.md) | **Next:** [Deployables](71_Deployables.md)
