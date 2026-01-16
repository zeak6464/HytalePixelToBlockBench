# Model Attachments

Learn how to attach models to entities and items for visual customization and equipment rendering.

## Overview

Model attachments allow models to have child models attached at specific points. Used for equipment (weapons, armor), cosmetics, and visual effects.

## Location
Model attachments are configured in `DefaultAttachments` array in model definitions.

## Basic Model Attachment Structure

```json
{
  "Model": "NPC/Intelligent/Goblin/Models/Model.blockymodel",
  "Texture": "NPC/Intelligent/Goblin/Models/Model_Textures/Default.png",
  "DefaultAttachments": [
    {
      "Model": "Items/Weapons/Sword/Iron.blockymodel",
      "Texture": "Items/Weapons/Sword/Iron_Texture.png"
    }
  ]
}
```

## Attachment Properties

### Model

```json
{
  "Model": "Items/Weapons/Sword/Iron.blockymodel"
}
```

Path to attached model file.

### Texture

```json
{
  "Texture": "Items/Weapons/Sword/Iron_Texture.png"
}
```

Texture for attached model.

### Position / Rotation

```json
{
  "Position": {
    "X": 0,
    "Y": 0,
    "Z": 0
  },
  "Rotation": {
    "X": 0,
    "Y": 0,
    "Z": 0
  }
}
```

Optional position and rotation offsets.

## Multiple Attachments

```json
{
  "DefaultAttachments": [
    {
      "Model": "Items/Weapons/Sword/Iron.blockymodel",
      "Texture": "Items/Weapons/Sword/Iron_Texture.png"
    },
    {
      "Model": "Items/Armor/Helmet/Iron.blockymodel",
      "Texture": "Items/Armor/Helmet/Iron_Texture.png"
    }
  ]
}
```

Multiple attachments can be defined.

## Equipment Attachments

NPCs with equipped items show attachments:

```json
{
  "Appearance": "Goblin_Scrapper",
  "DefaultAttachments": [
    {
      "Model": "Items/Weapons/Sword/Iron.blockymodel"
    }
  ]
}
```

NPC displays equipped weapon as attachment.

## Tips for Model Attachments

1. **Equipment rendering** - Attachments are how equipped items appear on models
2. **Cosmetic items** - Use attachments for visual-only cosmetics
3. **Multiple attachments** - Combine multiple attachments for full equipment sets
4. **Positioning** - Use Position/Rotation for precise attachment placement

---

**Previous:** [Inventory Operations](97_Inventory_Operations.md) | **Next:** [Animation Slots](99_Animation_Slots.md)
