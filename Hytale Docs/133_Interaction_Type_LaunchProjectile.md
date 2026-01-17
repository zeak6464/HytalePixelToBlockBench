# Interaction Type: LaunchProjectile

Fire projectiles from items or entities.

## Overview

`LaunchProjectile` launches projectiles (arrows, spells, thrown items) with configurable behavior. Supports animations, delays, and projectile configurations.

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

### Example 2: Arrow Shot

```json
{
  "Type": "LaunchProjectile",
  "RunTime": 0.1,
  "Effects": {
    "ItemAnimationId": "Shoot",
    "WorldSoundEventId": "SFX_Bow_Shoot"
  },
  "ProjectileId": "Arrow_Iron"
}
```

Shoots arrow with bow animation.

### Example 3: With Stats Check

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

## Tips

1. **RunTime** - Use for cast delays (0.1-0.25s typical)
2. **Effects** - Always include cast animations/sounds
3. **Projectile IDs** - Must match projectile definition IDs exactly
4. **Combine with conditions** - Check mana/stamina before launching

---

**Previous:** [BlockCondition](132_Interaction_Type_BlockCondition.md) | **Next:** [Projectile](134_Interaction_Type_Projectile.md)
