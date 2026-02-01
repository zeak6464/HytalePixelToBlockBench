# Model VFX

Learn how to create visual effects attached to models and entities using Model VFX configurations.

## Overview

Model VFX (Visual Effects) are advanced visual effects that can be applied to models, items, NPCs, and entities. They provide highlighting, color overlays, distortions, and animated effects. Model VFX are defined in `Server/Entity/ModelVFX/` and referenced by items, status effects, and entities.

## Location
`Server/Entity/ModelVFX/`

## Example from Game Files

### Burn Model VFX

From `Server/Entity/ModelVFX/Burn.json`:

```1:18:Server/Entity/ModelVFX/Burn.json
{
      "SwitchTo": "PostColor",
      "EffectDirection": "BottomUp",
      "AnimationDuration": 6.0,
      "AnimationRange": {
        "Y": 1.0
      },
      "HighlightColor": "#ff9021",
      "HighlightThickness": 0.3,
      "UseBloomOnHighlight": true,
      "NoiseScale": {
        "X": 29.0,
        "Y": 25.0
      },
      "PostColor": "#171313",
      "PostColorOpacity": 0.792,
      "UseProgessiveHighlight": true
}
```

This shows a model VFX configuration for burn effects with highlighting, color overlays, and animated effects.

## Basic Model VFX Structure

Create `Server/Entity/ModelVFX/MyCustom_Effect.json`:

```json
{
  "SwitchTo": "PostColor",
  "EffectDirection": "FromCenter",
  "AnimationDuration": 5.0,
  "HighlightColor": "#00ff00",
  "HighlightThickness": 1.0
}
```

## Model VFX Properties

### SwitchTo

```json
{
  "SwitchTo": "PostColor"
}
```

Effect type:
- **`"PostColor"`** - Color overlay/post-processing effect
- **`"Distortion"`** - Visual distortion effect

### EffectDirection

```json
{
  "EffectDirection": "FromCenter"
}
```

Direction of effect animation:
- **`"FromCenter"`** - Effect spreads from center
- **`"TopDown"`** - Effect moves from top to bottom
- **`"BottomUp"`** - Effect moves from bottom to top

### AnimationDuration

```json
{
  "AnimationDuration": 5.0
}
```

Duration of the animation in seconds.

### AnimationRange

```json
{
  "AnimationRange": {
    "Y": 0.37
  }
}
```

Range/area of the effect animation (typically Y-axis for vertical effects).

### HighlightColor

```json
{
  "HighlightColor": "#001ddf"
}
```

Color of the highlight effect (hex format, e.g., `#RRGGBB`).

### HighlightThickness

```json
{
  "HighlightThickness": 1.0
}
```

Thickness of the highlight outline.

### UseBloomOnHighlight

```json
{
  "UseBloomOnHighlight": false
}
```

Whether to apply bloom (glow) effect to the highlight:
- **`true`** - Adds glow effect
- **`false`** - No bloom

### PostColor

```json
{
  "PostColor": "#000890"
}
```

Color overlay for PostColor effect type (hex format).

### PostColorOpacity

```json
{
  "PostColorOpacity": 0.6
}
```

Opacity of the post color overlay (0.0 to 1.0).

### NoiseScale

```json
{
  "NoiseScale": {
    "X": 20.0,
    "Y": 50.0
  }
}
```

Noise texture scale for the effect.

### NoiseScrollSpeed

```json
{
  "NoiseScrollSpeed": {
    "Y": -0.1
  }
}
```

Speed at which noise texture scrolls.

### UseProgessiveHighlight

```json
{
  "UseProgessiveHighlight": true
}
```

Whether highlight effect progresses over time.

### LoopOption

```json
{
  "LoopOption": "LoopMirror"
}
```

Looping behavior:
- **`"LoopMirror"`** - Loops and mirrors animation
- **`"Loop"`** - Simple loop
- **`"Once"`** - Plays once

### CurveType

```json
{
  "CurveType": "QuartInOut"
}
```

Animation curve type for smooth transitions (e.g., `"QuartInOut"`, `"Linear"`).

## Common Model VFX

### Creative Tool Selection

`Server/Entity/ModelVFX/Prototype_CreativeTool_ModelVFX.json`:

```json
{
  "SwitchTo": "PostColor",
  "EffectDirection": "FromCenter",
  "AnimationDuration": 15.0,
  "AnimationRange": {
    "Y": 0.37
  },
  "HighlightColor": "#001ddf",
  "HighlightThickness": 1.0,
  "UseBloomOnHighlight": false,
  "NoiseScale": {
    "X": 20.0,
    "Y": 50.0
  },
  "PostColor": "#000890",
  "PostColorOpacity": 0.6,
  "UseProgessiveHighlight": true,
  "LoopOption": "LoopMirror",
  "CurveType": "QuartInOut",
  "NoiseScrollSpeed": {
    "Y": -0.1
  }
}
```

### Distortion Effect

`Server/Entity/ModelVFX/test.json`:

```json
{
  "SwitchTo": "Distortion",
  "EffectDirection": "TopDown",
  "AnimationDuration": 3,
  "HighlightColor": "#00f4ff",
  "HighlightThickness": 2,
  "UseBloomOnHighlight": true,
  "AnimationRange": {
    "Y": 1.0
  }
}
```

### Freeze Effect

`Server/Entity/ModelVFX/Freeze.json`:

```json
{
  "SwitchTo": "PostColor",
  "EffectDirection": "BottomUp",
  "AnimationDuration": 0.7,
  "AnimationRange": {
    "Y": 0.58
  },
  "HighlightColor": "#84ceff",
  "UseBloomOnHighlight": false,
  "NoiseScale": {
    "X": 200.0,
    "Y": 250.0
  },
  "PostColorOpacity": 1.0,
  "UseProgessiveHighlight": true,
  "NoiseScrollSpeed": {
    "Y": -0.1,
    "X": 0.02
  },
  "PostColor": "#e3fbff",
  "HighlightThickness": 1.0
}
```

### Poison Effect

`Server/Entity/ModelVFX/Poison.json`:

```json
{
  "SwitchTo": "PostColor",
  "EffectDirection": "BottomUp",
  "AnimationDuration": 6.0,
  "AnimationRange": {
    "Y": 1.0
  },
  "HighlightColor": "#33aa33",
  "HighlightThickness": 0.3,
  "UseBloomOnHighlight": true,
  "NoiseScale": {
    "X": 29.0,
    "Y": 25.0
  },
  "PostColor": "#082008",
  "PostColorOpacity": 0.792,
  "UseProgessiveHighlight": true
}
```

## Using Model VFX

### In Items

Items can reference Model VFX for visual effects:

```json
{
  "ItemAppearanceConditions": {
    "Health": [
      {
        "Condition": [0, 100.0],
        "ConditionValueType": "Percent",
        "ModelVFXId": "Prototype_CreativeTool_ModelVFX"
      }
    ]
  }
}
```

### In Status Effects

Status effects can apply Model VFX:

```json
{
  "ApplicationEffects": {
    "ModelVFXId": "Freeze"
  }
}
```

### In NPCs

NPCs can have Model VFX:

```json
{
  "DefaultModelVFX": "Poison"
}
```

## Complete Example: Custom Effect

### 1. Model VFX Definition

`Server/Entity/ModelVFX/MyCustom_Empowered.json`:

```json
{
  "SwitchTo": "PostColor",
  "EffectDirection": "FromCenter",
  "AnimationDuration": 10.0,
  "AnimationRange": {
    "Y": 0.5
  },
  "HighlightColor": "#ffaa00",
  "HighlightThickness": 2.0,
  "UseBloomOnHighlight": true,
  "NoiseScale": {
    "X": 15.0,
    "Y": 30.0
  },
  "PostColor": "#ff8800",
  "PostColorOpacity": 0.5,
  "UseProgessiveHighlight": true,
  "LoopOption": "LoopMirror",
  "CurveType": "QuartInOut",
  "NoiseScrollSpeed": {
    "Y": 0.15
  }
}
```

### 2. Using in Status Effect

`Server/Entity/Effects/Status/MyCustom_Empowered.json`:

```json
{
  "ApplicationEffects": {
    "ModelVFXId": "MyCustom_Empowered",
    "StatModifiers": [
      {
        "EntityStatId": "DamageMultiplier",
        "Value": 1.5,
        "Duration": 30.0
      }
    ]
  }
}
```

### 3. Using in Item

`Server/Item/Items/Trinkets/Trinket_MyCustom_Empowered.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Trinket_MyCustom_Empowered.name"
  },
  "ItemAppearanceConditions": {
    "Health": [
      {
        "Condition": [50.0, 100.0],
        "ConditionValueType": "Percent",
        "ModelVFXId": "MyCustom_Empowered"
      }
    ]
  }
}
```

## Tips for Creating Model VFX

1. **Start simple** - Begin with basic PostColor or Distortion effects
2. **Test colors** - Choose colors that match the effect theme
3. **Adjust duration** - Animation duration affects how noticeable the effect is
4. **Use bloom** - Enable bloom for magical/glowing effects
5. **Try directions** - Different directions create different visual styles
6. **Match theme** - Use colors and effects that match your item/effect theme
7. **Test in-game** - Visual effects can look different in-game than in configs

---

**Previous:** [Block Type Lists](50_Block_Type_Lists.md) | **Next:** [Farming Modifiers](52_Farming_Modifiers.md)
