# Macro Commands

Learn how to create custom console commands using macro commands in `Server/MacroCommands/`.

## Overview

Macro commands allow you to create custom console commands that execute sequences of game commands. They're useful for combining multiple commands into a single macro, adding aliases for common commands, and creating commands with parameters.

## Location

`Server/MacroCommands/`

## Basic Macro Command Structure

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

## Macro Command Properties

### Name

Command name (required):

```json
{
  "Name": "heal"
}
```

The command will be available as `/heal` in the console.

### Description

Command description (optional):

```json
{
  "Description": "Heals up to your max stamina and health"
}
```

Displayed when the user types `help heal` or similar.

### Commands

Array of commands to execute:

```json
{
  "Commands": [
    "player stat settomax Stamina",
    "player stat settomax Health"
  ]
}
```

Commands execute sequentially in order.

### Aliases

Array of command aliases (optional):

```json
{
  "Name": "setTimeAndGoHome",
  "Aliases": ["macrotest", "sethome"],
  "Commands": [...]
}
```

The command will be available as `/setTimeAndGoHome`, `/macrotest`, and `/sethome`.

### Parameters

Array of command parameters (optional):

```json
{
  "Name": "setTimeAndGoHome",
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

**Parameter Properties:**
- **`Name`** - Parameter name (used as `{name}` in commands)
- **`Description`** - Parameter description (shown in help)
- **`Requirement`** - `"Required"` or `"Default"` (optional with default)
- **`ArgType`** - Parameter type (`"Integer"`, `"String"`, `"World"`, etc.)
- **`DefaultValue`** - Default value if not provided (for `"Default"` parameters)
- **`DefaultValueDescription`** - Description of default value

**Using Parameters:**
Use `{parameterName}` in commands to substitute parameter values:
- `"time set {timeOfDay}"` - Substitutes the `timeOfDay` parameter
- `"tp home --world={world}"` - Substitutes the `world` parameter

## Common Macro Commands

### Heal Command

Heals player to maximum health and stamina:

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

### Unstuck Command

Teleports player to the top of the world:

`Server/MacroCommands/UnstuckCommand.json`:

```json
{
  "Name": "unstuck",
  "Description": "Teleport to the top of the world at your current position",
  "Commands": [
    "tp top"
  ]
}
```

### Fill Signature Command

Fills signature energy to maximum:

`Server/MacroCommands/FillSignatureCommand.json`:

```json
{
  "Name": "fillsignature",
  "Description": "Fills your signature energy",
  "Commands": [
    "player stat settomax SignatureEnergy"
  ]
}
```

### Noon Command

Sets time to noon:

`Server/MacroCommands/NoonCommand.json`:

```json
{
  "Name": "noon",
  "Description": "Sets time to noon",
  "Commands": [
    "time set 12000"
  ]
}
```

### Reset Rotation Command

Resets player rotation:

`Server/MacroCommands/ResetRotationCommand.json`:

```json
{
  "Name": "resetrotation",
  "Description": "Resets your rotation",
  "Commands": [
    "player rotation set 0 0"
  ]
}
```

### Delete Command

Deletes an item or entity:

`Server/MacroCommands/DeleteCommand.json`:

```json
{
  "Name": "delete",
  "Description": "Deletes the target item or entity",
  "Commands": [
    "delete target"
  ]
}
```

### Near Death Command

Sets player health to 1 (near death):

`Server/MacroCommands/NearDeathCommand.json`:

```json
{
  "Name": "neardeath",
  "Description": "Sets your health to 1",
  "Commands": [
    "player stat set Health 1"
  ]
}
```

## Advanced Macro Commands

### Commands with Parameters

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

**Usage:**
- `/setTimeAndGoHome 12000` - Sets time to noon and teleports home in default world
- `/setTimeAndGoHome 6000 MyWorld` - Sets time to 6:00 and teleports home in MyWorld

### Commands with Wait

Macro commands can include wait commands:

```json
{
  "Name": "teleportSequence",
  "Commands": [
    "tp 0 100 0",
    "wait 1",
    "tp 100 100 0",
    "wait 1",
    "tp 0 100 0"
  ]
}
```

## Parameter Types

Common parameter types:

- **`"Integer"`** - Whole numbers (e.g., `12000`, `5`)
- **`"Float"`** - Decimal numbers (e.g., `1.5`, `0.25`)
- **`"String"`** - Text strings (e.g., `"MyWorld"`, `"playerName"`)
- **`"World"`** - World identifier (e.g., `"Default"`, `"MyWorld"`)
- **`"Player"`** - Player name or identifier
- **`"Item"`** - Item ID (e.g., `"Weapon_Sword_Iron"`)
- **`"Entity"`** - Entity ID or type
- **`"Block"`** - Block ID (e.g., `"Rock_Stone"`)

## Creating Custom Macro Commands

### Step 1: Create Macro Command File

Create `Server/MacroCommands/MyCustom_Command.json`:

```json
{
  "Name": "mycustom",
  "Aliases": ["mc", "custom"],
  "Description": "My custom command that does something",
  "Parameters": [
    {
      "Name": "value",
      "Description": "The value to set",
      "Requirement": "Required",
      "ArgType": "Integer"
    }
  ],
  "Commands": [
    "player stat set Health {value}",
    "player stat set Stamina {value}"
  ]
}
```

### Step 2: Test Command

Use `/mycustom 50` in the console to test the command.

## Command Sequences

Macro commands can execute multiple commands in sequence:

```json
{
  "Name": "fullHeal",
  "Commands": [
    "player stat settomax Health",
    "player stat settomax Stamina",
    "player stat settomax SignatureEnergy",
    "player stat settomax Mana"
  ]
}
```

Commands execute one after another in order.

## Wait Commands

Use `wait` to add delays between commands:

```json
{
  "Name": "delayedTeleport",
  "Commands": [
    "echo Teleporting in 3 seconds...",
    "wait 3",
    "tp home"
  ]
}
```

**Wait Syntax:**
- `"wait 1"` - Wait 1 second
- `"wait 0.5"` - Wait 0.5 seconds
- `"wait 10"` - Wait 10 seconds

## Tips for Creating Macro Commands

1. **Use Descriptive Names** - Choose clear, memorable command names
2. **Add Descriptions** - Describe what the command does
3. **Use Aliases** - Add shortcuts for common commands
4. **Add Parameters** - Use parameters for flexible commands
5. **Test Thoroughly** - Test commands with different parameters
6. **Use Wait** - Add delays for multi-step commands
7. **Document Usage** - Note how to use commands in descriptions
8. **Default Values** - Use default values for optional parameters
9. **Error Handling** - Consider what happens if commands fail
10. **Command Order** - Order commands correctly (some depend on others)

---

**Previous:** [HytaleGenerator BlockMasks](166_HytaleGenerator_BlockMasks.md) | **Next:** Back to [README](../README.md)
