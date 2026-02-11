# NPC Spawn Beacons

Spawn beacons are **data-driven NPC spawners** used by the game’s spawning system and by NPC behaviors that need to “call in” helper/food/throwable NPCs.

This guide is grounded in **actual game files** (and matches the `NPC_Tutorial_Draft.pdf` patterns).

---

## Location

- Spawn beacon configs: `Server/NPC/Spawn/Beacons/`
- NPC groups (often used as filters): `Server/NPC/Groups/`
- NPC roles that trigger beacons: `Server/NPC/Roles/`

---

## Spawn beacon file format

### Example: `Edible_Rat`

```1:15:Server/NPC/Spawn/Beacons/Edible_Rat.json
{
  "Environments": [],
  "NPCs": [
    {
      "Weight": 1,
      "Id": "Edible_Rat"
    }
  ],
  "SpawnAfterGameTimeRange": [
    "PT5M",
    "PT10M"
  ],
  "NPCSpawnState": "Seek",
  "TargetSlot": "LockedTarget"
}
```

### Key fields

- **`NPCs`**: Weighted list of NPC role IDs to spawn.
- **`SpawnAfterGameTimeRange`**: ISO-8601 durations (example uses 5–10 minutes).
- **`NPCSpawnState`**: State the spawned NPC should start in.
- **`TargetSlot`**: Target slot used by the spawned NPC logic (commonly `LockedTarget`).
- **`Environments`**: Environment constraints (empty in this example).

---

## Making an NPC react to a spawn beacon

A common pattern is:

1. An “edible”/helper NPC sits in `Idle`.
2. It listens for a **beacon message** that provides a target slot.
3. It switches to a movement state (`Seek`) and approaches the target.

This is implemented by `Template_Edible_Critter`:

```53:121:Server/NPC/Roles/_Core/Templates/Template_Edible_Critter.json
  "Instructions": [
    {
      "Instructions": [
        {
          "Sensor": {
            "Type": "State",
            "State": "Idle"
          },
          "Instructions": [
            {
              "Sensor": {
                "Type": "Beacon",
                "Message": "Approach_Target",
                "TargetSlot": "LockedTarget"
              },
              "Actions": [
                {
                  "Type": "State",
                  "State": "Seek"
                }
              ]
            },
            {
              "ActionsBlocking": true,
              "Actions": [
                {
                  "Type": "Timeout",
                  "Delay": [ 1, 1 ]
                },
                {
                  "Type": "Despawn"
                }
              ]
            }
          ]
        },
        {
          "Sensor": {
            "Type": "State",
            "State": "Seek"
          },
          "Instructions": [
            {
              "Sensor": {
                "Type": "Target",
                "TargetSlot": "LockedTarget",
                "Range": { "Compute": "SeekRange" }
              },
              "BodyMotion": {
                "Type": "Seek",
                "SlowDownDistance": 0.1,
                "StopDistance": 0.1
              }
            },
            {
              "ActionsBlocking": true,
              "Actions": [
                {
                  "Type": "Timeout",
                  "Delay": [ 1, 1 ]
                },
                {
                  "Type": "State",
                  "State": "Idle"
                }
              ]
            }
          ]
        }
      ]
    }
  ],
```

Notes:

- **`ActionsBlocking: true`** is used to ensure the timeout completes before the next actions run.
- The `Idle` state includes a fallback **despawn** so “edible” critters don’t linger forever if not consumed.

---

## Triggering a spawn beacon from an NPC

`Template_Goblin_Ogre` triggers an edible-NPC spawn beacon when it enters `CallRat`:

```469:482:Server/NPC/Roles/Intelligent/Aggressive/Goblin/Templates/Template_Goblin_Ogre.json
          "Instructions": [
            {
              "Continue": true,
              "Sensor": {
                "Type": "Any",
                "Once": true
              },
              "Actions": [
                {
                  "Type": "TriggerSpawnBeacon",
                  "BeaconSpawn": { "Compute": "FoodNPCBeacon" },
                  "Range": 15
                }
              ]
            },
```

This is paired with parameters that define which beacon to trigger and which NPC groups to accept as “food”:

```36:43:Server/NPC/Roles/Intelligent/Aggressive/Goblin/Templates/Template_Goblin_Ogre.json
    "FoodNPCGroups": {
      "Value": [ "Edible_Rat" ],
      "Description": "The groups of edible NPCs that will come from triggering the beacon"
    },
    "FoodNPCBeacon": {
      "Value": "Edible_Rat",
      "Description": "The spawn beacon to trigger to create an edible NPC"
    },
```

---

## Official Resources

- **Generated NPC Documentation:** https://hytalemodding.dev/en/docs/official-documentation/npc-doc
- **NPC Tutorial:** https://hytalemodding.dev/en/docs/official-documentation/npc/1-know-your-enemy

---

## See also

- **NPC roles**: [Creating NPCs](03_NPCs.md)
- **NPC groups** (used by filters like `NPCGroup`): [NPC Groups](43_NPC_Groups.md)
- **NPC sensors** (Beacon/Target/Mob): [NPC Sensors](81_NPC_Sensors.md)
- **Advanced AI**: [Advanced AI & NPC Behavior](180_Advanced_AI_NPC_Behavior.md)

