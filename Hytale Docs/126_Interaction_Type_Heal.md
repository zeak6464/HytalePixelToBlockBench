# Interaction Type: Heal

Restore health to entities directly.

## Overview

> **Note:** The `"Type": "Heal"` interaction does NOT exist in game files. Use `ChangeStat` with the `Health` stat instead.

Healing in Hytale is done via `ChangeStat` interaction with the `Health` stat, or through status effects with regeneration.

## Correct Way to Heal

### Using ChangeStat

```json
{
  "Type": "ChangeStat",
  "EntityStatId": "Health",
  "Amount": 50
}
```

## Properties

### Amount

```json
{
  "Amount": 50
}
```

Health amount to restore. Can be absolute value or percentage.

### Entity

```json
{
  "Entity": "Target"
}
```

Target entity:
- **`"Target"`** - Selected entity (from Selector)
- **`"Self"`** - Entity performing interaction
- Omit for self

## Complete Examples

### Example 1: Basic Heal

```json
{
  "Type": "Heal",
  "Amount": 50
}
```

Restores 50 health to self.

### Example 2: Potion Heal

```json
{
  "Type": "Heal",
  "Amount": 100
}
```

Restores 100 health (potion effect).

### Example 3: Target Heal

```json
{
  "Type": "Heal",
  "Amount": 50,
  "Entity": "Target"
}
```

Restores 50 health to targeted entity.

### Example 4: With Effects

```json
{
  "Type": "Simple",
  "Effects": {
    "WorldSoundEventId": "SFX_Heal",
    "WorldParticles": [
      {
        "SystemId": "Heal_Effect"
      }
    ]
  },
  "Next": {
    "Type": "Heal",
    "Amount": 50
  }
}
```

Plays heal effects, then restores health.

## Tips

1. **Amount** - Use appropriate values (50-100 for potions, 10-25 for food)
2. **Target healing** - Use "Target" for healing spells on other entities
3. **Visual feedback** - Always include particles/sounds for feedback
4. **Overheal** - Amount can exceed max health (check game behavior)

---

**Previous:** [SpawnNPC](125_Interaction_Type_SpawnNPC.md) | **Next:** [PlaceBlock](127_Interaction_Type_PlaceBlock.md)
