# Creating Commands

Learn how to create custom macro commands that execute sequences of built-in commands.

## Overview

Macro commands in Hytale allow you to create custom commands that execute sequences of built-in commands. They're defined in `Server/MacroCommands/` and can accept parameters, have aliases, and chain multiple commands together.

## Location
`Server/MacroCommands/`

## Example from Game Files

### Heal Command

From `Server/MacroCommands/HealCommand.json`:

```1:8:Server/MacroCommands/HealCommand.json
{
  "Name": "heal",
  "Description": "Heals up to your max stamina and health",
  "Commands": [
    "player stat settomax Stamina",
    "player stat settomax Health"
  ]
}
```

This shows a macro command that executes multiple stat commands to heal the player.

## Basic Macro Command Structure

Create `Server/MacroCommands/MyCustom_Command.json`:

```json
{
  "Name": "mycustom",
  "Description": "Description of what this command does",
  "Commands": [
    "time set noon",
    "player stat settomax Health"
  ]
}
```

## Macro Command Properties

### Name

```json
{
  "Name": "heal"
}
```

Command name (used as `/heal`).

### Description

```json
{
  "Description": "Heals up to your max stamina and health"
}
```

Description shown in command help.

### Commands

```json
{
  "Commands": [
    "player stat settomax Stamina",
    "player stat settomax Health"
  ]
}
```

Array of commands executed in sequence.

### Aliases

```json
{
  "Aliases": [
    "/del",
    "/d"
  ]
}
```

Alternative names for the command.

## Simple Commands

### Heal Command

`Server/MacroCommands/HealCommand.json`:

```json
{
  "Name": "heal",
  "Description": "Heals up to your max stamina and health",
  "Commands": [
    "player stat settomax Stamina",
    "player stat settomax Health"
  ]
}
```

Usage: `/heal`

### Delete Command

`Server/MacroCommands/DeleteCommand.json`:

```json
{
  "Name": "delete",
  "Description": "Sets the current selection to empty / deletes things.",
  "Commands": [
    "set Empty"
  ],
  "Aliases": [
    "/del",
    "/d"
  ]
}
```

Usage: `/delete`, `/del`, or `/d`

### Noon Command

`Server/MacroCommands/NoonCommand.json`:

```json
{
  "Name": "noon",
  "Description": "Sets the time to noon and pauses",
  "Commands": [
    "time set noon",
    "time pause"
  ]
}
```

Usage: `/noon`

## Commands with Parameters

### Parameter Structure

```json
{
  "Parameters": [
    {
      "Name": "timeOfDay",
      "Description": "The time of day to set the world to",
      "Requirement": "Required",
      "ArgType": "Integer"
    },
    {
      "Name": "world",
      "Description": "The world to tp to",
      "Requirement": "Default",
      "ArgType": "World",
      "DefaultValue": "Default",
      "DefaultValueDescription": "The default world"
    }
  ]
}
```

### Parameter Properties

- **`Name`** - Parameter name (used in command strings as `{name}`)
- **`Description`** - Parameter description
- **`Requirement`** - `"Required"` or `"Default"` (optional with default)
- **`ArgType`** - Parameter type (`"Integer"`, `"World"`, `"String"`, etc.)
- **`DefaultValue`** - Default value if optional
- **`DefaultValueDescription`** - Description of default value

### Using Parameters in Commands

```json
{
  "Commands": [
    "time set {timeOfDay}",
    "wait 2",
    "tp home --world={world}"
  ]
}
```

Parameters are referenced using `{parameterName}`.

### Complete Example with Parameters

`Server/MacroCommands/_Examples/SetTimeAndGoHome.json`:

```json
{
  "Name": "setTimeAndGoHome",
  "Aliases": ["macrotest"],
  "Description": "Sets the time of day and teleports the user",
  "Parameters": [
    {
      "Name": "timeOfDay",
      "Description": "The time of day to set the world to",
      "Requirement": "Required",
      "ArgType": "Integer"
    },
    {
      "Name": "world",
      "Description": "The world to tp to",
      "Requirement": "Default",
      "ArgType": "World",
      "DefaultValue": "Default",
      "DefaultValueDescription": "The default world"
    }
  ],
  "Commands": [
    "time set {timeOfDay}",
    "wait 2",
    "tp home --world={world}"
  ]
}
```

Usage: `/setTimeAndGoHome 12` or `/setTimeAndGoHome 12 MyWorld`

## Common Built-in Commands

### Time Commands

- **`time set {hour}`** - Set time (0-24)
- **`time set noon`** - Set to noon
- **`time set midnight`** - Set to midnight
- **`time pause`** - Pause time
- **`time unpause`** - Unpause time

### Player Commands

- **`player stat settomax {StatName}`** - Set stat to maximum
- **`player stat set {StatName} {value}`** - Set stat to value
- **`player stat add {StatName} {value}`** - Add to stat

### Teleport Commands

- **`tp home`** - Teleport to home
- **`tp top`** - Teleport to top of world
- **`tp ~ ~ ~`** - Teleport to current position (with rotation flags)
- **`tp {x} {y} {z}`** - Teleport to coordinates
- **`tp {x} {y} {z} --pitch={p} --yaw={y} --roll={r}`** - Teleport with rotation

### Wait Command

- **`wait {seconds}`** - Wait before next command

### Set Command

- **`set Empty`** - Clear selection

## Parameter Types

### Integer

```json
{
  "Name": "timeOfDay",
  "ArgType": "Integer"
}
```

Numeric value.

### World

```json
{
  "Name": "world",
  "ArgType": "World"
}
```

World identifier.

### String

```json
{
  "Name": "message",
  "ArgType": "String"
}
```

Text string.

## Complete Examples

### Example 1: Simple Heal Command

`Server/MacroCommands/MyCustom_Heal.json`:

```json
{
  "Name": "myheal",
  "Description": "Fully heals the player",
  "Commands": [
    "player stat settomax Health",
    "player stat settomax Stamina",
    "player stat settomax Mana"
  ]
}
```

Usage: `/myheal`

### Example 2: Command with Required Parameter

`Server/MacroCommands/SetHealth.json`:

```json
{
  "Name": "sethealth",
  "Description": "Sets player health to specified value",
  "Parameters": [
    {
      "Name": "health",
      "Description": "Health value to set",
      "Requirement": "Required",
      "ArgType": "Integer"
    }
  ],
  "Commands": [
    "player stat set Health {health}"
  ]
}
```

Usage: `/sethealth 50`

### Example 3: Command with Optional Parameter

`Server/MacroCommands/TeleportHome.json`:

```json
{
  "Name": "home",
  "Description": "Teleports to home in specified world",
  "Parameters": [
    {
      "Name": "world",
      "Description": "World to teleport to",
      "Requirement": "Default",
      "ArgType": "World",
      "DefaultValue": "Default",
      "DefaultValueDescription": "Default world"
    }
  ],
  "Commands": [
    "tp home --world={world}"
  ]
}
```

Usage: `/home` or `/home MyWorld`

### Example 4: Complex Command Chain

`Server/MacroCommands/ResetAndHeal.json`:

```json
{
  "Name": "reset",
  "Description": "Resets player to full health and teleports home",
  "Aliases": ["respawn"],
  "Commands": [
    "player stat settomax Health",
    "player stat settomax Stamina",
    "wait 1",
    "tp home"
  ]
}
```

Usage: `/reset` or `/respawn`

## Command Execution Order

Commands execute sequentially:

```json
{
  "Commands": [
    "time set noon",      // Executes first
    "wait 2",             // Waits 2 seconds
    "tp home"             // Executes after wait
  ]
}
```

## Tips for Creating Commands

1. **Use descriptive names** - Command names should be clear
2. **Add descriptions** - Help users understand what commands do
3. **Use aliases** - Provide shortcuts for common commands
4. **Chain commands** - Combine multiple commands for complex actions
5. **Use wait** - Add delays between commands if needed
6. **Test thoroughly** - Ensure commands work as expected
7. **Document parameters** - Clear parameter descriptions help users

## Related Guides

- **[Using Commands In-Game](171_Using_Commands_In_Game.md)** - Complete guide for players on using console commands
- **[Macro Commands](167_Macro_Commands.md)** - Advanced macro command reference

---

**Previous:** [Block Sound Sets](40_Block_Sound_Sets.md) | **Next:** Back to [README](../README.md)
