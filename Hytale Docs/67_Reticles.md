# Reticles

Learn how to configure crosshair reticles that appear when wielding weapons, with event-based parts.

## Overview

Reticles (crosshairs) are UI elements displayed when holding weapons or items. They can change based on:
- Server events (hit, kill)
- Client events (wielding, movement)
- Weapon state

## Location
`Server/Item/Reticles/`

## Basic Reticle Structure

Create `Server/Item/Reticles/MyCustom.json`:

```json
{
  "Base": ["UI/Reticles/Melee.png"],
  "ServerEvents": {
    "OnHit": {
      "Parts": [
        "UI/Reticles/ServerEvents/OnHit/Default.png"
      ]
    }
  },
  "ClientEvents": {
    "Wielding": {
      "HideBase": true,
      "Parts": [
        "UI/Reticles/ClientEvents/Wielding/Blocking.png"
      ]
    }
  }
}
```

## Reticle Properties

### Base

```json
{
  "Base": ["UI/Reticles/Melee.png"]
}
```

Default/base reticle image(s). Array allows multiple layers.

### ServerEvents

```json
{
  "ServerEvents": {
    "OnHit": {
      "Parts": ["UI/Reticles/ServerEvents/OnHit/Default.png"]
    },
    "OnKill": {
      "HideBase": true,
      "Parts": [
        "UI/Reticles/ServerEvents/OnKill/Default.png"
      ]
    }
  }
}
```

Reticle parts shown on server-triggered events.

### ClientEvents

```json
{
  "ClientEvents": {
    "Wielding": {
      "HideBase": true,
      "Parts": ["UI/Reticles/ClientEvents/Wielding/Blocking.png"]
    },
    "OnMovementLeft": {
      "Parts": ["UI/Reticles/ClientEvents/OnMovementLeft/Default.png"]
    }
  }
}
```

Reticle parts shown on client-triggered events.

### HideBase

```json
{
  "HideBase": true
}
```

Hides base reticle when event parts are shown.

## Common Reticles

### Default Melee

`Server/Item/Reticles/DefaultMelee.json`:

Standard melee weapon reticle with hit/kill/blocking states.

### Mace Melee

`Server/Item/Reticles/MaceMelee.json`:

Mace-specific reticle with movement-based parts.

## Using Reticles

Reference in item definitions:

```json
{
  "Reticle": "DefaultMelee"
}
```

## Tips for Creating Reticles

1. **Base design** - Create clear, visible base reticle
2. **Event feedback** - Use server events for hit/kill feedback
3. **State indicators** - Use client events for weapon state (blocking, etc.)
4. **Image paths** - Store reticle images in `Common/UI/Reticles/`
5. **Layering** - Use Parts arrays for multiple overlay elements

---

**Previous:** [Item Animations](66_Item_Animations.md) | **Next:** [Player Tools Menu](68_Player_Tools_Menu.md)
