# Block Flags Reference

Complete reference for all block flags and boolean properties that control block behavior in Hytale.

## Overview

Flags control special block behaviors like stacking, usability, climbing, and more. They're defined in different locations within the `BlockType` configuration.

**Three main locations:**
1. **`BlockType.Flags`** - General behavioral flags
2. **`BlockType.MovementSettings`** - Movement-related properties
3. **`BlockType` (top-level)** - Special block type properties
4. **`BlockType.Gathering.*`** - Gathering-specific flags

---

## BlockType.Flags

General behavioral flags for blocks.

### IsStackable

**Type:** Boolean (`true` or `false`)  
**Default:** `false`  
**Location:** `BlockType.Flags.IsStackable`

Controls whether multiple instances of the same block can be placed on top of each other in the world.

**Note:** This is different from inventory stacking (controlled by `MaxStack`).

#### IsStackable: true - Plates Stack

From `Deco_Plate.json`:

```json
{
  "BlockType": {
    "Flags": {
      "IsStackable": true
    },
    "HitboxType": "Plant_Small"
  }
}
```

**Result:** You can place plates on top of each other, creating a vertical stack.

#### IsStackable: false - Campfire Doesn't Stack

From `Bench_Campfire.json`:

```json
{
  "BlockType": {
    "Flags": {
      "IsStackable": false
    }
  }
}
```

**Result:** Cannot stack campfires vertically - each needs its own floor space.

**Use cases:**
- ✅ **true**: Plates, bowls, coins, thin decorative items
- ❌ **false**: Furniture, benches, large decorations, functional blocks

---

### IsUsable

**Type:** Boolean (`true` or `false`)  
**Default:** `false` (varies by context)  
**Location:** `BlockType.Flags.IsUsable`

Controls whether the block shows an interaction prompt and can be used with the "Use" key (typically E or right-click).

#### IsUsable: true - Chest Can Be Opened

From `Furniture_Village_Chest_Small.json`:

```json
{
  "BlockType": {
    "DrawType": "Model",
    "Flags": {
      "IsUsable": true
    },
    "Interactions": {
      "Use": {
        "Type": "OpenContainer"
      }
    }
  }
}
```

**Result:** Players can press E to open the chest.

#### IsUsable: false - Wardrobe Is Decorative Only

From `Furniture_Village_Wardrobe.json`:

```json
{
  "BlockType": {
    "DrawType": "Model",
    "Flags": {
      "IsUsable": false
    }
  }
}
```

**Result:** No interaction prompt appears - purely decorative.

#### IsUsable: true - Potion Can Be Picked Up

From `Potion_Health_Greater.json`:

```json
{
  "BlockType": {
    "Flags": {
      "IsUsable": true
    },
    "Interactions": {
      "Use": {
        "Type": "PickupBlock"
      }
    }
  }
}
```

**Result:** Players can pick up the potion block.

**Use cases:**
- ✅ **true**: Chests, containers, doors, levers, switches, pickupable items, processing benches, portals
- ❌ **false**: Decorative furniture, signs, paintings, purely visual blocks

**Important:** Must have a `BlockType.Interactions.Use` defined for the interaction to work.

---

### IsBrush

**Type:** Boolean (`true` or `false`)  
**Default:** `false`  
**Location:** `BuilderTool.Tools[].IsBrush` (NOT in `BlockType.Flags`)

Controls whether an editor tool acts as a brush (continuous painting mode) or a single-action tool.

**Note:** This is specific to editor/builder tools and is located in the `BuilderTool` object, not `BlockType.Flags`.

#### IsBrush: true - Paint Tool

From `EditorTool_Paint.json`:

```json
{
  "BuilderTool": {
    "Tools": [
      {
        "Id": "Paint",
        "IsBrush": true,
        "BrushData": {
          "Width": { "Default": 5, "Min": 1, "Max": 100 },
          "Height": { "Default": 5, "Min": 1, "Max": 100 }
        }
      }
    ]
  }
}
```

**Result:** Tool paints continuously while held/dragged.

**Use cases:**
- ✅ **true**: Paint, sculpt, smooth, noise, flood fill, grass tools
- ❌ **false or omitted**: Selection, ruler, line, entity placement, prefab tools

**Important:** `IsBrush` is NOT a `BlockType.Flags` property. It's inside `BuilderTool.Tools[]`.

---

## BlockType.MovementSettings

Properties related to player movement and interaction with blocks.

### IsClimbable

**Type:** Boolean (`true` or `false`)  
**Default:** `false`  
**Location:** `BlockType.MovementSettings.IsClimbable`

Controls whether players can climb the block (like a ladder or rope).

#### IsClimbable: true - Ladder

From `Furniture_Village_Ladder.json`:

```json
{
  "BlockType": {
    "MovementSettings": {
      "IsClimbable": true
    },
    "HitboxType": "Ladder"
  }
}
```

**Result:** Players can climb up and down when touching the block.

#### IsClimbable: true - Rope

From `Deco_Rope.json`:

```json
{
  "BlockType": {
    "MovementSettings": {
      "IsClimbable": true
    }
  }
}
```

**Result:** Players can climb the rope.

**Note:** `IsClimbable` is in `MovementSettings`, not `Flags`.

**Use cases:**
- ✅ **true**: Ladders, ropes, vines, chains
- ❌ **false**: All other blocks

**Note:** Often paired with specific `HitboxType` (like `"Ladder"`) for proper collision detection.

---

### IsClimbable: true - Trapdoor (When Open)

From `Furniture_Village_Trapdoor.json`:

```json
{
  "BlockType": {
    "Flags": {
      "IsClimbable": true
    },
    "State": {
      "Definitions": {
        "OpenTrapdoor": {
          "MovementSettings": {
            "IsClimbable": true
          }
        }
      }
    }
  }
}
```

**Result:** Open trapdoor acts as a ladder.

---

## BlockType (Top-Level Properties)

Special properties defined directly on the BlockType object.

### IsDoor

**Type:** Boolean (`true` or `false`)  
**Default:** `false`  
**Location:** `BlockType.IsDoor`

Marks a block as a door, enabling door-specific behavior (opening/closing, multi-block structures, AI pathfinding).

#### IsDoor: true - Village Door

From `Furniture_Village_Door.json`:

```json
{
  "BlockType": {
    "IsDoor": true,
    "State": {
      "Definitions": {
        "OpenDoorIn": {
          "HitboxType": "Door_Open_In",
          "CustomModelAnimation": "Blocks/Animations/Door/Door_Open_In.blockyanim"
        },
        "CloseDoorIn": {
          "CustomModelAnimation": "Blocks/Animations/Door/Door_Close_In.blockyanim"
        }
      }
    },
    "Interactions": {
      "Use": "Door"
    }
  }
}
```

**Result:** Block functions as a door (open/close, double-block placement, NPC pathfinding).

#### IsDoor: true - Trapdoor

From `Furniture_Village_Trapdoor.json`:

```json
{
  "BlockType": {
    "IsDoor": true,
    "Interactions": {
      "Use": "Trapdoor"
    }
  }
}
```

**Result:** Horizontal door functionality.

#### IsDoor: true - Fence Gate

From `Wood_Softwood_Fence_Gate.json`:

```json
{
  "BlockType": {
    "IsDoor": true,
    "Interactions": {
      "Use": "Gate"
    }
  }
}
```

**Result:** Gate can open/close like a door.

**Use cases:**
- ✅ **true**: Doors, trapdoors, fence gates, any openable barrier
- ❌ **false**: All other blocks

**Important:** Must have appropriate `State` definitions and `Interactions.Use` for full functionality.

---

## BlockType.Gathering Properties

Flags within gathering configurations that control how blocks break.

### IsWeaponBreakable

**Type:** Boolean (`true` or `false`)  
**Default:** Varies by gathering type  
**Location:** `BlockType.Gathering.[GatherType].IsWeaponBreakable`

Controls whether a block can be broken by hitting it with a weapon (instead of requiring a tool).

#### IsWeaponBreakable: true - Pot Breaks with Weapons

From `Furniture_Village_Pot.json`:

```json
{
  "BlockType": {
    "Gathering": {
      "Soft": {
        "IsWeaponBreakable": true
      }
    }
  }
}
```

**Result:** Can break pot by hitting it with sword, axe, or fists.

#### IsWeaponBreakable: false - Torch Requires Tool

From `Wood_Torch_Wall.json`:

```json
{
  "BlockType": {
    "Gathering": {
      "Soft": {
        "IsWeaponBreakable": false
      }
    }
  }
}
```

**Result:** Must use proper tool, cannot break with weapons/fists.

#### IsWeaponBreakable: false - Sign Is Protected

From `Furniture_Village_Sign.json`:

```json
{
  "BlockType": {
    "Gathering": {
      "Soft": {
        "IsWeaponBreakable": false
      }
    }
  }
}
```

**Result:** Prevents accidental breaking during combat.

**Use cases:**
- ✅ **true**: Pots, vases, breakable decorations, destructible items
- ❌ **false**: Important decorations, signs, paintings, torches, valuable items

---

## Complete Example: Chest

From `Furniture_Village_Chest_Small.json`:

```json
{
  "BlockType": {
    "Material": "Solid",
    "DrawType": "Model",
    "CustomModel": "Blocks/Decorative_Sets/Village/Chest_Small.blockymodel",
    "CustomModelTexture": [{
      "Weight": 1,
      "Texture": "Blocks/Decorative_Sets/Village/Chest_Small_Texture.png"
    }],
    "Flags": {
      "IsUsable": true
    },
    "Gathering": {
      "Breaking": {
        "GatherType": "Woods"
      }
    },
    "Container": {
      "Capacity": 15
    },
    "Interactions": {
      "Use": {
        "Type": "OpenContainer"
      }
    }
  }
}
```

**Flags explained:**
- `"IsUsable": true` - Shows "Open" prompt when player looks at it
- No `IsStackable` - Defaults to false (chests don't stack)
- No `IsWeaponBreakable` - Must use proper tool to break
- `Interactions.Use` - Defines what happens when player presses E

---

## Flag Combinations

### Functional Container

```json
{
  "Flags": {
    "IsUsable": true
  },
  "Gathering": {
    "Breaking": {
      "GatherType": "Woods"
    }
  }
}
```

**Result:** Can open and use, requires tool to break.

---

### Decorative Item

```json
{
  "Flags": {
    "IsStackable": false,
    "IsUsable": false
  },
  "Gathering": {
    "Soft": {
      "IsWeaponBreakable": false
    }
  }
}
```

**Result:** Cannot interact, cannot stack, requires tool to break.

---

### Breakable Decoration

```json
{
  "Flags": {
    "IsUsable": false
  },
  "Gathering": {
    "Soft": {
      "IsWeaponBreakable": true
    }
  }
}
```

**Result:** No interaction, but breaks easily with any weapon.

---

### Stackable Pickupable

```json
{
  "Flags": {
    "IsStackable": true,
    "IsUsable": true
  },
  "Interactions": {
    "Use": {
      "Type": "PickupBlock"
    }
  }
}
```

**Result:** Plates or coins that stack and can be picked up.

---

### Climbable Door

```json
{
  "IsDoor": true,
  "MovementSettings": {
    "IsClimbable": true
  },
  "Interactions": {
    "Use": "Trapdoor"
  }
}
```

**Result:** Trapdoor that opens/closes and can be climbed when open.

---

## Quick Reference Table

| Flag | Location | Default | Controls |
|------|----------|---------|----------|
| `IsStackable` | `BlockType.Flags` | `false` | Vertical stacking in world |
| `IsUsable` | `BlockType.Flags` | `false` | Shows interaction prompt |
| `IsBrush` | `BlockType.Flags` | `false` | Editor tool brush mode |
| `IsClimbable` | `BlockType.MovementSettings` | `false` | Can be climbed like ladder |
| `IsDoor` | `BlockType` | `false` | Door functionality |
| `IsWeaponBreakable` | `BlockType.Gathering.*` | Varies | Breakable with weapons/fists |

---

## Common Mistakes

### ❌ Wrong Location

```json
{
  "Flags": {
    "IsDoor": true  // WRONG! Should be BlockType.IsDoor
  }
}
```

**Correct:**

```json
{
  "IsDoor": true,  // At BlockType level
  "Flags": {}
}
```

---

### ❌ IsUsable Without Interactions

```json
{
  "Flags": {
    "IsUsable": true
  }
  // Missing Interactions.Use!
}
```

**Correct:**

```json
{
  "Flags": {
    "IsUsable": true
  },
  "Interactions": {
    "Use": {
      "Type": "OpenContainer"
    }
  }
}
```

---

### ❌ Confusing IsStackable with MaxStack

```json
{
  "MaxStack": 64,  // Inventory stacking
  "BlockType": {
    "Flags": {
      "IsStackable": true  // World stacking
    }
  }
}
```

**These control different things:**
- `MaxStack` - How many in one inventory slot (e.g., 64 plates)
- `IsStackable` - Can place plates on top of each other in world

---

## Tips & Best Practices

1. **IsUsable requires Interactions** - Always define `Interactions.Use` when setting `IsUsable: true`
2. **IsStackable for thin items** - Use for plates, coins, thin decorations
3. **IsWeaponBreakable for destructibles** - Pots, vases, breakable decorations
4. **IsDoor for openable barriers** - Doors, gates, trapdoors
5. **IsClimbable with proper hitbox** - Use `"HitboxType": "Ladder"` for best results
6. **Test in-game** - Some flags interact in unexpected ways

---

## State-Specific Flags

Flags can change based on block state:

```json
{
  "BlockType": {
    "State": {
      "Definitions": {
        "Lit": {
          "Flags": {
            "IsUsable": true
          }
        },
        "Unlit": {
          "Flags": {
            "IsUsable": false
          }
        }
      }
    }
  }
}
```

**Result:** Block becomes usable only when lit.

---

## Related Properties

While not flags, these properties often work with flags:

- **`HitboxType`** - Collision shape (pairs with IsClimbable, IsDoor)
- **`Material`** - Block material type
- **`Support`** - What faces block can attach to
- **`Gathering`** - How block is harvested
- **`Interactions`** - What happens on Primary/Secondary/Use

---

## Related Guides

- **[Block Support](72_Block_Support.md)** - Block placement rules
- **[Block States](76_Block_States.md)** - Dynamic block states
- **[Gathering Types](91_Gathering_Types.md)** - Tool requirements
- **[Block Entity Components](90_Block_Entity_Components.md)** - Special block functionality
- **[Collision Events](53_Collision_Events.md)** - Collision triggers and hitboxes

---

**Previous:** [System Integration Guide](177_System_Integration.md) | **Next:** [Advanced Particle Systems](179_Advanced_Particle_Systems.md)
