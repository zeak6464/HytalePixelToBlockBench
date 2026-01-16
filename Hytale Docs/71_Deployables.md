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
`Server/Item/Items/Deployable/`

## Basic Deployable Structure

Create `Server/Item/Items/Deployable/Deployable_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Deployable_MyCustom.name"
  },
  "Icon": "Icons/ItemsGenerated/Deployable_MyCustom.png",
  "Categories": ["Items.Deployables"],
  "Deployable": {
    "EntityRoleId": "MyCustom_Totem",
    "Offset": {
      "X": 0,
      "Y": 0,
      "Z": 0
    }
  },
  "Tags": {
    "Type": ["Deployable"]
  }
}
```

## Deployable Properties

### EntityRoleId

```json
{
  "Deployable": {
    "EntityRoleId": "Healing_Totem"
  }
}
```

NPC Role ID of the entity to spawn when deployed.

### Offset

```json
{
  "Offset": {
    "X": 0,
    "Y": 0,
    "Z": 0
  }
}
```

Position offset from placement location.

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
