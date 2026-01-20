# Interaction Type: Projectile

Configure projectile behavior and interactions.

## Overview

`Projectile` configures projectile behavior, not a launch interaction. Used to define projectile properties, hit interactions, and effects. This type appears in projectile configurations themselves.

## Example from Game Files

### Projectile Interaction

Projectile interactions create projectile entities. These are used for arrows, spells, thrown items, and ranged attacks.

## Basic Structure

```json
{
  "Type": "Projectile",
  "Config": "Projectile_Config_Debug"
}
```

## Properties

### Config

```json
{
  "Config": "Projectile_Config_Debug"
}
```

Reference to projectile configuration. See [Projectiles](37_Projectiles.md) for details.

## Notes

**Note:** `Projectile` is primarily a configuration type rather than an interaction type. For launching projectiles, use `LaunchProjectile` instead.

See [Projectiles](37_Projectiles.md) and [Guns and Spellbooks](101_Guns_and_Spellbooks.md) for comprehensive projectile documentation.

---

**Previous:** [LaunchProjectile](133_Interaction_Type_LaunchProjectile.md) | **Next:** [ModifyInventory](135_Interaction_Type_ModifyInventory.md)
