---
sidebar_position: 3
---

# Alya API

The Alya API provides functions to interact with the Alya client's modules and features.

All functions in this API are accessible directly through the `alya` table.

## Module Management

> **Hint**: Modules are the core components of the Alya client. Each module provides a specific functionality (like KillAura, Speed, ESP, etc.). Understanding how to interact with modules is essential for creating powerful scripts.

> **Hint**: Module names are case-sensitive and typically follow PascalCase naming convention (e.g., "KillAura", "AntiVoid", "TargetStrafe").

### Common Modules

> **Hint**: Here are some commonly used modules you might want to interact with:
> - Combat: "KillAura", "AttackDelay"
> - Movement: "Speed", "Flight", "Sprint"
> - Render: "ESP", "HUD"
> - Utility: "NoFall", "AntiVoid"
> - World: "Nuker", "Timer", "Scaffold"

### getModule

```lua
alya.getModule(moduleName)
```

Returns a module object by its name.

**Parameters:**
- `moduleName` (string) - The name of the module

**Returns:**
- `Module` - The module object, or `nil` if the module doesn't exist

> **Hint**: Always check if the module exists before trying to use it, as shown in the example below.

**Example:**
```lua
local killaura = alya.getModule("KillAura")
if killaura then
    alya.util.chatInfo("KillAura module found!")
else
    alya.util.chatInfo("KillAura module not found! Check the spelling or if it's available in your version.")
end
```

### isModuleEnabled

```lua
alya.isModuleEnabled(moduleName)
```

Checks if a module is enabled.

**Parameters:**
- `moduleName` (string) - The name of the module

**Returns:**
- `boolean` - `true` if the module is enabled, `false` otherwise

**Example:**
```lua
if alya.isModuleEnabled("Speed") then
    alya.util.chatInfo("Speed is enabled!")
else
    alya.util.chatInfo("Speed is disabled!")
end
```

### enableModule

```lua
alya.enableModule(moduleName)
```

Enables a module.

**Parameters:**
- `moduleName` (string) - The name of the module

**Returns:**
- `boolean` - `true` if the module was successfully enabled, `false` otherwise

**Example:**
```lua
if alya.enableModule("Flight") then
    alya.util.chatInfo("Flight enabled!")
else
    alya.util.chatInfo("Failed to enable Flight!")
end
```

### disableModule

```lua
alya.disableModule(moduleName)
```

Disables a module.

**Parameters:**
- `moduleName` (string) - The name of the module

**Returns:**
- `boolean` - `true` if the module was successfully disabled, `false` otherwise

**Example:**
```lua
if alya.disableModule("ESP") then
    alya.util.chatInfo("ESP disabled!")
else
    alya.util.chatInfo("Failed to disable ESP!")
end
```

### toggleModule

```lua
alya.toggleModule(moduleName)
```

Toggles a module's enabled state.

**Parameters:**
- `moduleName` (string) - The name of the module

**Returns:**
- `boolean` - The new state of the module (`true` if enabled, `false` if disabled)

**Example:**
```lua
local newState = alya.toggleModule("Scaffold")
alya.util.chatInfo("Scaffold is now " .. (newState and "enabled" or "disabled"))
```

## Module Information

### getModules

```lua
alya.getModules()
```

Returns a list of all modules in the client.

**Returns:**
- `table` - A list of module objects

**Example:**
```lua
local modules = alya.getModules()
alya.util.chatInfo("Total modules: " .. #modules)
for i, module in ipairs(modules) do
    alya.util.chatInfo(i .. ". " .. module:getName())
end
```

### getEnabledModules

```lua
alya.getEnabledModules()
```

Returns a list of all enabled modules.

**Returns:**
- `table` - A list of enabled module objects

**Example:**
```lua
local enabledModules = alya.getEnabledModules()
alya.util.chatInfo("Enabled modules: " .. #enabledModules)
for i, module in ipairs(enabledModules) do
    alya.util.chatInfo(i .. ". " .. module:getName())
end
```

## Working with Module Objects

When you get a module object using `getModule()` or from the lists returned by `getModules()` and `getEnabledModules()`, you can interact with it using the following methods:

> **Hint**: Using module objects directly is often more efficient than repeatedly calling functions like `isModuleEnabled()` and `enableModule()` with the module name.

> **Hint**: Best practices for working with modules:
> 1. Store frequently used modules as variables at the beginning of your script
> 2. Always check if a module exists before trying to use it
> 3. Use module objects for multiple operations on the same module
> 4. Consider the performance impact of enabling/disabling modules frequently
> 5. Some modules may conflict with each other when enabled simultaneously

### Module:getName()

Returns the name of the module.

**Example:**
```lua
local module = alya.getModule("Speed")
alya.util.chatInfo("Module name: " .. module:getName())
```

### Module:isEnabled()

Returns whether the module is enabled.

**Example:**
```lua
local module = alya.getModule("Speed")
alya.util.chatInfo("Module enabled: " .. tostring(module:isEnabled()))
```

### Module:setEnabled(state)

Sets the enabled state of the module.

**Example:**
```lua
local module = alya.getModule("Speed")
module:setEnabled(true) -- Enable the module
```

### Module:toggle()

Toggles the enabled state of the module.

**Example:**
```lua
local module = alya.getModule("Speed")
module:toggle() -- Toggle the module
```

## Practical Examples

> **Hint**: Here are some practical examples of how to use the Alya API with modules in real scripts:

### Example 1: Combat Assistant

This script enables KillAura when enemies are nearby and disables it when you're safe:

```lua
-- Metadata
name = "Combat Assistant"
description = "Automatically manages combat modules based on surroundings"
version = "1.0.0"

-- Store modules we'll use frequently
local killAura = nil
local criticals = nil

function onEnable()
    -- Initialize module references
    killAura = alya.getModule("KillAura")
    criticals = alya.getModule("Criticals")

    if not killAura or not criticals then
        alya.util.chatInfo("§cWarning: Required modules not found!")
        return
    end

    alya.util.chatInfo("§aCombat Assistant enabled!")
end

function onUpdate()
    if not killAura or not criticals then return end

    -- Check if enemies are nearby (within 5 blocks)
    local nearbyEntities = alya.mc.getEntitiesInRadius(5)
    local enemiesNearby = false

    for _, entity in ipairs(nearbyEntities) do
        if entity:isPlayer() and not entity:isFriend() then
            enemiesNearby = true
            break
        end
    end

    -- Enable combat modules when enemies are nearby
    if enemiesNearby and not killAura:isEnabled() then
        killAura:setEnabled(true)
        criticals:setEnabled(true)
        alya.util.chatInfo("§cEnemies nearby! Combat modules activated.")
    elseif not enemiesNearby and killAura:isEnabled() then
        killAura:setEnabled(false)
        criticals:setEnabled(false)
        alya.util.chatInfo("§aArea clear. Combat modules deactivated.")
    end
end

function onDisable()
    -- Clean up - make sure we don't leave modules enabled
    if killAura and killAura:isEnabled() then
        killAura:setEnabled(false)
    end

    if criticals and criticals:isEnabled() then
        criticals:setEnabled(false)
    end

    alya.util.chatInfo("§cCombat Assistant disabled!")
end
```
