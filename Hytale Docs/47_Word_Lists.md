# Word Lists

Learn how to create word lists for dynamic name generation and randomization.

## Overview

Word lists provide collections of words used for generating dynamic names, text, and labels. They're used by various game systems that need randomized or procedural text generation. Word lists are defined in `Server/WordLists/` and can be referenced by items, NPCs, objectives, and other systems.

## Location
`Server/WordLists/`

## Basic Word List Structure

Create `Server/WordLists/MyCustom_Names.json`:

```json
{
  "TranslationKeys": [
    "word1",
    "word2",
    "word3",
    "word4"
  ]
}
```

## Word List Properties

### TranslationKeys

```json
{
  "TranslationKeys": [
    "feyun",
    "urox",
    "thuris",
    "ansur"
  ]
}
```

Array of translation keys or literal words used for name generation.

## Common Word Lists

### Runes

`Server/WordLists/Runes.json`:

```json
{
  "TranslationKeys": [
    "feyun",
    "urox",
    "thuris",
    "ansur",
    "katapo",
    "kinas",
    "geboan",
    "zunjo",
    "latal",
    "naudiz",
    "issa",
    "jeran",
    "eihwas",
    "pertho",
    "algas",
    "solas",
    "tyrin",
    "berkan",
    "woz",
    "manala",
    "lagus",
    "inguz",
    "othaka",
    "digas"
  ]
}
```

Ancient rune names used for spell names, item names, or lore.

## Using Word Lists

### Direct Translation Keys

Word lists can contain translation keys that reference language files:

`Server/WordLists/MyCustom_Names.json`:

```json
{
  "TranslationKeys": [
    "server.names.myCustom.name1",
    "server.names.myCustom.name2",
    "server.names.myCustom.name3"
  ]
}
```

Then in `Server/Languages/en-US/server.lang`:

```
server.names.myCustom.name1 = Excalibur
server.names.myCustom.name2 = Caliburn
server.names.myCustom.name3 = Durandal
```

### Literal Words

Word lists can also contain literal words (not translation keys):

`Server/WordLists/Weapon_Names.json`:

```json
{
  "TranslationKeys": [
    "Blade",
    "Sword",
    "Edge",
    "Sharp"
  ]
}
```

Note: Even when using literal words, they're stored in the `TranslationKeys` array.

## Complete Example: Custom Name Generation

### 1. Word List Definition

`Server/WordLists/Prefixes.json`:

```json
{
  "TranslationKeys": [
    "Ancient",
    "Mythic",
    "Forgotten",
    "Eternal",
    "Sacred"
  ]
}
```

`Server/WordLists/Suffixes.json`:

```json
{
  "TranslationKeys": [
    "Blade",
    "Sword",
    "Edge",
    "Fang",
    "Claw"
  ]
}
```

### 2. Using in Item Generation

Items can reference word lists for randomized names:

```json
{
  "TranslationProperties": {
    "Name": {
      "WordListIds": ["Prefixes", "Suffixes"],
      "Format": "{0} {1}"
    }
  }
}
```

This would generate names like:
- "Ancient Blade"
- "Mythic Sword"
- "Forgotten Edge"

### 3. Using in NPC Names

NPCs can use word lists for randomized names:

```json
{
  "NameTranslationKey": {
    "WordListIds": ["NPC_FirstNames", "NPC_LastNames"],
    "Format": "{0} {1}"
  }
}
```

## Language Files

Word lists that use translation keys require entries in language files:

`Server/Languages/en-US/server.lang`:

```
server.names.prefixes.ancient = Ancient
server.names.prefixes.mythic = Mythic
server.names.suffixes.blade = Blade
server.names.suffixes.sword = Sword
```

## Tips for Creating Word Lists

1. **Organize by category** - Group related words together (names, adjectives, nouns)
2. **Use translation keys** - For multilingual support, use translation keys
3. **Keep lists focused** - Each list should have a specific purpose
4. **Add variety** - Include enough words for good randomization
5. **Test generation** - Verify word combinations work well together
6. **Document usage** - Note which systems use each word list
7. **Consider themes** - Match word lists to your mod's theme and style

---

**Previous:** [Portal Types](46_Portal_Types.md) | **Next:** Back to [README](../README.md)
