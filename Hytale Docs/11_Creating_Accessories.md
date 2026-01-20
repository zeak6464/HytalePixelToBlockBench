# Creating Accessories

Learn how to create cosmetic accessories (head accessories, face accessories, and ear accessories) for character customization.

## Overview

Accessories in Hytale are **character cosmetics** that appear in the character customization menu. They're separate from armor items and are defined as JSON arrays in the `Cosmetics/CharacterCreator/` folder.

**Note:** Accessories are cosmetic only - they don't provide stats or abilities. Use armor items if you need functional equipment.

## Accessory Types

There are three types of accessories:
1. **Head Accessories** - Hats, crowns, headbands, etc.
2. **Face Accessories** - Glasses, eyepatches, masks
3. **Ear Accessories** - Earrings

## Location

`Cosmetics/CharacterCreator/`

## Example from Game Files

### Head Accessories

From `Cosmetics/CharacterCreator/HeadAccessory.json`:

```1:30:Cosmetics/CharacterCreator/HeadAccessory.json
[
  {
    "HeadAccessoryType": "Simple",
    "Id": "Goggles",
    "Name": "avatarCustomization.headaccessory.Goggles.name",
    "Model": "Cosmetics/Head/Goggles.blockymodel",
    "GreyscaleTexture": "Cosmetics/Head/Goggles_Texture.png",
    "GradientSet": "Colored_Cotton",
    "Entitlements": [
      "game.base",
      "game.deluxe",
      "game.founder"
    ]
  },
  {
    "HeadAccessoryType": "FullyCovering",
    "Id": "Hoodie",
    "Name": "avatarCustomization.headaccessory.Hoodie.name",
    "Model": "Cosmetics/Head/Hoodie.blockymodel",
    "GreyscaleTexture": "Cosmetics/Head/Hoodie_Textures/Hoodie_Fantasy_Greyscale.png",
    "GradientSet": "Colored_Cotton"
  },
  {
    "HeadAccessoryType": "Simple",
    "Id": "GiHeadband",
    "Name": "avatarCustomization.headaccessory.GiHeadband.name",
    "Model": "Cosmetics/Head/GiHeadband.blockymodel",
    "GreyscaleTexture": "Cosmetics/Head/GiHeadband_Textures/GiHeadband_Greyscale_Texture.png",
    "GradientSet": "Colored_Cotton"
  },
```

This shows the structure of the head accessories JSON array with different accessory types, models, textures, and gradient sets.

Files:
- `HeadAccessory.json` - Head accessories
- `FaceAccessory.json` - Face accessories
- `EarAccessory.json` - Ear accessories

---

## Creating Head Accessories

### Location
`Cosmetics/CharacterCreator/HeadAccessory.json`

### Basic Head Accessory Structure

The file is a **JSON array**. Add your accessory to the array:

```json
[
  {
    "HeadAccessoryType": "Simple",
    "Id": "MyCustomHat",
    "Name": "avatarCustomization.headaccessory.MyCustomHat.name",
    "Model": "Cosmetics/Head/MyCustomHat.blockymodel",
    "GreyscaleTexture": "Cosmetics/Head/MyCustomHat_Greyscale.png",
    "GradientSet": "Colored_Cotton"
  }
]
```

### Head Accessory Types

| Type | Description | Examples |
|------|-------------|----------|
| `Simple` | Small accessories that don't cover much | Headbands, crowns, flowers |
| `HalfCovering` | Covers part of the head/hair | Beanies, caps, bandanas |
| `FullyCovering` | Covers the entire head | Hoodies, full masks |

### Using Greyscale Textures with Gradient Sets

Greyscale textures can be colored using gradient sets:

```json
{
  "HeadAccessoryType": "Simple",
  "Id": "MyCustomHat",
  "Name": "avatarCustomization.headaccessory.MyCustomHat.name",
  "Model": "Cosmetics/Head/MyCustomHat.blockymodel",
  "GreyscaleTexture": "Cosmetics/Head/MyCustomHat_Greyscale.png",
  "GradientSet": "Colored_Cotton"
}
```

**Available Gradient Sets:**
- `Colored_Cotton` - Fabric materials
- `Ornamented_Metal` - Metallic materials
- `Flashy_Synthetic` - Synthetic/plastic materials
- `Pastel_Cotton` - Soft pastel colors
- `Shiny_Fabric` - Glossy fabric
- `Faded_Leather` - Worn leather

### Using Fixed Textures

For accessories with fixed colors, use the `Textures` object:

```json
{
  "HeadAccessoryType": "Simple",
  "Id": "MyCustomCrown",
  "Name": "avatarCustomization.headaccessory.MyCustomCrown.name",
  "Model": "Cosmetics/Head/MyCustomCrown.blockymodel",
  "Textures": {
    "Gold": {
      "Texture": "Cosmetics/Head/MyCustomCrown_Gold.png",
      "BaseColor": ["#d4af37"]
    },
    "Silver": {
      "Texture": "Cosmetics/Head/MyCustomCrown_Silver.png",
      "BaseColor": ["#c0c0c0"]
    }
  }
}
```

### Complete Example: Custom Headband

```json
{
  "HeadAccessoryType": "Simple",
  "Id": "CustomHeadband",
  "Name": "avatarCustomization.headaccessory.CustomHeadband.name",
  "Model": "Cosmetics/Head/CustomHeadband.blockymodel",
  "GreyscaleTexture": "Cosmetics/Head/CustomHeadband_Greyscale.png",
  "GradientSet": "Colored_Cotton"
}
```

### Entitlements (Optional)

You can restrict accessories to specific game editions:

```json
{
  "HeadAccessoryType": "Simple",
  "Id": "ExclusiveHat",
  "Name": "avatarCustomization.headaccessory.ExclusiveHat.name",
  "Model": "Cosmetics/Head/ExclusiveHat.blockymodel",
  "GreyscaleTexture": "Cosmetics/Head/ExclusiveHat_Greyscale.png",
  "GradientSet": "Colored_Cotton",
  "Entitlements": [
    "game.base",
    "game.deluxe",
    "game.founder"
  ]
}
```

**Entitlement Options:**
- `game.base` - Base game
- `game.deluxe` - Deluxe edition
- `game.founder` - Founder edition

---

## Creating Face Accessories

### Location
`Cosmetics/CharacterCreator/FaceAccessory.json`

### Basic Face Accessory Structure

```json
[
  {
    "Id": "MyCustomGlasses",
    "Name": "avatarCustomization.faceaccessory.MyCustomGlasses.name",
    "Model": "Cosmetics/Face_Accessories/MyCustomGlasses.blockymodel",
    "GreyscaleTexture": "Cosmetics/Face_Accessories/MyCustomGlasses_Greyscale.png",
    "GradientSet": "Colored_Cotton"
  }
]
```

### Using Fixed Textures

```json
{
  "Id": "MyCustomEyePatch",
  "Name": "avatarCustomization.faceaccessory.MyCustomEyePatch.name",
  "Model": "Cosmetics/Face_Accessories/MyCustomEyePatch.blockymodel",
  "Textures": {
    "Black": {
      "Texture": "Cosmetics/Face_Accessories/MyCustomEyePatch_Black.png",
      "BaseColor": ["#000000"]
    },
    "White": {
      "Texture": "Cosmetics/Face_Accessories/MyCustomEyePatch_White.png",
      "BaseColor": ["#ffffff"]
    }
  }
}
```

### Complete Example: Custom Glasses

```json
{
  "Id": "CustomGlasses",
  "Name": "avatarCustomization.faceaccessory.CustomGlasses.name",
  "Model": "Cosmetics/Face_Accessories/Glasses.blockymodel",
  "GreyscaleTexture": "Cosmetics/Face_Accessories/Glasses_Textures/CustomGlasses_Greyscale.png",
  "GradientSet": "Colored_Cotton"
}
```

---

## Creating Ear Accessories

### Location
`Cosmetics/CharacterCreator/EarAccessory.json`

### Basic Ear Accessory Structure

Ear accessories can have **variants** for left, right, or both ears:

```json
[
  {
    "Id": "MyCustomEarring",
    "Name": "avatarCustomization.earaccessory.MyCustomEarring.name",
    "Variants": {
      "Both": {
        "Model": "Cosmetics/Head/MyCustomEarring.blockymodel",
        "GreyscaleTexture": "Cosmetics/Head/Earring_Texture_Greyscale.png",
        "NameKey": "avatarCustomization.variants.earSide.both"
      },
      "Left": {
        "Model": "Cosmetics/Head/MyCustomEarringLeft.blockymodel",
        "GreyscaleTexture": "Cosmetics/Head/Earring_Texture_Greyscale.png",
        "NameKey": "avatarCustomization.variants.earSide.left"
      },
      "Right": {
        "Model": "Cosmetics/Head/MyCustomEarringRight.blockymodel",
        "GreyscaleTexture": "Cosmetics/Head/Earring_Texture_Greyscale.png",
        "NameKey": "avatarCustomization.variants.earSide.right"
      }
    },
    "GreyscaleTexture": "Cosmetics/Head/Earring_Texture_Greyscale.png",
    "GradientSet": "Ornamented_Metal",
    "VariantLocalizationKey": "avatarCustomization.variants.earSide"
  }
]
```

### Simplified Ear Accessory (No Variants)

If you only provide a base model/texture, it will be used for all variants:

```json
{
  "Id": "SimpleEarring",
  "Name": "avatarCustomization.earaccessory.SimpleEarring.name",
  "Model": "Cosmetics/Head/SimpleEarring.blockymodel",
  "GreyscaleTexture": "Cosmetics/Head/MetalEarring_Texture_Greyscale.png",
  "GradientSet": "Ornamented_Metal",
  "VariantLocalizationKey": "avatarCustomization.variants.earSide"
}
```

### Ear Accessory with Fixed Textures

```json
{
  "Id": "BeadedEarring",
  "Name": "avatarCustomization.earaccessory.BeadedEarring.name",
  "Variants": {
    "Both": {
      "Model": "Cosmetics/Head/BeadedEarring.blockymodel",
      "Textures": {
        "Red": {
          "Texture": "Cosmetics/Head/BeadedEarring_Red.png",
          "BaseColor": ["#9c1d1d"]
        },
        "Blue": {
          "Texture": "Cosmetics/Head/BeadedEarring_Blue.png",
          "BaseColor": ["#3080a8"]
        }
      },
      "NameKey": "avatarCustomization.variants.earSide.both"
    }
  },
  "VariantLocalizationKey": "avatarCustomization.variants.earSide"
}
```

---

## Accessory Properties Reference

### Head Accessory Fields

| Field | Required | Description |
|-------|----------|-------------|
| `HeadAccessoryType` | Yes | `"Simple"`, `"HalfCovering"`, or `"FullyCovering"` |
| `Id` | Yes | Unique identifier (no spaces, use underscores) |
| `Name` | Yes | Translation key for the display name |
| `Model` | Yes | Path to `.blockymodel` file (relative to `Common/`) |
| `GreyscaleTexture` | No* | Path to greyscale texture (use with `GradientSet`) |
| `Textures` | No* | Object with fixed texture variants |
| `GradientSet` | No | Color gradient set name (for greyscale textures) |
| `Entitlements` | No | Array of game editions that can use this |

*Either `GreyscaleTexture` or `Textures` is required

### Face Accessory Fields

| Field | Required | Description |
|-------|----------|-------------|
| `Id` | Yes | Unique identifier |
| `Name` | Yes | Translation key for the display name |
| `Model` | Yes | Path to `.blockymodel` file |
| `GreyscaleTexture` | No* | Path to greyscale texture |
| `Textures` | No* | Object with fixed texture variants |
| `GradientSet` | No | Color gradient set name |
| `Entitlements` | No | Array of game editions |

### Ear Accessory Fields

| Field | Required | Description |
|-------|----------|-------------|
| `Id` | Yes | Unique identifier |
| `Name` | Yes | Translation key for the display name |
| `Variants` | No* | Object with `Left`, `Right`, `Both` variants |
| `Model` | No* | Base model (used if no variants) |
| `GreyscaleTexture` | No* | Base texture (used if no variants) |
| `Textures` | No* | Base textures object |
| `GradientSet` | No | Color gradient set name |
| `VariantLocalizationKey` | Yes | Translation key for variant selection UI |

---

## How Armor Hides Accessories

When you create armor items (especially helmets), you can hide accessories using the `CosmeticsToHide` field:

```json
{
  "Armor": {
    "ArmorSlot": "Head",
    "CosmeticsToHide": [
      "EarAccessory",
      "HeadAccessory",
      "Haircut"
    ]
  }
}
```

**Common Cosmetics to Hide:**
- `HeadAccessory` - Hides head accessories
- `FaceAccessory` - Hides face accessories
- `EarAccessory` - Hides ear accessories
- `Haircut` - Hides hair

---

## Asset Paths

All paths are **relative to the `Common/` folder**:

**Models:**
- Head: `Cosmetics/Head/{Name}.blockymodel`
- Face: `Cosmetics/Face_Accessories/{Name}.blockymodel`
- Ear: `Cosmetics/Head/{Name}.blockymodel`

**Textures:**
- Head: `Cosmetics/Head/{Name}_Greyscale.png` or `Cosmetics/Head/{Name}_Textures/{Variant}.png`
- Face: `Cosmetics/Face_Accessories/{Name}_Greyscale.png` or `Cosmetics/Face_Accessories/{Name}_Texture.png`
- Ear: `Cosmetics/Head/{Name}_Texture_Greyscale.png`

---

## Translation Keys

You'll need to add translation keys to language files. Add entries in:
`Common/Languages/en-US/avatarCustomization/`

**For Head Accessories:**
```
avatarCustomization.headaccessory.MyCustomHat.name = "My Custom Hat"
```

**For Face Accessories:**
```
avatarCustomization.faceaccessory.MyCustomGlasses.name = "My Custom Glasses"
```

**For Ear Accessories:**
```
avatarCustomization.earaccessory.MyCustomEarring.name = "My Custom Earring"
```

**For Ear Variants (usually already defined):**
```
avatarCustomization.variants.earSide.both = "Both"
avatarCustomization.variants.earSide.left = "Left"
avatarCustomization.variants.earSide.right = "Right"
```

---

## Complete Examples

### Example 1: Simple Headband (Greyscale)

**1. Add to `Cosmetics/CharacterCreator/HeadAccessory.json`:**
```json
{
  "HeadAccessoryType": "Simple",
  "Id": "WarriorHeadband",
  "Name": "avatarCustomization.headaccessory.WarriorHeadband.name",
  "Model": "Cosmetics/Head/WarriorHeadband.blockymodel",
  "GreyscaleTexture": "Cosmetics/Head/WarriorHeadband_Greyscale.png",
  "GradientSet": "Colored_Cotton"
}
```

**2. Add translation to language file:**
```
avatarCustomization.headaccessory.WarriorHeadband.name = "Warrior Headband"
```

### Example 2: Fixed-Color Crown

```json
{
  "HeadAccessoryType": "Simple",
  "Id": "RoyalCrown",
  "Name": "avatarCustomization.headaccessory.RoyalCrown.name",
  "Model": "Cosmetics/Head/RoyalCrown.blockymodel",
  "Textures": {
    "Gold": {
      "Texture": "Cosmetics/Head/RoyalCrown_Gold.png",
      "BaseColor": ["#d4af37"]
    },
    "Silver": {
      "Texture": "Cosmetics/Head/RoyalCrown_Silver.png",
      "BaseColor": ["#c0c0c0"]
    },
    "Diamond": {
      "Texture": "Cosmetics/Head/RoyalCrown_Diamond.png",
      "BaseColor": ["#b9f2ff"]
    }
  }
}
```

### Example 3: Face Accessory - Glasses

```json
{
  "Id": "TechGlasses",
  "Name": "avatarCustomization.faceaccessory.TechGlasses.name",
  "Model": "Cosmetics/Face_Accessories/Glasses.blockymodel",
  "GreyscaleTexture": "Cosmetics/Face_Accessories/Glasses_Textures/TechGlasses_Greyscale.png",
  "GradientSet": "Ornamented_Metal"
}
```

### Example 4: Ear Accessory with Variants

```json
{
  "Id": "GoldHoops",
  "Name": "avatarCustomization.earaccessory.GoldHoops.name",
  "Variants": {
    "Both": {
      "Model": "Cosmetics/Head/GoldHoops.blockymodel",
      "Textures": {
        "Gold": {
          "Texture": "Cosmetics/Head/GoldHoops_Texture.png",
          "BaseColor": ["#d4af37"]
        }
      },
      "NameKey": "avatarCustomization.variants.earSide.both"
    },
    "Left": {
      "Model": "Cosmetics/Head/GoldHoopsLeft.blockymodel",
      "Textures": {
        "Gold": {
          "Texture": "Cosmetics/Head/GoldHoops_Texture.png",
          "BaseColor": ["#d4af37"]
        }
      },
      "NameKey": "avatarCustomization.variants.earSide.left"
    },
    "Right": {
      "Model": "Cosmetics/Head/GoldHoopsRight.blockymodel",
      "Textures": {
        "Gold": {
          "Texture": "Cosmetics/Head/GoldHoops_Texture.png",
          "BaseColor": ["#d4af37"]
        }
      },
      "NameKey": "avatarCustomization.variants.earSide.right"
    }
  },
  "VariantLocalizationKey": "avatarCustomization.variants.earSide"
}
```

---

## Tips for Creating Accessories

1. **Use existing models** - Reuse existing accessory models when possible (e.g., glasses models for different face accessories)
2. **Greyscale for flexibility** - Greyscale textures with gradient sets allow players to color accessories
3. **Fixed textures for specific looks** - Use fixed textures when you want exact colors/styles
4. **Match model paths** - Ensure your model files exist in the `Common/Cosmetics/` folder structure
5. **Test with armor** - Verify accessories hide/show correctly when armor is equipped
6. **Unique IDs** - Use descriptive, unique IDs (no spaces, use underscores or CamelCase)
7. **HeadAccessoryType matters** - Choose the correct type to ensure proper hair/head visibility

---

## Differences: Accessories vs Armor

| Feature | Accessories | Armor |
|---------|-------------|-------|
| **Location** | `Cosmetics/CharacterCreator/` | `Server/Item/Items/Armor/` |
| **Type** | JSON array | JSON object |
| **Function** | Cosmetic only | Provides stats/defense |
| **Equipable** | No (character customization) | Yes (equipable item) |
| **Craftable** | No | Yes (via recipes) |
| **Hideable** | Can be hidden by armor | Can hide accessories |

## Equipment Slots Reference

For **functional armor items**, Hytale uses the following equipment slots:

| Slot Name | Description | Example Items |
|-----------|-------------|---------------|
| **`Head`** | Helmets and head armor | Bronze Helm, Iron Helm, Cloth Hood |
| **`Chest`** | Chest plates and cuirasses | Bronze Cuirass, Iron Cuirass, Cloth Tunic |
| **`Hands`** | Gauntlets and gloves | Bronze Gauntlets, Iron Gauntlets, Cloth Gloves |
| **`Legs`** | Leg armor and greaves | Bronze Greaves, Iron Greaves, Cloth Pants |
| **`Back`** | Back slot items | Capes, backpacks |
| **`Trinket`** | Accessory items | Rings, amulets, charms |

**Usage in armor items:**

```json
{
  "EquipmentSlot": "Head",
  "ArmorType": "Heavy",
  "DamageResistance": {
    "Physical": 10
  }
}
```

**Note:** These slots are for **functional** armor items that provide stats. Cosmetic accessories (from this guide) are separate from the equipment system and appear only in character customization.

See the [Creating Items](02_Items.md#armor) guide for creating functional armor that uses these equipment slots.

---

**Previous:** [Audio & VFX](10_Audio_and_VFX.md) | **Next:** [Server Modding](12_Server_Modding.md)
