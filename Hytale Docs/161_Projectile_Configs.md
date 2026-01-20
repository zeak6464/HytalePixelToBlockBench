# Projectile Configs

Learn how to configure projectile behavior for weapons using `Server/ProjectileConfigs/`.

## Overview

Projectile configs define how projectiles behave when fired from weapons (bows, crossbows, guns, spellbooks). They're separate from base projectile definitions (`Server/Projectiles/`) and allow weapons to customize projectile behavior, damage, and interactions.

## Example from Game Files

Projectile configs define how projectiles behave when fired from weapons. They're separate from base projectile definitions and allow weapons to customize projectile behavior, damage, and interactions.

## Location

`Server/ProjectileConfigs/Weapons/{WeaponType}/`

Examples:
- `Server/ProjectileConfigs/Weapons/Bows/`
- `Server/ProjectileConfigs/Weapons/Shortbow/`
- `Server/ProjectileConfigs/Weapons/Crossbow/`
- `Server/ProjectileConfigs/Weapons/Arrows/`
- `Server/ProjectileConfigs/Weapons/Staff/`
- `Server/ProjectileConfigs/Weapons/Deployables/`

## Basic Projectile Config Structure

`Server/ProjectileConfigs/Weapons/Arrows/Projectile_Config_Arrow_Base.json`:

```json
{
  "Model": "Arrow_Crude",
  "SpawnRotationOffset": {
    "Pitch": 2,
    "Yaw": 0.25,
    "Roll": 0
  },
  "Physics": {
    "Type": "Standard",
    "Gravity": 15,
    "TerminalVelocityAir": 50,
    "TerminalVelocityWater": 15,
    "RotationMode": "VelocityDamped",
    "Bounciness": 0.0,
    "SticksVertically": true
  },
  "LaunchForce": 30,
  "SpawnOffset": {
    "X": 0.15,
    "Y": -0.25,
    "Z": 0
  }
}
```

## Projectile Config Properties

### Model

Projectile model appearance:

```json
{
  "Model": "Arrow_Crude"
}
```

References a model ID from `Server/Models/Projectiles/`. The base projectile definition (`Server/Projectiles/`) should also define this model.

### Physics

Projectile physics configuration:

```json
{
  "Physics": {
    "Type": "Standard",
    "Gravity": 15,
    "TerminalVelocityAir": 50,
    "TerminalVelocityWater": 15,
    "RotationMode": "VelocityDamped",
    "Bounciness": 0.0,
    "SticksVertically": true
  }
}
```

**Properties:**
- **`Type`** - Physics type (`"Standard"`)
- **`Gravity`** - Gravity strength (15 = falls downward)
- **`TerminalVelocityAir`** - Maximum speed in air (50)
- **`TerminalVelocityWater`** - Maximum speed in water (15)
- **`RotationMode`** - How projectile rotates (`"VelocityDamped"` = aligns with velocity)
- **`Bounciness`** - Bounce on impact (0.0 = no bounce)
- **`SticksVertically`** - If `true`, arrow sticks vertically when it hits ground

### LaunchForce

Initial projectile velocity:

```json
{
  "LaunchForce": 30
}
```

Higher values = faster projectiles. Typically ranges from 15 (weak) to 80 (strong).

### SpawnOffset

Spawn position offset from weapon:

```json
{
  "SpawnOffset": {
    "X": 0.15,
    "Y": -0.25,
    "Z": 0
  }
}
```

Offsets projectile spawn position:
- **`X`** - Left/right offset
- **`Y`** - Up/down offset (negative = lower)
- **`Z`** - Forward/backward offset

### SpawnRotationOffset

Initial rotation offset:

```json
{
  "SpawnRotationOffset": {
    "Pitch": 2,
    "Yaw": 0.25,
    "Roll": 0
  }
}
```

Adjusts initial projectile rotation (degrees).

### LaunchSoundEventId

Sound played when projectile is launched:

```json
{
  "LaunchLocalSoundEventId": "SFX_Bow_T2_Shoot_Local",
  "LaunchWorldSoundEventId": "SFX_Bow_T2_Shoot"
}
```

- **`LaunchLocalSoundEventId`** - Sound heard by shooter
- **`LaunchWorldSoundEventId`** - Sound heard by nearby players

## Projectile Interactions

Projectile configs can define interactions for projectile events:

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
          },
          "DamageEffects": {
            "Knockback": {
              "Force": 3
            }
          },
          "EntityStatsOnHit": [
            {
              "EntityStatId": "SignatureEnergy",
              "Amount": 0.54
            }
          ]
        },
        "Bow_Combat_Projectile_Damage_End"
      ]
    },
    "ProjectileMiss": {
      "Interactions": [
        "Bow_Combat_Projectile_Miss",
        "Bow_Combat_Projectile_Miss_End"
      ]
    },
    "ProjectileSpawn": {
      "Interactions": [
        {
          "Type": "SendMessage",
          "Message": "Arrow fired!"
        }
      ]
    }
  }
}
```

### ProjectileHit

Interactions when projectile hits an entity or block:

```json
{
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
```

Common interactions:
- **`DamageEntity`** - Apply damage to hit entity
- **`ApplyEffect`** - Apply status effects
- **`ChangeBlock`** - Modify hit blocks
- **`SpawnParticles`** - Visual effects

### ProjectileMiss

Interactions when projectile doesn't hit anything:

```json
{
  "ProjectileMiss": {
    "Interactions": [
      "Bow_Combat_Projectile_Miss",
      "Common_Projectile_Despawn"
    ]
  }
}
```

Common interactions:
- **`RemoveEntity`** - Despawn projectile
- **`SpawnParticles`** - Miss effects
- **`SpawnDrops`** - Drop arrow item

### ProjectileSpawn

Interactions when projectile is first created:

```json
{
  "ProjectileSpawn": {
    "Interactions": [
      {
        "Type": "SendMessage",
        "Message": "Arrow fired!"
      }
    ]
  }
}
```

Useful for:
- Spawn effects
- Initial setup
- Debug messages

## Inheritance with Parent

Projectile configs can inherit from parent configs:

```json
{
  "Parent": "Projectile_Config_Bow_Combat",
  "LaunchForce": 8,
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

**Benefits:**
- Inherit base physics and properties
- Override specific values (damage, force, interactions)
- Share common behavior across variants

## Weapon Strength Variants

Projectile configs often use strength levels (0 = weak, 10 = max charge):

`Server/ProjectileConfigs/Weapons/Bows/Two_Handed/Projectile_Config_Bow_Two_Handed_Charge_10.json`:

```json
{
  "Parent": "Projectile_Config_Arrow_Two_Handed_Bow",
  "LaunchForce": 80,
  "Interactions": {
    "ProjectileHit": {
      "Interactions": [
        {
          "Parent": "Bow_Two_Handed_Projectile_Damage",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 20
            }
          },
          "DamageEffects": {
            "Knockback": {
              "Force": 12
            }
          }
        }
      ]
    }
  }
}
```

**Strength Levels:**
- **0** - No charge / quick shot (low damage, low force)
- **5** - Half charge (medium damage, medium force)
- **10** - Full charge (high damage, high force)

## Using Replace Interaction

Projectile configs can use `Replace` interactions to swap variables:

`Server/ProjectileConfigs/Weapons/Shortbow/Projectile_Config_Arrow_Shortbow_Strength_0.json`:

```json
{
  "Parent": "Projectile_Config_Arrow_Shortbow",
  "Interactions": {
    "ProjectileHit": {
      "Interactions": [
        {
          "Type": "Replace",
          "Var": "Primary_Shoot_Damage_Strength_0",
          "DefaultValue": {
            "Interactions": [
              "Weapon_Shortbow_Primary_Shoot_Damage"
            ]
          }
        },
        {
          "Type": "Replace",
          "Var": "Primary_Shoot_Impact_Strength_0",
          "DefaultValue": {
            "Interactions": [
              "Common_Projectile_Impact_Effects"
            ]
          }
        }
      ]
    }
  }
}
```

**How Replace Works:**
- **`Var`** - Variable name to replace
- **`DefaultValue`** - Default interaction if variable not set
- Weapon interactions can set these variables before launching projectile

## Creating a Custom Projectile Config

### Step 1: Create Config File

Create `Server/ProjectileConfigs/Weapons/MyWeapon/Projectile_Config_MyWeapon.json`:

```json
{
  "Parent": "Projectile_Config_Arrow_Base",
  "Model": "Arrow_Crude",
  "LaunchForce": 25,
  "Physics": {
    "Type": "Standard",
    "Gravity": 15,
    "TerminalVelocityAir": 50,
    "Bounciness": 0.0,
    "SticksVertically": true
  },
  "LaunchLocalSoundEventId": "SFX_MyWeapon_Shoot_Local",
  "LaunchWorldSoundEventId": "SFX_MyWeapon_Shoot"
}
```

### Step 2: Add Hit Interactions

```json
{
  "Interactions": {
    "ProjectileHit": {
      "Interactions": [
        {
          "Type": "DamageEntity",
          "DamageCalculator": {
            "BaseDamage": {
              "Projectile": 5
            }
          }
        }
      ]
    }
  }
}
```

### Step 3: Reference in Weapon

In your weapon item, reference the config:

```json
{
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "Projectile",
          "Config": "Projectile_Config_MyWeapon"
        }
      ]
    }
  }
}
```

## Common Projectile Config Types

### Arrow Configs

`Server/ProjectileConfigs/Weapons/Arrows/`

Base arrow projectile configs:
- **`Projectile_Config_Arrow_Base`** - Basic arrow
- **`Projectile_Config_Arrow_Shortbow`** - Shortbow arrows
- **`Projectile_Config_Arrow_Crossbow`** - Crossbow bolts
- **`Projectile_Config_Arrow_Two_Handed_Bow`** - Two-handed bow arrows

### Bow Charge Levels

`Server/ProjectileConfigs/Weapons/Bows/Two_Handed/`

Charge level variants:
- **`Projectile_Config_Bow_Two_Handed_Charge_00`** - No charge
- **`Projectile_Config_Bow_Two_Handed_Charge_05`** - Half charge
- **`Projectile_Config_Bow_Two_Handed_Charge_10`** - Full charge

### Spell Projectiles

`Server/ProjectileConfigs/Weapons/Staff/`

Spell projectile configs:
- **`Projectile_Config_Ice_Bolt`** - Ice bolt spell
- **`Projectile_Config_Ice_Ball`** - Ice ball spell

### Deployable Projectiles

`Server/ProjectileConfigs/Weapons/Deployables/`

Deployable spawn projectiles:
- **`Projectile_Config_Healing_Totem_Deploy`** - Healing totem spawn
- **`Projectile_Config_Slowness_Totem_Deploy`** - Slowness totem spawn
- **`Projectile_Config_Turret_Deploy`** - Turret spawn

## Differences from Base Projectiles

### ProjectileConfigs vs Projectiles

| Feature | `Server/Projectiles/` | `Server/ProjectileConfigs/` |
|---------|----------------------|----------------------------|
| **Purpose** | Base projectile definitions | Weapon-specific projectile behavior |
| **Usage** | Direct projectile spawning | Referenced by weapon interactions |
| **Model** | Always defines model | Can override model |
| **Physics** | Base physics settings | Weapon-specific physics |
| **Damage** | Simple `Damage` property | Uses `DamageCalculator` in interactions |
| **Interactions** | Basic hit/miss | Full interaction chains |

### When to Use Which

**Use `Server/Projectiles/`:**
- Simple projectiles with direct damage
- Reusable projectiles across multiple weapon types
- Basic arrow/spell projectiles

**Use `Server/ProjectileConfigs/`:**
- Weapon-specific projectile behavior
- Charge levels and strength variants
- Complex interactions (damage modifiers, effects)
- Weapon-specific physics customization

## Tips

1. **Inherit from Base** - Start with `Projectile_Config_Arrow_Base` or similar parent
2. **Charge Variants** - Create multiple configs for different charge levels
3. **Parent Interactions** - Use parent interactions and override damage values
4. **Physics Tuning** - Adjust `Gravity` and `LaunchForce` for desired trajectory
5. **Sound Events** - Always include launch sounds for feedback
6. **SpawnOffset** - Fine-tune spawn position for visual alignment
7. **Replace Variables** - Use `Replace` for dynamic interaction swapping
8. **Reference in Weapons** - Reference config ID in weapon's `Projectile` interaction

---

**Previous:** [World Configuration](160_World_Configuration.md) | **Next:** [Prefab Editor Settings](162_Prefab_Editor_Settings.md)
