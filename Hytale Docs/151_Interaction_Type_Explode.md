# Interaction Type: Explode

Create explosions with damage, block destruction, and effects.

## Overview

`Explode` creates explosions at the target location. Supports damage, block destruction, visual effects, knockback, and configurable block gathering.

## Example from Game Files

### Full Explosion Config

From `Server/Item/Interactions/Explosions/Explode_Generic.json`:

```json
{
  "Type": "Explode",
  "Config": {
    "DamageEntities": true,
    "DamageBlocks": true,
    "BlockDamageRadius": 3,
    "BlockDropChance": 0.4,
    "EntityDamage": 20.0,
    "EntityDamageRadius": 4,
    "Knockback": {
      "Type": "Point",
      "VelocityConfig": {
        "AirResistance": 0.97,
        "AirResistanceMax": 0.96,
        "Direction": { "X": 0.0, "Y": 5.0, "Z": 0.0 },
        "GroundResistance": 0.94,
        "GroundResistanceMax": 0.3,
        "Threshold": 3.0
      },
      "Force": 5,
      "VelocityType": "Set"
    },
    "ItemTool": {
      "Specs": [
        { "Power": 2, "GatherType": "SoftBlocks" },
        { "Power": 2, "GatherType": "Soils" },
        { "Power": 2, "GatherType": "Woods" },
        { "Power": 2, "GatherType": "Rocks" },
        { "Power": 2, "GatherType": "Benches" },
        { "Power": 0.001, "GatherType": "VolcanicRocks" },
        { "Power": 1, "GatherType": "OreCopper" },
        { "Power": 1, "GatherType": "OreIron" },
        { "Power": 1, "GatherType": "OreSilver" },
        { "Power": 1, "GatherType": "OreGold" }
      ]
    }
  }
}
```

### Explosion with Effects

From `Server/Item/Interactions/Weapons/Bomb/Bomb_Explode.json`:

```json
{
  "Type": "Explode",
  "Parent": "Explode_Generic_Entities",
  "RunTime": 0.1,
  "Effects": {
    "WorldSoundEventId": "SFX_Goblin_Lobber_Bomb_Death",
    "LocalSoundEventId": "SFX_Goblin_Lobber_Bomb_Death",
    "Particles": [
      {
        "SystemId": "Explosion_Medium",
        "TargetEntityPart": "Entity"
      }
    ]
  }
}
```

### Block Explosion Override

From `Server/Item/Interactions/Block/Block_Explosion.json`:

```json
{
  "Type": "Explode",
  "Parent": "Explode_Generic",
  "Effects": {
    "WorldSoundEventId": "SFX_Explosion",
    "Particles": [
      { "SystemId": "Explosion_Medium", "TargetEntityPart": "Entity" }
    ]
  },
  "Config": {
    "BlockDamageRadius": 5,
    "EntityDamageRadius": 5
  }
}
```

## Properties

### Config Properties

| Property | Type | Description |
|----------|------|-------------|
| `DamageEntities` | Bool | Whether explosion damages entities |
| `DamageBlocks` | Bool | Whether explosion destroys blocks |
| `BlockDamageRadius` | Float | Radius for block destruction |
| `EntityDamageRadius` | Float | Radius for entity damage |
| `EntityDamage` | Float | Base damage to entities |
| `BlockDropChance` | Float | Chance blocks drop items (0-1) |
| `Knockback` | Object | Knockback configuration |
| `ItemTool` | Object | Block gathering specifications |

### Knockback Configuration

```json
{
  "Knockback": {
    "Type": "Point",
    "Force": 5,
    "VelocityType": "Set",
    "VelocityConfig": {
      "AirResistance": 0.97,
      "GroundResistance": 0.94,
      "Direction": { "X": 0, "Y": 5, "Z": 0 },
      "Threshold": 3.0
    }
  }
}
```

| Property | Description |
|----------|-------------|
| `Type` | `"Point"` (radial from center) |
| `Force` | Knockback strength |
| `VelocityType` | `"Set"` or `"Add"` |
| `VelocityConfig` | Air/ground resistance settings |

### ItemTool Specs

Controls which blocks can be destroyed:

```json
{
  "ItemTool": {
    "Specs": [
      { "Power": 2, "GatherType": "SoftBlocks" },
      { "Power": 2, "GatherType": "Rocks" },
      { "Power": 0.001, "GatherType": "VolcanicRocks" }
    ]
  }
}
```

- **`Power`** - Destruction strength (higher = more damage)
- **`GatherType`** - Block category to affect

## Use Cases

- **Bombs** - Throwable explosives
- **TNT Blocks** - Block-triggered explosions
- **NPC Abilities** - Enemy explosion attacks
- **Environmental** - Exploding objects

## Tips

1. **Use Parent** - Inherit from `Explode_Generic` for consistent behavior
2. **BlockDropChance** - Set to 0.4 for balanced loot
3. **ItemTool Specs** - Control which block types are destroyed
4. **Knockback** - Add vertical Y direction for realistic feel
5. **Effects** - Always add particles and sounds

---

**Previous:** [OpenProcessingBench](150_Interaction_Type_OpenProcessingBench.md) | **Next:** [SpawnPrefab](152_Interaction_Type_SpawnPrefab.md)
