# Creating New Spells

Learn how to create spellbooks, staffs, wands, and their magical effects.

## Overview

Spells in Hytale use magic weapons (Spellbooks, Staffs, or Wands) that cast magical effects. Creating a spell involves:
1. **Creating a Projectile** (for projectile-based spells)
2. **Creating Interaction Files** (casting mechanics)
3. **Creating the Spell Weapon Item** (the spellbook/staff/wand)

## Spell Weapon Types

| Type | Use Case | Examples |
|------|----------|----------|
| **Spellbook** | Projectile spells (fireballs, orbs) | Fire spellbook, Frost spellbook |
| **Staff** | Projectile spells with visual effects | Frost staff, Fire staff |
| **Wand** | Instant cast spells (targeting/effects) | Root wand, Stoneskin wand |

---

## Step 1: Create a Projectile (For Projectile Spells)

### Location
`Server/Projectiles/Spells/`

### Basic Projectile Structure

Create `Server/Projectiles/Spells/MyCustom_Fireball.json`:

```json
{
  "Appearance": "Fireball",
  "Radius": 0.1,
  "Height": 0.2,
  "VerticalCenterShot": 0.1,
  "HorizontalCenterShot": 0.1,
  "DepthShot": 1,
  "PitchAdjustShot": false,
  "SticksVertically": true,
  "MuzzleVelocity": 40,
  "TerminalVelocity": 100,
  "Gravity": 4,
  "Bounciness": 0,
  "ImpactSlowdown": 0,
  "TimeToLive": 0,
  "Damage": 60,
  "DeadTime": 0,
  "DeadTimeMiss": 0,
  "MissParticles": {
    "SystemId": "Explosion_Medium"
  },
  "DeathParticles": {
    "SystemId": "Explosion_Medium"
  },
  "DeathEffectsOnHit": true,
  "DeathSoundEventId": "SFX_Fireball_Death"
}
```

### Projectile Field Reference

| Field | Description | Example Values |
|-------|-------------|----------------|
| `Appearance` | Visual model name | `"Fireball"`, `"Ice_Ball"` |
| `Radius` | Hitbox radius | `0.1` (small), `2.2` (large) |
| `Height` | Projectile height | `0.2` |
| `MuzzleVelocity` | Initial speed | `30-40` (slow), `500` (instant) |
| `Gravity` | Fall speed | `0` (no fall), `4` (arcs downward) |
| `Damage` | Base damage on hit | `20`, `60` |
| `TimeToLive` | Lifetime in seconds | `0` (until hit), `3`, `25` |
| `MissParticles` | Particle effect on miss | `{"SystemId": "Explosion_Medium"}` |
| `DeathParticles` | Particle effect on hit | `{"SystemId": "Explosion_Medium"}` |
| `DeathSoundEventId` | Sound on impact | `"SFX_Fireball_Death"` |

### Explosion Configuration (Optional)

```json
{
  "ExplosionConfig": {
    "DamageEntities": true,
    "DamageBlocks": false,
    "BlockDamageRadius": 20,
    "EntityDamageRadius": 5,
    "EntityDamageFalloff": 1.0,
    "Knockback": {
      "Type": "Point",
      "Force": 5,
      "VelocityType": "Set"
    }
  }
}
```

---

## Step 2: Create Interaction Files

### Location
`Server/Item/Interactions/Weapons/{WeaponType}/`

### For Spellbook/Staff: Charged Cast Pattern

#### Primary Interaction

Create `Server/Item/Interactions/Weapons/Spellbook/Attacks/MyCustom_Cast_Charged.json`:

```json
{
  "Type": "StatsCondition",
  "Costs": {
    "Mana": 100
  },
  "RunTime": 0.167,
  "Next": {
    "Type": "Parallel",
    "Interactions": [
      {
        "Interactions": [
          {
            "Type": "Replace",
            "DefaultValue": {
              "Interactions": ["Spellbook_Cast_Cost"]
            },
            "Var": "MyCustom_Cast_Cost"
          }
        ]
      },
      {
        "Interactions": [
          {
            "Type": "Replace",
            "DefaultValue": {
              "Interactions": ["Spellbook_Cast_Launch"]
            },
            "Var": "MyCustom_Cast_Launch"
          }
        ]
      },
      {
        "Interactions": [
          {
            "Type": "Replace",
            "DefaultValue": {
              "Interactions": ["Spellbook_Cast_Effect"]
            },
            "Var": "MyCustom_Cast_Effect"
          }
        ]
      }
    ]
  },
  "Failed": {
    "Type": "Replace",
    "DefaultValue": {
      "Interactions": ["Spellbook_Cast_Fail"]
    },
    "Var": "MyCustom_Cast_Fail"
  }
}
```

#### Launch Interaction

Create `Server/Item/Interactions/Weapons/Spellbook/MyCustom_Cast_Launch.json`:

```json
{
  "Type": "LaunchProjectile",
  "RunTime": 0.25,
  "Effects": {
    "ItemAnimationId": "CastHurlCharged"
  },
  "ProjectileId": "MyCustom_Fireball"
}
```

**Important:** Set `ProjectileId` to match your projectile file name (without `.json`).

---

## Step 3: Create the Spell Weapon Item

### For Spellbook

Create `Server/Item/Items/Weapon/Spellbook/Weapon_Spellbook_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Spellbook_MyCustom.name"
  },
  "Categories": ["Items.Weapons"],
  "Quality": "Common",
  "ItemLevel": 40,
  "Model": "Items/Weapons/Spellbook/Book.blockymodel",
  "Texture": "Items/Weapons/Spellbook/Book_Fire_Texture.png",
  "PlayerAnimationsId": "Spellbook",
  "Interactions": {
    "Primary": "Spellbook_Primary",
    "Secondary": "Spellbook_Primary"
  },
  "InteractionVars": {
    "Spellbook_Cast_Hurl_Charged": {
      "Interactions": [
        {
          "Parent": "MyCustom_Cast_Charged",
          "Costs": {
            "Mana": 100
          }
        }
      ]
    },
    "Spellbook_Cast_Hurl_Cost": {
      "Interactions": [
        {
          "Parent": "Spellbook_Cast_Cost",
          "StatModifiers": {
            "Mana": -100
          }
        }
      ]
    },
    "Spellbook_Cast_Hurl_Launch": "MyCustom_Cast_Launch",
    "Spellbook_Cast_Hurl_Effect": "Spellbook_Cast_Effect",
    "Spellbook_Cast_Hurl_Fail": "Spellbook_Cast_Fail"
  },
  "Icon": "Icons/ItemsGenerated/Weapon_Spellbook_Fire.png",
  "Tags": {
    "Type": ["Weapon"],
    "Family": ["Spellbook"]
  },
  "Weapon": {},
  "ItemSoundSetId": "ISS_Weapons_Books"
}
```

### For Staff

Create `Server/Item/Items/Weapon/Staff/Weapon_Staff_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Staff_MyCustom.name"
  },
  "Categories": ["Items.Weapons"],
  "Quality": "Common",
  "ItemLevel": 40,
  "Model": "Items/Weapons/Staff/Frost.blockymodel",
  "Texture": "Items/Weapons/Staff/Frost_Texture.png",
  "PlayerAnimationsId": "Staff",
  "Interactions": {
    "Primary": "Staff_Primary",
    "Secondary": "Staff_Primary"
  },
  "InteractionVars": {
    "Staff_Cast_Summon_Charged": {
      "Interactions": [
        {
          "Parent": "Staff_Cast_Summon_Charged",
          "Costs": {
            "Mana": 50
          }
        }
      ]
    },
    "Staff_Cast_Summon_Cost": {
      "Interactions": [
        {
          "Parent": "Staff_Cast_Cost",
          "StatModifiers": {
            "Mana": -50
          }
        }
      ]
    },
    "Staff_Cast_Summon_Launch": {
      "Interactions": [
        {
          "Parent": "Staff_Cast_Launch",
          "ProjectileId": "Ice_Ball"
        }
      ]
    },
    "Staff_Cast_Summon_Effect": "Staff_Cast_Effect",
    "Staff_Cast_Summon_Fail": "Staff_Cast_Fail"
  },
  "Icon": "Icons/ItemsGenerated/Weapon_Staff_Frost.png",
  "Tags": {
    "Type": ["Weapon"],
    "Family": ["Staff"]
  },
  "Weapon": {},
  "ItemSoundSetId": "ISS_Weapons_Wood"
}
```

### For Wand (Instant Cast)

Create `Server/Item/Items/Weapon/Wand/Weapon_Wand_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Wand_MyCustom.name"
  },
  "Categories": ["Items.Weapons"],
  "Quality": "Common",
  "ItemLevel": 40,
  "Model": "Items/Weapons/Wand/Wood.blockymodel",
  "Texture": "Items/Weapons/Wand/Wood_Texture.png",
  "PlayerAnimationsId": "Wand",
  "Interactions": {
    "Primary": "MyCustom_Cast"
  },
  "Icon": "Icons/ItemsGenerated/Weapon_Wand_Wood.png",
  "Tags": {
    "Type": ["Weapon"],
    "Family": ["Wand"]
  },
  "Weapon": {},
  "ItemSoundSetId": "ISS_Weapons_Wand"
}
```

#### Wand Cast Interaction (Instant Effect)

Create `Server/Item/Interactions/Weapons/Wand/MyCustom_Cast.json`:

```json
{
  "Type": "Simple",
  "RunTime": 0.2,
  "Effects": {
    "ItemAnimationId": "CastLeftCharged",
    "Particles": [
      {
        "SystemId": "NatureBeam",
        "TargetNodeName": "Handle"
      }
    ],
    "WorldSoundEventId": "SFX_Skeleton_Mage_Spellbook_Impact"
  },
  "Next": {
    "Type": "Selector",
    "Selector": {
      "Id": "Raycast",
      "Offset": {
        "Y": 1.6
      }
    },
    "HitEntityRules": [
      {
        "Matchers": [
          {
            "Type": "Vulnerable"
          }
        ],
        "Next": {
          "Interactions": [
            {
              "Type": "ApplyEffect",
              "EffectId": "Root",
              "Entity": "Target"
            },
            {
              "Type": "ChangeStat",
              "Entity": "Target",
              "StatModifiers": {
                "Health": -25
              }
            }
          ]
        }
      }
    ]
  }
}
```

This wand spell:
- Instantly casts on primary click
- Raycasts to target
- Applies Root effect
- Deals damage

---

## Complete Example: Fireball Spellbook

### 1. Projectile (`Server/Projectiles/Spells/Fireball.json`)
```json
{
  "Appearance": "Fireball",
  "Radius": 0.1,
  "MuzzleVelocity": 40,
  "Gravity": 4,
  "Damage": 60,
  "DeathParticles": {
    "SystemId": "Explosion_Medium"
  },
  "ExplosionConfig": {
    "DamageEntities": true,
    "EntityDamageRadius": 5,
    "Knockback": {
      "Type": "Point",
      "Force": 5
    }
  }
}
```

### 2. Launch Interaction (`Server/Item/Interactions/Weapons/Spellbook/Fireball_Launch.json`)
```json
{
  "Type": "LaunchProjectile",
  "RunTime": 0.25,
  "Effects": {
    "ItemAnimationId": "CastHurlCharged"
  },
  "ProjectileId": "Fireball"
}
```

### 3. Spell Weapon (`Server/Item/Items/Weapon/Spellbook/Weapon_Spellbook_Fire.json`)
```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Spellbook_Fire.name"
  },
  "Categories": ["Items.Weapons"],
  "Quality": "Common",
  "ItemLevel": 40,
  "Model": "Items/Weapons/Spellbook/Book.blockymodel",
  "Texture": "Items/Weapons/Spellbook/Book_Fire_Texture.png",
  "PlayerAnimationsId": "Spellbook",
  "Interactions": {
    "Primary": "Spellbook_Primary",
    "Secondary": "Spellbook_Primary"
  },
  "InteractionVars": {
    "Spellbook_Cast_Hurl_Launch": "Fireball_Launch"
  },
  "Icon": "Icons/ItemsGenerated/Weapon_Spellbook_Fire.png",
  "Tags": {
    "Type": ["Weapon"],
    "Family": ["Spellbook"]
  },
  "Weapon": {},
  "ItemSoundSetId": "ISS_Weapons_Books"
}
```

---

## Spell Types Comparison

| Type | Charging | Projectile | Best For |
|------|----------|------------|----------|
| **Spellbook** | Yes | Yes | Fireballs, Orbs, Explosive spells |
| **Staff** | Yes | Yes | Elemental projectiles with visual effects |
| **Wand** | No | Optional | Instant effects, debuffs, targeted abilities |

---

## Tips for Creating Spells

1. **Start with existing spells** - Copy a similar spell and modify it
2. **Match ProjectileId** - The `ProjectileId` in launch interaction must match your projectile filename (without `.json`)
3. **Mana costs** - Balance mana costs in both the InteractionVars and Charged interaction
4. **Animation IDs** - Use appropriate animation IDs:
   - Spellbook: `"CastHurlCharging"`, `"CastHurlCharged"`
   - Staff: `"CastSummonCharging"`, `"CastSummonCharged"`
   - Wand: `"CastLeftCharged"`
5. **Particle effects** - Reference existing particle systems from `Common/Particles/Textures/`
6. **Sound events** - Use existing sound events or create new ones in `Server/Audio/`
7. **Test damage values** - Start conservative and adjust based on gameplay feel

---

**Previous:** [Blocks & Portals](06_Blocks_and_Portals.md) | **Next:** [Special Items](08_Special_Items.md)
