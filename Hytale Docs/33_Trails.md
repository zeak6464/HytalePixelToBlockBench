# Trails

Learn how to create and configure visual trails for projectiles, items, and weapons.

## Overview

Trails in Hytale create visual effects that follow projectiles and items. They're defined in `Server/Entity/Trails/` and referenced by ID in projectiles, items, and interactions.

## Location
- Trail definitions: `Server/Entity/Trails/`
- Trail textures: `Common/Trails/`

## Example from Game Files

### Arrow Trail

From `Server/Entity/Trails/Weapons/Arrow/Arrow.json`:

```1:15:Server/Entity/Trails/Weapons/Arrow/Arrow.json
{
  "TexturePath": "Trails/Arrow.png",
  "LifeSpan": 20,
  "Roll": 90,
  "LightInfluence": 0,
  "Start": {
    "Color": "#ffffff40",
    "Width": 0.2
  },
  "End": {
    "Color": "#ffffff00",
    "Width": 0.1
  },
  "RenderMode": "BlendAdd",
  "Smooth": true
}
```

This shows a trail configuration for arrow projectiles with texture, width, color gradient, and rendering settings.

## Basic Trail Structure

Create `Server/Entity/Trails/MyCustom_Trail.json`:

```json
{
  "TexturePath": "Trails/White.png",
  "LifeSpan": 15,
  "Roll": 0,
  "Start": {
    "Color": "#ffffff65",
    "Width": 0.4
  },
  "End": {
    "Color": "#FFFFFF00",
    "Width": 0
  },
  "LightInfluence": 1,
  "Smooth": true,
  "RenderMode": "BlendLinear"
}
```

## Trail Properties

### Texture Path

```json
{
  "TexturePath": "Trails/White.png"
}
```

Path to trail texture (relative to `Common/`).

### Life Span

```json
{
  "LifeSpan": 15
}
```

How long the trail persists (frames or time).

### Roll

```json
{
  "Roll": 0
}
```

Rotation of the trail (degrees):
- **`0`** - Horizontal
- **`90`** - Vertical
- **`45`** - Diagonal

### Start Properties

```json
{
  "Start": {
    "Color": "#ffffff65",
    "Width": 0.4
  }
}
```

- **`Color`** - Hex color with alpha (e.g., `#ffffff65` = white with 65% opacity)
- **`Width`** - Starting width of the trail

### End Properties

```json
{
  "End": {
    "Color": "#FFFFFF00",
    "Width": 0
  }
}
```

- **`Color`** - Ending color (usually transparent `#FFFFFF00`)
- **`Width`** - Ending width (usually `0` for fade-out)

### Light Influence

```json
{
  "LightInfluence": 1
}
```

- **`1`** - Trail affected by lighting
- **`0`** - Trail unaffected by lighting

### Smooth

```json
{
  "Smooth": true
}
```

Whether the trail has smooth interpolation.

### Render Mode

```json
{
  "RenderMode": "BlendLinear"
}
```

Rendering mode:
- **`"BlendLinear"`** - Standard blending
- **`"BlendAdd"`** - Additive blending (glowing effect)

## Common Trail Types

### Arrow Trail

`Server/Entity/Trails/Weapons/Arrow/Arrow.json`:

```json
{
  "TexturePath": "Trails/Arrow.png",
  "LifeSpan": 20,
  "Roll": 90,
  "LightInfluence": 0,
  "Start": {
    "Color": "#ffffff40",
    "Width": 0.2
  },
  "End": {
    "Color": "#ffffff00",
    "Width": 0.1
  },
  "RenderMode": "BlendAdd",
  "Smooth": true
}
```

### Projectile Trail

`Server/Entity/Trails/Medium_Projectile.json`:

```json
{
  "TexturePath": "Trails/White.png",
  "LifeSpan": 15,
  "Roll": 0,
  "Start": {
    "Color": "#ffffff65",
    "Width": 0.4
  },
  "End": {
    "Color": "#FFFFFF00",
    "Width": 0
  },
  "LightInfluence": 1,
  "Smooth": true,
  "RenderMode": "BlendLinear"
}
```

## Using Trails in Projectiles

### Projectile Model Trails

In `Server/Models/Projectiles/Weapons/Arrow/Arrow_MyCustom.json`:

```json
{
  "Trails": [
    {
      "PositionOffset": {
        "X": 0.5,
        "Y": 0,
        "Z": 0
      },
      "TargetNodeName": "Handle",
      "TrailId": "Arrow",
      "RotationOffset": {
        "Yaw": 90,
        "Pitch": 0,
        "Roll": 45
      }
    }
  ]
}
```

### Trail Properties in Projectiles

- **`PositionOffset`** - Offset from target node (X, Y, Z)
- **`TargetNodeName`** - Node/bone to attach trail to (e.g., `"Handle"`)
- **`TrailId`** - ID of trail definition (e.g., `"Arrow"`)
- **`RotationOffset`** - Rotation offset (Yaw, Pitch, Roll in degrees)

## Using Trails in Interactions

### Item Interaction Trails

```json
{
  "Effects": {
    "Trails": [
      {
        "PositionOffset": {
          "X": 0.8,
          "Y": 0,
          "Z": 0
        },
        "RotationOffset": {
          "Pitch": 0,
          "Roll": 90,
          "Yaw": 0
        },
        "TargetNodeName": "Handle",
        "TrailId": "Medium_Default"
      }
    ]
  }
}
```

## Trail Color Examples

### White Trail

```json
{
  "Start": {
    "Color": "#ffffff65",  // White with 65% opacity
    "Width": 0.4
  },
  "End": {
    "Color": "#FFFFFF00",  // Fully transparent
    "Width": 0
  }
}
```

### Colored Trail

```json
{
  "Start": {
    "Color": "#ff000065",  // Red with 65% opacity
    "Width": 0.5
  },
  "End": {
    "Color": "#ff000000",  // Fully transparent red
    "Width": 0
  }
}
```

### Glowing Trail (Additive)

```json
{
  "RenderMode": "BlendAdd",
  "Start": {
    "Color": "#00ffffff",  // Cyan, fully opaque
    "Width": 0.3
  },
  "End": {
    "Color": "#00ffffff00",  // Transparent
    "Width": 0
  }
}
```

## Complete Example: Custom Projectile Trail

### 1. Trail Definition

`Server/Entity/Trails/MyCustom_Projectile_Trail.json`:

```json
{
  "TexturePath": "Trails/White.png",
  "LifeSpan": 20,
  "Roll": 0,
  "Start": {
    "Color": "#00ff0065",
    "Width": 0.5
  },
  "End": {
    "Color": "#00ff0000",
    "Width": 0
  },
  "LightInfluence": 0,
  "Smooth": true,
  "RenderMode": "BlendAdd"
}
```

### 2. Projectile Model

`Server/Models/Projectiles/Weapons/Arrow/Arrow_MyCustom.json`:

```json
{
  "Model": "Items/Projectiles/Projectile.blockymodel",
  "Texture": "Items/Projectiles/Projectile_default.png",
  "Trails": [
    {
      "PositionOffset": {
        "X": 0.5,
        "Y": 0,
        "Z": 0
      },
      "TargetNodeName": "Handle",
      "TrailId": "MyCustom_Projectile_Trail",
      "RotationOffset": {
        "Yaw": 90,
        "Pitch": 0,
        "Roll": 45
      }
    }
  ]
}
```

## Common Trail IDs

- **`"Arrow"`** - Standard arrow trail
- **`"Medium_Projectile"`** - Medium-sized projectile trail
- **`"Small_Projectile"`** - Small projectile trail
- **`"Large_Projectile"`** - Large projectile trail
- **`"Unarmed"`** - Unarmed melee trail

## Tips for Creating Trails

1. **Use appropriate textures** - Match trail texture to effect style
2. **Set life spans carefully** - Too short = invisible, too long = cluttered
3. **Use smooth interpolation** - Enables smooth fade-out
4. **Adjust colors** - Use alpha for fade effects
5. **Choose render mode** - `BlendLinear` for standard, `BlendAdd` for glowing
6. **Test light influence** - Set to `0` for glowing trails, `1` for realistic
7. **Configure offsets** - Position trails at appropriate attachment points

---

**Previous:** [Particle Systems](32_Particle_Systems.md) | **Next:** [Camera Effects](34_Camera_Effects.md)
