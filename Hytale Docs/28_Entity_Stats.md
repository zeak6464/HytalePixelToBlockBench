# Entity Stats

Learn how entity stats work, their configuration, regeneration, and conditions.

## Overview

Entity stats like Health, Mana, and Stamina are defined in `Server/Entity/Stats/`. They control initial values, limits, regeneration, and special behaviors.

## Location
`Server/Entity/Stats/`

## Basic Stat Structure

Create `Server/Entity/Stats/MyCustom_Stat.json`:

```json
{
  "InitialValue": 100,
  "Min": 0,
  "Max": 100,
  "Shared": false,
  "ResetType": "MaxValue",
  "Regenerating": [
    {
      "Interval": 0.5,
      "Amount": 1.0,
      "RegenType": "Additive",
      "Conditions": [
        {
          "Id": "Alive"
        }
      ]
    }
  ]
}
```

## Stat Properties

### Initial Value

```json
"InitialValue": 100
```

Starting value when entity is created/spawned.

### Min/Max Values

```json
"Min": 0,
"Max": 100
```

- **`Min`** - Minimum value (cannot go below)
- **`Max`** - Maximum value (cannot go above)
- Can be negative (e.g., Stamina: `"Min": -4` for overdrawn state)

### Shared

```json
"Shared": false
```

- **`true`** - Stat is shared across all entities (global)
- **`false`** - Stat is per-entity (local)

### Reset Type

```json
"ResetType": "MaxValue"
```

How stat resets:
- **`"MaxValue"`** - Resets to maximum value
- **`"InitialValue"`** - Resets to initial value
- **`"None"`** - No automatic reset

## Stat Regeneration

Stats can regenerate over time based on conditions.

### Basic Regeneration

```json
"Regenerating": [
  {
    "Interval": 0.5,      // Check every 0.5 seconds
    "Amount": 1.0,        // Add 1.0 per interval
    "RegenType": "Additive",  // or "Percentage"
    "Conditions": [
      {
        "Id": "Alive"
      }
    ]
  }
]
```

### Regeneration Types

#### Additive

```json
{
  "Amount": 1.0,
  "RegenType": "Additive"  // Adds fixed amount
}
```

Adds a fixed amount per interval (e.g., +1 mana per 0.2 seconds).

#### Percentage

```json
{
  "Amount": 0.05,
  "RegenType": "Percentage"  // Adds percentage of max
}
```

Adds a percentage of maximum value (e.g., 5% health per interval).

## Regeneration Conditions

Regeneration only occurs when all conditions are met.

### Common Conditions

#### Alive

```json
{
  "Id": "Alive"
}
```

Entity must be alive.

#### Player

```json
{
  "Id": "Player",
  "Inverse": false  // true = NOT a player
}
```

Entity must be (or must not be) a player.

#### Game Mode

```json
{
  "Id": "Player",
  "GameMode": "Creative"
}
```

Player must be in specific game mode.

#### Stat Condition

```json
{
  "Id": "Stat",
  "Stat": "Stamina",
  "Amount": 0,
  "Comparison": "Gte"  // Greater than or equal
}
```

Another stat must meet a condition.

Comparisons:
- **`"Gte"`** - Greater than or equal (≥)
- **`"Lt"`** - Less than (<)
- **`"Eq"`** - Equal (==)

#### No Damage Taken

```json
{
  "Id": "NoDamageTaken",
  "Delay": 15  // Seconds without damage
}
```

No damage taken for specified delay period.

#### Actions (Inverse)

```json
{
  "Id": "Sprinting",
  "Inverse": true  // NOT sprinting
}
```

Entity must NOT be performing this action.

Common action IDs:
- `"Wielding"` - Holding a weapon
- `"Sprinting"` - Sprinting
- `"Gliding"` - Gliding
- `"Charging"` - Charging an ability

#### Clamp at Zero

```json
{
  "Amount": -0.1,
  "ClampAtZero": true
}
```

Prevents stat from going below 0 when draining.

## Common Stats

### Health

`Server/Entity/Stats/Health.json`:

```json
{
  "InitialValue": 100,
  "Min": 0,
  "Max": 100,
  "Shared": true,
  "ResetType": "MaxValue",
  "Regenerating": [
    {
      "Interval": 0.5,
      "Amount": 0.05,
      "RegenType": "Percentage",
      "Conditions": [
        { "Id": "Alive" },
        { "Id": "Player", "Inverse": true },  // NPCs only
        { "Id": "NoDamageTaken", "Delay": 15 },
        { "Id": "RegenHealth" }
      ]
    }
  ]
}
```

### Mana

`Server/Entity/Stats/Mana.json`:

```json
{
  "InitialValue": 0,
  "Min": 0,
  "Max": 0,  // Max set by items/equipment
  "Shared": false,
  "ResetType": "MaxValue",
  "Regenerating": [
    {
      "Interval": 0.2,
      "Amount": 1,
      "RegenType": "Additive",
      "Conditions": [
        { "Id": "Alive" },
        { "Id": "NoDamageTaken", "Delay": 6 },
        { "Id": "Charging", "Inverse": true }
      ]
    }
  ]
}
```

### Stamina

`Server/Entity/Stats/Stamina.json`:

```json
{
  "InitialValue": 10,
  "Min": -4,  // Can be overdrawn
  "Max": 10,
  "Shared": false,
  "Regenerating": [
    {
      "Interval": 0.1,
      "Amount": 0.3,
      "RegenType": "Additive",
      "Conditions": [
        { "Id": "Stat", "Stat": "StaminaRegenDelay", "Amount": 0 },
        { "Id": "Stat", "Stat": "Stamina", "Amount": 0, "Comparison": "Gte" },
        { "Id": "Wielding", "Inverse": true },
        { "Id": "Sprinting", "Inverse": true },
        { "Id": "Gliding", "Inverse": true }
      ]
    },
    {
      "Interval": 0.1,
      "Amount": -0.1,  // Drains while sprinting
      "ClampAtZero": true,
      "RegenType": "Additive",
      "Conditions": [
        { "Id": "Sprinting" }
      ]
    }
  ]
}
```

## Stat Modifiers in Items

Items can modify stats using `StatModifiers`:

```json
{
  "StatModifiers": {
    "Health": [
      {
        "Amount": 20,
        "CalculationType": "Additive"
      }
    ],
    "Mana": [
      {
        "Amount": 50,
        "CalculationType": "Additive"
      }
    ],
    "Stamina": [
      {
        "Amount": 5,
        "CalculationType": "Additive"
      }
    ]
  }
}
```

- **`CalculationType: "Additive"`** - Adds to base stat
- **`CalculationType: "Multiplicative"`** - Multiplies stat (percentage)

## Stat Effects

Stats can trigger effects at min/max values:

### Min Value Effects

```json
{
  "MinValueEffects": {
    "TriggerAtZero": true,
    "Interactions": {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": "Stamina_Broken"
        }
      ]
    }
  }
}
```

Triggers when stat reaches minimum (or zero if `TriggerAtZero: true`).

### Max Value Effects

```json
{
  "MaxValueEffects": {
    "Interactions": {
      "Interactions": [
        {
          "Type": "ClearEntityEffect",
          "EntityEffectId": "Stamina_Broken"
        }
      ]
    }
  }
}
```

Triggers when stat reaches maximum.

## Custom Stats

### Creating a Custom Stat

Create `Server/Entity/Stats/MyCustom_Energy.json`:

```json
{
  "InitialValue": 0,
  "Min": 0,
  "Max": 100,
  "Shared": false,
  "ResetType": "MaxValue",
  "Regenerating": [
    {
      "Interval": 1.0,
      "Amount": 5,
      "RegenType": "Additive",
      "Conditions": [
        { "Id": "Alive" },
        { "Id": "Player" }
      ]
    }
  ],
  "MinValueEffects": {
    "TriggerAtZero": true,
    "Interactions": {
      "Interactions": [
        {
          "Type": "ApplyEffect",
          "EffectId": "Energy_Depleted"
        }
      ]
    }
  }
}
```

Use in items:

```json
{
  "StatModifiers": {
    "MyCustom_Energy": [
      {
        "Amount": 25,
        "CalculationType": "Additive"
      }
    ]
  }
}
```

## Complete Example: Complex Stamina Regeneration

```json
{
  "InitialValue": 10,
  "Min": -4,
  "Max": 10,
  "Regenerating": [
    {
      "$Comment": "Regenerate when not in use",
      "Interval": 0.1,
      "Amount": 0.3,
      "RegenType": "Additive",
      "Conditions": [
        { "Id": "Stat", "Stat": "StaminaRegenDelay", "Amount": 0 },
        { "Id": "Wielding", "Inverse": true },
        { "Id": "Sprinting", "Inverse": true }
      ]
    },
    {
      "$Comment": "Drain while sprinting",
      "Interval": 0.1,
      "Amount": -0.1,
      "ClampAtZero": true,
      "RegenType": "Additive",
      "Conditions": [
        { "Id": "Sprinting" }
      ]
    }
  ]
}
```

## Tips for Configuring Stats

1. **Set appropriate initial values** - Match to entity type and game balance
2. **Define min/max carefully** - Prevents stat abuse or exploits
3. **Use conditions effectively** - Control when regeneration occurs
4. **Balance regeneration rates** - Test to ensure fair gameplay
5. **Add min/max effects** - Provide feedback when stats reach limits
6. **Use additive for fixed amounts** - Use percentage for scaling regen
7. **Test stat interactions** - Ensure stats work correctly with items and effects

---

**Previous:** [Animations](27_Animations.md) | **Next:** [Localization](29_Localization.md)
