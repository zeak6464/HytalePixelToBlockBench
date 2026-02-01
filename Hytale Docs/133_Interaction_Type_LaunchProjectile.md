# Interaction Type: LaunchProjectile

Fire projectiles from items or entities.

## Overview

`LaunchProjectile` launches projectiles (arrows, spells, thrown items) with configurable behavior. Supports animations, delays, and projectile configurations.

## Example from Game Files

### Launch Projectile Interaction

From `Server/Item/Interactions/Weapons/Wand/Wand_Cast_Launch.json`:

```1:8:Server/Item/Interactions/Weapons/Wand/Wand_Cast_Launch.json
{
  "Type": "LaunchProjectile",
  "RunTime": 0.25,
  "Effects": {
    "ItemAnimationId": "CastLeftCharged"
  },
  "ProjectileId": "Skeleton_Mage_Corruption_Orb"
}
```

This shows a launch projectile interaction that fires a projectile with animation effects.

## Basic Structure

```json
{
  "Type": "LaunchProjectile",
  "RunTime": 0.25,
  "Effects": {
    "ItemAnimationId": "CastLeftCharged"
  },
  "ProjectileId": "Skeleton_Mage_Corruption_Orb"
}
```

## Properties

### ProjectileId

```json
{
  "ProjectileId": "Skeleton_Mage_Corruption_Orb"
}
```

ID of projectile to launch. Must match projectile definition ID.

### RunTime

```json
{
  "RunTime": 0.25
}
```

Delay before projectile launches (wind-up time).

### Effects

```json
{
  "Effects": {
    "ItemAnimationId": "CastLeftCharged",
    "WorldSoundEventId": "SFX_Cast",
    "WorldParticles": [
      {
        "SystemId": "Cast_Effect"
      }
    ]
  }
}
```

Effects to play when launching projectile.

## Complete Examples

### Example 1: Spell Cast

```json
{
  "Type": "LaunchProjectile",
  "RunTime": 0.25,
  "Effects": {
    "ItemAnimationId": "CastLeftCharged",
    "WorldSoundEventId": "SFX_Spell_Cast",
    "WorldParticles": [
      {
        "SystemId": "Magic_Cast"
      }
    ]
  },
  "ProjectileId": "Fireball"
}
```

Casts fireball spell with cast effects.

### Example 2: Gun Shot with Camera Effect

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
        "PositionOffset": { "Z": 0.85, "Y": 0.15 },
        "RotationOffset": { "Pitch": 90 }
      }
    ]
  },
  "ProjectileId": "GunPvP_Assault_Rifle_Bullet"
}
```

Gun shot with muzzle flash particles and camera shake.

### Example 3: NPC Projectile with Tags

```json
{
  "Type": "LaunchProjectile",
  "RunTime": 0.2,
  "Effects": {
    "WorldSoundEventId": "SFX_Eye_Void_Attack_Blast"
  },
  "ProjectileId": "Eye_Void_Blast",
  "Tags": {
    "AimingReference": []
  }
}
```

NPC attack projectile with aiming tags.

### Example 4: With Stats Check

```json
{
  "Type": "StatsCondition",
  "Costs": {
    "Mana": 50
  },
  "Next": {
    "Type": "LaunchProjectile",
    "RunTime": 0.25,
    "Effects": {
      "ItemAnimationId": "Cast",
      "WorldSoundEventId": "SFX_Cast"
    },
    "ProjectileId": "Ice_Bolt"
  },
  "Failed": {
    "Type": "SendMessage",
    "Key": "server.spells.insufficient.mana"
  }
}
```

Checks mana cost before casting.

## Properties Reference

| Property | Type | Description |
|----------|------|-------------|
| `ProjectileId` | String | ID of projectile to launch |
| `RunTime` | Float | Wind-up delay before launch |
| `Effects` | Object | Visual/audio effects |
| `Tags` | Object | Behavior tags |

### Effects Properties

| Property | Description |
|----------|-------------|
| `ItemAnimationId` | Animation to play |
| `WorldSoundEventId` | Sound heard by everyone |
| `LocalSoundEventId` | Sound heard by user only |
| `CameraEffect` | Camera shake/effect |
| `Particles` | Particle effects with position/rotation |

## Tips

1. **RunTime** - Use for cast delays (0.1-0.25s typical)
2. **Effects** - Always include cast animations/sounds
3. **Projectile IDs** - Must match projectile definition IDs exactly
4. **Combine with conditions** - Check mana/stamina before launching
5. **CameraEffect** - Add for impactful feel
6. **Tags** - Use `AimingReference` for aimed projectiles

---

**Previous:** [BlockCondition](132_Interaction_Type_BlockCondition.md) | **Next:** [Projectile](134_Interaction_Type_Projectile.md)
