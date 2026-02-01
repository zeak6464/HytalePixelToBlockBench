# Deployables

Learn how to create deployable items that place interactive entities like totems, turrets, and other structures.

## Overview

Deployables are items that, when used, spawn entities in the world. Common deployables include:
- Healing totems
- Damage/slowness totems
- Turrets
- Traps
- Other interactive structures

## Location
`Server/Item/Items/Weapon/Deployable/`

## Example from Game Files

### Healing Totem Deployable

From `Server/Item/Items/Weapon/Deployable/Weapon_Deployable_Healing_Totem.json`:

```1:40:Server/Item/Items/Weapon/Deployable/Weapon_Deployable_Healing_Totem.json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Deployable_Healing_Totem.name"
  },
  "Categories": [
    "Items.Weapons",
    "Items.Utility",
    "Items.Debug"
  ],
  "Recipe": {
    "TimeSeconds": 3,
    "Input": [
      {
        "ItemId": "Ingredient_Life_Essence",
        "Quantity": 50
      },
      {
        "ItemId": "Ingredient_Bar_Thorium",
        "Quantity": 20
      },
      {
        "ItemId": "Potion_Health_Greater",
        "Quantity": 10
      }
    ],
    "BenchRequirement": [
      {
        "Id": "Arcanebench",
        "Type": "Crafting",
        "Categories": [
          "Arcane_Misc"
        ]
      }
    ]
  },
  "Scale": 0.305,
  "Quality": "Common",
  "ItemLevel": 40,
  "MaxStack": 1,
  "Model": "Items/Deployables/Healing_Totem/Healing_Totem_Projectile.blockymodel",
```

This shows a deployable item configuration for a healing totem with recipe, crafting requirements, and model properties.

## Basic Deployable Structure

Deployables use projectile interactions to spawn entities. Create `Server/Item/Items/Weapon/Deployable/Weapon_Deployable_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Deployable_MyCustom.name"
  },
  "Categories": ["Items.Weapons", "Items.Utility"],
  "Icon": "Icons/ItemsGenerated/Weapon_Deployable_MyCustom.png",
  "Model": "Items/Deployables/MyCustom/MyCustom_Projectile.blockymodel",
  "Texture": "Items/Deployables/MyCustom/MyCustom_Texture.png",
  "PlayerAnimationsId": "Item",
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "Serial",
          "Interactions": [
            {
              "Type": "Simple",
              "RunTime": 0.25,
              "Effects": {
                "ItemPlayerAnimationsId": "Item",
                "ItemAnimationId": "Throw"
              }
            },
            {
              "Type": "Projectile",
              "Config": "Projectile_Config_MyCustom_Deploy"
            }
          ]
        }
      ],
      "Cooldown": {
        "Id": "DeployableUtility",
        "Cooldown": 10
      }
    }
  },
  "Tags": {
    "Type": ["Weapon"],
    "Family": ["Deployable"]
  }
}
```

## Deployable Properties

### Projectile Config

Deployables use a projectile configuration to spawn the entity:

```json
{
  "Type": "Projectile",
  "Config": "Projectile_Config_Healing_Totem_Deploy"
}
```

The projectile config defines what entity spawns when it lands.

### Cooldown

```json
{
  "Cooldown": {
    "Id": "DeployableUtility",
    "Cooldown": 10
  }
}
```

Limits how often the deployable can be used.

## Example: Healing Totem

`Server/Item/Items/Deployable/Deployable_Healing_Totem.json`:

Deploys a healing totem entity that provides area healing.

## Example: Crossbow Turret

`Server/Item/Items/Deployable/Deployable_Crossbow_Turret.json`:

Deploys a turret entity that automatically attacks enemies.

## Deployable Entities

Deployables spawn NPC entities. These entities need:
- NPC Role definitions
- Behavior components
- Interaction systems
- Visual models

## Tips for Creating Deployables

1. **Entity roles** - Create NPC roles for deployable entities
2. **Placement rules** - Configure where deployables can be placed
3. **Entity behavior** - Set up AI/behavior for deployed entities
4. **Balance** - Consider resource cost vs deployable power
5. **Cleanup** - Configure entity lifetime/despawn rules

---

**Previous:** [Item Qualities](70_Item_Qualities.md) | **Next:** [Block Support](72_Block_Support.md)
