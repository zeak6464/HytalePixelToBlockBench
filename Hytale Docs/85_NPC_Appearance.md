# NPC Appearance

Learn how NPC appearance works and how models are assigned to NPCs.

## Overview

NPC appearance is defined by models in `Server/Models/`. The `Appearance` property in NPC Roles references model IDs. NPCs can use player models or custom creature models.

## Location
- NPC appearance assignment: `Appearance` property in NPC Roles
- Model definitions: `Server/Models/`

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

## Tips for NPC Appearance

1. **Model reference** - `Appearance` must match model ID
2. **Player models** - Can use player-based models for humanoid NPCs
3. **Parameters** - Parameterize appearance in templates
4. **Attachments** - Models can include attachments (gear, cosmetics)

---

**Previous:** [NPC Components](84_NPC_Components.md) | **Next:** [Salvage Recipes](86_Salvage_Recipes.md)
