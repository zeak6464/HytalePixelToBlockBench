# Hitbox Collision

Learn how to configure entity hitbox collision behavior for soft vs hard collision detection.

## Overview

Hitbox collision configs determine how entities collide with each other. "Soft" collision allows slight overlap, while "Hard" collision prevents any overlap.

## Location
`Server/Entity/HitboxCollision/`

## Example from Game Files

### Soft Collision

From `Server/Entity/HitboxCollision/SoftCollision.json`:

```1:4:Server/Entity/HitboxCollision/SoftCollision.json
{
	"CollisionType": "Soft",
	"SoftCollisionOffsetRatio": 1.25
}
```

This shows a soft collision configuration that allows slight overlap between entities.

## Collision Types

### Soft Collision

`Server/Entity/HitboxCollision/SoftCollision.json`:

```json
{
  "CollisionType": "Soft",
  "SoftCollisionOffsetRatio": 1.25
}
```

- **`SoftCollisionOffsetRatio`** - Allows entities to overlap by this ratio
- More forgiving, feels smoother
- Used for default player collision

### Hard Collision

`Server/Entity/HitboxCollision/HardCollision.json`:

```json
{
  "CollisionType": "Hard"
}
```

- Prevents all overlap
- More rigid, precise collision
- Used for certain game modes or NPCs

## Using Hitbox Collision

Reference in gameplay configs:

```json
{
  "Player": {
    "HitboxCollisionConfig": "SoftCollision"
  }
}
```

## Tips for Hitbox Collision

1. **Soft for players** - Usually preferred for better feel
2. **Hard for precision** - Use when exact positioning matters
3. **Test collision feel** - Affects player movement experience

---

**Previous:** [Movement Configs](58_Movement_Configs.md) | **Next:** [Game Mode Configs](60_Game_Mode_Configs.md)
