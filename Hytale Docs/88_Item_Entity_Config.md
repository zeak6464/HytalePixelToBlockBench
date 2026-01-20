# Item Entity Config

Learn how to configure item entities (dropped items) and their properties.

## Overview

Item entities are items that have been dropped in the world. They can have lifetimes, physics, particle effects, and pickup behaviors.

## Location
Item entity config is in gameplay configs under `ItemEntity`.

## Example from Game Files

### Item Entity Lifetime

From `Server/GameplayConfigs/Default.json`:

```17:19:Server/GameplayConfigs/Default.json
  "ItemEntity": {
    "Lifetime": 600.0
  },
```

This shows item entity configuration with a lifetime of 600 seconds (10 minutes) for dropped items.

### Item Entity Particle System

From `Server/Item/Items/Ore/Iron/Ore_Iron.json`:

```46:48:Server/Item/Items/Ore/Iron/Ore_Iron.json
  "ItemEntity": {
    "ParticleSystemId": null
  },
```

This shows item entity configuration at the item level, where individual items can override particle systems for dropped entities.

## Basic Item Entity Configuration

```json
{
  "ItemEntity": {
    "Lifetime": 600.0
  }
}
```

## Item Entity Properties

### Lifetime

```json
{
  "ItemEntity": {
    "Lifetime": 600.0
  }
}
```

Time in seconds before item despawns. `600.0` = 10 minutes.

## Quality-Based Item Entities

Item qualities can define item entity configs:

```json
{
  "Quality": {
    "ItemEntityConfig": {
      "ParticleSystemId": "Drop_Common"
    }
  }
}
```

Particle effects for dropped items based on quality.

## Tips for Item Entity Config

1. **Lifetime balance** - Too short = items disappear quickly, too long = world clutter
2. **Quality particles** - Different qualities can have different drop particles
3. **Physics** - Item entities have physics and can be pushed by players/NPCs

---

**Previous:** [Respawn Controllers](87_Respawn_Controllers.md) | **Next:** [State Evaluators](89_State_Evaluators.md)
