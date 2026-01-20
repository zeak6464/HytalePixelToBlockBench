# Interaction Type: UseEntity

Interact with entities (NPCs, animals, interactive entities).

## Overview

`UseEntity` triggers entity interactions like talking to NPCs, milking cows, or interacting with interactive entities. Used when right-clicking/interacting with entities.

## Example from Game Files

### Use Entity Interaction

Use entity interactions interact with entities. These are used for NPC interactions, entity activation, and entity-based mechanics.

## Basic Structure

```json
{
  "Type": "UseEntity",
  "Failed": {
    "Type": "Simple",
    "RunTime": 0.5
  }
}
```

## Properties

### Failed

```json
{
  "Failed": {
    "Type": "Simple",
    "RunTime": 0.5
  }
}
```

Interaction to execute if entity interaction fails (entity has no Use interaction).

## Complete Examples

### Example 1: Basic Entity Use

```json
{
  "Type": "UseEntity"
}
```

Interacts with targeted entity.

### Example 2: With Fallback

```json
{
  "Type": "UseEntity",
  "Failed": {
    "Type": "Simple",
    "RunTime": 0.5
  }
}
```

Interacts with entity, does nothing if entity has no Use interaction.

### Example 3: With Animation

```json
{
  "Type": "Simple",
  "Effects": {
    "ItemAnimationId": "Interact",
    "WorldSoundEventId": "SFX_Interact"
  },
  "Next": {
    "Type": "UseEntity"
  }
}
```

Plays interaction animation, then uses entity.

## Tips

1. **Entity interactions** - Entity must define Use interaction in its role
2. **Failed fallback** - Always provide `Failed` for better UX
3. **Animation** - Add Simple interaction before for interaction animation
4. **Context-aware** - Different entities have different Use behaviors

---

**Previous:** [SpawnDrops](153_Interaction_Type_SpawnDrops.md) | **Next:** [UseCoop](155_Interaction_Type_UseCoop.md)
