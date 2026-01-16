# Game Mode Configs

Learn how to configure game mode-specific entity behaviors like permissions and effects.

## Overview

Game mode configs define entity behaviors for different game modes (Adventure, Creative). They control permissions, effects applied on entering the mode, and mode-specific interactions.

## Location
`Server/Entity/GameMode/`

## Basic Game Mode Structure

Create `Server/Entity/GameMode/MyCustom.json`:

```json
{
  "PermissionGroups": [
    "Adventure"
  ],
  "InteractionsOnEnter": {
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "MyCustomEffect"
      }
    ]
  }
}
```

## Game Mode Properties

### Permission Groups

```json
{
  "PermissionGroups": [
    "Adventure",
    "Creative"
  ]
}
```

Game modes this config applies to.

### Interactions on Enter

```json
{
  "InteractionsOnEnter": {
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "Creative"
      }
    ]
  }
}
```

Interactions triggered when entity enters this game mode.

## Common Presets

### Adventure

`Server/Entity/GameMode/Adventure.json`:

```json
{
  "PermissionGroups": [
    "Adventure"
  ],
  "InteractionsOnEnter": {
    "Interactions": [
      {
        "Type": "Simple",
        "Effects": {
          "LocalSoundEventId": "SFX_Avatar_Powers_Disable_Local",
          "WorldSoundEventId": "SFX_Avatar_Powers_Disable"
        }
      }
    ]
  }
}
```

Disables creative powers when entering Adventure mode.

### Creative

`Server/Entity/GameMode/Creative.json`:

```json
{
  "PermissionGroups": [
    "Creative",
    "Adventure"
  ],
  "InteractionsOnEnter": {
    "Interactions": [
      {
        "Type": "ApplyEffect",
        "EffectId": "Creative"
      }
    ]
  }
}
```

Applies Creative effect (flying, no-clip, etc.) when entering Creative mode.

## Tips for Game Mode Configs

1. **Match permissions** - Ensure PermissionGroups match actual game modes
2. **Enter effects** - Use to enable/disable mode-specific abilities
3. **Mode transitions** - Configure smooth transitions between modes

---

**Previous:** [Hitbox Collision](59_Hitbox_Collision.md) | **Next:** [Custom Connected Blocks](61_Custom_Connected_Blocks.md)
