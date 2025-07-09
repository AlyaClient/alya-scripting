---
sidebar_position: 5
---

# Utility API

The Utility API provides helper functions for logging, chat messages, movement, and other miscellaneous utilities.

All functions in this API are accessible through the `alya.util` table.

## Logging and Chat

### log

```lua
alya.util.log(message)
```

Logs a message to the console.

**Parameters:**
- `message` (string) - The message to log

**Example:**
```lua
alya.util.log("Script initialized")
```

### chatInfo

```lua
alya.util.chatInfo(message)
```

Sends an informational message to the in-game chat.

**Parameters:**
- `message` (string) - The message to send

**Example:**
```lua
alya.util.chatInfo("Script is running")
```

### chatError

```lua
alya.util.chatError(message)
```

Sends an error message to the in-game chat.

**Parameters:**
- `message` (string) - The error message to send

**Example:**
```lua
alya.util.chatError("Failed to execute command")
```

## Movement

### setSpeed

```lua
alya.util.setSpeed(speed, strafe)
```

Sets the player's movement speed.

**Parameters:**
- `speed` (number) - The speed value
- `strafe` (boolean) - Whether to apply strafing movement

**Example:**
```lua
-- Set player speed to 0.5 with strafing
alya.util.setSpeed(0.5, true)

-- Set player speed to 0.3 without strafing
alya.util.setSpeed(0.3, false)
```

## Time and Timing

### currentTimeMillis

```lua
alya.util.currentTimeMillis()
```

Returns the current system time in milliseconds.

**Returns:**
- `number` - The current time in milliseconds

**Example:**
```lua
local startTime = alya.util.currentTimeMillis()
-- Do some operations
local endTime = alya.util.currentTimeMillis()
local elapsedTime = endTime - startTime
alya.util.chatInfo("Operation took " .. elapsedTime .. " ms")
```

### sleep

```lua
alya.util.sleep(milliseconds)
```

Pauses the script execution for the specified number of milliseconds.

**Parameters:**
- `milliseconds` (number) - The number of milliseconds to sleep

**Example:**
```lua
alya.util.chatInfo("Waiting for 2 seconds...")
alya.util.sleep(2000)
alya.util.chatInfo("Done waiting!")
```

## Random Number Generation

### randomInt

```lua
alya.util.randomInt(min, max)
```

Generates a random integer between min and max (inclusive).

**Parameters:**
- `min` (number) - The minimum value
- `max` (number) - The maximum value

**Returns:**
- `number` - A random integer between min and max

**Example:**
```lua
local randomNumber = alya.util.randomInt(1, 10)
alya.util.chatInfo("Random number: " .. randomNumber)
```

### randomDouble

```lua
alya.util.randomDouble(min, max)
```

Generates a random floating-point number between min and max.

**Parameters:**
- `min` (number) - The minimum value
- `max` (number) - The maximum value

**Returns:**
- `number` - A random floating-point number between min and max

**Example:**
```lua
local randomValue = alya.util.randomDouble(0.1, 0.5)
alya.util.chatInfo("Random value: " .. randomValue)
```

## Usage Examples

### Creating a Timer

```lua
local lastActionTime = alya.util.currentTimeMillis()
local actionInterval = 5000 -- 5 seconds

function onUpdate()
    local currentTime = alya.util.currentTimeMillis()
    if currentTime - lastActionTime >= actionInterval then
        alya.util.chatInfo("Performing scheduled action")
        -- Perform your action here
        lastActionTime = currentTime
    end
end
```

### Random Delays

```lua
function performActionWithRandomDelay()
    -- Perform first part of the action
    alya.util.chatInfo("Starting action sequence")
    
    -- Wait for a random time between 1 and 3 seconds
    local delay = alya.util.randomInt(1000, 3000)
    alya.util.chatInfo("Waiting for " .. delay .. " ms")
    alya.util.sleep(delay)
    
    -- Perform second part of the action
    alya.util.chatInfo("Completing action sequence")
end
```

### Logging with Timestamps

```lua
function logWithTimestamp(message)
    local timestamp = alya.util.currentTimeMillis()
    alya.util.log("[" .. timestamp .. "] " .. message)
end

-- Usage
logWithTimestamp("Script started")
```