# Modding Languages & Scripting

What Hytale uses for modding: **Java** (plugins), **JSON** (data assets), and **visual scripting** (coming soon). **JavaScript is not used.**

---

## Does Hytale use JavaScript?

**No.** Hytale modding does **not** use JavaScript. **Java** plugins provide custom logic; game content uses **JSON** configs. A **node-based visual scripting** system (similar to Unreal Blueprints) is planned but not yet available.

---

## What Hytale Uses

### 1. Java — Plugins

- **Role:** Custom commands, events, game logic, minigames, economy, admin tools.
- **Format:** Java code → `.jar` plugins (e.g. in a `plugins/` folder; see [Hytale Modding](https://hytalemodding.dev/en/docs/guides/plugin/setting-up-env)).
- **Docs:** [Java Plugins](183_Java_Server_Plugins.md) (setup, Maven, template).  
- **Official:** [Hytale Modding](https://hytalemodding.dev), [Hytale-Docs Plugins](https://hytale-docs.com/docs/modding/plugins/overview). The setup guide is under **Server Plugins** and uses the Hytale **Server** dependency.

### 2. JSON — Data assets

- **Role:** Items, NPCs, blocks, loot, world gen, recipes, etc. No code.
- **Location:** `Server/` (e.g. `Server/Item/`, `Server/NPC/`). Editable in the **Asset Editor**.
- **Docs:** [Getting Started](01_Getting_Started.md), [Creating Items](02_Items.md), [Creating NPCs](03_NPCs.md), [Server Modding](12_Server_Modding.md), plus the rest of the guide set.

### 3. Visual scripting (coming soon)

- **Role:** Node-based logic (connect nodes, no writing code). Described as Blueprints-like.
- **Status:** In development. Not released yet.
- **Official:** [Hytale Modding Strategy](https://hytale.com/news/2025/11/hytale-modding-strategy-and-status), [Hytale-Docs Modding Overview](https://hytale-docs.com/docs/modding/overview).

---

## Quick reference

| You want…              | Use…           | Guide / resource                                      |
|------------------------|----------------|--------------------------------------------------------|
| Custom commands, logic | **Java**       | [Java Plugins](183_Java_Server_Plugins.md)     |
| Items, NPCs, blocks    | **JSON**       | [02 Items](02_Items.md), [03 NPCs](03_NPCs.md), etc.  |
| Visual “scripting”     | **Nodes**      | Coming soon; see Hytale modding strategy / overview   |
| **JavaScript**         | **Not used**   | Use Java or JSON instead                              |

---

## “Scripted” in Hytale

- **Scripted brushes** = JSON ops in `Server/ScriptedBrushes/` (mask, pattern, set, etc.). See [Scripted Brushes](45_Scripted_Brushes.md). Not JavaScript.
- **Visual scripting** = Future node-based system. Not JavaScript.

---

## Official links

- [Hytale Modding Strategy](https://hytale.com/news/2025/11/hytale-modding-strategy-and-status)
- [Hytale-Docs Modding Overview](https://hytale-docs.com/docs/modding/overview)
- [Hytale Modding](https://hytalemodding.dev) — Java plugins, setup, tutorials

---

**Previous:** [Java Plugins](183_Java_Server_Plugins.md) | **Next:** [Getting Started](01_Getting_Started.md)


**See also:** [Java Plugins](183_Java_Server_Plugins.md) · [Server Modding](12_Server_Modding.md) · [Scripted Brushes](45_Scripted_Brushes.md)
