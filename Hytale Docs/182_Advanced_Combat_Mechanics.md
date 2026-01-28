# Advanced Combat Mechanics

Master Hytale's combat system: damage calculations, combos, charged attacks, signature moves, knockback, and advanced combat techniques.

## Overview

Hytale's combat system combines multiple mechanics:
- **Damage Types** - Physical, Fire, Ice, Poison, etc.
- **Combo Systems** - Chained attacks with varying damage
- **Charged Attacks** - Hold to power up
- **Signature Moves** - Ultimate abilities
- **Knockback Physics** - Force, resistance, velocity
- **Stat Integration** - Stamina, energy, health
- **Status Effects** - Buffs, debuffs in combat

All examples from actual game files.

---

## Damage System

### Basic Damage Calculator

From `Weapon_Sword_Cobalt.json`:

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 11
    }
  }
}
```

**Deals 11 physical damage per hit.**

---

### Multiple Damage Types

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 10,
      "Fire": 5
    }
  }
}
```

**Deals:**
- 10 physical damage
- 5 fire damage
- **Total: 15 damage** (split between types)

**Use:** Elemental weapons, enchanted items

---

### Damage Variance

```json
{
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 20
    },
    "RandomPercentageModifier": 0.15
  }
}
```

**Damage range:**
- Minimum: 17 damage (85% of 20)
- Maximum: 23 damage (115% of 20)

**±15% variance** - Makes combat less predictable

---

## Combo System

### Sword Combo Chain

From `Weapon_Sword_Cobalt.json`:

```json
{
  "InteractionVars": {
    "Swing_Left_Damage": {
      "Interactions": [{
        "DamageCalculator": {
          "BaseDamage": {
            "Physical": 11
          }
        }
      }]
    },
    "Swing_Right_Damage": {
      "Interactions": [{
        "DamageCalculator": {
          "BaseDamage": {
            "Physical": 14
          }
        }
      }]
    },
    "Swing_Down_Damage": {
      "Interactions": [{
        "DamageCalculator": {
          "BaseDamage": {
            "Physical": 21
          }
        }
      }]
    },
    "Thrust_Damage": {
      "Interactions": [{
        "DamageCalculator": {
          "BaseDamage": {
            "Physical": 32
          }
        },
        "EntityStatsOnHit": [
          {
            "EntityStatId": "SignatureEnergy",
            "Amount": 3
          }
        ]
      }]
    }
  }
}
```

**Combo progression:**
- **Hit 1** (Swing Left): 11 damage
- **Hit 2** (Swing Right): 14 damage
- **Hit 3** (Swing Down): 21 damage
- **Hit 4** (Thrust): 32 damage + 3 signature energy

**Total combo damage: 78 damage**

**Pattern:** Increasing damage rewards completing combo

---

### Mace Combo (Heavy Hits)

From `Weapon_Mace_Thorium.json`:

```json
{
  "Swing_Left_Damage": {
    "DamageCalculator": {
      "BaseDamage": {
        "Physical": 36
      }
    }
  },
  "Swing_Right_Damage": {
    "DamageCalculator": {
      "BaseDamage": {
        "Physical": 36
      }
    }
  },
  "Swing_Up_Left_Damage": {
    "DamageCalculator": {
      "BaseDamage": {
        "Physical": 36
      }
    }
  }
}
```

**All hits deal same damage (36)** - Maces are consistent, heavy hitters

---

## Charged Attacks

### Mace Charged Hit

From `Weapon_Mace_Thorium.json`:

```json
{
  "Swing_Left_Charged_Damage": {
    "Interactions": [{
      "DamageCalculator": {
        "BaseDamage": {
          "Physical": 52
        }
      },
      "EntityStatsOnHit": [
        {
          "EntityStatId": "SignatureEnergy",
          "Amount": 2
        }
      ]
    }]
  }
}
```

**Comparison:**
- **Normal hit**: 36 damage
- **Charged hit**: 52 damage (+44% damage)
- **Bonus**: +2 signature energy

**Hold attack button to charge** for increased damage

---

### Charging Pattern

```json
{
  "Primary": {
    "Interactions": [
      {
        "Type": "Charging",
        "ChargeTime": 1.5,
        "HoldToContinue": true,
        "Charged": {
          "Type": "DamageEntity",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 50
            }
          }
        },
        "NotCharged": {
          "Type": "DamageEntity",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 25
            }
          }
        }
      }
    ]
  }
}
```

**Logic:**
- Hold 1.5 seconds: 50 damage (charged)
- Release early: 25 damage (normal)

---

## Signature Moves (Ultimate Abilities)

### Sword Vortexstrike

From `Weapon_Sword_Cobalt.json`:

```json
{
  "Vortexstrike_Spin_Damage": {
    "Interactions": [{
      "DamageCalculator": {
        "BaseDamage": {
          "Physical": 23
        }
      }
    }]
  },
  "Vortexstrike_Stab_Damage": {
    "Interactions": [{
      "DamageCalculator": {
        "BaseDamage": {
          "Physical": 70
        }
      }
    }]
  }
}
```

**Two-part signature move:**
1. **Spin attack**: 23 damage (AoE)
2. **Finishing stab**: 70 damage (single target)

**Total signature damage: 93 damage**

---

### Mace Groundslam

From `Weapon_Mace_Thorium.json`:

```json
{
  "Groundslam_Damage": {
    "Interactions": [{
      "DamageCalculator": {
        "BaseDamage": {
          "Physical": 116
        }
      }
    }]
  }
}
```

**Massive 116 damage** - Highest single-hit damage

---

### Signature Energy System

Build up energy through combat:

```json
{
  "EntityStatsOnHit": [
    {
      "EntityStatId": "SignatureEnergy",
      "Amount": 3
    }
  ]
}
```

**Each thrust attack** grants 3 signature energy  
**When full** (typically 100 energy) → Can use signature move

**Pattern:**
1. Land regular hits (build energy)
2. Energy reaches threshold
3. Signature move becomes available
4. Use signature (powerful attack)
5. Energy resets

---

## Knockback System

### Basic Knockback

From `Explode_Generic.json`:

```json
{
  "Knockback": {
    "Type": "Point",
    "Force": 5,
    "VelocityType": "Set",
    "VelocityConfig": {
      "Direction": {
        "X": 0.0,
        "Y": 5.0,
        "Z": 0.0
      },
      "AirResistance": 0.97,
      "GroundResistance": 0.94,
      "Threshold": 3.0
    }
  }
}
```

**Properties:**

| Property | Value | Effect |
|----------|-------|--------|
| **Force** | 5 | Knockback strength |
| **Direction.Y** | 5.0 | Upward bias |
| **AirResistance** | 0.97 | Slow down in air (3% per frame) |
| **GroundResistance** | 0.94 | Slow down on ground (6% per frame) |
| **Threshold** | 3.0 | Min velocity before stopping |

**Result:** Entities fly up and outward, gradually slow down

---

### Knockback Types

#### Point Knockback

```json
{
  "Knockback": {
    "Type": "Point",
    "Force": 3
  }
}
```

**Effect:** Pushes away from hit point (radial)

---

#### Directional Knockback

```json
{
  "Knockback": {
    "Type": "Directional",
    "Force": 4,
    "Direction": {
      "X": 0,
      "Y": 2,
      "Z": 1
    }
  }
}
```

**Effect:** Fixed direction (useful for abilities)

---

### Velocity Types

```json
{
  "VelocityType": "Set"
}
```

**Types:**
- **Set** - Override current velocity (explosion)
- **Add** - Add to current velocity (cumulative)

---

## Damage Effects

### Visual & Audio Feedback

```json
{
  "DamageEffects": {
    "WorldSoundEventId": "SFX_Sword_T2_Impact",
    "LocalSoundEventId": "SFX_Sword_T2_Impact",
    "Knockback": {
      "Force": 0.5
    },
    "WorldParticles": [
      {
        "SystemId": "Impact_Sword_Blood"
      }
    ]
  }
}
```

**On damage:**
- Play impact sound (world + local)
- Apply knockback (force 0.5)
- Spawn blood particles

---

### Damage Types

All available damage types from `Server/Entity/Damage/`:

| Damage Type | File | Properties |
|-------------|------|------------|
| **Physical** | `Physical.json` | Standard melee/ranged |
| **Bludgeoning** | `Bludgeoning.json` | Maces, clubs |
| **Slashing** | `Slashing.json` | Swords, axes |
| **Fire** | `Fire.json` | Fire damage over time |
| **Ice** | `Ice.json` | Slowing effect |
| **Poison** | `Poison.json` | DoT poison |
| **Elemental** | `Elemental.json` | Generic magic |
| **Projectile** | `Projectile.json` | Arrow, bullet damage |
| **Fall** | `Fall.json` | Fall damage |
| **Drowning** | `Drowning.json` | Underwater suffocation |
| **Environmental** | `Environmental.json` | Lava, traps |

---

### Physical Damage Type

From `Physical.json`:

```json
{
  "$Comment": "This damage type exists to facilitate sub types",
  "DurabilityLoss": true,
  "StaminaLoss": true
}
```

**Properties:**
- **DurabilityLoss** - Damages armor/shields
- **StaminaLoss** - Drains stamina when hit

**All physical damage** drains durability and stamina

---

## Combat Resource Management

### Stamina System

#### Stamina Cost

```json
{
  "Guard_Wield": {
    "Interactions": [{
      "StaminaCost": {
        "Value": 12.22,
        "CostType": "Damage"
      }
    }]
  }
}
```

**Blocking costs stamina** per damage blocked

---

#### Stamina Drain on Attack

```json
{
  "Type": "ChangeStat",
  "StatModifiers": {
    "Stamina": -5
  }
}
```

**Each attack costs 5 stamina**

---

#### Stamina Regen Delay

```json
{
  "Type": "ChangeStat",
  "Behaviour": "Set",
  "StatModifiers": {
    "StaminaRegenDelay": -1.5
  }
}
```

**Delays stamina regen by 1.5 seconds after attacking**

---

## Explosion Damage

### Complete Explosion

From `Explode_Generic.json`:

```json
{
  "Type": "Explode",
  "Config": {
    "DamageEntities": true,
    "DamageBlocks": true,
    "BlockDamageRadius": 3,
    "BlockDropChance": 0.4,
    "EntityDamage": 20.0,
    "EntityDamageRadius": 4,
    "Knockback": {
      "Type": "Point",
      "Force": 5,
      "VelocityConfig": {
        "AirResistance": 0.97,
        "Direction": {
          "Y": 5.0
        }
      }
    },
    "ItemTool": {
      "Specs": [
        {
          "Power": 2,
          "GatherType": "SoftBlocks"
        },
        {
          "Power": 2,
          "GatherType": "Soils"
        },
        {
          "Power": 2,
          "GatherType": "Woods"
        },
        {
          "Power": 2,
          "GatherType": "Rocks"
        }
      ]
    }
  }
}
```

**Effects:**
- **Entity damage**: 20 damage, 4-block radius
- **Block damage**: Power 2 mining, 3-block radius, 40% drop chance
- **Knockback**: Force 5, launches upward
- **Visual**: Explosion particles, sound, camera shake

---

## Weapon Damage Comparison

### Damage by Weapon Type

| Weapon | Normal Hit | Combo Finisher | Charged | Signature | Pattern |
|--------|-----------|----------------|---------|-----------|---------|
| **Sword** | 11-14 | 32 (thrust) | N/A | 70 (stab) | Fast combos |
| **Mace** | 36 | 36 | 52 | 116 (slam) | Slow, heavy |
| **Dagger** | 8-10 | 15 | N/A | 40 (dash) | Rapid attacks |
| **Spear** | 15-18 | 25 | N/A | 50 (throw) | Medium range |

---

### Damage Scaling by Tier

**Example: Sword progression**

```json
// Crude Sword (Tier 1)
{
  "BaseDamage": {
    "Physical": 5
  }
}

// Bronze Sword (Tier 2)
{
  "BaseDamage": {
    "Physical": 8
  }
}

// Iron Sword (Tier 3)
{
  "BaseDamage": {
    "Physical": 11
  }
}

// Cobalt Sword (Tier 4)
{
  "BaseDamage": {
    "Physical": 14
  }
}
```

**~30-40% damage increase per tier**

---

## Combat Stats Integration

### EntityStatsOnHit

```json
{
  "EntityStatsOnHit": [
    {
      "EntityStatId": "SignatureEnergy",
      "Amount": 3
    }
  ]
}
```

**Grants 3 signature energy on hit**

---

### Multiple Stat Changes

```json
{
  "EntityStatsOnHit": [
    {
      "EntityStatId": "SignatureEnergy",
      "Amount": 2
    },
    {
      "EntityStatId": "Mana",
      "Amount": 5
    },
    {
      "EntityStatId": "Health",
      "Amount": -2
    }
  ]
}
```

**On hit:**
- +2 signature energy
- +5 mana (life steal?)
- -2 health (blood magic?)

---

## Guard/Block System

### Blocking with Stamina Cost

From `Weapon_Sword_Cobalt.json`:

```json
{
  "Guard_Wield": {
    "Interactions": [{
      "Parent": "Weapon_Sword_Secondary_Guard_Wield",
      "StaminaCost": {
        "Value": 12.22,
        "CostType": "Damage"
      }
    }]
  }
}
```

**Blocking:**
- Hold secondary button to block
- Costs 12.22 stamina per damage blocked
- When stamina runs out, guard breaks

---

### Perfect Block Window

```json
{
  "Type": "Wielding",
  "HoldToContinue": true,
  "WieldTime": 0.3,
  "Wielding": {
    "Type": "ApplyEffect",
    "EffectId": "Perfect_Block_Window"
  },
  "PostWielding": {
    "Type": "ApplyEffect",
    "EffectId": "Block_Active"
  }
}
```

**Mechanic:**
- First 0.3 seconds: Perfect block (full damage reduction)
- After: Normal block (partial reduction, stamina cost)

---

## Critical Hits

### Critical Chance System

```json
{
  "Type": "Condition",
  "Probability": 0.15,
  "Next": {
    "Type": "DamageEntity",
    "DamageCalculator": {
      "BaseDamage": {
        "Physical": 40
      }
    },
    "DamageEffects": {
      "WorldParticles": [{
        "SystemId": "Critical_Hit_Sparkle"
      }]
    }
  },
  "Failed": {
    "Type": "DamageEntity",
    "DamageCalculator": {
      "BaseDamage": {
        "Physical": 20
      }
    }
  }
}
```

**15% chance:**
- **Critical**: 40 damage + sparkle particles
- **Normal**: 20 damage

---

## Status Effects in Combat

### Poison on Hit

```json
{
  "Type": "DamageEntity",
  "DamageCalculator": {
    "BaseDamage": {
      "Poison": 5
    }
  },
  "Next": {
    "Type": "ApplyEffect",
    "EffectId": "Poison_Damage_Over_Time",
    "Duration": 10
  }
}
```

**Effect:**
- 5 instant poison damage
- 10-second poison DoT effect

---

### Fire DoT

```json
{
  "Type": "ApplyEffect",
  "EffectId": "Burning",
  "Duration": 5,
  "Effect": {
    "DamageCalculator": {
      "Type": "Dps",
      "BaseDamage": {
        "Fire": 3
      }
    }
  }
}
```

**3 fire damage per second for 5 seconds** = 15 total damage

---

## AoE Combat

### Area Damage

```json
{
  "Type": "DamageEntity",
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 30
    }
  },
  "Selector": {
    "Type": "Area",
    "Range": 5,
    "EntityTypes": ["NPC"],
    "MaxCount": 10
  }
}
```

**Hits up to 10 NPCs within 5 blocks** for 30 damage each

---

### Cone Attack

```json
{
  "Selector": {
    "Type": "Cone",
    "Range": 8,
    "Angle": 60,
    "EntityTypes": ["NPC", "Player"]
  }
}
```

**Frontal cone** (60° angle, 8 blocks range)

---

## Advanced Combat Patterns

### Pattern 1: Elemental Combo

```json
{
  "InteractionVars": {
    "Hit1": {
      "Interactions": [{
        "DamageCalculator": {
          "BaseDamage": {
            "Physical": 10
          }
        }
      }]
    },
    "Hit2": {
      "Interactions": [{
        "DamageCalculator": {
          "BaseDamage": {
            "Physical": 10,
            "Fire": 5
          }
        }
      }]
    },
    "Hit3": {
      "Interactions": [{
        "DamageCalculator": {
          "BaseDamage": {
            "Fire": 20
          }
        },
        "Next": {
          "Type": "ApplyEffect",
          "EffectId": "Burning"
        }
      }]
    }
  }
}
```

**Progression:**
1. Physical (10)
2. Physical + Fire (10 + 5)
3. Fire + Burning effect (20 + DoT)

**Rewards completing combo** with element application

---

### Pattern 2: Resource → Damage

```json
{
  "Type": "StatsCondition",
  "Stats": {
    "Mana": 20
  },
  "Next": {
    "Type": "Parallel",
    "Interactions": [
      {
        "Interactions": [{
          "Type": "ChangeStat",
          "StatModifiers": {
            "Mana": -20
          }
        }]
      },
      {
        "Interactions": [{
          "Type": "DamageEntity",
          "DamageCalculator": {
            "BaseDamage": {
              "Physical": 50
            }
          }
        }]
      }
    ]
  },
  "Failed": {
    "Type": "DamageEntity",
    "DamageCalculator": {
      "BaseDamage": {
        "Physical": 15
      }
    }
  }
}
```

**Mana-empowered attack:**
- **With 20 mana**: Consume mana, deal 50 damage
- **Without mana**: Deal 15 damage

---

### Pattern 3: Lifesteal

```json
{
  "Type": "DamageEntity",
  "DamageCalculator": {
    "BaseDamage": {
      "Physical": 20
    }
  },
  "Next": {
    "Type": "ChangeStat",
    "Selector": {
      "Type": "User"
    },
    "StatModifiers": {
      "Health": 5
    }
  }
}
```

**Deal 20 damage, heal self 5 health** (25% lifesteal)

---

### Pattern 4: Execute Mechanic

```json
{
  "Type": "StatsCondition",
  "Selector": {
    "Type": "Target"
  },
  "Stats": {
    "Health": {
      "Max": 20,
      "Percentage": true
    }
  },
  "Next": {
    "Type": "DamageEntity",
    "DamageCalculator": {
      "BaseDamage": {
        "Physical": 999
      }
    }
  },
  "Failed": {
    "Type": "DamageEntity",
    "DamageCalculator": {
      "BaseDamage": {
        "Physical": 30
      }
    }
  }
}
```

**If target below 20% health:**
- **Execute**: 999 damage (instant kill)
- **Else**: 30 damage

---

## Complete Combat System Example

### Elemental Spear

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Spear_Flame.name"
  },
  "Parent": "Template_Weapon_Spear",
  "Model": "Items/Weapons/Spear/Flame.blockymodel",
  "Texture": "Items/Weapons/Spear/Flame_Texture.png",
  "Quality": "Epic",
  "ItemLevel": 40,
  "MaxDurability": 200,
  "InteractionVars": {
    "Stab_Damage": {
      "Interactions": [{
        "DamageCalculator": {
          "BaseDamage": {
            "Physical": 15,
            "Fire": 5
          },
          "RandomPercentageModifier": 0.1
        },
        "EntityStatsOnHit": [
          {
            "EntityStatId": "SignatureEnergy",
            "Amount": 2
          }
        ],
        "Next": {
          "Type": "Condition",
          "Probability": 0.2,
          "Next": {
            "Type": "ApplyEffect",
            "EffectId": "Burning",
            "Duration": 5
          }
        }
      }]
    },
    "Thrust_Damage": {
      "Interactions": [{
        "DamageCalculator": {
          "BaseDamage": {
            "Physical": 25,
            "Fire": 10
          }
        },
        "EntityStatsOnHit": [
          {
            "EntityStatId": "SignatureEnergy",
            "Amount": 4
          }
        ]
      }]
    },
    "Signature_Flame_Lance": {
      "Interactions": [{
        "Type": "StatsCondition",
        "Stats": {
          "SignatureEnergy": 100
        },
        "Next": {
          "Type": "Serial",
          "Interactions": [
            {
              "Type": "ChangeStat",
              "StatModifiers": {
                "SignatureEnergy": -100
              }
            },
            {
              "Type": "LaunchProjectile",
              "Config": "Projectile_Flame_Lance",
              "ProjectileInteractions": {
                "OnHit": {
                  "Type": "Parallel",
                  "Interactions": [
                    {
                      "Interactions": [{
                        "Type": "DamageEntity",
                        "DamageCalculator": {
                          "BaseDamage": {
                            "Fire": 80
                          }
                        },
                        "Selector": {
                          "Type": "Area",
                          "Range": 3
                        }
                      }]
                    },
                    {
                      "Interactions": [{
                        "Type": "ApplyEffect",
                        "EffectId": "Burning",
                        "Duration": 10,
                        "Selector": {
                          "Type": "Area",
                          "Range": 3
                        }
                      }]
                    },
                    {
                      "Interactions": [{
                        "Type": "Explode",
                        "Config": "Explosion_Fire_Medium"
                      }]
                    }
                  ]
                }
              }
            }
          ]
        },
        "Failed": {
          "Type": "SendMessage",
          "Message": "Not enough signature energy!"
        }
      }]
    }
  },
  "Particles": [
    {
      "SystemId": "Flame_Weapon_Trail",
      "TargetNodeName": "Blade"
    }
  ]
}
```

**Combat system:**
- **Stab**: 15+5 damage, ±10% variance, 20% burn chance, +2 energy
- **Thrust**: 25+10 damage, +4 energy
- **Signature** (100 energy): Launch fireball → 80 AoE fire damage + burning + explosion

---

## Best Practices

### ✅ Do

1. **Scale damage by tier** - ~30-40% increase per tier
2. **Vary combo damage** - Reward completing combos
3. **Add signature energy** on hits
4. **Include variance** (±10-15%) for unpredictability
5. **Provide feedback** - Sounds, particles on hit
6. **Balance stamina costs** - Heavy attacks cost more
7. **Use appropriate damage types** - Fire for DoT, Physical for instant
8. **Add knockback** for impact feel
9. **Test combo timing** - Not too fast, not too slow
10. **Document damage values** - Helps with balancing

---

### ❌ Don't

1. **Don't make all hits same damage** - Combos become boring
2. **Don't forget RandomPercentageModifier** - Combat feels robotic
3. **Don't skip EntityStatsOnHit** - Signature moves won't charge
4. **Don't make signature too weak** - Should feel powerful
5. **Don't forget damage resistance** - Some NPCs resist types
6. **Don't skip knockback** - Combat feels flat
7. **Don't use only Physical** - Elemental variety is fun
8. **Don't make stamina costs too high** - Frustrating gameplay

---

## Troubleshooting

### Combo Not Chaining

**Check:**
- Interaction sequence (FirstClick → next hit)
- Animation timing (RunTime matches animation length)
- Cooldowns (not blocking combo)

### Damage Too Low/High

**Check:**
- BaseDamage values
- Tier scaling (compare to similar weapons)
- Enemy armor/resistance
- Damage type (check enemy weaknesses)

### Signature Not Charging

**Check:**
- EntityStatsOnHit defined
- SignatureEnergy stat exists
- Threshold value (default 100)

### Blocking Not Working

**Check:**
- StaminaCost defined
- Player has stamina
- Wielding interaction configured
- Block animation playing

---

## Related Guides

- **[Damage Calculator](104_Damage_Calculator.md)** - Damage calculation reference
- **[Damage Types](25_Damage_Types.md)** - Damage type definitions
- **[Entity Stats](28_Entity_Stats.md)** - Health, mana, stamina
- **[Entity Effects](96_Entity_Effects.md)** - Status effects
- **[Advanced Techniques](176_Advanced_Techniques.md)** - Complex interactions

---

**Previous:** [Procedural World Generation](181_Procedural_World_Generation.md) | **Next:** [Java Server Plugins](183_Java_Server_Plugins.md)
