# Interaction Type: UseCoop

Interact with coop blocks (collect produce, manage animals).

## Overview

`UseCoop` triggers coop block interactions for collecting produce and managing animals. Used when interacting with chicken coops and other animal housing blocks.

## Example from Game Files

### Chicken Coop Interaction

From `Server/Item/Items/Coops/Coop_Chicken.json`:

```json
{
  "BlockType": {
    "Interactions": {
      "Use": {
        "Interactions": [
          {
            "Type": "UseCoop"
          }
        ]
      }
    }
  }
}
```

## Basic Structure

```json
{
  "Type": "UseCoop"
}
```

## Properties

`UseCoop` is a simple interaction type with no additional properties. The behavior is determined by the coop block's configuration.

## Coop Block Configuration

The coop block defines what `UseCoop` does:

```json
{
  "BlockType": {
    "BlockEntity": {
      "Components": {
        "Coop": {
          "ProduceItem": "Food_Egg",
          "ProduceInterval": 12000,
          "MaxCapacity": 3,
          "AnimalType": "Chicken"
        }
      }
    },
    "Interactions": {
      "Use": {
        "Interactions": [
          { "Type": "UseCoop" }
        ]
      }
    }
  }
}
```

## Use Cases

- **Egg Collection** - Collect eggs from chicken coops
- **Animal Products** - Collect milk, wool, etc.
- **Coop Management** - Interact with coop UI

## Related

For complete coop configuration:
- [Farming Coops](107_Farming_Coops.md) - Complete coop documentation
- Coop block configurations in `Server/Item/Items/Coops/`

---

**Previous:** [UseEntity](154_Interaction_Type_UseEntity.md) | **Next:** [ResetCooldown](156_Interaction_Type_ResetCooldown.md)
