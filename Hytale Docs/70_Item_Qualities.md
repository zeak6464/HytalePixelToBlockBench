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

## Basic Quality Structure

Create `Server/Item/Qualities/MyCustom.json`:

```json
{
  "Color": "#FFFFFF",
  "DisplayNameSuffix": " [Common]"
}
```

## Quality Properties

### Color

```json
{
  "Color": "#FFFFFF"
}
```

Color tint for quality (used in UI, names, etc.).

### DisplayNameSuffix

```json
{
  "DisplayNameSuffix": " [Common]"
}
```

Suffix added to item display name.

## Common Qualities

### Common

`Server/Item/Qualities/Common.json`:

Basic quality tier.

### Uncommon

Higher tier than Common.

### Rare

High-tier quality.

### Epic

Very high-tier quality.

### Legendary

Highest tier quality.

### Technical / Developer

Special qualities for technical/dev items.

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
