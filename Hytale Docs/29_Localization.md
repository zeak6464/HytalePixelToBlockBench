# Localization & Translations

Learn how to create translations for items, NPCs, UI, and other game text.

## Overview

Hytale uses translation keys in JSON files and `.lang` files for text. Translation keys reference entries in language files that provide localized text for different languages.

## Location
- Language files: `Server/Languages/{locale}/server.lang`
- Common languages: `en-US`, `de-DE`, `es-ES`, `fr-FR`, `ru-RU`, `pt-BR`

## Example from Game Files

### Language File

From `Server/Languages/en-US/server.lang`:

```
server.items.Ore_Iron.name=Iron Ore
server.items.Ore_Iron.description=A common metal ore found throughout Zone 1.
server.items.Weapon_Sword_Iron.name=Iron Sword
server.items.Weapon_Sword_Iron.description=A basic melee weapon crafted from iron.
```

This shows how translation keys map to localized text strings.

## Translation Keys in JSON

Items, NPCs, and other assets use `TranslationProperties` to reference translation keys:

```json
{
  "TranslationProperties": {
    "Name": "server.items.MyCustom_Sword.name",
    "Description": "server.items.MyCustom_Sword.description"
  }
}
```

### Common Translation Properties

- **`Name`** - Display name (required for most items)
- **`Description`** - Item/NPC description (optional)

## Language File Format

Language files use a simple key-value format:

`Server/Languages/en-US/server.lang`:

```
server.items.MyCustom_Sword.name = My Custom Sword
server.items.MyCustom_Sword.description = A powerful blade forged by the ancients.

server.npcRoles.MyCustom_Goblin.name = My Custom Goblin
server.npcRoles.MyCustom_Goblin.description = A fierce warrior.

server.interactionHints.harvest = Harvest
server.interactionHints.use = Use
```

### Translation Key Format

Keys use dot notation:

```
{category}.{subcategory}.{itemId}.{property}
```

Common categories:
- `server.items` - Items
- `server.npcRoles` - NPCs
- `server.interactionHints` - Interaction hints
- `general.qualities` - Item quality names
- `avatarCustomization` - Cosmetic items

## Adding Translations to Items

### Basic Item Translation

```json
{
  "TranslationProperties": {
    "Name": "server.items.MyCustom_Sword.name",
    "Description": "server.items.MyCustom_Sword.description"
  }
}
```

In language file:

```
server.items.MyCustom_Sword.name = My Custom Sword
server.items.MyCustom_Sword.description = A powerful blade with ancient runes.
```

### Using Existing Keys

You can reference existing translation keys:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Template_Weapon_Sword.name"
  }
}
```

## Adding Translations to NPCs

```json
{
  "TranslationProperties": {
    "Name": "server.npcRoles.MyCustom_Goblin.name",
    "Description": "server.npcRoles.MyCustom_Goblin.description"
  }
}
```

In language file:

```
server.npcRoles.MyCustom_Goblin.name = My Custom Goblin
server.npcRoles.MyCustom_Goblin.description = A fierce warrior of the mountains.
```

## UI Translation Keys

UI files use `%` prefix for translation keys:

```ui
Label {
  Text: %server.customUI.myPage.title;
}

TextButton {
  Text: %server.customUI.myPage.saveButton;
}
```

In language file:

```
server.customUI.myPage.title = My Custom Page
server.customUI.myPage.saveButton = Save
server.customUI.myPage.cancelButton = Cancel
```

## Parameter Substitution

Translation keys can include parameters using `{param}`:

```
general.pickedUpItem = {item}
general.killedBy = You were killed by {damageSource}!
general.teleport.teleportedTo = Teleported to {username}
```

### Number Formatting

```
formatting.list.header = {header} ({count, number}):
```

The `{count, number}` syntax formats numbers with locale-specific formatting.

## Common Translation Categories

### Item Qualities

```
general.qualities.Common = Common
general.qualities.Uncommon = Uncommon
general.qualities.Rare = Rare
general.qualities.Epic = Epic
general.qualities.Legendary = Legendary
```

### Interaction Hints

```
server.interactionHints.use = Use
server.interactionHints.harvest = Harvest
server.interactionHints.open = Open
server.interactionHints.sit = Sit
server.interactionHints.place = Place
```

### Game Modes

```
general.gamemodes.adventure = Adventure
general.gamemodes.creative = Creative
```

### Damage Causes

```
general.damageCauses.fall = falling
general.damageCauses.drowning = drowning
general.damageCauses.burn = burning
general.damageCauses.poison_t1 = poison damage
```

## Multi-Language Support

Create language files for each locale:

### English (en-US)

`Server/Languages/en-US/server.lang`:

```
server.items.MyCustom_Sword.name = My Custom Sword
server.items.MyCustom_Sword.description = A powerful blade.
```

### German (de-DE)

`Server/Languages/de-DE/server.lang`:

```
server.items.MyCustom_Sword.name = Mein Benutzerdefiniertes Schwert
server.items.MyCustom_Sword.description = Eine mächtige Klinge.
```

### Spanish (es-ES)

`Server/Languages/es-ES/server.lang`:

```
server.items.MyCustom_Sword.name = Mi Espada Personalizada
server.items.MyCustom_Sword.description = Una hoja poderosa.
```

### Language Fallback

`Server/Languages/fallback.lang` defines fallback mappings:

```
de-AT = de-DE
de-CH = de-DE
en-GB = en-US
en-CA = en-US
es-MX = es-ES
fr-CA = fr-FR
```

## Translation Key Best Practices

### Naming Conventions

Use consistent naming:

```
server.items.{ItemId}.name
server.items.{ItemId}.description
server.npcRoles.{NPCRoleId}.name
server.npcRoles.{NPCRoleId}.description
```

### Organization

Group related translations:

```
# === Items ===
server.items.Sword_Iron.name = Iron Sword
server.items.Sword_Steel.name = Steel Sword

# === NPCs ===
server.npcRoles.Goblin_Warrior.name = Goblin Warrior
server.npcRoles.Goblin_Shaman.name = Goblin Shaman
```

### Comments in Language Files

Language files support comments:

```
# Custom Items
server.items.MyCustom_Sword.name = My Custom Sword

# Custom NPCs
server.npcRoles.MyCustom_Goblin.name = My Custom Goblin
```

## Complete Example: Full Localization

### 1. Item Definition

`Server/Item/Items/Weapon/Weapon_MyCustom_Sword.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Weapon_MyCustom_Sword.name",
    "Description": "server.items.Weapon_MyCustom_Sword.description"
  },
  "Quality": "Rare"
}
```

### 2. Language Files

`Server/Languages/en-US/server.lang`:

```
# === Custom Items ===
server.items.Weapon_MyCustom_Sword.name = My Custom Sword
server.items.Weapon_MyCustom_Sword.description = A powerful blade forged by ancient smiths.
```

`Server/Languages/de-DE/server.lang`:

```
# === Custom Items ===
server.items.Weapon_MyCustom_Sword.name = Mein Benutzerdefiniertes Schwert
server.items.Weapon_MyCustom_Sword.description = Eine mächtige Klinge, geschmiedet von alten Schmieden.
```

`Server/Languages/es-ES/server.lang`:

```
# === Custom Items ===
server.items.Weapon_MyCustom_Sword.name = Mi Espada Personalizada
server.items.Weapon_MyCustom_Sword.description = Una hoja poderosa forjada por herreros antiguos.
```

## Tips for Creating Translations

1. **Use consistent naming** - Follow `server.items.{ItemId}.name` pattern
2. **Include descriptions** - Add helpful item/NPC descriptions
3. **Test all languages** - Ensure translations work in all supported locales
4. **Use parameters** - Leverage `{param}` for dynamic text
5. **Organize with comments** - Group related translations
6. **Reference existing keys** - Reuse common translations when possible
7. **Keep keys short** - Use descriptive but concise keys

---

**Previous:** [Entity Stats](28_Entity_Stats.md) | **Next:** [Tags System](30_Tags_System.md)
