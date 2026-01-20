# Prefab Lists

Learn how to organize prefabs into lists for world generation and prefab management.

## Overview

Prefab lists organize prefabs by grouping directory paths. They're used in world generation to reference collections of prefabs (e.g., all bamboo trees, all oak trees) without listing individual prefabs. Prefab lists make it easy to manage large collections of prefabs.

## Location
`Server/PrefabList/`

## Example from Game Files

### Oak Tree Prefab List

From `Server/PrefabList/Trees_Oak.json`:

```1:53:Server/PrefabList/Trees_Oak.json
{
    "Prefabs": [
        {
            "RootDirectory": "Asset",
            "Path": "Trees/Oak/Stage_0/",
            "Recursive": true
        },
        {
            "RootDirectory": "Asset",
            "Path": "Trees/Oak/Stage_00/",
            "Recursive": true
        },
        {
            "RootDirectory": "Asset",
            "Path": "Trees/Oak/Stage_1/",
            "Recursive": true
        },
        {
            "RootDirectory": "Asset",
            "Path": "Trees/Oak/Stage_2/",
            "Recursive": true
        },
        {
            "RootDirectory": "Asset",
            "Path": "Trees/Oak/Stumps/",
            "Recursive": true
        }
    ]
}
```

This shows a prefab list that groups oak tree prefabs by directory path for world generation.

## Basic Prefab List Structure

Create `Server/PrefabList/MyCustom_Trees.json`:

```json
{
  "Prefabs": [
    {
      "RootDirectory": "Asset",
      "Path": "Trees/MyCustom/",
      "Recursive": true
    }
  ]
}
```

## Prefab List Properties

### Prefabs Array

```json
{
  "Prefabs": [
    {
      "RootDirectory": "Asset",
      "Path": "Trees/Bamboo/Giant/",
      "Recursive": true
    }
  ]
}
```

### RootDirectory

```json
{
  "RootDirectory": "Asset"
}
```

The root directory type:
- **`"Asset"`** - Assets directory (prefabs in `Server/Prefabs/`)

### Path

```json
{
  "Path": "Trees/Bamboo/Giant/"
}
```

Directory path relative to the root directory. Paths use forward slashes `/`.

### Recursive

```json
{
  "Recursive": true
}
```

Whether to include subdirectories:
- **`true`** - Includes all prefabs in subdirectories
- **`false`** - Only includes prefabs in the specified directory

## Common Prefab Lists

### Trees - Bamboo

`Server/PrefabList/Trees_Bamboo.json`:

```json
{
  "Prefabs": [
    {
      "RootDirectory": "Asset",
      "Path": "Trees/Bamboo/Giant/",
      "Recursive": true
    },
    {
      "RootDirectory": "Asset",
      "Path": "Trees/Bamboo/Stage_0/",
      "Recursive": true
    },
    {
      "RootDirectory": "Asset",
      "Path": "Trees/Bamboo/Stage_1/",
      "Recursive": true
    },
    {
      "RootDirectory": "Asset",
      "Path": "Trees/Bamboo/Stage_2/",
      "Recursive": true
    },
    {
      "RootDirectory": "Asset",
      "Path": "Trees/Bamboo/Stage_3/",
      "Recursive": true
    }
  ]
}
```

### Trees - Oak

`Server/PrefabList/Trees_Oak.json`:

```json
{
  "Prefabs": [
    {
      "RootDirectory": "Asset",
      "Path": "Trees/Oak/Giant/",
      "Recursive": true
    },
    {
      "RootDirectory": "Asset",
      "Path": "Trees/Oak/Large/",
      "Recursive": true
    },
    {
      "RootDirectory": "Asset",
      "Path": "Trees/Oak/Medium/",
      "Recursive": true
    },
    {
      "RootDirectory": "Asset",
      "Path": "Trees/Oak/Small/",
      "Recursive": true
    }
  ]
}
```

### Trees - All

`Server/PrefabList/Trees.json`:

```json
{
  "Prefabs": [
    {
      "RootDirectory": "Asset",
      "Path": "Trees/",
      "Recursive": true
    }
  ]
}
```

Includes all prefabs in the `Trees/` directory and subdirectories.

## Using Prefab Lists

### In World Generation

Prefab lists are referenced in world generation configurations:

```json
{
  "WorldStructures": {
    "PrefabLists": [
      "Trees_Oak",
      "Trees_Bamboo"
    ]
  }
}
```

### In Biome Configuration

Biomes can use prefab lists for vegetation:

```json
{
  "Biome": "Forest",
  "Vegetation": {
    "PrefabLists": [
      "Trees_Oak",
      "Trees_Birch"
    ],
    "Density": 0.1
  }
}
```

### In Prefab Spawners

Block spawners can reference prefab lists:

```json
{
  "BlockSpawner": {
    "PrefabList": "Trees_Oak",
    "SpawnRate": 0.05
  }
}
```

## Complete Example: Custom Tree Collection

### 1. Organize Prefabs

Place your tree prefabs in organized directories:

```
Server/Prefabs/
└── Trees/
    └── MyCustom/
        ├── Stage_0/
        │   ├── Tree_Small.prefab.json
        │   └── Tree_Tiny.prefab.json
        ├── Stage_1/
        │   └── Tree_Medium.prefab.json
        ├── Stage_2/
        │   └── Tree_Large.prefab.json
        └── Stage_3/
            └── Tree_Giant.prefab.json
```

### 2. Create Prefab List

`Server/PrefabList/Trees_MyCustom.json`:

```json
{
  "Prefabs": [
    {
      "RootDirectory": "Asset",
      "Path": "Trees/MyCustom/Stage_0/",
      "Recursive": true
    },
    {
      "RootDirectory": "Asset",
      "Path": "Trees/MyCustom/Stage_1/",
      "Recursive": true
    },
    {
      "RootDirectory": "Asset",
      "Path": "Trees/MyCustom/Stage_2/",
      "Recursive": true
    },
    {
      "RootDirectory": "Asset",
      "Path": "Trees/MyCustom/Stage_3/",
      "Recursive": true
    }
  ]
}
```

### 3. Use in World Generation

Reference in world generation:

```json
{
  "Biome": "MyCustom_Forest",
  "Trees": {
    "PrefabLists": ["Trees_MyCustom"],
    "Density": 0.08
  }
}
```

## Multiple Directory Entries

Prefab lists can include multiple directories:

```json
{
  "Prefabs": [
    {
      "RootDirectory": "Asset",
      "Path": "Structures/Houses/",
      "Recursive": true
    },
    {
      "RootDirectory": "Asset",
      "Path": "Structures/Buildings/",
      "Recursive": true
    },
    {
      "RootDirectory": "Asset",
      "Path": "Structures/Ruins/",
      "Recursive": false
    }
  ]
}
```

## Tips for Creating Prefab Lists

1. **Organize by category** - Group related prefabs (trees, buildings, decorations)
2. **Use descriptive names** - Name lists clearly (e.g., `Trees_Oak` not `List1`)
3. **Use recursive** - Set `Recursive: true` to include subdirectories automatically
4. **Multiple directories** - Combine multiple directories in one list
5. **Reference in generation** - Use prefab lists in world generation configs
6. **Keep updated** - Update lists when adding/removing prefabs
7. **Test placement** - Verify prefabs from lists spawn correctly

---

**Previous:** [Tag Patterns](48_Tag_Patterns.md) | **Next:** [Block Type Lists](50_Block_Type_Lists.md)
