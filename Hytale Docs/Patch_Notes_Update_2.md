# Hytale Patch Notes — Update 2 (January 24, 2026)

Modding-relevant changes from [Hytale Update 2](https://hytale.com/news/2026/1/hytale-patch-notes-update-2). Use this alongside the official patch notes when updating mods and guides.

---

## World Generation

| Change | Impact on mods |
|--------|-----------------|
| **World Gen V2** prepared for public documentation | New worldgen system; docs incoming. |
| **Default_Flat** and **Default_Void** templates | New World Gen V2 templates available. |
| **Creative Hub Portal** | Spawns with World Gen V2 flat worldgen (clear weather, limited fog). New creative worlds only. |
| **Ore placement** refactor in all zones | **More ores**, especially in Devastated Lands underground. **Existing worlds:** explore **new chunks** to see changes. |

---

## Mining & Pickaxe Progression

| Change | Impact on mods |
|--------|-----------------|
| **Adamantite** | Now requires **Thorium or Cobalt** pickaxe to mine. |
| **Mining / pickaxe progression** | Lower-tier pickaxes deal **much less damage** to higher-tier ores. Upgrading pickaxe **greatly reduces** hits needed. |
| **Hoe tiers** | Hoes unlock at **different farming bench tiers**. Recipe costs adjusted. |
| **Copper** | Now **Tier 2** (was Tier 4). |
| **Iron** | Now **Tier 4** (was Tier 8). |
| **Thorium** | **Tier 6** (new). |

**Pickaxe tier order (post–Update 2):** Crude → Copper (T2) → … → Iron (T4) → … → Thorium (T6) → Cobalt → etc. Adamantite: Thorium or Cobalt required.

---

## Farming

| Change | Impact on mods |
|--------|-----------------|
| **Eternal crops** | No longer break from **accidental weapon damage**. |
| **Tilled soil lifetime** | **1.2–1.5 days** (increased). |
| **Soil decay** | Fixed under **fully grown** crops. |
| **Offhand** | Can hold **torch in offhand** while using Hoe and Seeds. |
| **Eternal crops** | Breaking fully grown Eternal crops **drops Seeds** back. |
| **Harvested crops** | Can be **placed on ground when crouching**. |
| **Hoes** | Unlock at different farming bench tiers; recipe costs adjusted. |
| **Petals** | **Craftable** at furniture bench (**Textiles**). Renewable (e.g. Yellow Petals from Sunflower Seeds). All colored Petal recipes use renewable resources. |
| **Cloth blocks** | **Lighter variants** craftable with **Petals**. |

---

## NPCs & Drops

| Change | Impact on mods |
|--------|-----------------|
| **Skeletal minion item** | New item to raise skeletal minions from nearby bone piles. Drops from **large Praetorian Skeleton**. |
| **Magma Toad** | Tongue damage **reduced**. Now alternates **Tongue** attack and **Headbutt**. |
| **Polar Bears** | **Buffed**. |
| **Hide drops** → **Medium Hide** (was Light Hide) | **Bison, Boar, Cow, Horse, Warthog**. |

---

## Scripted Brushes

| Change | Impact on mods |
|--------|-----------------|
| **JumpIfBlockType** | Now uses a **single offset** instead of a **list of offsets**. Update old brushes that use `Offset: [ ... ]`. |
| **QoL** | New decoration and cave brushes; missing filter entries added; invalid blocks removed; redundant brushes removed; assorted bug fixes. |

**Migration:** Replace `Offset: [ { "X": ..., "Y": ..., "Z": ... } ]` with a single `Offset: { "X": ..., "Y": ..., "Z": ... }` object.

---

## UI / Editor / Builder

| Change | Impact on mods |
|--------|-----------------|
| **Block tooltips** | Improved in editor. |
| **Hotbar** | Clearer **active-slot** visibility (contrast + diamond indicator). |
| **Memories** | Categories **sorted alphabetically** in "The Heart of Orbis". |
| **Inventory** | Better double-click; **Shift+Click** armor to equip if slot empty; **QERT** shortcuts for quick actions (put all, pull all, quickstack, sort). |
| **FPS counter** | New option: **Settings → General → User Interface**. |
| **Player markers** | On compass; map updates (e.g. up/down indicators, toggles). |
| **Ophidiophobia mode** | **Gameplay → Accessibility**; snake models become Hatworms. |

---

## Server / Tech

| Change | Impact on mods |
|--------|-----------------|
| **Server auto-update** | New system with config options and **`/update`** commands for check, download, apply, and management. |
| **UpdateModule** | Core plugin; server checks for updates, notifies players, handles staged updates. |
| **UpdateConfig** | In `HytaleServerConfig`: enable updates, check interval, notifications, patchline, backup, auto-apply. |
| **ShutdownReason.UPDATE** | New reason when server restarts for a staged update. |
| **Launcher scripts** | `start.sh`, `start.bat` in release workflow. |

---

## Guides Updated for Update 2

These docs were revised to match Update 2:

- **[Item System Overview](174_Item_System_Overview.md)** — Mining tiers, pickaxe progression, Adamantite.
- **[Tools](14_Tools.md)** — Pickaxe/hoe tiers, progression.
- **[Plants & Farming](15_Plants_and_Farming.md)** — Soil lifetime, Eternal crops, petals, offhand torch, placing crops.
- **[Farming Modifiers](52_Farming_Modifiers.md)** — Soil lifetime.
- **[World Generation](38_World_Generation.md)** — World Gen V2, Default_Flat, Default_Void, ore refactor.
- **[World Configuration](160_World_Configuration.md)** — World Gen V2, new templates.
- **[Procedural World Generation](181_Procedural_World_Generation.md)** — World Gen V2 note.
- **[Scripted Brushes](45_Scripted_Brushes.md)** — JumpIfBlockType single-offset change.
- **[Using Commands In-Game](171_Using_Commands_In_Game.md)** — `/update` server commands.
- **[System Integration](177_System_Integration.md)** — Pickaxe tiers (Copper T2, Iron T4, Thorium T6).
- **[Advanced Techniques](176_Advanced_Techniques.md)** — Pickaxe progression footnote.
- **[Drop Tables](105_Drop_Tables.md)** — Bison/Boar/Cow/Horse/Warthog → Medium Hide.
- **[Gathering Types](91_Gathering_Types.md)** — Adamantite, tier-based mining damage.
- **[Materials & Resources](19_Materials_and_Resources.md)** — Mining tiers, Adamantite requirement.
- **[NPCs](03_NPCs.md)** — Hide drops (Creating Drops).
- **[Builder Tools Guide](172_Builder_Tools_Guide.md)** — Scripted brush QoL, hotbar visibility.

---

## Official patch notes

Full notes: [Hytale Patch Notes - Update 2](https://hytale.com/news/2026/1/hytale-patch-notes-update-2).

**See also:** [README](../README.md) · [Getting Started](01_Getting_Started.md)
