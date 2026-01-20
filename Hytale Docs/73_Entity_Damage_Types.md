# Entity Damage Types

Learn how entity damage type configurations work and how they define damage properties.

## Overview

Entity damage types define properties of different damage types like Physical, Fire, Poison, etc. These are referenced by items, effects, and combat systems to determine damage behavior.

## Location
`Server/Entity/Damage/`

## Example from Game Files

### Fire Damage Type

From `Server/Entity/Damage/Fire.json`:

```1:4:Server/Entity/Damage/Fire.json
{
  "Parent": "Elemental",
  "Inherits": "Elemental"
}
```

This shows a damage type configuration for Fire damage that inherits from Elemental.

## Basic Damage Type Structure

Create `Server/Entity/Damage/MyCustom.json`:

```json
{
  "Parent": "Physical",
  "DurabilityLoss": true,
  "StaminaLoss": true
}
```

## Damage Type Properties

### Parent Inheritance

```json
{
  "Parent": "Physical"
}
```

Inherits from parent damage type.

### DurabilityLoss

```json
{
  "DurabilityLoss": true
}
```

Whether this damage type causes durability loss on items.

### StaminaLoss

```json
{
  "StaminaLoss": true
}
```

Whether this damage type causes stamina loss.

## Common Damage Types

### Physical

`Server/Entity/Damage/Physical.json`:

Base physical damage type with durability and stamina loss.

### Fire

`Server/Entity/Damage/Fire.json`:

```json
{
  "Parent": "Elemental",
  "Inherits": "Elemental"
}
```

Fire damage inheriting from Elemental.

### Elemental

Base damage type for elemental damage types.

### Projectile

Damage from projectiles (arrows, bolts, etc.).

### Slashing / Bludgeoning

Subtypes of physical damage.

### Environment

Environmental damage (fall, drowning, suffocation).

**Update 1 Note:** Cactus and brambles now deal **Environmental** damage type instead of other damage types. This means NPCs that are immune to environmental damage (like Kweebecs) will not take damage from these sources.

## Tips for Damage Types

1. **Inheritance** - Use parent types to avoid duplication
2. **Durability** - Most combat damage types cause durability loss
3. **Stamina** - Physical damage typically causes stamina loss

---

**Previous:** [Block Support](72_Block_Support.md) | **Next:** [NPC Pathfinding](74_NPC_Pathfinding.md)
