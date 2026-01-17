# Interaction Type: SendMessage

Send messages to players via chat or UI.

## Overview

`SendMessage` displays messages to players through chat or UI notifications. Useful for feedback, error messages, hints, and information display.

## Basic Structure

```json
{
  "Type": "SendMessage",
  "Key": "server.interactions.teleporter.failed",
  "Message": "Custom message text"
}
```

## Properties

### Key

```json
{
  "Key": "server.interactions.teleporter.failed"
}
```

Translation key for localized message. References language file entry.

### Message

```json
{
  "Message": "Custom message text"
}
```

Direct message text (not localized). Use `Key` for localization instead.

## Complete Examples

### Example 1: Localized Message

```json
{
  "Type": "SendMessage",
  "Key": "server.interactions.teleporter.failed"
}
```

Shows localized message from language files.

### Example 2: Direct Message

```json
{
  "Type": "SendMessage",
  "Message": "Not enough mana!"
}
```

Shows direct message text.

### Example 3: Error Feedback

```json
{
  "Type": "Condition",
  "Condition": {
    "Stat": "Mana",
    "Comparison": "LessThan",
    "Value": 50
  },
  "Failed": {
    "Type": "LaunchProjectile",
    "ProjectileId": "Fireball"
  },
  "Next": {
    "Type": "SendMessage",
    "Key": "server.spells.insufficient.mana"
  }
}
```

Shows error message when mana is insufficient.

## Tips

1. **Use Keys** - Prefer `Key` for localization support
2. **Direct messages** - Use `Message` for development/testing only
3. **Error feedback** - Always provide messages for failed actions
4. **Localization** - Define messages in language files for all supported languages

---

**Previous:** [Consume](137_Interaction_Type_Consume.md) | **Next:** [ChainFlag](139_Interaction_Type_ChainFlag.md)
