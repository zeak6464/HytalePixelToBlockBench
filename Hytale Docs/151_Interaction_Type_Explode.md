# Interaction Type: Explode

Create explosions with damage, block destruction, and effects.

## Overview

`Explode` creates explosions at the target location. Supports damage, block destruction, visual effects, and configurable behavior.

## Example from Game Files

### Explode Interaction

Explode interactions create explosions. These are used for explosives, blast effects, area damage, and explosion mechanics.

## Basic Structure

```json
{
  "Type": "Explode",
  "Parent": "Explode_Generic",
  "Config": {
    "DamageBlocks": false
  }
}
```

## Properties

### Parent

```json
{
  "Parent": "Explode_Generic"
}
```

Parent explosion configuration to inherit from.

### Config

```json
{
  "Config": {
    "DamageBlocks": false,
    "DamageEntities": true,
    "Radius": 5.0,
    "Damage": 50
  }
}
```

Explosion configuration:
- **`DamageBlocks`** - If `true`, destroys blocks
- **`DamageEntities`** - If `true`, damages entities
- **`Radius`** - Explosion radius (blocks)
- **`Damage`** - Damage amount

## Complete Examples

### Example 1: Generic Explosion

```json
{
  "Type": "Explode",
  "Parent": "Explode_Generic",
  "Config": {
    "DamageBlocks": false
  }
}
```

Creates explosion that damages entities but not blocks.

### Example 2: Full Explosion

```json
{
  "Type": "Explode",
  "Parent": "Explode_Generic",
  "Config": {
    "DamageBlocks": true,
    "DamageEntities": true,
    "Radius": 5.0,
    "Damage": 50
  }
}
```

Creates explosion that damages both blocks and entities.

## Tips

1. **Parent configs** - Use parent configurations for consistent explosion behavior
2. **DamageBlocks** - Set to `false` for non-destructive explosions
3. **Radius** - Adjust for different explosion sizes
4. **Damage** - Set appropriate damage values for different contexts

---

**Previous:** [OpenProcessingBench](150_Interaction_Type_OpenProcessingBench.md) | **Next:** [SpawnPrefab](152_Interaction_Type_SpawnPrefab.md)
