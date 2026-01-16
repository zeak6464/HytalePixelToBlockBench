# Guns & Spellbooks

Learn how to create ranged weapons like guns (rifles, handguns) and spellbooks with projectile mechanics.

## Overview

Guns and spellbooks are ranged weapons that fire projectiles. Both use the projectile system but differ in:
- **Guns**: Consume ammo (arrows/bullets), have cooldowns, physical projectiles
- **Spellbooks**: Consume mana, have charging mechanics, magical projectiles

## Location
- Guns: `Server/Item/Items/Weapon/Gun/`
- Spellbooks: `Server/Item/Items/Weapon/Spellbook/`
- Interactions: `Server/Item/Interactions/Weapons/Gun/` or `Spellbook/`
- Projectiles: `Server/Projectiles/` or `Server/Models/Projectiles/`

## Creating a Gun

### Basic Gun Structure

Create `Server/Item/Items/Weapon/Gun/Weapon_Gun_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Gun_MyCustom.name"
  },
  "Categories": ["Items.Weapons"],
  "Icon": "Icons/ItemsGenerated/Weapon_Gun_MyCustom.png",
  "Model": "Items/Weapons/Gun/MyCustom.blockymodel",
  "Texture": "Items/Weapons/Gun/MyCustom_Texture.png",
  "PlayerAnimationsId": "Rifle",
  "Interactions": {
    "Primary": {
      "Cooldown": {
        "Id": "Shoot",
        "Cooldown": 0.2
      },
      "Interactions": [
        {
          "Type": "ModifyInventory",
          "ItemToRemove": {
            "Id": "Weapon_Arrow_Crude",
            "Quantity": 1
          },
          "Next": {
            "Parent": "Gun_Shoot",
            "RunTime": 0,
            "Effects": {
              "ItemAnimationId": "Shoot",
              "CameraEffect": "Handgun_Shoot",
              "Particles": [
                {
                  "SystemId": "RifleShooting",
                  "PositionOffset": {
                    "Z": 0.35,
                    "Y": 0.15
                  },
                  "RotationOffset": {
                    "Pitch": 90
                  }
                }
              ],
              "WorldSoundEventId": "SFX_Handgun_Fire",
              "LocalSoundEventId": "SFX_Handgun_Fire_Local"
            },
            "ProjectileId": "GunPvP_Handgun_Bullet"
          },
          "Failed": "Gun_Shoot_Fail"
        }
      ]
    },
    "Secondary": "Gun_Attack"
  },
  "Weapon": {},
  "Tags": {
    "Type": ["Weapon"]
  }
}
```

### Gun Properties

#### PlayerAnimationsId

```json
{
  "PlayerAnimationsId": "Rifle"  // or "Handgun"
}
```

Animation set for gun. `"Rifle"` for rifles/assault rifles, `"Handgun"` for pistols.

#### Custom Animation Override

```json
{
  "PlayerAnimationsId": {
    "Parent": "Rifle",
    "Animations": {
      "Shoot": {
        "ThirdPerson": "Characters/Animations/Items/Dual_Handed/Rifle/Attacks/Shoot/Shoot.blockyanim",
        "ThirdPersonMoving": "Characters/Animations/Items/Dual_Handed/Rifle/Attacks/Shoot/Shoot_Moving.blockyanim",
        "FirstPerson": "Characters/Animations/Items/Dual_Handed/Rifle/Attacks/Shoot/Shoot_FPS.blockyanim",
        "Speed": 3,
        "BlendingDuration": 0,
        "ThirdPersonFace": "Characters/Animations/Expressions/Frown.blockyanim"
      }
    }
  }
}
```

Override specific animations for custom gun behavior.

### Gun Shoot Interaction

The `Gun_Shoot` interaction launches the projectile:

```json
{
  "Type": "LaunchProjectile",
  "RunTime": 0.25,
  "Effects": {
    "ItemAnimationId": "Shoot",
    "CameraEffect": "Handgun_Shoot",
    "Particles": [
      {
        "SystemId": "RifleShooting",
        "PositionOffset": {
          "Z": 0.85,
          "Y": 0.15
        },
        "RotationOffset": {
          "Pitch": 90
        }
      }
    ]
  },
  "ProjectileId": "GunPvP_Handgun_Bullet"
}
```

- **`ProjectileId`** - References projectile model/configuration
- **`CameraEffect`** - Camera shake/recoil effect
- **`Particles`** - Muzzle flash particle system
- **`SoundEventId`** - Fire sound effects

### Ammo Consumption

Guns consume ammo from inventory:

```json
{
  "Type": "ModifyInventory",
  "ItemToRemove": {
    "Id": "Weapon_Arrow_Crude",
    "Quantity": 1
  },
  "Next": {
    "Parent": "Gun_Shoot"
  },
  "Failed": "Gun_Shoot_Fail"
}
```

- **`ItemToRemove`** - Ammo item ID (can be arrows, bullets, etc.)
- **`Failed`** - Interaction when out of ammo

### Gun Cooldowns

```json
{
  "Cooldown": {
    "Id": "Shoot",
    "Cooldown": 0.2  // 0.2 seconds between shots (rapid fire)
  }
}
```

Cooldowns control fire rate:
- **`0.05-0.1`** - Very rapid fire (assault rifles)
- **`0.2-0.5`** - Semi-automatic (handguns)
- **`1.0+`** - Slow fire (sniper rifles, blunderbuss)

### Gun Projectiles

Projectiles are defined in `Server/Models/Projectiles/Weapons/Gun/`:

```json
{
  "HitBox": {
    "Min": { "X": -0.05, "Y": -0.05, "Z": -0.05 },
    "Max": { "X": 0.05, "Y": 0.05, "Z": 0.05 }
  },
  "Trails": [
    {
      "TrailId": "Bullet",
      "PositionOffset": { "X": 0, "Y": 0, "Z": 0 }
    }
  ],
  "DefaultAttachments": [
    {
      "Model": "Items/Weapons/Gun/Bullet.blockymodel",
      "Texture": "Items/Weapons/Gun/Bullet_Texture.png"
    }
  ]
}
```

### Gun Types

#### Handgun (Pistol)

```json
{
  "PlayerAnimationsId": "Handgun",
  "Cooldown": {
    "Cooldown": 0.2
  }
}
```

Single-handed, semi-automatic.

#### Rifle

```json
{
  "PlayerAnimationsId": "Rifle",
  "Cooldown": {
    "Cooldown": 0.05
  }
}
```

Two-handed, rapid fire.

#### Blunderbuss

```json
{
  "Interactions": {
    "Primary": "Gun_Shoot_Flintlock_Charging"
  }
}
```

Charging/loading mechanic before firing.

---

## Creating a Spellbook

### Basic Spellbook Structure

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
  "Texture": "Items/Weapons/Spellbook/Book_MyCustom_Texture.png",
  "PlayerAnimationsId": "Spellbook",
  "Interactions": {
    "Primary": {
      "Cooldown": {
        "Id": "Spellbook_Cast",
        "Cooldown": 1.5
      },
      "Interactions": [
        "Spellbook_Primary"
      ]
    },
    "Secondary": "Spellbook_Primary"
  },
  "InteractionVars": {
    "Spellbook_Cast_Hurl_Charged": {
      "Interactions": [
        {
          "Parent": "Spellbook_Cast_Hurl_Charged",
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
    "Spellbook_Cast_Hurl_Launch": "Spellbook_Cast_Launch",
    "Spellbook_Cast_Hurl_Effect": "Spellbook_Cast_Effect",
    "Spellbook_Cast_Hurl_Fail": "Spellbook_Cast_Fail"
  },
  "Icon": "Icons/ItemsGenerated/Weapon_Spellbook_MyCustom.png",
  "Tags": {
    "Type": ["Weapon"],
    "Family": ["Spellbook"]
  },
  "Weapon": {},
  "ItemSoundSetId": "ISS_Weapons_Books"
}
```

### Spellbook Properties

#### PlayerAnimationsId

```json
{
  "PlayerAnimationsId": "Spellbook"
}
```

Uses spellbook animation set for casting animations.

#### Mana Cost

```json
{
  "Spellbook_Cast_Hurl_Charged": {
    "Interactions": [
      {
        "Parent": "Spellbook_Cast_Hurl_Charged",
        "Costs": {
          "Mana": 100
        }
      }
    ]
  }
}
```

Mana consumed when casting the spell.

### Spellbook Casting Interaction

Spellbooks use charging mechanics:

```json
{
  "Type": "StatsCondition",
  "Costs": {
    "Mana": 100
  },
  "Next": {
    "Type": "Simple",
    "RunTime": 1.5,
    "Effects": {
      "ItemAnimationId": "CastHurlCharged",
      "Particles": [
        {
          "SystemId": "Spellbook_Charge"
        }
      ]
    },
    "Next": {
      "Type": "LaunchProjectile",
      "ProjectileId": "Fireball_MyCustom",
      "Effects": {
        "WorldSoundEventId": "SFX_Spellbook_Cast",
        "LocalSoundEventId": "SFX_Spellbook_Cast_Local"
      }
    }
  },
  "Failed": {
    "Interactions": [
      {
        "Type": "SendMessage",
        "Message": "Not enough mana!"
      }
    ]
  }
}
```

### Spellbook Charging

Spellbooks charge before casting:

1. **Hold** - Player holds primary button
2. **Charge** - Mana consumed, particles/effects play
3. **Release** - Projectile launched

The charging duration is controlled by `RunTime` in the interaction.

### Spellbook Cooldowns

Add a cooldown to prevent rapid casting:

```json
{
  "Interactions": {
    "Primary": {
      "Cooldown": {
        "Id": "Spellbook_Cast",
        "Cooldown": 1.5
      },
      "Interactions": [
        "Spellbook_Primary"
      ]
    }
  }
}
```

Cooldown properties:
- **`Id`** - Unique cooldown identifier (shared across interactions with same ID)
- **`Cooldown`** - Time in seconds before spellbook can cast again

**Cooldown duration recommendations:**
- **`0.5-1.0`** - Fast casting (rapid fire spells)
- **`1.5-2.5`** - Normal casting (standard spellbooks)
- **`3.0+`** - Slow casting (powerful spells)

The cooldown starts after the spell is cast (when projectile is launched), not when charging begins.

### Spellbook Projectiles

Spell projectiles are defined in `Server/Projectiles/Spells/` or `Server/ProjectileConfigs/`:

**Simple projectile with direct damage:**
```json
{
  "Appearance": "Fireball",
  "Radius": 0.5,
  "Height": 0.8,
  "MuzzleVelocity": 40,
  "TerminalVelocity": 100,
  "Gravity": 4,
  "Damage": 60,
  "DeathParticles": {
    "SystemId": "Explosion_Medium"
  },
  "DeathSoundEventId": "SFX_Fireball_Death"
}
```

**Projectile config with damage inheritance:**
```json
{
  "Interactions": {
    "ProjectileHit": {
      "Interactions": [
        {
          "Parent": "Spellbook_Fire_Projectile_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Fire": 60
            }
          }
        }
      ]
    }
  }
}
```

This uses `Parent` to inherit damage from the `Spellbook_Fire_Projectile_Damage` interaction while allowing override of the damage calculator.

### Spellbook Types

#### Fire Spellbook

Launches fireball projectiles that explode on impact.

#### Frost Spellbook

Launches ice projectiles that slow/freeze targets.

#### Lightning Spellbook

Launches lightning projectiles with chain effects.

---

## Common Patterns

### Ammo vs Mana

- **Guns** - Use `ModifyInventory` to remove ammo items
- **Spellbooks** - Use `StatsCondition` to consume mana

### Cooldowns

- **Guns** - Cooldown on `Primary` interaction for fire rate
- **Spellbooks** - Optional cooldown on `Primary` interaction, or rely on mana/mana regen for balance

### Projectile Effects

Both guns and spellbooks can use:
- **Muzzle flash** - Particle systems at firing point
- **Camera effects** - Recoil/shake on firing
- **Sound effects** - Fire/cast sounds
- **Trails** - Visual trails following projectiles

### Failure Handling

```json
{
  "Failed": "Gun_Shoot_Fail"  // or "Spellbook_Cast_Fail"
}
```

Failure interaction when:
- Out of ammo (guns)
- Not enough mana (spellbooks)
- Weapon on cooldown

---

## Projectile Damage Inheritance

### Inheriting Damage from Weapon

Projectiles can inherit damage from weapon interactions using `Parent` references:

**1. Create Damage Interaction:**

`Server/Item/Interactions/Weapons/Bow/Bow_Combat_Projectile_Damage.json`:
```json
{
  "Parent": "DamageEntityParent",
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 6
    }
  },
  "DamageEffects": {
    "Knockback": {
      "Force": 5.5
    }
  }
}
```

**2. Reference in Projectile Config:**

`Server/ProjectileConfigs/Weapons/Bows/Projectile_Config_Bow_Combat_Charge_01.json`:
```json
{
  "Interactions": {
    "ProjectileHit": {
      "Interactions": [
        {
          "Parent": "Bow_Combat_Projectile_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Projectile": 2
            }
          }
        }
      ]
    }
  }
}
```

**How it works:**
- The projectile config uses `Parent: "Bow_Combat_Projectile_Damage"` to inherit the damage interaction
- You can override `DamageCalculator` in the projectile config to modify base damage
- The projectile inherits all other properties (knockback, particles, etc.) from the parent interaction

### Overriding Damage in Projectile Config

You can override specific damage values:

```json
{
  "Parent": "Bow_Combat_Projectile_Damage",
  "DamageCalculator": {
    "BaseDamage": {
      "Projectile": 10  // Override physical damage to 10
    }
  }
}
```

This inherits everything from `Bow_Combat_Projectile_Damage` but sets damage to 10.

### Multiple Damage Types

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 10,
      "Fire": 5
    }
  }
}
```

Applies both physical and fire damage.

### Direct Damage with Modifiers

When using direct damage in projectiles (`"Damage": 4`), you can add modifiers by using `DamageCalculator` in the projectile config's `ProjectileHit` interaction:

**1. Direct Damage in Projectile:**
```json
{
  "Appearance": "Arrow_MyCustom",
  "Damage": 6,
  "SticksVertically": true
}
```

**2. Add Modifiers in Projectile Config:**
`Server/ProjectileConfigs/Weapons/Bows/Projectile_Config_MyCustom.json`:
```json
{
  "Interactions": {
    "ProjectileHit": {
      "Interactions": [
        {
          "Parent": "DamageEntityParent",
          "DamageCalculator": {
            "Type": "Absolute",
            "BaseDamage": {
              "Projectile": 6
            },
            "RandomPercentageModifier": 0.1
          }
        }
      ]
    }
  }
}
```

**DamageCalculator Modifiers:**

```json
{
  "DamageCalculator": {
    "Type": "Absolute",
    "BaseDamage": {
      "Physical": 10
    },
    "RandomPercentageModifier": 0.1
  }
}
```

- **`Type`**: `"Absolute"` (fixed damage) or `"Dps"` (damage per second)
- **`BaseDamage`**: Base damage values (must match the `Damage` value from projectile definition)
- **`RandomPercentageModifier`**: Random variance (0.1 = ±10% damage variation)

**Example with Random Modifier:**
```json
{
  "DamageCalculator": {
    "Type": "Absolute",
    "BaseDamage": {
      "Projectile": 6
    },
    "RandomPercentageModifier": 0.15
  }
}
```

With `RandomPercentageModifier: 0.15`, base damage of 6 can vary between 5.1 and 6.9 (85% to 115% of base).

**Note:** When using `DamageCalculator` in projectile configs, you override the direct `Damage` property. The `BaseDamage` value should match your desired damage, and `RandomPercentageModifier` adds variance.

## Tips for Guns & Spellbooks

1. **Fire rate balance** - Faster guns = more ammo consumption
2. **Mana balance** - Spellbooks should consume meaningful mana
3. **Projectile speed** - Guns typically faster, spellbooks slower (more visible)
4. **Cooldowns** - Use cooldowns for guns, mana for spellbooks
5. **Particles** - Always add muzzle flash/charge particles for feedback
6. **Sounds** - Distinct fire/cast sounds improve feel
7. **Camera effects** - Recoil makes guns feel impactful
8. **Damage inheritance** - Use `Parent` in projectile configs to inherit damage interactions
9. **Damage override** - Override `DamageCalculator` in projectile configs to modify damage per projectile

---

**Previous:** [Path Definitions](100_Path_Definitions.md) | **Next:** Back to [README](README.md)
