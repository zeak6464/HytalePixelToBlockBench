# Interaction Type: Projectile

Configure projectile behavior and interactions.

## Overview

`Projectile` creates a projectile entity at the user's position. Unlike `LaunchProjectile`, this type spawns the projectile directly without launch mechanics.

## Example from Game Files

### Debug Projectile

From `Server/Item/Interactions/_Debug/Debug_Projectile.json`:

```json
{
  "Type": "Projectile",
  "Config": "Projectile_Config_Debug"
}
```

### Projectile with Effects and Continuation

```json
{
  "Type": "Projectile",
  "Effects": {
    "ItemAnimationId": "Throw"
  },
  "Config": "Projectile_Config_Rubble",
  "Next": {
    "Type": "Simple",
    "RunTime": 0.75
  }
}
```

### Projectile with Tags

```json
{
  "Type": "Projectile",
  "Config": "Projectile_Config_Goblin_Duke_Ice",
  "RunTime": 0.2,
  "Effects": {},
  "Tags": {
    "AimingReference": []
  }
}
```

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `Config` | String | Reference to ProjectileConfig file |
| `RunTime` | Float | Duration of interaction |
| `Effects` | Object | Visual/audio effects during spawn |
| `Next` | Interaction | Interaction to run after |
| `Tags` | Object | Tags for projectile behavior |

### Config

```json
{
  "Config": "Projectile_Config_Debug"
}
```

Reference to ProjectileConfig in `Server/ProjectileConfigs/`.

### Effects

```json
{
  "Effects": {
    "ItemAnimationId": "Throw",
    "WorldSoundEventId": "SFX_Throw",
    "Particles": [{ "SystemId": "Throw_Effect" }]
  }
}
```

### Tags

```json
{
  "Tags": {
    "AimingReference": []
  }
}
```

Controls projectile aiming behavior.

## Use Cases

- **Thrown Items** - Items that create projectiles when thrown
- **NPC Attacks** - NPCs spawning projectiles
- **Spawned Effects** - Projectile-based visual effects

## Difference from LaunchProjectile

| Aspect | Projectile | LaunchProjectile |
|--------|------------|------------------|
| Purpose | Spawn projectile | Launch from weapon |
| Reference | `Config` | `ProjectileId` |
| Typical Use | Thrown items, NPC | Bows, crossbows, guns |
| Launch Force | From config | Built-in |

## Tips

1. **Use Config** - Always reference a valid ProjectileConfig
2. **Effects** - Add animations for throwing/spawning visuals
3. **RunTime** - Set appropriate duration for animation sync
4. **Tags** - Use `AimingReference` for aimed projectiles

---

**Previous:** [LaunchProjectile](133_Interaction_Type_LaunchProjectile.md) | **Next:** [ModifyInventory](135_Interaction_Type_ModifyInventory.md)
