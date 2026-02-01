# Model Organization

Reference guide for NPC model categories and organization.

## Overview

NPC models are organized in `Server/Models/` by creature type and category. Understanding this organization helps find appropriate models for custom NPCs.

## Location
`Server/Models/`

---

## Model Categories

### Beast (44 models)
Large hostile creatures.

```
Server/Models/Beast/
├── Bear_Grizzly.json
├── Bear_Polar.json
├── Boar.json
├── Crocodile.json
├── Emberwulf.json
├── Fen_Stalker.json
├── Hyena.json
├── Lion.json
├── Scarak_Broodmother.json
├── Tiger.json
├── Wolf.json
├── Wolf_Dire.json
└── ...
```

### Boss (1 model)
Major boss creatures.

```
Server/Models/Boss/
└── Golem_Guardian_Void.json
```

### Critter (9 models)
Small ambient creatures.

```
Server/Models/Critter/
├── Beetle.json
├── Crab.json
├── Frog.json
├── Lizard.json
├── Rat.json
├── Scorpion.json
├── Snake.json
├── Spider.json
└── Turtle.json
```

### Elemental (13 models)
Magical/elemental beings.

```
Server/Models/Elemental/
├── Dragon_Fire.json
├── Dragon_Frost.json
├── Dragon_Void.json
├── Golem_Crystal_Earth.json
├── Golem_Crystal_Flame.json
├── Golem_Crystal_Frost.json
├── Golem_Crystal_Sand.json
├── Golem_Crystal_Thunder.json
├── Golem_Firesteel.json
├── Spirit_Ember.json
├── Spirit_Frost.json
├── Spirit_Root.json
└── Spirit_Thunder.json
```

### Flying_Beast (4 models)
Large flying hostile creatures.

```
Server/Models/Flying_Beast/
├── Archaeopteryx.json
├── Pterodactyl.json
├── Scarak_Seeker.json
└── Vulture.json
```

### Flying_Critter (11 models)
Small flying creatures.

```
Server/Models/Flying_Critter/
├── Bat.json
├── Bee.json
├── Butterfly.json
├── Dragonfly.json
├── Firefly.json
├── Moth.json
└── ...
```

### Flying_Wildlife (5 models)
Birds and flying animals.

```
Server/Models/Flying_Wildlife/
├── Crow.json
├── Eagle.json
├── Owl.json
├── Parrot.json
└── Seagull.json
```

### Human (4 models)
Human NPC models.

```
Server/Models/Human/
├── Human_Female.json
├── Human_Male.json
├── Outlander_Female.json
└── Outlander_Male.json
```

### Intelligent (73+ models)
Intelligent NPC species, organized by race.

```
Server/Models/Intelligent/
├── Bramblekin/
│   ├── Bramblekin.json
│   └── Bramblekin_Shaman.json
├── Feran/
│   ├── Feran_Burrower.json
│   ├── Feran_Civilian.json
│   ├── Feran_Cub.json
│   └── ...
├── Goblin/
│   ├── Goblin_Duke.json
│   ├── Goblin_Lobber.json
│   ├── Goblin_Scrapper.json
│   └── ...
├── Klops/
│   ├── Klops.json
│   ├── Klops_Gentleman.json
│   └── ...
├── Kweebec/
│   ├── Kweebec_Elder.json
│   ├── Kweebec_Merchant.json
│   ├── Kweebec_Rootling.json
│   └── ...
├── Outlander/
│   ├── Outlander_Civilian.json
│   ├── Outlander_Guard.json
│   └── ...
├── Slothian/
│   ├── Slothian_Civilian.json
│   ├── Slothian_Elder.json
│   └── ...
└── Trork/
    ├── Trork_Brute.json
    ├── Trork_Grunt.json
    ├── Trork_Warlord.json
    └── ...
```

### Livestock (34 models)
Farm animals and mounts.

```
Server/Models/Livestock/
├── Bison.json, Bison_Calf.json
├── Boar.json, Boar_Piglet.json
├── Bunny.json
├── Camel.json, Camel_Calf.json
├── Chicken.json, Chick.json
├── Cow.json, Calf.json
├── Goat.json, Goat_Kid.json
├── Horse.json, Horse_Foal.json
├── Pig.json, Piglet.json
├── Rabbit.json
├── Sheep.json, Lamb.json
├── Skrill.json, Skrill_Chick.json
├── Turkey.json, Turkey_Chick.json
└── Warthog.json, Warthog_Piglet.json
```

### Pets (4 models)
Pet companions.

```
Server/Models/Pets/
├── Cat.json
├── Dog.json
├── Ferret.json
└── Parrot.json
```

### Projectiles (79 models)
Projectile visuals.

```
Server/Models/Projectiles/
├── Weapons/
│   ├── Arrow/
│   ├── Bolt/
│   └── Spear/
├── NPCs/
│   ├── Beast/
│   └── Intelligent/
└── Spells/
```

### Swimming_Beast (12 models)
Large aquatic creatures.

```
Server/Models/Swimming_Beast/
├── Crocodile.json
├── Eel.json
├── Jellyfish_Giant.json
├── Octopus.json
├── Shark.json
├── Squid.json
└── ...
```

### Swimming_Critter (7 models)
Small aquatic creatures.

```
Server/Models/Swimming_Critter/
├── Crab.json
├── Jellyfish.json
├── Seahorse.json
├── Starfish.json
└── ...
```

### Swimming_Wildlife (13 models)
Fish and aquatic animals.

```
Server/Models/Swimming_Wildlife/
├── Dolphin.json
├── Minnow.json
├── Salmon.json
├── Trout.json
├── Whale.json
└── ...
```

### Undead (33 models)
Undead creatures.

```
Server/Models/Undead/
├── Ghoul.json
├── Phantom.json
├── Skeleton_Archer.json
├── Skeleton_Lord.json
├── Skeleton_Mage.json
├── Skeleton_Warrior.json
├── Wraith.json
├── Zombie.json
├── Zombie_Drowned.json
└── ...
```

### Vehicles (2 models)
Vehicle models.

```
Server/Models/Vehicles/
├── Boat.json
└── Minecart.json
```

### Void (6 models)
Void creatures.

```
Server/Models/Void/
├── Crawler_Void.json
├── Eye_Void.json
├── Larva_Void.json
├── Necromancer_Void.json
├── Spawn_Void.json
└── Spectre_Void.json
```

### Wildlife (12+ models)
Wild animals.

```
Server/Models/Wildlife/
├── Antelope.json
├── Armadillo.json
├── Deer_Doe.json
├── Deer_Stag.json
├── Moose_Bull.json
├── Moose_Cow.json
├── Mosshorn.json
├── Penguin.json
├── Tortoise.json
└── ...
```

---

## Special Models

```
Server/Models/
├── NPC_Elf.json           # Special elf NPC
├── NPC_Santa.json         # Holiday NPC
├── NPC_Path_Marker.json   # Pathfinding marker
├── NPC_Spawn_Marker.json  # Spawn marker
├── Objective_Location_Marker.json  # Quest marker
└── Warp.json              # Warp point
```

---

## Naming Conventions

| Pattern | Description |
|---------|-------------|
| `{Species}.json` | Base model |
| `{Species}_{Variant}.json` | Variant (e.g., `Bear_Polar`) |
| `{Species}_{Age}.json` | Age variant (e.g., `Chicken`, `Chick`) |
| `{Species}_{Role}.json` | Role variant (e.g., `Kweebec_Merchant`) |

---

## Using Models in NPCs

Reference model in NPC role:

```json
{
  "Model": "Beast/Wolf",
  "ModelConfig": {
    "Scale": 1.2,
    "Texture": "NPCs/Wolf/Wolf_Dire.png"
  }
}
```

---

## Tips

1. **Category matching** - Use appropriate category for NPC type
2. **Variants** - Check for existing variants before creating new
3. **Scale adjustments** - Use `ModelConfig.Scale` for size changes
4. **Age variants** - Baby versions often exist (e.g., Chick, Calf)
5. **Role models** - Intelligent NPCs have role-specific models

---

**Related:** [NPCs](03_NPCs.md) | [NPC Appearance](85_NPC_Appearance.md) | [NPC Models](169_NPC_Models.md)
