# Interaction Type: SpawnDrops

Spawn item drops at target location using drop tables.

## Overview

`SpawnDrops` spawns item drops at a location using drop table configurations. Useful for loot drops, harvesting, and reward systems.

## Notes

**Note:** This type may not exist as a direct interaction type. Item drops are typically handled through:
- Block breaking (`BreakBlock` with `Harvest: true`)
- NPC drops (configured in NPC drop tables)
- Direct `ModifyInventory` for player rewards

For drop tables, see:
- [Drop Tables](105_Drop_Tables.md) - Complete drop table documentation
- Block/NPC drop configurations

If direct spawn needed, may require custom logic or use `ModifyInventory` for player rewards.

---

**Previous:** [SpawnPrefab](152_Interaction_Type_SpawnPrefab.md) | **Next:** [UseEntity](154_Interaction_Type_UseEntity.md)
