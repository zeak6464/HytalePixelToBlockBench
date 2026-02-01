# Spawn Suppression

Control NPC spawn suppression in areas like camps and settlements.

## Overview

Spawn Suppression prevents certain NPC groups from spawning within defined areas. Used for player camps, villages, and safe zones where hostile mobs shouldn't spawn.

## Location
`Server/NPC/Spawn/Suppression/`

## Example from Game Files

### Spawn Camp Suppression

From `Server/NPC/Spawn/Suppression/Spawn_Camp.json`:

```json
{
  "SuppressionRadius": 45,
  "SuppressedGroups": [
    "Aggressive",
    "Passive",
    "Neutral"
  ],
  "SuppressSpawnMarkers": true
}
```

### Trork Tier 3 Suppression

From `Server/NPC/Spawn/Suppression/Trork_Tier_3.json`:

```json
{
  "SuppressionRadius": 45,
  "SuppressedGroups": [
    "Passive"
  ],
  "SuppressSpawnMarkers": false
}
```

---

## Suppression Properties

| Property | Type | Description |
|----------|------|-------------|
| `SuppressionRadius` | Number | Radius in blocks where spawning is suppressed |
| `SuppressedGroups` | Array | NPC groups to suppress |
| `SuppressSpawnMarkers` | Boolean | Also suppress spawn marker NPCs |

---

## NPC Groups

Common NPC groups that can be suppressed:

| Group | Description |
|-------|-------------|
| `Aggressive` | Hostile NPCs (goblins, zombies, etc.) |
| `Passive` | Non-hostile animals (chickens, cows, etc.) |
| `Neutral` | NPCs that only attack when provoked |

---

## Instance Suppression Controller

Suppression can also be controlled at the instance level.

From `Server/Instances/Default/resources/SpawnSuppressionController.json`:

```json
{
  "Suppressions": []
}
```

This file tracks active suppressions within an instance.

---

## Using Suppression in Prefabs

Link suppression to prefabs or blocks:

```json
{
  "BlockType": {
    "BlockEntity": {
      "Components": {
        "SpawnSuppression": {
          "SuppressionId": "Spawn_Camp"
        }
      }
    }
  }
}
```

---

## Creating Custom Suppression

### Safe Zone (All NPCs)

`Server/NPC/Spawn/Suppression/Safe_Zone.json`:

```json
{
  "SuppressionRadius": 100,
  "SuppressedGroups": [
    "Aggressive",
    "Passive",
    "Neutral"
  ],
  "SuppressSpawnMarkers": true
}
```

### Hostile-Only Suppression

`Server/NPC/Spawn/Suppression/Village_Safe.json`:

```json
{
  "SuppressionRadius": 60,
  "SuppressedGroups": [
    "Aggressive"
  ],
  "SuppressSpawnMarkers": false
}
```

### Small Camp

`Server/NPC/Spawn/Suppression/Small_Camp.json`:

```json
{
  "SuppressionRadius": 25,
  "SuppressedGroups": [
    "Aggressive"
  ],
  "SuppressSpawnMarkers": false
}
```

---

## Use Cases

| Use Case | Radius | Groups | Markers |
|----------|--------|--------|---------|
| Player Camp | 45 | All | Yes |
| Village | 60 | Aggressive | No |
| Small Outpost | 25 | Aggressive | No |
| Safe Zone | 100 | All | Yes |
| Boss Arena | 50 | Passive | No |

---

## Spawn Markers vs Groups

- **`SuppressSpawnMarkers: true`** - Also prevents spawn marker NPCs (like beacons)
- **`SuppressSpawnMarkers: false`** - Only suppresses natural spawns, markers still work

Use `false` when you want custom spawns (like boss fights) but no random mobs.

---

## Tips

1. **Balance radius** - Too large = empty areas, too small = overwhelmed
2. **Group selection** - Only suppress what's needed
3. **Spawn markers** - Keep `false` for custom encounters
4. **Overlap** - Multiple suppressions stack
5. **Instance tracking** - Check `SpawnSuppressionController.json` for active areas

---

**Related:** [NPCs](03_NPCs.md) | [NPC Spawn Beacons](185_NPC_Spawn_Beacons.md) | [Server Modding](12_Server_Modding.md)
