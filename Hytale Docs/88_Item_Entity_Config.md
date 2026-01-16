# Item Entity Config

Learn how to configure item entities (dropped items) and their properties.

## Overview

Item entities are items that have been dropped in the world. They can have lifetimes, physics, particle effects, and pickup behaviors.

## Location
Item entity config is in gameplay configs under `ItemEntity`.

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
