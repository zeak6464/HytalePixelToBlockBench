# Creating Totems

Learn how to create totems - deployable items that create area-of-effect (AoE) entities providing buffs, debuffs, or other effects.

## Overview

Totems are thrown items that deploy AoE entities at their landing location. They provide continuous effects to entities within their radius, such as:
- **Healing totems** - Restore health over time
- **Slowness totems** - Slow down enemies
- **Damage totems** - Deal damage over time
- **Buff totems** - Provide stat bonuses

## Location
- Totem items: `Server/Item/Items/Weapon/Deployable/`
- Projectile configs: `Server/ProjectileConfigs/Weapons/Deployables/`
- Entity effects: `Server/Entity/Effects/Deployables/`
- Models: `Server/Models/Deployables/`

---

## Totem Item Structure

### Basic Totem Item

Create `Server/Item/Items/Weapon/Deployable/Weapon_Deployable_MyCustom_Totem.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Deployable_MyCustom_Totem.name"
  },
  "Categories": [
    "Items.Weapons",
    "Items.Utility"
  ],
  "Quality": "Common",
  "ItemLevel": 40,
  "MaxStack": 1,
  "Model": "Items/Deployables/MyCustom_Totem/MyCustom_Totem_Projectile.blockymodel",
  "Texture": "Items/Deployables/MyCustom_Totem/MyCustom_Totem_Texture.png",
  "PlayerAnimationsId": "Item",
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "Serial",
          "Interactions": [
            {
              "Type": "Simple",
              "RunTime": 0.25,
              "Effects": {
                "ItemPlayerAnimationsId": "Item",
                "ItemAnimationId": "Throw"
              }
            },
            {
              "Type": "Projectile",
              "Config": "Projectile_Config_MyCustom_Totem_Deploy"
            }
          ]
        }
      ],
      "Cooldown": {
        "Id": "DeployableUtility",
        "Cooldown": 10
      }
    }
  },
  "Icon": "Icons/ItemsGenerated/MyCustom_Totem.png",
  "Tags": {
    "Type": ["Weapon"]
  }
}
```

### Key Properties

- **`MaxStack: 1`** - Totems typically don't stack
- **`PlayerAnimationsId: "Item"`** - Animation set for holding the totem
- **`ItemAnimationId: "Throw"`** - Animation played when throwing
- **`Cooldown`** - Prevents rapid deployment (10 seconds typical)

---

## Projectile Configuration

### Basic Projectile Config

Create `Server/ProjectileConfigs/Weapons/Deployables/Projectile_Config_MyCustom_Totem_Deploy.json`:

```json
{
  "Model": "MyCustom_Totem_Projectile",
  "Physics": {
    "Type": "Standard",
    "Gravity": 15.0,
    "TerminalVelocityAir": 50,
    "TerminalVelocityWater": 15,
    "SticksVertically": false,
    "Bounciness": 0
  },
  "LaunchForce": 10.0,
  "SpawnOffset": {
    "X": 0.3,
    "Y": -0.5,
    "Z": -0.6
  },
  "SpawnRotationOffset": {
    "Pitch": 10.0,
    "Yaw": 5,
    "Roll": 0
  },
  "Interactions": {
    "ProjectileMiss": {
      "Interactions": [
        {
          "Type": "SpawnDeployableAtHitLocation",
          "Config": {
            "Type": "Aoe",
            "Shape": "Cylinder",
            "Id": "MyCustom_Totem",
            "LiveDuration": 10,
            "Model": "MyCustom_Totem",
            "ModelScale": 1.0,
            "StartRadius": 0.1,
            "EndRadius": 5,
            "Height": 2,
            "RadiusChangeTime": 2,
            "DamageInterval": 0.5,
            "DamageAmount": 0.0,
            "HitboxCollisionConfig": "HardCollision",
            "DeploySoundEventId": "SFX_Deployable_Totem_Spawn",
            "DespawnSoundEventId": "SFX_Deployable_Totem_Despawn",
            "ApplyEffects": [
              "MyCustom_Totem_Effect"
            ],
            "SpawnParticles": [
              {
                "SystemId": "Explosion_Medium",
                "TargetEntityPart": "Entity"
              }
            ],
            "DespawnParticles": [
              {
                "SystemId": "Explosion_Medium",
                "TargetEntityPart": "Entity"
              }
            ]
          }
        },
        {
          "Type": "RemoveEntity",
          "Entity": "User"
        }
      ]
    }
  }
}
```

### Projectile Physics Properties

```json
{
  "Physics": {
    "Type": "Standard",
    "Gravity": 15.0,
    "TerminalVelocityAir": 50,
    "TerminalVelocityWater": 15,
    "SticksVertically": false,
    "Bounciness": 0
  }
}
```

- **`Gravity`** - How fast the projectile falls (15.0 typical)
- **`TerminalVelocityAir`** - Max fall speed in air
- **`TerminalVelocityWater`** - Max fall speed in water
- **`SticksVertically`** - Whether projectile sticks to surfaces (false for totems)
- **`Bounciness`** - Bounce on impact (0 = no bounce, 0.9 = high bounce)

### Launch Properties

```json
{
  "LaunchForce": 10.0,
  "SpawnOffset": {
    "X": 0.3,
    "Y": -0.5,
    "Z": -0.6
  },
  "SpawnRotationOffset": {
    "Pitch": 10.0,
    "Yaw": 5,
    "Roll": 0
  }
}
```

- **`LaunchForce`** - Initial throw velocity (10.0 typical)
- **`SpawnOffset`** - Position offset from player hand
- **`SpawnRotationOffset`** - Initial rotation of projectile

---

## AoE Deployable Configuration

### SpawnDeployableAtHitLocation Config

The `SpawnDeployableAtHitLocation` interaction creates the totem entity when the projectile lands:

```json
{
  "Type": "SpawnDeployableAtHitLocation",
  "Config": {
    "Type": "Aoe",
    "Shape": "Cylinder",
    "Id": "MyCustom_Totem",
    "LiveDuration": 10,
    "Model": "MyCustom_Totem",
    "ModelScale": 1.0,
    "StartRadius": 0.1,
    "EndRadius": 5,
    "Height": 2,
    "RadiusChangeTime": 2,
    "DamageInterval": 0.5,
    "DamageAmount": 0.0,
    "HitboxCollisionConfig": "HardCollision",
    "ApplyEffects": [
      "MyCustom_Totem_Effect"
    ]
  }
}
```

### AoE Properties

#### Type and Shape

```json
{
  "Type": "Aoe",
  "Shape": "Cylinder"
}
```

- **`Type: "Aoe"`** - Area-of-effect deployable
- **`Shape: "Cylinder"`** - Cylindrical AoE area (most common for totems)

#### Duration and Lifetime

```json
{
  "Id": "MyCustom_Totem",
  "LiveDuration": 10
}
```

- **`Id`** - Unique identifier for the totem
- **`LiveDuration`** - How long the totem exists (seconds)

#### Model Configuration

```json
{
  "Model": "MyCustom_Totem",
  "ModelScale": 1.0
}
```

- **`Model`** - Model ID from `Server/Models/Deployables/`
- **`ModelScale`** - Scale multiplier for the model (1.0 = normal size)

#### Radius Configuration

```json
{
  "StartRadius": 0.1,
  "EndRadius": 5,
  "Height": 2,
  "RadiusChangeTime": 2
}
```

- **`StartRadius`** - Initial AoE radius when deployed (blocks)
- **`EndRadius`** - Final AoE radius (blocks)
- **`Height`** - Height of the AoE cylinder (blocks)
- **`RadiusChangeTime`** - Time to expand from StartRadius to EndRadius (seconds)

The radius expands from `StartRadius` to `EndRadius` over `RadiusChangeTime` seconds.

#### Damage Configuration

```json
{
  "DamageInterval": 0.5,
  "DamageAmount": 0.0
}
```

- **`DamageInterval`** - How often damage/effects are applied (seconds)
- **`DamageAmount`** - Damage per interval (0.0 for healing/buff totems)

For healing totems, set `DamageAmount: 0.0` and use effects instead.

#### Collision Configuration

```json
{
  "HitboxCollisionConfig": "HardCollision"
}
```

- **`HardCollision`** - Entities collide with the totem (blocks movement)
- **`SoftCollision`** - Entities can pass through (no collision)

#### Effects

```json
{
  "ApplyEffects": [
    "MyCustom_Totem_Effect"
  ]
}
```

Array of entity effect IDs applied to entities within the AoE every `DamageInterval` seconds.

#### Audio

```json
{
  "DeploySoundEventId": "SFX_Deployable_Totem_Spawn",
  "DespawnSoundEventId": "SFX_Deployable_Totem_Despawn"
}
```

- **`DeploySoundEventId`** - Sound played when totem spawns
- **`DespawnSoundEventId`** - Sound played when totem despawns

#### Particles

```json
{
  "SpawnParticles": [
    {
      "SystemId": "Explosion_Medium",
      "TargetEntityPart": "Entity",
      "Scale": 1.0
    }
  ],
  "DespawnParticles": [
    {
      "SystemId": "Explosion_Medium",
      "TargetEntityPart": "Entity"
    }
  ]
}
```

- **`SpawnParticles`** - Particle systems when totem spawns
- **`DespawnParticles`** - Particle systems when totem despawns
- **`TargetEntityPart`** - Where particles attach ("Entity" = totem center)
- **`Scale`** - Particle system scale multiplier

---

## Entity Effects

### Healing Totem Effect

Create `Server/Entity/Effects/Deployables/Healing_Totem_Heal.json`:

```json
{
  "ApplicationEffects": {
    "EntityTopTint": "#02de45",
    "ScreenEffect": "ScreenEffects/Immune.png",
    "LocalSoundEventId": "SFX_Deployable_Totem_Heal_Effect_Local"
  },
  "Duration": 1.0,
  "OverlapBehavior": "Overwrite",
  "StatModifiers": {
    "Health": 2
  },
  "Debuff": false,
  "StatusEffectIcon": "UI/StatusEffects/AddHealth/Tiny.png"
}
```

### Slowness Totem Effect

Create `Server/Entity/Effects/Deployables/Slowness_Totem_Slow.json`:

```json
{
  "ApplicationEffects": {
    "EntityTopTint": "#00beff",
    "EntityBottomTint": "#00ffff",
    "HorizontalSpeedMultiplier": 0.5,
    "LocalSoundEventId": "SFX_Deployable_Totem_Slowing_Effect_Local"
  },
  "Duration": 1.0,
  "OverlapBehavior": "Overwrite",
  "Debuff": true,
  "StatusEffectIcon": "UI/StatusEffects/Icon_Slow.png"
}
```

### Effect Properties

#### Application Effects

```json
{
  "ApplicationEffects": {
    "EntityTopTint": "#02de45",
    "EntityBottomTint": "#00ffff",
    "HorizontalSpeedMultiplier": 0.5,
    "ScreenEffect": "ScreenEffects/Immune.png",
    "LocalSoundEventId": "SFX_Effect_Local"
  }
}
```

- **`EntityTopTint`** - Color tint for top of entity (hex color)
- **`EntityBottomTint`** - Color tint for bottom of entity (hex color)
- **`HorizontalSpeedMultiplier`** - Movement speed multiplier (0.5 = 50% speed)
- **`ScreenEffect`** - Screen overlay effect image
- **`LocalSoundEventId`** - Sound played when effect is applied

#### Stat Modifiers

```json
{
  "StatModifiers": {
    "Health": 2,
    "Mana": 5
  }
}
```

Modifies entity stats per interval:
- **`Health`** - Health restored per interval (positive = healing)
- **`Mana`** - Mana restored per interval
- Other stat types as needed

#### Duration and Overlap

```json
{
  "Duration": 1.0,
  "OverlapBehavior": "Overwrite"
}
```

- **`Duration`** - How long the effect lasts (seconds)
- **`OverlapBehavior`** - How overlapping effects are handled:
  - **`"Overwrite"`** - New effect replaces old one
  - **`"Stack"`** - Effects stack
  - **`"Ignore"`** - New effect ignored if old one exists

#### Debuff Flag

```json
{
  "Debuff": false
}
```

- **`false`** - Beneficial effect (healing, buffs)
- **`true`** - Harmful effect (slowness, damage)

#### Status Effect Icon

```json
{
  "StatusEffectIcon": "UI/StatusEffects/AddHealth/Tiny.png"
}
```

Icon displayed in the UI when the effect is active.

---

## Totem Model Configuration

### Basic Totem Model

Create `Server/Models/Deployables/MyCustom_Totem.json`:

```json
{
  "Model": "Items/Deployables/MyCustom_Totem/MyCustom_Totem.blockymodel",
  "Texture": "Items/Deployables/MyCustom_Totem/MyCustom_Totem_Texture.png",
  "EyeHeight": 0.9,
  "CrouchOffset": 0.0,
  "MinScale": 1.5,
  "MaxScale": 1.5,
  "HitBox": {
    "Max": {
      "X": 0.05,
      "Y": 1.5,
      "Z": 0.05
    },
    "Min": {
      "X": -0.05,
      "Y": 0,
      "Z": -0.05
    }
  },
  "Trails": [
    {
      "TargetNodeName": "GA1",
      "TrailId": "Wind"
    }
  ],
  "Particles": [
    {
      "SystemId": "Totem_Effect_Attach",
      "TargetEntityPart": "Entity",
      "Scale": 1.0
    }
  ]
}
```

### Model Properties

- **`EyeHeight`** - Eye level height (for targeting)
- **`MinScale`/`MaxScale`** - Scale range (same value = fixed size)
- **`HitBox`** - Collision box for the totem
- **`Trails`** - Trail effects attached to model nodes
- **`Particles`** - Particle systems attached to the model

---

## Complete Example: Healing Totem

### Item

`Server/Item/Items/Weapon/Deployable/Weapon_Deployable_Healing_Totem.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Deployable_Healing_Totem.name"
  },
  "Categories": [
    "Items.Weapons",
    "Items.Utility"
  ],
  "Quality": "Common",
  "ItemLevel": 40,
  "MaxStack": 1,
  "Model": "Items/Deployables/Healing_Totem/Healing_Totem_Projectile.blockymodel",
  "Texture": "Items/Deployables/Healing_Totem/Healing_Totem_Texture.png",
  "PlayerAnimationsId": "Item",
  "Interactions": {
    "Primary": {
      "Interactions": [
        {
          "Type": "Serial",
          "Interactions": [
            {
              "Type": "Simple",
              "RunTime": 0.25,
              "Effects": {
                "ItemPlayerAnimationsId": "Item",
                "ItemAnimationId": "Throw"
              }
            },
            {
              "Type": "Projectile",
              "Config": "Projectile_Config_Healing_Totem_Deploy"
            }
          ]
        }
      ],
      "Cooldown": {
        "Id": "DeployableUtility",
        "Cooldown": 10
      }
    }
  },
  "Icon": "Icons/ItemsGenerated/Healing_Totem.png",
  "Tags": {
    "Type": ["Weapon"]
  }
}
```

### Projectile Config

`Server/ProjectileConfigs/Weapons/Deployables/Projectile_Config_Healing_Totem_Deploy.json`:

```json
{
  "Model": "Healing_Totem_Projectile",
  "Physics": {
    "Type": "Standard",
    "Gravity": 15.0,
    "TerminalVelocityAir": 50,
    "TerminalVelocityWater": 15,
    "SticksVertically": false,
    "Bounciness": 0
  },
  "LaunchForce": 10.0,
  "SpawnOffset": {
    "X": 0.3,
    "Y": -0.5,
    "Z": -0.6
  },
  "SpawnRotationOffset": {
    "Pitch": 10.0,
    "Yaw": 5,
    "Roll": 0
  },
  "Interactions": {
    "ProjectileMiss": {
      "Interactions": [
        {
          "Type": "SpawnDeployableAtHitLocation",
          "Config": {
            "Type": "Aoe",
            "Shape": "Cylinder",
            "Id": "Healing_Totem",
            "LiveDuration": 10,
            "Model": "Healing_Totem",
            "ModelScale": 1.0,
            "StartRadius": 0.1,
            "EndRadius": 5,
            "Height": 2,
            "RadiusChangeTime": 2,
            "DamageInterval": 0.5,
            "DamageAmount": 0.0,
            "HitboxCollisionConfig": "HardCollision",
            "DeploySoundEventId": "SFX_Deployable_Totem_Heal_Spawn",
            "DespawnSoundEventId": "SFX_Deployable_Totem_Heal_Despawn",
            "ApplyEffects": [
              "Healing_Totem_Heal"
            ],
            "SpawnParticles": [
              {
                "SystemId": "Explosion_Medium",
                "TargetEntityPart": "Entity"
              },
              {
                "SystemId": "Totem_Heal_Simple_Test",
                "TargetEntityPart": "Entity",
                "Scale": 0.6
              }
            ],
            "DespawnParticles": [
              {
                "SystemId": "Explosion_Medium",
                "TargetEntityPart": "Entity"
              }
            ]
          }
        },
        {
          "Type": "RemoveEntity",
          "Entity": "User"
        }
      ]
    }
  }
}
```

### Effect

`Server/Entity/Effects/Deployables/Healing_Totem_Heal.json`:

```json
{
  "ApplicationEffects": {
    "EntityTopTint": "#02de45",
    "ScreenEffect": "ScreenEffects/Immune.png",
    "LocalSoundEventId": "SFX_Deployable_Totem_Heal_Effect_Local"
  },
  "Duration": 1.0,
  "OverlapBehavior": "Overwrite",
  "StatModifiers": {
    "Health": 2
  },
  "Debuff": false,
  "StatusEffectIcon": "UI/StatusEffects/AddHealth/Tiny.png"
}
```

This healing totem:
- Deploys for 10 seconds
- Expands from 0.1 to 5 block radius over 2 seconds
- Heals 2 health every 0.5 seconds
- Applies green tint and healing sound
- Shows healing particles

---

## Complete Example: Slowness Totem

### Projectile Config

`Server/ProjectileConfigs/Weapons/Deployables/Projectile_Config_Slowness_Totem_Deploy.json`:

```json
{
  "Model": "Slowness_Totem",
  "Physics": {
    "Type": "Standard",
    "Gravity": 15.0,
    "TerminalVelocityAir": 50,
    "TerminalVelocityWater": 15,
    "RotationMode": "None",
    "Bounciness": 0.9,
    "SticksVertically": false,
    "BounceCount": 0,
    "BounceLimit": 0.4
  },
  "LaunchForce": 10.0,
  "Interactions": {
    "ProjectileMiss": {
      "Interactions": [
        {
          "Type": "SpawnDeployableAtHitLocation",
          "Config": {
            "Type": "Aoe",
            "Id": "Slowness_Totem",
            "LiveDuration": 10,
            "Model": "Slowness_Totem",
            "ModelScale": 1.0,
            "StartRadius": 0.1,
            "EndRadius": 5,
            "Height": 2,
            "RadiusChangeTime": 2,
            "DeploySoundEventId": "SFX_Deployable_Totem_Slowing_Spawn",
            "DespawnSoundEventId": "SFX_Deployable_Totem_Slowing_Despawn",
            "ApplyEffects": [
              "Slowness_Totem_Slow"
            ],
            "DamageInterval": 0.5,
            "DamageAmount": 0.0,
            "HitboxCollisionConfig": "HardCollision",
            "SpawnParticles": [
              {
                "SystemId": "Explosion_Medium",
                "TargetEntityPart": "Entity"
              },
              {
                "SystemId": "Totem_Slow_Simple_Test",
                "TargetEntityPart": "Entity",
                "Scale": 0.6
              }
            ],
            "DespawnParticles": [
              {
                "SystemId": "Explosion_Medium",
                "TargetEntityPart": "Entity"
              }
            ]
          }
        },
        {
          "Type": "RemoveEntity",
          "Entity": "User"
        }
      ]
    }
  }
}
```

### Effect

`Server/Entity/Effects/Deployables/Slowness_Totem_Slow.json`:

```json
{
  "ApplicationEffects": {
    "EntityTopTint": "#00beff",
    "EntityBottomTint": "#00ffff",
    "HorizontalSpeedMultiplier": 0.5,
    "LocalSoundEventId": "SFX_Deployable_Totem_Slowing_Effect_Local"
  },
  "Duration": 1.0,
  "OverlapBehavior": "Overwrite",
  "Debuff": true,
  "StatusEffectIcon": "UI/StatusEffects/Icon_Slow.png"
}
```

This slowness totem:
- Deploys for 10 seconds
- Expands from 0.1 to 5 block radius over 2 seconds
- Slows entities to 50% speed every 0.5 seconds
- Applies blue tint and slowness sound
- Shows slowing particles

---

## Tips for Creating Totems

1. **Radius expansion** - Use `StartRadius` and `RadiusChangeTime` for smooth expansion effect
2. **Effect intervals** - Balance `DamageInterval` (0.3-1.0 seconds typical)
3. **Duration balance** - `LiveDuration` should match totem power (5-15 seconds typical)
4. **Visual feedback** - Use particles and entity tints to show effect type
5. **Sound design** - Distinct spawn/despawn sounds help players identify totems
6. **Collision** - Use `HardCollision` for blocking totems, `SoftCollision` for pass-through
7. **Effect duration** - Set effect `Duration` slightly longer than `DamageInterval` for smooth application
8. **Debuff flag** - Correctly set `Debuff: true/false` for proper UI display
9. **Model scale** - Adjust `ModelScale` to match visual size expectations
10. **Cooldown balance** - Set item cooldown to prevent spam (10 seconds typical)

---

## Advanced: Damage Totem

For a damage totem, set `DamageAmount` instead of using healing effects:

```json
{
  "DamageInterval": 0.5,
  "DamageAmount": 5.0,
  "ApplyEffects": [
    "Damage_Totem_Burn"
  ]
}
```

The `DamageAmount` is applied every `DamageInterval` seconds to entities within the AoE.

---

**Previous:** [Creating Pets](102_Pets.md) | **Next:** [Damage Calculator](104_Damage_Calculator.md)
