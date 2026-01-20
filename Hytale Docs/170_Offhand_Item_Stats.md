# Off-Hand Item Stats

Learn how to add stat bonuses to off-hand items like shields, torches, and dual-wielded weapons.

## Overview

Off-hand items can provide stat bonuses when equipped using the `Weapon.StatModifiers` property. This works for shields, torches, lanterns, and any item that can be held in the off-hand slot. Stats are applied when the item is equipped and removed when unequipped.

## Example from Game Files

Off-hand items can provide stat bonuses when equipped using the `Weapon.StatModifiers` property. This works for shields, torches, lanterns, and any item that can be held in the off-hand slot. Stats are applied when the item is equipped and removed when unequipped.

## Location

- Shields: `Server/Item/Items/Weapon/Shield/`
- Torches: `Server/Item/Items/Furniture/{Set}/Unique/`
- Custom off-hand items: `Server/Item/Items/Weapon/` or `Server/Item/Items/Tool/`

## Basic Off-Hand Stats Structure

Add stats to any weapon/off-hand item using the `Weapon` property:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Custom_Shield.name"
  },
  "Parent": "Template_Weapon_Shield",
  "Weapon": {
    "StatModifiers": {
      "Health": [
        {
          "Amount": 25,
          "CalculationType": "Additive"
        }
      ],
      "Stamina": [
        {
          "Amount": 10,
          "CalculationType": "Additive"
        }
      ]
    }
  }
}
```

## Weapon.StatModifiers Properties

### Amount

The value to modify the stat by:

```json
{
  "Amount": 25
}
```

### CalculationType

How the stat is modified:

```json
{
  "CalculationType": "Additive"
}
```

**Calculation Types:**
- **`Additive`** - Adds flat value to stat (e.g., +25 Health)
- **`Multiplicative`** - Multiplies stat by percentage (e.g., 0.1 = +10%)

## Common Stats for Off-Hand Items

| Stat | Description | Example Use |
|------|-------------|-------------|
| `Health` | Max health bonus | Defensive shields |
| `Stamina` | Max stamina bonus | Combat shields |
| `DamageResistance` | Damage reduction | Heavy shields |
| `MagicCharges` | Magic resource | Magic off-hands |
| `SignatureEnergy` | Signature ability resource | Special weapons |
| `ManaRegen` | Mana regeneration | Magic items |
| `StaminaRegen` | Stamina regeneration | Utility items |

## Example from Game Files

### Staff with Magic Stats

From `Server/Item/Items/Weapon/Staff/Weapon_Staff_Crystal_Flame.json`:

```216:234:Server/Item/Items/Weapon/Staff/Weapon_Staff_Crystal_Flame.json
  "Weapon": {
    "EntityStatsToClear": [
      "MagicCharges",
      "SignatureEnergy"
    ],
    "StatModifiers": {
      "MagicCharges": [
        {
          "Amount": 4,
          "CalculationType": "Additive"
        }
      ],
      "SignatureEnergy": [
        {
          "Amount": 10,
          "CalculationType": "Additive"
        }
      ]
    }
  },
```

This staff provides 4 magic charges and 10 signature energy when equipped. The stats are cleared when switching weapons.

### Torch with Off-Hand Priority

From `Server/Item/Items/Furniture/Crude/Unique/Furniture_Crude_Torch.json`:

```6:17:Server/Item/Items/Furniture/Crude/Unique/Furniture_Crude_Torch.json
  "InteractionConfig": {
    "Priorities": {
      "Secondary": {
        "MainHand": 1,
        "OffHand": -1
      }
    }
  },
  "Interactions": {
    "Primary": "Root_Unarmed_Attack_Swing_Left",
    "Secondary": "Block_Secondary"
  },
```

This torch sets interaction priorities so the main-hand item's secondary action takes precedence over the torch's secondary action.

## Complete Examples

### Defensive Shield with Health Bonus

```json
{
  "TranslationProperties": {
    "Name": "server.items.Shield_Guardian.name"
  },
  "Parent": "Template_Weapon_Shield",
  "Model": "Items/Weapons/Shield/Guardian.blockymodel",
  "Texture": "Items/Weapons/Shield/Guardian_Texture.png",
  "Icon": "Icons/ItemsGenerated/Shield_Guardian.png",
  "Quality": "Rare",
  "ItemLevel": 35,
  "Weapon": {
    "StatModifiers": {
      "Health": [
        {
          "Amount": 50,
          "CalculationType": "Additive"
        }
      ]
    }
  }
}
```

### Combat Shield with Stamina

```json
{
  "TranslationProperties": {
    "Name": "server.items.Shield_Warrior.name"
  },
  "Parent": "Template_Weapon_Shield",
  "Quality": "Uncommon",
  "ItemLevel": 25,
  "Weapon": {
    "StatModifiers": {
      "Stamina": [
        {
          "Amount": 15,
          "CalculationType": "Additive"
        }
      ],
      "Health": [
        {
          "Amount": 20,
          "CalculationType": "Additive"
        }
      ]
    }
  }
}
```

### Magic Off-Hand with Mana

```json
{
  "TranslationProperties": {
    "Name": "server.items.Orb_Arcane.name"
  },
  "Model": "Items/Weapons/Orb/Arcane.blockymodel",
  "Texture": "Items/Weapons/Orb/Arcane_Texture.png",
  "Icon": "Icons/ItemsGenerated/Orb_Arcane.png",
  "PlayerAnimationsId": "Shield",
  "Quality": "Rare",
  "ItemLevel": 40,
  "Categories": ["Items.Weapons"],
  "Utility": {
    "Usable": true
  },
  "Interactions": {
    "Primary": "Root_Unarmed_Attack_Swing_Left",
    "Secondary": "Block_Secondary"
  },
  "Weapon": {
    "StatModifiers": {
      "MagicCharges": [
        {
          "Amount": 3,
          "CalculationType": "Additive"
        }
      ],
      "SignatureEnergy": [
        {
          "Amount": 15,
          "CalculationType": "Additive"
        }
      ]
    }
  },
  "Tags": {
    "Type": ["Weapon"],
    "Family": ["Magic"]
  }
}
```

### Torch with Light Bonus

```json
{
  "TranslationProperties": {
    "Name": "server.items.Torch_Enchanted.name"
  },
  "Parent": "Furniture_Crude_Torch",
  "Quality": "Uncommon",
  "Weapon": {
    "StatModifiers": {
      "Health": [
        {
          "Amount": 5,
          "CalculationType": "Additive"
        }
      ]
    }
  },
  "Light": {
    "Radius": 12,
    "Color": "#ffa"
  }
}
```

## EntityStatsToClear

Reset stats when switching weapons:

```json
{
  "Weapon": {
    "EntityStatsToClear": [
      "SignatureEnergy",
      "MagicCharges"
    ],
    "StatModifiers": {
      "SignatureEnergy": [
        {
          "Amount": 20,
          "CalculationType": "Additive"
        }
      ]
    }
  }
}
```

**Properties:**
- **`EntityStatsToClear`** - Stats to reset to zero when weapon is equipped
- Useful for signature abilities that should reset when switching weapons

## Interaction Priorities (MainHand vs OffHand)

Control which hand's interactions take priority:

```json
{
  "InteractionConfig": {
    "Priorities": {
      "Secondary": {
        "MainHand": 1,
        "OffHand": -1
      }
    }
  }
}
```

**Properties:**
- **`MainHand`** - Priority when in main hand (higher = takes precedence)
- **`OffHand`** - Priority when in off-hand (negative = lower priority)

This ensures main-hand weapon's secondary action (like blocking) takes precedence over off-hand item's secondary action.

## Comparison: Armor vs Weapon Stats

### Armor Stats (Armor.StatModifiers)

Used for armor pieces:

```json
{
  "Armor": {
    "ArmorSlot": "Chest",
    "StatModifiers": {
      "Health": [
        {
          "Amount": 25,
          "CalculationType": "Additive"
        }
      ]
    },
    "DamageResistance": {
      "Physical": [
        {
          "Amount": 0.1,
          "CalculationType": "Multiplicative"
        }
      ]
    }
  }
}
```

### Weapon Stats (Weapon.StatModifiers)

Used for weapons and off-hand items:

```json
{
  "Weapon": {
    "StatModifiers": {
      "Health": [
        {
          "Amount": 25,
          "CalculationType": "Additive"
        }
      ]
    }
  }
}
```

Both systems work the same way - stats are applied when equipped and removed when unequipped.

## Tips

1. **Stack Stats**: Players can have stats from main-hand weapon, off-hand item, AND all armor pieces
2. **Balance Carefully**: Off-hand stats stack with main-hand, so consider total stat budget
3. **Use Multiplicative for %**: Use `Multiplicative` for percentage-based bonuses
4. **Clear on Switch**: Use `EntityStatsToClear` for resource stats that should reset

---

**Previous:** [NPC Models](169_NPC_Models.md) | **Next:** Back to [README](../README.md)
