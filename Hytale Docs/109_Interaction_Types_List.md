# Interaction Types - Complete List

A comprehensive reference of all interaction types available in Hytale.

## Overview

This document lists all interaction types organized by category. Each type has its own detailed guide. Interaction types define what happens when items, blocks, or entities interact with the game world.

## Example from Game Files

All interaction types are used in `Server/Item/Interactions/` and referenced in item/block definitions. Each interaction type has its own detailed guide with examples from game files.

## Location
All interaction types are used in `Server/Item/Interactions/` and referenced in item/block definitions.

## Complete Type List

### Control Flow Types
Control how interactions execute and chain together.

1. **[Simple](110_Interaction_Type_Simple.md)** - Basic interaction with effects and delays
2. **[Serial](111_Interaction_Type_Serial.md)** - Execute interactions sequentially
3. **[Parallel](112_Interaction_Type_Parallel.md)** - Execute interactions simultaneously
4. **[Condition](113_Interaction_Type_Condition.md)** - Conditional branching based on game state
5. **[Chaining](114_Interaction_Type_Chaining.md)** - Combo system for sequential actions
6. **[Charging](115_Interaction_Type_Charging.md)** - Hold-to-charge mechanics
7. **[Wielding](116_Interaction_Type_Wielding.md)** - Weapon blocking and parrying
8. **[Replace](117_Interaction_Type_Replace.md)** - Dynamic variable replacement
9. **[Selector](118_Interaction_Type_Selector.md)** - Targeting system for entities/blocks

### Entity Action Types
Actions that affect entities (players/NPCs).

10. **[DamageEntity](119_Interaction_Type_DamageEntity.md)** - Deal damage to entities
11. **[ApplyEffect](120_Interaction_Type_ApplyEffect.md)** - Apply status effects to entities
12. **[ClearEntityEffect](121_Interaction_Type_ClearEntityEffect.md)** - Remove status effects
13. **[ChangeStat](122_Interaction_Type_ChangeStat.md)** - Directly modify entity stats
14. **[ChangeStatWithModifier](123_Interaction_Type_ChangeStatWithModifier.md)** - Modify stats with multipliers
15. **[ApplyForce](124_Interaction_Type_ApplyForce.md)** - Apply physics force to entities
16. **[SpawnNPC](125_Interaction_Type_SpawnNPC.md)** - Spawn NPCs into the world
17. **[Heal](126_Interaction_Type_Heal.md)** - Restore health to entities

### Block Action Types
Actions that affect blocks in the world.

18. **[PlaceBlock](127_Interaction_Type_PlaceBlock.md)** - Place blocks in the world
19. **[ChangeBlock](128_Interaction_Type_ChangeBlock.md)** - Transform blocks to other types
20. **[ChangeState](129_Interaction_Type_ChangeState.md)** - Change block states
21. **[BreakBlock](130_Interaction_Type_BreakBlock.md)** - Break/destroy blocks
22. **[UseBlock](131_Interaction_Type_UseBlock.md)** - Interact with blocks
23. **[BlockCondition](132_Interaction_Type_BlockCondition.md)** - Conditional logic based on blocks

### Projectile Types
Projectile and ranged attack mechanics.

24. **[LaunchProjectile](133_Interaction_Type_LaunchProjectile.md)** - Fire projectiles
25. **[Projectile](134_Interaction_Type_Projectile.md)** - Projectile behavior configuration

### Inventory/Item Types
Inventory and item management.

26. **[ModifyInventory](135_Interaction_Type_ModifyInventory.md)** - Add/remove/equip items
27. **[RefillContainer](136_Interaction_Type_RefillContainer.md)** - Fill containers with fluids
28. **[Consume](137_Interaction_Type_Consume.md)** - Consume items (food, potions)

### UI/Communication Types
User interface and messaging.

29. **[SendMessage](138_Interaction_Type_SendMessage.md)** - Send messages to players
30. **[ChainFlag](139_Interaction_Type_ChainFlag.md)** - Set flags for combo chains
31. **[FirstClick](140_Interaction_Type_FirstClick.md)** - Handle first-click actions

### Condition Types
Specialized conditional checks.

32. **[StatsCondition](141_Interaction_Type_StatsCondition.md)** - Check and consume stats
33. **[StatsConditionWithModifier](142_Interaction_Type_StatsConditionWithModifier.md)** - Check stats with modifiers
34. **[EffectCondition](143_Interaction_Type_EffectCondition.md)** - Check entity effects
35. **[PlacementCountCondition](144_Interaction_Type_PlacementCountCondition.md)** - Check placed block count
36. **[MemoriesCondition](145_Interaction_Type_MemoriesCondition.md)** - Check interaction memory/history
37. **[MovementCondition](146_Interaction_Type_MovementCondition.md)** - Check entity movement state

### Teleportation Types
Teleportation mechanics.

38. **[TeleportInstance](147_Interaction_Type_TeleportInstance.md)** - Teleport to instance/area
39. **[TeleportConfigInstance](148_Interaction_Type_TeleportConfigInstance.md)** - Teleport using config (if exists)

### Special Action Types
Specialized interactions.

40. **[OpenContainer](149_Interaction_Type_OpenContainer.md)** - Open container UI
41. **[OpenProcessingBench](150_Interaction_Type_OpenProcessingBench.md)** - Open crafting/processing UI
42. **[Explode](151_Interaction_Type_Explode.md)** - Create explosions
43. **[SpawnPrefab](152_Interaction_Type_SpawnPrefab.md)** - Spawn prefab structures
44. **[SpawnDrops](153_Interaction_Type_SpawnDrops.md)** - Spawn item drops
45. **[UseEntity](154_Interaction_Type_UseEntity.md)** - Interact with entities
46. **[UseCoop](155_Interaction_Type_UseCoop.md)** - Interact with coop blocks

### Cooldown Types
Cooldown management.

47. **[ResetCooldown](156_Interaction_Type_ResetCooldown.md)** - Reset interaction cooldowns

### Additional Types (Undocumented)
These types are found in game files but don't have dedicated guides yet.

48. **ContextualUseNPC** - Context-specific NPC interactions (e.g., shearing, milking)
    ```json
    { "Type": "ContextualUseNPC", "Context": "Shear" }
    ```

49. **Seating** - Sit on blocks/furniture
    ```json
    { "Type": "Seating" }
    ```

50. **UseWateringCan** - Watering can tool interactions
    ```json
    { "Type": "UseWateringCan", "Next": {...}, "Effects": [...], "RunTime": 0.5 }
    ```

51. **FertilizeSoil** - Apply fertilizer to farmland
    ```json
    { "Type": "FertilizeSoil", "RefreshModifiers": [...], "Effects": [...] }
    ```

52. **DestroyCondition** - Conditional block destruction
    ```json
    { "Type": "DestroyCondition", "Next": {...} }
    ```

53. **Door** - Door open/close interactions
    ```json
    { "Type": "Door", "Effects": [{ "ItemAnimationId": "..." }] }
    ```

54. **OpenCustomUI** - Open custom UI pages
    ```json
    { "Type": "OpenCustomUI", "Page": { "Id": "CustomPage" } }
    ```

55. **Repeat** - Loop interactions multiple times
    ```json
    {
      "Type": "Repeat",
      "Repeat": 3,
      "Rules": {
        "InterruptedBy": ["Damage"],
        "ForkInteractions": {...},
        "Failed": {...}
      }
    }
    ```
    - `Repeat: -1` for infinite loop until interrupted

## Type Categories Summary

| Category | Count | Types |
|----------|-------|-------|
| Control Flow | 9 | Simple, Serial, Parallel, Condition, Chaining, Charging, Wielding, Replace, Selector |
| Entity Actions | 8 | DamageEntity, ApplyEffect, ClearEntityEffect, ChangeStat, ChangeStatWithModifier, ApplyForce, SpawnNPC, Heal |
| Block Actions | 6 | PlaceBlock, ChangeBlock, ChangeState, BreakBlock, UseBlock, BlockCondition |
| Projectiles | 2 | LaunchProjectile, Projectile |
| Inventory/Items | 3 | ModifyInventory, RefillContainer, Consume |
| UI/Communication | 3 | SendMessage, ChainFlag, FirstClick |
| Conditions | 6 | StatsCondition, StatsConditionWithModifier, EffectCondition, PlacementCountCondition, MemoriesCondition, MovementCondition |
| Teleportation | 2 | TeleportInstance, TeleportConfigInstance |
| Special Actions | 7 | OpenContainer, OpenProcessingBench, Explode, SpawnPrefab, SpawnDrops, UseEntity, UseCoop |
| Cooldowns | 1 | ResetCooldown |
| Additional | 8 | ContextualUseNPC, Seating, UseWateringCan, FertilizeSoil, DestroyCondition, Door, OpenCustomUI, Repeat |
| **Total** | **55** | All interaction types |

## Quick Reference by Use Case

### Basic Interactions
- **Simple** - Most basic interaction
- **Condition** - Add conditional logic
- **Serial/Parallel** - Chain multiple actions

### Combat
- **DamageEntity** - Deal damage
- **ApplyEffect** - Apply buffs/debuffs
- **Selector** - Target entities
- **Wielding** - Block/parry

### Building/Blocks
- **PlaceBlock** - Place blocks
- **ChangeBlock** - Transform blocks
- **BreakBlock** - Destroy blocks
- **ChangeState** - Toggle block states

### Items/Inventory
- **ModifyInventory** - Manage inventory
- **Consume** - Use items
- **RefillContainer** - Fill containers

### Movement
- **ApplyForce** - Push entities
- **TeleportInstance** - Teleport players
- **MovementCondition** - Check movement

### Projectiles
- **LaunchProjectile** - Fire projectiles
- **Projectile** - Configure projectiles

### NPCs
- **SpawnNPC** - Spawn NPCs
- **UseEntity** - Interact with entities

### UI/Feedback
- **SendMessage** - Show messages
- **OpenContainer** - Open UI
- **OpenProcessingBench** - Open crafting UI

### Conditions
- **StatsCondition** - Check stats
- **EffectCondition** - Check effects
- **BlockCondition** - Check blocks

## Navigation

**Previous:** [Item Sound Sets](108_Item_Sound_Sets.md) | **Next:** [Simple Interaction Type](110_Interaction_Type_Simple.md)

---

## Detailed Guides

Each interaction type has its own detailed guide with:
- Complete property reference
- Usage examples
- Common patterns
- Best practices
- Tips and tricks

Start with the **Simple** type guide for the most basic interaction, then explore other types based on your needs.
