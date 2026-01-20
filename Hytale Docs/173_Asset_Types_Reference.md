# Asset Types Reference

A complete reference of all asset types available in Hytale's modding system.

## Overview

Hytale uses many different asset types to define game content. Each asset type serves a specific purpose and has its own file structure and properties. This guide provides a complete reference of all available asset types.

## Asset Type Categories

All asset types available in Hytale (from `server.lang`):

### Audio and Visual Effects

| Asset Type | Description | Location |
|------------|-------------|----------|
| **AmbienceFX** | Environmental audio and music | `Server/AmbienceFX/` |
| **SoundEvent** | Sound effect events | `Client/Sound/` |
| **Trail** | Visual trails for projectiles/items | `Server/Trail/` |
| **FluidFX** | Fluid block visual effects | `Server/FluidFX/` |
| **BlockParticleSet** | Particle effects for blocks | `Server/Item/Block/ParticleSets/` |
| **BlockBreakingDecal** | Visual damage on blocks | `Server/Item/Block/BreakingDecals/` |

### Items and Equipment

| Asset Type | Description | Location |
|------------|-------------|----------|
| **Item** | All item definitions | `Server/Item/Items/` |
| **ItemDropList** | Loot tables and drop lists | `Server/Drops/` |
| **ItemCategory** | Item organization categories | `Server/Item/Category/` |
| **FieldcraftCategory** | Fieldcraft menu categories | `Server/Item/Category/Fieldcraft/` |
| **ItemPlayerAnimations** | Player animations for items | `Server/Item/PlayerAnimations/` |
| **ItemToolSpec** | Tool specifications | `Server/Item/ToolSpecs/` |
| **UnarmedInteractions** | Unarmed gathering/combat | `Server/Item/Unarmed/` |
| **ResourceType** | Resource type definitions | `Server/Item/ResourceTypes/` |

### Blocks

| Asset Type | Description | Location |
|------------|-------------|----------|
| **BlockSet** | Block categorization sets | `Server/Item/Block/Sets/` |
| **BlockSoundSet** | Block sound effects | `Server/Item/Block/SoundSets/` |
| **BlockBoundingBoxes** | Block collision boxes | `Server/Item/Block/BoundingBoxes/` |
| **BlockMigration** | Block ID migrations | `Server/Item/Block/Migrations/` |
| **BlockSpawnerTable** | Block spawner configurations | `Server/Item/Block/Spawners/` |

### NPCs and Entities

| Asset Type | Description | Location |
|------------|-------------|----------|
| **NPCRole** | NPC behavior definitions | `Server/NPC/Roles/` |
| **NPCGroup** | NPC faction groups | `Server/NPC/Groups/` |
| **AttitudeGroup** | NPC attitude/hostility | `Server/NPC/AttitudeGroups/` |
| **BeaconNPCSpawn** | Beacon-based NPC spawning | `Server/NPC/Spawning/` |
| **SpawnMarker** | Spawn point markers | `Server/NPC/Spawning/` |
| **SpawnSuppression** | Spawn blocking zones | `Server/NPC/Spawning/` |
| **WorldNPCSpawn** | World NPC spawn tables | `Server/NPC/Spawning/` |
| **EntityEffect** | Status effects for entities | `Server/EntityEffect/` |
| **EntityStatType** | Entity stat definitions | `Server/Entity/Stats/` |
| **ModelAsset** | NPC/Entity models | `Server/Models/` |
| **FlockAsset** | Flocking behavior (birds) | `Server/NPC/` |

### Interactions and Mechanics

| Asset Type | Description | Location |
|------------|-------------|----------|
| **Interaction** | Item/block interactions | `Server/Item/Interactions/` |
| **Condition** | Conditional logic | `Server/Conditions/` |
| **TickProcedure** | Block tick behaviors | `Server/Item/Block/TickProcedures/` |
| **Projectile** | Projectile configurations | `Server/Projectile/` |
| **ResponseCurve** | Math curves for animations | `Server/ResponseCurves/` |
| **TagPattern** | Tag matching patterns | `Server/TagPatterns/` |

### World and Environment

| Asset Type | Description | Location |
|------------|-------------|----------|
| **Environment** | Biome/environment configs | `Server/Environments/` |
| **Weather** | Weather system configs | `Server/Weather/` |
| **GrowthModifierAsset** | Crop growth modifiers | `Server/GrowthModifiers/` |

### Quests and Objectives

| Asset Type | Description | Location |
|------------|-------------|----------|
| **ObjectiveAsset** | Quest objectives | `Server/Objective/Objectives/` |
| **ObjectiveLineAsset** | Objective line definitions | `Server/Objective/` |
| **ObjectiveLocationMarkerAsset** | Location markers for quests | `Server/Objective/` |
| **ReachLocationMarkerAsset** | Reach location objectives | `Server/Objective/` |

### Commerce

| Asset Type | Description | Location |
|------------|-------------|----------|
| **ShopAsset** | Barter/trading shops | `Server/Shop/` |
| **ReputationGroup** | Reputation factions | `Server/Reputation/Groups/` |
| **ReputationRank** | Reputation rank tiers | `Server/Reputation/Ranks/` |

### Configuration

| Asset Type | Description | Location |
|------------|-------------|----------|
| **GameplayConfig** | Gameplay rule configurations | `Server/GameplayConfig/` |
| **BalanceAsset** | Game balance settings | `Server/Balance/` |

## Using Asset Types

### Finding Assets

**List assets with specific tag:**
```
/assets tags <assetType> <tag>
```

Example:
```
/assets tags Item Type=Weapon
```

### Asset File Structure

Each asset type has its own file structure:

**Items:**
```
Server/Item/Items/Weapon/Sword/Weapon_Sword_Iron.json
```

**NPCs:**
```
Server/NPC/Roles/Intelligent/Aggressive/Goblin/Goblin_Scrapper.json
```

**Environments:**
```
Server/Environments/Zone1/Env_Zone1_Plains.json
```

**Interactions:**
```
Server/Item/Interactions/Weapons/Sword/Weapon_Sword_Primary.json
```

## Asset Naming Conventions

### General Patterns

Most assets follow these naming patterns:

**Items:**
- `[Type]_[Material]_[Variant]` (e.g., `Weapon_Sword_Iron`)
- `[Category]_[Type]_[Name]` (e.g., `Furniture_Crude_Bed`)

**NPCs:**
- `[Species]_[Variant]` (e.g., `Chicken_Desert`)
- `[Faction]_[Role]` (e.g., `Goblin_Scrapper`)

**Environments:**
- `Env_[Zone]_[Biome]` (e.g., `Env_Zone1_Plains`)

**Effects:**
- `[Type]_[Purpose]_[Tier]` (e.g., `Food_Buff_T1`)

**Interactions:**
- `[Item]_[Type]_[Action]` (e.g., `Weapon_Sword_Primary_Swing`)

### File Extensions

| Extension | Asset Type |
|-----------|------------|
| `.json` | All configuration files |
| `.blockymodel` | 3D models |
| `.blockyanim` | Animations |
| `.png` | Textures and icons |
| `.ui` | UI layouts |
| `.lang` | Translation files |

## Translation Keys

Assets reference translations using keys:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_Sword_Iron.name",
    "Description": "server.items.Weapon_Sword_Iron.description"
  }
}
```

**Translation key patterns:**
- Items: `server.items.[ItemId].name`
- NPCs: `server.npcRoles.[NPCId].name`
- Bench categories: `server.benchCategories.[categoryId]`
- UI: `server.ui.[component]`

## Asset Validation

**Validate prefab files:**
```
/validatecpb [path]
```

Validates Compressed Prefab Buffer files for errors.

## Asset Packs

**List loaded asset packs:**
```
/packs list
```

Shows all currently loaded asset packs and their locations.

## Related Guides

### Item-Related
- [Creating Items](02_Items.md) - Item asset creation
- [Item Categories](79_Item_Categories.md) - Item organization
- [Item Sound Sets](108_Item_Sound_Sets.md) - Item audio

### NPC-Related
- [Creating NPCs](03_NPCs.md) - NPC role assets
- [NPC Groups](43_NPC_Groups.md) - NPC faction groups
- [NPC Appearance](85_NPC_Appearance.md) - NPC model assets

### Block-Related
- [Blocks & Portals](06_Blocks_and_Portals.md) - Block type creation
- [Block Sets](39_Block_Sets.md) - Block categorization
- [Block Sound Sets](40_Block_Sound_Sets.md) - Block audio

### Interaction-Related
- [Advanced Item Interactions](21_Advanced_Item_Interactions.md) - Interaction assets
- [Interaction Types List](109_Interaction_Types_List.md) - All interaction types

### Environment-Related
- [Biomes & Environments](24_Biomes_and_Environments.md) - Environment assets
- [Weather Systems](23_Weather.md) - Weather assets

### Effect-Related
- [Potions & Effects](04_Potions_and_Effects.md) - Entity effect assets
- [Particle Systems](32_Particle_Systems.md) - Particle system assets

### Quest-Related
- [Trading & Quests](05_Trading_and_Quests.md) - Shop and objective assets
- [Objective System](168_Objective_System.md) - Quest objective assets

---

**Previous:** [Builder Tools Guide](172_Builder_Tools_Guide.md) | **Next:** [Item System Overview](174_Item_System_Overview.md)
