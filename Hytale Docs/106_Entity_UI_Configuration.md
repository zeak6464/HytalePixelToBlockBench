# Entity UI Configuration

Learn how to configure UI elements displayed above entities, including health bars, combat text, and damage numbers.

## Overview

Entity UI configurations control visual UI elements displayed above entities (players and NPCs). These include health bars, damage numbers, combat text, and other entity-specific UI overlays. They're defined in `Server/Entity/UI/` and referenced by entity stats or other systems.

## Example from Game Files

### Combat Text Configuration

From `Server/Entity/UI/CombatText.json`:

```1:47:Server/Entity/UI/CombatText.json
{
  "Type": "CombatText",
  "HitboxOffset": {
    "X": 0,
    "Y": -100
  },
  "RandomPositionOffsetRange": {
    "X": {
      "Min": 20,
      "Max": 60
    },
    "Y": {
      "Min": 10,
      "Max": 30
    }
  },
  "ViewportMargin": 100,
  "Duration": 0.4,
  "HitAngleModifierStrength": 2.0,
  "FontSize": 48,
  "TextColor": "#ffffff",
  "AnimationEvents": [
    {
      "Type": "Scale",
      "StartAt": 0,
      "EndAt": 0.4,
      "StartScale": 1,
      "EndScale": 0.5
    },
    {
      "Type": "Position",
      "StartAt": 0.1,
      "EndAt": 1,
      "PositionOffset": {
        "X": 0,
        "Y": -80
      }
    },
    {
      "Type": "Opacity",
      "StartAt": 0.4,
      "EndAt": 1,
      "StartOpacity": 1,
      "EndOpacity": 0
    }
  ]
}
```

This shows a combat text UI configuration with offsets, animations, and styling for damage numbers.

## Location
`Server/Entity/UI/`

## UI Configuration Types

### Healthbar

Displays entity health bar above the entity.

`Server/Entity/UI/Healthbar.json`:

```json
{
  "Type": "EntityStat",
  "EntityStat": "Health",
  "HitboxOffset": {
    "X": 0,
    "Y": -30
  }
}
```

**Properties:**
- **`Type`** - Must be `"EntityStat"` for stat-based UI
- **`EntityStat`** - Stat to display (`"Health"`, `"Mana"`, `"Stamina"`, etc.)
- **`HitboxOffset`** - Position offset relative to entity hitbox
  - **`X`** - Horizontal offset (pixels, negative = left, positive = right)
  - **`Y`** - Vertical offset (pixels, negative = up, positive = down)

**HitboxOffset:**
- Positioned relative to entity's hitbox center
- `Y: -30` means 30 pixels above the entity
- `X: 0` means centered horizontally

### CombatText

Displays floating damage numbers and combat text.

`Server/Entity/UI/CombatText.json`:

```json
{
  "Type": "CombatText",
  "HitboxOffset": {
    "X": 0,
    "Y": -100
  },
  "RandomPositionOffsetRange": {
    "X": {
      "Min": 20,
      "Max": 60
    },
    "Y": {
      "Min": 10,
      "Max": 30
    }
  },
  "ViewportMargin": 100,
  "Duration": 0.4,
  "HitAngleModifierStrength": 2.0,
  "FontSize": 48,
  "TextColor": "#ffffff",
  "AnimationEvents": [
    {
      "Type": "Scale",
      "StartAt": 0,
      "EndAt": 0.4,
      "StartScale": 1,
      "EndScale": 0.5
    },
    {
      "Type": "Position",
      "StartAt": 0.1,
      "EndAt": 1,
      "PositionOffset": {
        "X": 0,
        "Y": -80
      }
    },
    {
      "Type": "Opacity",
      "StartAt": 0.4,
      "EndAt": 1,
      "StartOpacity": 1,
      "EndOpacity": 0
    }
  ]
}
```

**Properties:**
- **`Type`** - Must be `"CombatText"` for damage numbers
- **`HitboxOffset`** - Base position offset from entity
- **`RandomPositionOffsetRange`** - Random offset range for variety
  - **`X.Min/Max`** - Horizontal random offset range
  - **`Y.Min/Max`** - Vertical random offset range
- **`ViewportMargin`** - Margin from viewport edge (keeps text on screen)
- **`Duration`** - How long text displays (seconds)
- **`HitAngleModifierStrength`** - How much hit angle affects position
- **`FontSize`** - Text font size (pixels)
- **`TextColor`** - Text color (hex format: `#ffffff`)

**AnimationEvents:**
- **`Type: "Scale"`** - Scales text over time
  - **`StartAt`** - Animation start time (0-1, 0 = start, 1 = end)
  - **`EndAt`** - Animation end time (0-1)
  - **`StartScale`** - Starting scale (1.0 = 100%)
  - **`EndScale`** - Ending scale
- **`Type: "Position"`** - Moves text over time
  - **`StartAt`/`EndAt`** - Animation time range
  - **`PositionOffset`** - Movement offset (pixels)
    - **`X`** - Horizontal movement
    - **`Y`** - Vertical movement (negative = up)
- **`Type: "Opacity"`** - Fades text over time
  - **`StartAt`/`EndAt`** - Animation time range
  - **`StartOpacity`** - Starting opacity (0-1, 1 = fully visible)
  - **`EndOpacity`** - Ending opacity (0 = invisible)

## Complete Examples

### Example 1: Simple Healthbar

```json
{
  "Type": "EntityStat",
  "EntityStat": "Health",
  "HitboxOffset": {
    "X": 0,
    "Y": -30
  }
}
```

Centered health bar 30 pixels above entity.

### Example 2: Healthbar with Offset

```json
{
  "Type": "EntityStat",
  "EntityStat": "Health",
  "HitboxOffset": {
    "X": 20,
    "Y": -40
  }
}
```

Health bar 20 pixels to the right, 40 pixels above entity.

### Example 3: Damage Numbers

```json
{
  "Type": "CombatText",
  "HitboxOffset": {
    "X": 0,
    "Y": -100
  },
  "RandomPositionOffsetRange": {
    "X": {
      "Min": -30,
      "Max": 30
    },
    "Y": {
      "Min": -10,
      "Max": 10
    }
  },
  "ViewportMargin": 100,
  "Duration": 0.5,
  "FontSize": 48,
  "TextColor": "#ff0000",
  "AnimationEvents": [
    {
      "Type": "Scale",
      "StartAt": 0,
      "EndAt": 0.3,
      "StartScale": 0.8,
      "EndScale": 1.2
    },
    {
      "Type": "Position",
      "StartAt": 0,
      "EndAt": 1,
      "PositionOffset": {
        "X": 0,
        "Y": -100
      }
    },
    {
      "Type": "Opacity",
      "StartAt": 0.3,
      "EndAt": 1,
      "StartOpacity": 1,
      "EndOpacity": 0
    }
  ]
}
```

Red damage numbers that:
- Start small (0.8x), grow to 1.2x, then shrink
- Float upward 100 pixels
- Fade out after 0.3 seconds

### Example 4: Critical Hit Text

```json
{
  "Type": "CombatText",
  "HitboxOffset": {
    "X": 0,
    "Y": -120
  },
  "RandomPositionOffsetRange": {
    "X": {
      "Min": -40,
      "Max": 40
    },
    "Y": {
      "Min": 0,
      "Max": 20
    }
  },
  "ViewportMargin": 100,
  "Duration": 0.6,
  "HitAngleModifierStrength": 3.0,
  "FontSize": 64,
  "TextColor": "#ffff00",
  "AnimationEvents": [
    {
      "Type": "Scale",
      "StartAt": 0,
      "EndAt": 0.2,
      "StartScale": 0.5,
      "EndScale": 1.5
    },
    {
      "Type": "Position",
      "StartAt": 0,
      "EndAt": 1,
      "PositionOffset": {
        "X": 0,
        "Y": -120
      }
    },
    {
      "Type": "Opacity",
      "StartAt": 0.4,
      "EndAt": 1,
      "StartOpacity": 1,
      "EndOpacity": 0
    }
  ]
}
```

Yellow critical hit text that:
- Starts very small (0.5x), grows large (1.5x), then normal
- Floats up 120 pixels
- More affected by hit angle (3.0 vs 2.0)
- Longer duration (0.6s)

### Example 5: Mana Bar

```json
{
  "Type": "EntityStat",
  "EntityStat": "Mana",
  "HitboxOffset": {
    "X": 0,
    "Y": -50
  }
}
```

Mana bar 50 pixels above entity (below health bar if both exist).

## Animation Event Types

### Scale Animation

Scales UI element over time:

```json
{
  "Type": "Scale",
  "StartAt": 0,
  "EndAt": 0.4,
  "StartScale": 1,
  "EndScale": 0.5
}
```

**Properties:**
- **`StartAt`** - Animation start (0-1, 0 = beginning of duration)
- **`EndAt`** - Animation end (0-1, 1 = end of duration)
- **`StartScale`** - Starting scale multiplier (1.0 = 100%)
- **`EndScale`** - Ending scale multiplier

### Position Animation

Moves UI element over time:

```json
{
  "Type": "Position",
  "StartAt": 0.1,
  "EndAt": 1,
  "PositionOffset": {
    "X": 0,
    "Y": -80
  }
}
```

**Properties:**
- **`StartAt`/`EndAt`** - Animation time range (0-1)
- **`PositionOffset`** - Movement offset in pixels
  - **`X`** - Horizontal (negative = left, positive = right)
  - **`Y`** - Vertical (negative = up, positive = down)

### Opacity Animation

Fades UI element over time:

```json
{
  "Type": "Opacity",
  "StartAt": 0.4,
  "EndAt": 1,
  "StartOpacity": 1,
  "EndOpacity": 0
}
```

**Properties:**
- **`StartAt`/`EndAt`** - Animation time range (0-1)
- **`StartOpacity`** - Starting opacity (0-1, 1 = fully visible)
- **`EndOpacity`** - Ending opacity (0 = invisible)

## Color Formats

Colors use hex format:

```json
{
  "TextColor": "#ffffff"  // White
}
```

**Common Colors:**
- `"#ffffff"` - White
- `"#ff0000"` - Red (damage)
- `"#00ff00"` - Green (healing)
- `"#0000ff"` - Blue (mana)
- `"#ffff00"` - Yellow (critical)
- `"#ff00ff"` - Magenta
- `"#00ffff"` - Cyan

## Multiple Animation Events

You can have multiple animation events running simultaneously or sequentially:

```json
{
  "AnimationEvents": [
    {
      "Type": "Scale",
      "StartAt": 0,
      "EndAt": 0.3,
      "StartScale": 0.8,
      "EndScale": 1.2
    },
    {
      "Type": "Position",
      "StartAt": 0,
      "EndAt": 1,
      "PositionOffset": {
        "X": 0,
        "Y": -100
      }
    },
    {
      "Type": "Opacity",
      "StartAt": 0.3,
      "EndAt": 1,
      "StartOpacity": 1,
      "EndOpacity": 0
    }
  ]
}
```

All animations run simultaneously based on their `StartAt`/`EndAt` times:
- Scale runs from 0.0s to 0.3s (first 30% of duration)
- Position runs from 0.0s to 1.0s (entire duration)
- Opacity runs from 0.3s to 1.0s (last 70% of duration)

## Tips for Entity UI

1. **HitboxOffset positioning** - Use negative Y values to position above entity
2. **RandomPositionOffsetRange** - Add variety to combat text positions
3. **Animation timing** - Coordinate `StartAt`/`EndAt` for smooth animations
4. **ViewportMargin** - Keep text on screen even with camera movement
5. **Duration** - Match duration to animation events (usually 0.4-0.6s for damage)
6. **FontSize** - 48-64 typical for damage numbers, adjust for readability
7. **Color coding** - Use different colors for different damage types (red=physical, blue=magic)
8. **Scale animations** - Use pop-in effect (0.8 → 1.2 → 1.0) for impact
9. **Multiple stats** - Offset health/mana bars vertically (Y: -30, Y: -50)

## Stat-Based UI

Entity stat UI (`EntityStat` type) can display any stat:

```json
{
  "Type": "EntityStat",
  "EntityStat": "Health"
}
```

Common stat types:
- `"Health"`
- `"Mana"`
- `"Stamina"`
- `"SignatureEnergy"`
- Custom stats defined in `Server/Entity/Stats/`

---

**Previous:** [Drop Tables](105_Drop_Tables.md) | **Next:** [Farming Coops](107_Farming_Coops.md)
