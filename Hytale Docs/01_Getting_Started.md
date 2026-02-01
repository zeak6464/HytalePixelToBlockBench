# Getting Started

This guide covers the fundamental concepts you need to understand before creating mods in Hytale.

## Project Structure Overview

```
Assets/
├── Common/                    # Visual assets (models, textures, animations, sounds)
│   ├── Blocks/               # Block models and textures
│   ├── BlockTextures/        # Block texture PNGs
│   ├── Characters/           # Player models, animations, attachments
│   ├── Icons/                # UI icons for items, categories, etc.
│   ├── Items/                # Item models, textures, animations
│   ├── Music/                # Background music (.ogg)
│   ├── NPC/                  # NPC models, textures, animations
│   ├── Particles/            # Particle effect textures
│   ├── Sounds/               # Sound effects (.ogg)
│   └── UI/                   # UI elements and layouts
│
├── Server/                   # Game logic definitions (JSON configs)
│   ├── Audio/                # Audio configurations (sound events, ambience, reverb)
│   ├── BarterShops/          # Trading shops and merchants
│   ├── BlockTypeList/        # Block type lists for categorization
│   ├── Camera/               # Camera effects and shake
│   ├── Drops/                # Loot tables and drop configurations
│   ├── Entity/               # Entity stats, effects, damage types
│   ├── Environments/         # Environment and biome configurations
│   ├── Farming/              # Farming systems and modifiers
│   ├── GameplayConfigs/      # Gameplay configuration settings
│   ├── HytaleGenerator/      # World generation settings
│   ├── Instances/            # Server instance configurations
│   ├── Item/                 # Item definitions, interactions, recipes
│   ├── Languages/            # Language and translation files
│   ├── MacroCommands/        # Custom command definitions
│   ├── Models/               # NPC appearance configs
│   ├── NPC/                  # NPC behaviors, AI, spawning
│   ├── Objective/            # Quest and objective definitions
│   ├── Particles/            # Particle system definitions
│   ├── PortalTypes/          # Portal destination configurations
│   ├── Prefabs/              # World structure definitions
│   ├── PrefabList/           # Prefab lists for world generation
│   ├── PrefabEditorCreationSettings/ # Prefab editor settings
│   ├── Projectiles/          # Projectile definitions
│   ├── ProjectileConfigs/    # Projectile configurations for weapons
│   ├── ResponseCurves/       # Mathematical curves for animations
│   ├── ScriptedBrushes/      # World editing brush scripts
│   ├── TagPatterns/          # Advanced tag pattern matching
│   ├── Weathers/             # Weather system configurations
│   ├── WordLists/            # Word lists for name generation
│   └── World/                # World-specific configurations
│
└── Cosmetics/                # Character customization options
```

---

## Core Concepts

### 1. Parent/Template Inheritance

Hytale uses a **parent-child inheritance system**. Instead of duplicating data, you reference a parent and only override what's different.

```json
{
  "Parent": "Template_Weapon_Sword",
  "TranslationProperties": {
    "Name": "server.items.My_Custom_Sword.name"
  },
  "Quality": "Rare",
  "ItemLevel": 25
}
```

### 2. Reference vs Variant Types

- **`"Type": "Abstract"`** - A template that cannot be spawned directly
- **`"Type": "Variant"`** - An instantiable entity based on a reference

### 3. Asset Path Format

All paths are **relative to the `Common/` folder**:

```json
"Model": "Items/Weapons/Sword/Iron.blockymodel"
"Texture": "Items/Weapons/Sword/Iron_Texture.png"
"Icon": "Icons/ItemsGenerated/Weapon_Sword_Iron.png"
```

### 4. The Compute Pattern

Parameters can be dynamically computed from configurable values:

```json
"Parameters": {
  "MaxHealth": {
    "Value": 100,
    "Description": "Max health for the NPC"
  }
},
"MaxHealth": { "Compute": "MaxHealth" }
```

---

## Asset Paths Reference

### Models (.blockymodel)

| Category | Path Pattern |
|----------|--------------|
| Items | `Items/Weapons/{Type}/{Name}.blockymodel` |
| NPCs | `NPC/{Category}/{Type}/Models/Model.blockymodel` |
| Blocks | `Blocks/{Category}/{Name}.blockymodel` |

### Textures (.png)

| Category | Path Pattern |
|----------|--------------|
| Items | `Items/Weapons/{Type}/{Name}_Texture.png` |
| NPCs | `NPC/{Category}/{Type}/Models/Model_Textures/{Variant}.png` |
| Blocks | `BlockTextures/{Name}.png` |

### Animations (.blockyanim)

| Category | Path Pattern |
|----------|--------------|
| Characters | `Characters/Animations/{Category}/{Action}.blockyanim` |
| Items | `Items/Animations/{Action}.blockyanim` |
| NPCs | `NPC/{Category}/{Type}/{Action}.blockyanim` |

### Icons (.png)

| Category | Path |
|----------|------|
| Items | `Icons/ItemsGenerated/{ItemId}.png` |
| Categories | `Icons/ItemCategories/{Category}.png` |
| Resources | `Icons/ResourceTypes/{Type}.png` |

### Sounds (.ogg)

| Category | Path |
|----------|------|
| Effects | `Sounds/{Category}/{Sound}.ogg` |
| Music | `Music/{Zone}/{Track}.ogg` |

---

## Common Patterns

### 1. Reusing Existing Visuals with New Stats

```json
{
  "Parent": "Template_Weapon_Sword",
  "Model": "Items/Weapons/Sword/Iron.blockymodel",
  "Texture": "Items/Weapons/Sword/Iron_Texture.png",
  "Quality": "Epic",
  "ItemLevel": 50,
  "MaxDurability": 200
}
```

### 2. Creating Tier Variants

Name your files with tier suffixes:
- `Weapon_Sword_Custom_T1.json`
- `Weapon_Sword_Custom_T2.json`
- `Weapon_Sword_Custom_T3.json`

### 3. Changing NPC Weapons

```json
{
  "Type": "Variant",
  "Reference": "Template_Goblin_Scrapper",
  "Modify": {
    "Weapons": ["Weapon_Sword_Iron", "Weapon_Shield_Iron"]
  }
}
```

### 4. Adding Visual Effects

```json
{
  "ItemAppearanceConditions": {
    "SignatureEnergy": [
      {
        "Condition": [100, 100],
        "ConditionValueType": "Percent",
        "Particles": [
          {
            "SystemId": "Sword_Signature_Ready",
            "TargetNodeName": "Handle",
            "TargetEntityPart": "PrimaryItem"
          }
        ]
      }
    ]
  }
}
```

---

## Quick Reference: Existing Asset IDs

### Weapon Templates
- `Template_Weapon_Sword`
- `Template_Weapon_Longsword`
- `Template_Weapon_Axe`
- `Template_Weapon_Battleaxe`
- `Template_Weapon_Spear`
- `Template_Weapon_Daggers`
- `Template_Weapon_Staff`
- `Template_Weapon_Crossbow`
- `Template_Weapon_Shortbow`

### Material Tiers
- `Crude`, `Scrap`, `Bone`, `Wood`
- `Copper`, `Bronze`, `Iron`, `Silver`, `Gold`
- `Cobalt`, `Mithril`, `Thorium`
- `Adamantite`, `Prisma`, `Onyxium`

### NPC Templates
- `Template_Goblin` / `Template_Goblin_Scrapper`
- `Template_Trork`
- `Template_Skeleton`
- `Template_Kweebec`

### Ingredient IDs
- `Ingredient_Bar_Iron`, `Ingredient_Bar_Copper`, etc.
- `Ingredient_Leather_Light`, `Ingredient_Leather_Heavy`
- `Ingredient_Fabric_Scrap_Linen`, `Ingredient_Fabric_Scrap_Wool`, etc.

---

## Tips

1. **Start with existing files** - Copy an existing item/NPC and modify it
2. **Use Parent inheritance** - Only override what you need to change
3. **Check paths carefully** - Asset paths are case-sensitive
4. **Reference existing IDs** - Browse `Server/Item/Items/` for valid ItemIds
5. **Test incrementally** - Make small changes and test frequently

---

**Next:** [Creating Items](02_Items.md) → [Creating NPCs](03_NPCs.md)
