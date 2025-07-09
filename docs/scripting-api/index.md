---
sidebar_position: 1
---

# Scripting API

Welcome to the Alya Client Scripting API documentation. This API allows you to create custom scripts for the Alya Minecraft client using Lua.

## Overview

**NOTICE:** As we continue to work on scripting, changes may be made and the documentation may not always
resemble the API, we plan to smoothen things out around Q4 2025

The Alya Scripting API provides access to various aspects of the Minecraft game and the Alya client through several modules:

- **Minecraft API** (`mc`) - Access to basic Minecraft game elements like player, world, and entities
- **Alya API** (`alya`) - Interact with Alya client modules and features
- **Render API** (`render`) - Draw text and shapes on the screen
- **Utility API** (`util`) - Helper functions for logging, chat messages, and more
- **World API** (`world`) - Interact with the Minecraft world, blocks, and environment
- **Player API** (`player`) - Access and modify player properties and actions
- **Inventory API** (`inventory`) - Manage player inventory and items
- **Math API** (`math`) - Mathematical utilities for calculations
- **Network API** (`network`) - Send packets and interact with the network
- **Sound API** (`sound`) - Play sounds in the game
- **Entity API** (`entity`) - Interact with entities in the world

## Getting Started

To use the Alya Scripting API, you need to create a Lua script file and place it in the `scripts` folder of your Alya client installation.

> **Hint**: The script folder is found at: `.minecraft/Alya/scripts`
> The exact location depends on your operating system:
> - Windows: `%appdata%\.minecraft\Alya\scripts`
> - macOS: `~/Library/Application Support/minecraft/Alya/scripts`
> - Linux: `~/.minecraft/Alya/scripts`

### Script Lifecycle

Every Alya script has a lifecycle with several key functions that are called automatically:

> **Hint**: These functions are optional, but they're very useful for controlling when your code runs.

- `onEnable()` - Called when the script is enabled
- `onDisable()` - Called when the script is disabled
- `onRender2D()` - Called every frame when rendering 2D elements (for drawing on screen)
- `onRender3D()` - Called every frame when rendering 3D elements (for drawing in the world)
- `onUpdate()` - Called every game tick (20 times per second)
- `onPacket(packet, outgoing)` - Called when a packet is sent or received

Here's a simple example script that displays a message in the chat when enabled and draws text on the screen:

```lua
-- Example script
function onEnable()
    alya.util.chatInfo("Script enabled!")
end

function onDisable()
    alya.util.chatInfo("Script disabled!")
end

function onRender2D()
    alya.render.drawText("Hello, World!", 10, 10, alya.render.color("RED"), true)
end
```

### Script Metadata

Every script needs metadata to identify it in the client. This metadata should be defined at the top of your script file.

> **Hint**: Metadata is required for your script to appear in the Alya client's script manager.

Required metadata:
- `name` - The name of your script (displayed in the UI)
- `description` - A brief description of what your script does

Optional metadata:
- `version` - The version of your script (e.g., "1.0.0")
- `author` - Your name or username

Here's an example:

```lua
-- Metadata (put this at the top of your script)
name = "My First Script!"
description = "This is my first script for Alya"
version = "1.0.0"
author = "YourName"
```

### Debugging Tips

> **Hint**: When your script isn't working as expected, these debugging techniques can help:

1. Use `alya.util.chatInfo()` to print debug messages to the chat
2. [FEATURE NOT AVAILABLE YET] Check the Alya console for error messages (press F8 to open it)
3. Use `print()` to output to the console
4. Break down complex operations into smaller steps and test each one
5. Remember that some functions may only work in certain game states (e.g., when connected to a server)

Example debugging code:

```lua
function onUpdate()
    local pos = alya.mc.getPlayerPosition()
    alya.util.chatInfo("Debug - Position: " .. pos.x .. ", " .. pos.y .. ", " .. pos.z)

    -- Check if a condition is met
    if pos.y < 64 then
        alya.util.chatInfo("Debug - Player is below sea level")
    end
end
```

## API Structure

All API functions are accessible through the global `alya` table. Each module is a sub-table of the `alya` table.

> **Hint**: The API is organized into logical modules that group related functionality together. Understanding this structure will help you find the functions you need more easily.

Here's how the API is structured:

```
alya
├── mc - Minecraft core functionality
├── render - Drawing on screen and in the world
├── util - Utility functions
├── world - World interaction
├── player - Player-related functions
├── inventory - Inventory management
├── math - Mathematical utilities
├── network - Network and packet handling
└── sound - Sound playing
```

> **Hint**: You can use tab completion in the [FEATURE NOT AVAILABLE YET] Alya console (F8) to explore available functions. Type `alya.` and press Tab to see available modules, then continue with `alya.module.` to see functions.

For example, to access the player's position:

```lua
local position = alya.mc.getPlayerPosition()
alya.util.chatInfo("X: " .. position.x .. ", Y: " .. position.y .. ", Z: " .. position.z)
```

> **Hint**: Most functions return useful values that you can store in variables for later use. Check the documentation for each function to understand what it returns.

Explore the documentation for each module to learn about all available functions and how to use them.
