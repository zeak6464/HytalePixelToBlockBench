# Tag Patterns

Learn how to create advanced tag pattern matching using logical operations for complex tag queries.

## Overview

Tag patterns provide advanced pattern matching for tags using logical operations (Or, And, Equals). They're used when simple tag matching isn't sufficient, such as crops that can grow on multiple block types or items with complex tag requirements.

## Location
`Server/TagPatterns/`

## Example from Game Files

### Soil or Grass Tag Pattern

From `Server/TagPatterns/Soil_Or_Grass.json`:

```1:7:Server/TagPatterns/Soil_Or_Grass.json
{
	"Op": "Or",
	"Patterns": [{
		"Op": "Equals",
		"Tag": "Type=Soil"
	}]
}
```

This shows a tag pattern using the Or operator to match blocks with either Soil or Grass tags.

## Basic Tag Pattern Structure

Create `Server/TagPatterns/MyCustom_Pattern.json`:

```json
{
  "Op": "Or",
  "Patterns": [
    {
      "Op": "Equals",
      "Tag": "Type=Soil"
    },
    {
      "Op": "Equals",
      "Tag": "Type=Grass"
    }
  ]
}
```

## Tag Pattern Operations

### Equals

Matches a specific tag:

```json
{
  "Op": "Equals",
  "Tag": "Vine"
}
```

or with tag category:

```json
{
  "Op": "Equals",
  "Tag": "Type=Soil"
}
```

### Or

Matches if any pattern is true:

```json
{
  "Op": "Or",
  "Patterns": [
    {
      "Op": "Equals",
      "Tag": "Bush"
    },
    {
      "Op": "Equals",
      "Tag": "Seed"
    }
  ]
}
```

### And

Matches if all patterns are true:

```json
{
  "Op": "And",
  "Patterns": [
    {
      "Op": "Equals",
      "Tag": "Type=Rock"
    },
    {
      "Op": "Equals",
      "Tag": "Family=Stone"
    }
  ]
}
```

## Common Tag Patterns

### Soil or Grass

`Server/TagPatterns/Soil_Or_Grass.json`:

```json
{
  "Op": "Or",
  "Patterns": [
    {
      "Op": "Equals",
      "Tag": "Type=Soil"
    }
  ]
}
```

Used by crops that can grow on soil or grass blocks.

### Bush or Seed

`Server/TagPatterns/Bush_Or_Seed.json`:

```json
{
  "Op": "Or",
  "Patterns": [
    {
      "Op": "Equals",
      "Tag": "Bush"
    },
    {
      "Op": "Equals",
      "Tag": "Seed"
    }
  ]
}
```

Matches items tagged as either Bush or Seed.

### Vine

`Server/TagPatterns/Vine.json`:

```json
{
  "Op": "Equals",
  "Tag": "Vine"
}
```

Simple pattern matching a single tag.

### Cactus Growth

`Server/TagPatterns/CactusGrowth.json`:

```json
{
  "Op": "Equals",
  "Tag": "CactusGrowth"
}
```

Specific tag for cactus growth requirements.

## Using Tag Patterns

### In Crop Definitions

Crops use tag patterns to specify compatible blocks:

```json
{
  "Farming": {
    "Stages": [
      {
        "BlockTagPattern": "Soil_Or_Grass",
        "DurationMinutes": 30
      }
    ]
  }
}
```

### In Item Conditions

Items can check tag patterns:

```json
{
  "Conditions": [
    {
      "Type": "BlockTagPattern",
      "TagPatternId": "Soil_Or_Grass"
    }
  ]
}
```

### In Block Requirements

Blocks can require tag patterns:

```json
{
  "Requirements": {
    "BaseBlockTagPattern": "Soil_Or_Grass"
  }
}
```

## Complete Example: Custom Crop Pattern

### 1. Tag Pattern Definition

`Server/TagPatterns/Custom_Soil_Or_Sand.json`:

```json
{
  "Op": "Or",
  "Patterns": [
    {
      "Op": "Equals",
      "Tag": "Type=Soil"
    },
    {
      "Op": "Equals",
      "Tag": "Type=Sand"
    }
  ]
}
```

### 2. Using in Crop

`Server/Item/Items/Plant/Crop/Plant_Crop_MyCustom.json`:

```json
{
  "TranslationProperties": {
    "Name": "server.items.Plant_Crop_MyCustom.name"
  },
  "Categories": ["Blocks.Plants"],
  "BlockType": {
    "Material": "Plant",
    "DrawType": "Model",
    "CustomModel": "Blocks/Farming/MyCustom/Stage_0.blockymodel",
    "CustomModelTexture": [
      {
        "Texture": "Blocks/Farming/MyCustom/Stage_0.png",
        "Weight": 1
      }
    ]
  },
  "Farming": {
    "Stages": [
      {
        "BlockTagPattern": "Custom_Soil_Or_Sand",
        "DurationMinutes": 20,
        "BlockChange": {
          "BlockType": "Plant_Crop_MyCustom_Stage_1"
        }
      }
    ]
  }
}
```

## Complex Nested Patterns

### Multiple Conditions

```json
{
  "Op": "And",
  "Patterns": [
    {
      "Op": "Or",
      "Patterns": [
        {
          "Op": "Equals",
          "Tag": "Type=Soil"
        },
        {
          "Op": "Equals",
          "Tag": "Type=Sand"
        }
      ]
    },
    {
      "Op": "Equals",
      "Tag": "Family=Wet"
    }
  ]
}
```

Matches blocks that are (Soil OR Sand) AND have the Wet family tag.

## Tips for Creating Tag Patterns

1. **Use for complex logic** - Tag patterns are for cases where simple tags aren't enough
2. **Keep it simple** - Start with Equals, add Or/And only when needed
3. **Test patterns** - Verify patterns match intended blocks/items
4. **Use in crops** - Tag patterns are commonly used for crop growth requirements
5. **Document purpose** - Note what each pattern is used for
6. **Reuse patterns** - Create reusable patterns for common combinations

---

**Previous:** [Word Lists](47_Word_Lists.md) | **Next:** [Prefab Lists](49_Prefab_Lists.md)
