# Entity Repulsion

Learn how to configure entity repulsion systems that push entities apart when they get too close.

## Overview

Entity repulsion prevents entities (players, NPCs) from overlapping by pushing them apart when they're within a certain radius. This creates natural spacing and prevents clustering.

## Location
`Server/Entity/Repulsion/`

## Basic Repulsion Structure

Create `Server/Entity/Repulsion/MyCustom_Repulsion.json`:

```json
{
  "Radius": 5,
  "MaxForce": 5
}
```

## Repulsion Properties

### Radius

```json
{
  "Radius": 5
}
```

Distance at which repulsion starts affecting entities. Units are in blocks.

### MaxForce / MinForce

```json
{
  "MaxForce": 5,
  "MinForce": 1.5
}
```

- **`MaxForce`** - Maximum push force applied
- **`MinForce`** - Minimum push force (optional)

## Common Repulsion Presets

### Default

`Server/Entity/Repulsion/DefaultRepulsion.json`:

```json
{
  "Radius": 5,
  "MaxForce": 5
}
```

Standard repulsion for general gameplay.

### Minigames

`Server/Entity/Repulsion/Minigames.json`:

```json
{
  "Radius": 1.1,
  "MinForce": 1.5,
  "MaxForce": 1.5
}
```

Tighter spacing with fixed force for minigames.

## Using Repulsion

Repulsion is typically configured in gameplay configs or entity movement configs. It's automatically applied to prevent entity overlap.

## Tips for Creating Repulsion

1. **Radius balance** - Too large = entities push too far, too small = clustering
2. **Force balance** - Too high = jittery movement, too low = ineffective
3. **Context matters** - Different repulsion for different game modes
4. **Test gameplay** - Repulsion affects player experience significantly

---

**Previous:** [EQ](56_EQ.md) | **Next:** [Movement Configs](58_Movement_Configs.md)
