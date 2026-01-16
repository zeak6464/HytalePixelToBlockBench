# State Evaluators

Learn how NPC state evaluators automatically change NPC states based on conditions.

## Overview

State evaluators automatically transition NPCs between states based on conditions like time of day, proximity, or other game state. They're an alternative to sensor-based state changes.

## Location
State evaluators are configured in `StateEvaluator` in NPC Role definitions.

## Basic State Evaluator Structure

```json
{
  "StateEvaluator": {
    "Options": [
      {
        "State": "Idle",
        "Description": "Go to idle state",
        "Conditions": [
          {
            "Type": "TimeOfDay",
            "Curve": {
              "ResponseCurve": "ConstantMidpoint",
              "XRange": [0, 24]
            }
          }
        ]
      }
    ]
  }
}
```

## State Evaluator Properties

### Options

Array of possible states to evaluate:

```json
{
  "Options": [
    {
      "State": "Sleep",
      "Description": "Go to sleep at night",
      "Conditions": [
        {
          "Type": "TimeOfDay",
          "Curve": {
            "ResponseCurve": "SimpleLogistic",
            "XRange": [12, 24]
          }
        }
      ]
    }
  ]
}
```

### Conditions

```json
{
  "Conditions": [
    {
      "Type": "TimeOfDay",
      "Curve": {
        "ResponseCurve": "SimpleDescendingLogistic",
        "XRange": [0, 12]
      }
    }
  ]
}
```

Conditions that trigger state change. Uses response curves for evaluation.

## Time of Day Evaluation

```json
{
  "StateEvaluator": {
    "Options": [
      {
        "State": "Idle",
        "Conditions": [
          {
            "Type": "TimeOfDay",
            "Curve": {
              "ResponseCurve": "ConstantMidpoint",
              "XRange": [0, 24]
            }
          }
        ]
      },
      {
        "State": "Sleep",
        "Description": "Sleep at night",
        "Conditions": [
          {
            "Type": "TimeOfDay",
            "Curve": {
              "ResponseCurve": "SimpleLogistic",
              "XRange": [12, 24]
            }
          }
        ]
      },
      {
        "State": "Sleep",
        "Description": "Sleep during day",
        "Conditions": [
          {
            "Type": "TimeOfDay",
            "Curve": {
              "ResponseCurve": "SimpleDescendingLogistic",
              "XRange": [0, 12]
            }
          }
        ]
      }
    ]
  }
}
```

NPC sleeps at night (12-24) and during early day (0-12).

## Response Curves

State evaluators use response curves (see Response Curves guide) to evaluate conditions over ranges.

## Tips for State Evaluators

1. **Automatic transitions** - State evaluators change states automatically
2. **Response curves** - Use curves for smooth state transitions
3. **Multiple options** - Evaluator picks best-matching state
4. **Time-based** - Common use is time-of-day based behaviors

---

**Previous:** [Item Entity Config](88_Item_Entity_Config.md) | **Next:** [Block Entity Components](90_Block_Entity_Components.md)
