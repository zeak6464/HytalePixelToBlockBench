# NPC Appearance

Learn how NPC appearance works and how models are assigned to NPCs.

## Overview

NPC appearance is defined by models in `Server/Models/`. The `Appearance` property in NPC Roles references model IDs. NPCs can use player models or custom creature models.

**Appearance Configuration:**
- Model and texture assignment
- Display names and translation keys
- Spawn/death/despawn visual effects
- Head rotation and camera settings
- Scale variations

## Location
- NPC appearance assignment: `Appearance` property in NPC Roles
- Model definitions: `Server/Models/`

## Official Documentation Reference
See [Hytale NPC Documentation](https://hytalemodding.dev/en/docs/official-documentation/npc-doc) for Role appearance attributes.

## Example from Game Files

### Emberwulf NPC Model

From `Server/Models/Beast/Emberwulf.json`:

```1:40:Server/Models/Beast/Emberwulf.json
{
  "Model": "NPC/Beast/Emberwulf/Models/Model.blockymodel",
  "Texture": "NPC/Beast/Emberwulf/Models/Texture.png",
  "EyeHeight": 1.4,
  "CrouchOffset": -0.2,
  "HitBox": {
    "Max": {
      "X": 0.9,
      "Y": 1.7,
      "Z": 0.9
    },
    "Min": {
      "X": -0.9,
      "Y": 0,
      "Z": -0.9
    }
  },
  "MinScale": 0.8,
  "MaxScale": 1.2,
  "Camera": {
    "Pitch": {
      "AngleRange": {
        "Max": 30,
        "Min": -45
      },
      "TargetNodes": [
        "Head"
      ]
    },
    "Yaw": {
      "AngleRange": {
        "Max": 45,
        "Min": -45
      },
      "TargetNodes": [
        "Head"
      ]
    }
  },
```

This shows an NPC model configuration with model path, texture, hitbox, scale range, and camera settings.

## Basic Appearance Assignment

```json
{
  "Appearance": "Goblin_Scrapper"
}
```

References a model ID in `Server/Models/`.

## Appearance via Parameters

```json
{
  "Parameters": {
    "Appearance": {
      "Value": "Goblin_Scrapper",
      "Description": "Model to be used"
    }
  },
  "Appearance": { "Compute": "Appearance" }
}
```

Allows appearance to be parameterized in templates.

## Common Appearance Types

### Player-Based Models

```json
{
  "Appearance": "PlayerTestModel_V"
}
```

Uses player character model.

### Creature Models

```json
{
  "Appearance": "Sheep",
  "Appearance": "Bear_Grizzly",
  "Appearance": "Dragon_Frost"
}
```

Uses creature/NPC models.

### Intelligent NPC Models

```json
{
  "Appearance": "Goblin_Scrapper",
  "Appearance": "Kweebec_Sapling_Orange"
}
```

Uses intelligent NPC models.

## Model Definition

Models define visual appearance:

```json
{
  "Parent": "Goblin",
  "Model": "NPC/Intelligent/Goblin/Models/Model.blockymodel",
  "Texture": "NPC/Intelligent/Goblin/Models/Model_Textures/Moldy.png",
  "DefaultAttachments": [
    {
      "Model": "...",
      "Texture": "..."
    }
  ]
}
```

## Role Appearance Attributes

From the official NPC documentation:

### Display Name Configuration

```json
{
  "DisplayNames": ["Guard", "Soldier", "Watchman"],
  "NameTranslationKey": "npc.guard.name"
}
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `DisplayNames` | StringList | null | Possible display names (random selection) |
| `NameTranslationKey` | String | Required | Translation key for NPC name |

---

### Visual Effects

```json
{
  "SpawnParticles": "NPC_Spawn_Smoke",
  "SpawnParticlesOffset": [0, 0.5, 0],
  "SpawnViewDistance": 75,
  "DeathAnimationTime": 5.0,
  "DespawnAnimationTime": 0.8
}
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `SpawnParticles` | String | null | Particle system when spawning |
| `SpawnParticlesOffset` | Double[] | null | Offset from foot point relative to heading |
| `SpawnViewDistance` | Double | 75.0 | View distance for spawn particles |
| `DeathAnimationTime` | Double | 5.0 | How long death animation plays |
| `DespawnAnimationTime` | Double | 0.8 | How long despawn animation plays |

---

### Head Rotation

```json
{
  "OverrideHeadPitchAngle": true,
  "HeadPitchAngleRange": [-89, 89]
}
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `OverrideHeadPitchAngle` | Boolean | true | Override model camera settings |
| `HeadPitchAngleRange` | Double[] | [-89, 89] | Head rotation pitch range |

---

### Memories System

```json
{
  "IsMemory": true,
  "MemoriesCategory": "Creatures",
  "MemoriesNameOverride": "Rare Dragon"
}
```

| Attribute | Type | Default | Description |
|-----------|------|---------|-------------|
| `IsMemory` | Boolean | false | Record NPC in memories |
| `MemoriesCategory` | String | "Other" | Category for memories plugin |
| `MemoriesNameOverride` | String | "" | Override memory display name |

---

## Appearance Actions

### Change Appearance at Runtime

```json
{
  "Type": "Appearance",
  "Appearance": "Goblin_Armored"
}
```

Changes NPC model during gameplay.

---

### Model Attachments

```json
{
  "Type": "ModelAttachment",
  "Slot": "helmet",
  "Attachment": "Iron_Helmet"
}
```

| Attribute | Type | Description |
|-----------|------|-------------|
| `Slot` | String | Attachment slot name |
| `Attachment` | String | Attachment to set (empty to remove) |

---

### Display Name Action

```json
{
  "Type": "DisplayName",
  "DisplayName": "Elite Guard"
}
```

Changes the name displayed above NPC.

---

## Tips for NPC Appearance

1. **Model reference** - `Appearance` must match model ID in `Server/Models/`
2. **Player models** - Can use player-based models for humanoid NPCs
3. **Parameters** - Parameterize appearance in templates with `{ "Compute": "Appearance" }`
4. **Attachments** - Models can include default attachments (gear, cosmetics)
5. **Display names** - Use array for random name selection
6. **Translation keys** - Use for localization support
7. **Spawn effects** - Add particles for dramatic entrances
8. **Death timing** - Adjust `DeathAnimationTime` to match death animation length
9. **Head tracking** - Configure `HeadPitchAngleRange` for natural head movement
10. **Memories** - Enable `IsMemory` for collectible/notable NPCs

---

## Appearance Quick Reference

| Property | Location | Purpose |
|----------|----------|---------|
| `Appearance` | Role | Model ID reference |
| `DisplayNames` | Role | Random name pool |
| `NameTranslationKey` | Role | Localized name key |
| `SpawnParticles` | Role | Spawn visual effect |
| `DeathAnimationTime` | Role | Death animation duration |
| `Model` | Model JSON | 3D model file path |
| `Texture` | Model JSON | Texture file path |
| `HitBox` | Model JSON | Collision bounds |
| `MinScale`/`MaxScale` | Model JSON | Scale variation |

---

**Previous:** [NPC Components](84_NPC_Components.md) | **Next:** [Salvage Recipes](86_Salvage_Recipes.md)
