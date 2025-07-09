---
sidebar_position: 2
---

# Minecraft API

The Minecraft API provides direct access to basic Minecraft game elements like the player, world, and entities.

All functions in this API are accessible through the `alya.mc` table.

## Player Functions

### getPlayer

```lua
alya.mc.getPlayer()
```

Returns the player object or `nil` if the player is not available.

**Returns:**
- `PlayerEntity` - The player object

**Example:**
```lua
local player = alya.mc.getPlayer()
if player then
    alya.util.chatInfo("Player name: " .. player:getName():getString())
end
```

### getPlayerPosition

```lua
alya.mc.getPlayerPosition()
```

Returns the player's position as a table with x, y, and z coordinates.

**Returns:**
- `table` - A table with the following fields:
  - `x` - The x coordinate
  - `y` - The y coordinate
  - `z` - The z coordinate

**Example:**
```lua
local pos = alya.mc.getPlayerPosition()
alya.util.chatInfo("Position: " .. pos.x .. ", " .. pos.y .. ", " .. pos.z)
```

### getPlayerSpeed

```lua
alya.mc.getPlayerSpeed()
```

Returns the player's current speed in blocks per second.

**Returns:**
- `number` - The player's speed

**Example:**
```lua
local speed = alya.mc.getPlayerSpeed()
alya.util.chatInfo("Current speed: " .. speed .. " blocks/s")
```

## World Functions

### getWorld

```lua
alya.mc.getWorld()
```

Returns the world object or `nil` if the world is not available.

**Returns:**
- `World` - The world object

**Example:**
```lua
local world = alya.mc.getWorld()
if world then
    alya.util.chatInfo("World time: " .. world:getTime())
end
```

### getEntities

```lua
alya.mc.getEntities()
```

Returns a list of all entities in the world.

**Returns:**
- `table` - A list of entity objects

**Example:**
```lua
local entities = alya.mc.getEntities()
alya.util.chatInfo("Number of entities: " .. #entities)
```

### getPlayers

```lua
alya.mc.getPlayers()
```

Returns a list of all player entities in the world.

**Returns:**
- `table` - A list of player entity objects

**Example:**
```lua
local players = alya.mc.getPlayers()
alya.util.chatInfo("Number of players: " .. #players)
```

## Game Information

### getFPS

```lua
alya.mc.getFPS()
```

Returns the current frames per second.

**Returns:**
- `number` - The current FPS

**Example:**
```lua
local fps = alya.mc.getFPS()
alya.util.chatInfo("Current FPS: " .. fps)
```

### getGameTime

```lua
alya.mc.getGameTime()
```

Returns the current game time in ticks.

**Returns:**
- `number` - The game time

**Example:**
```lua
local gameTime = alya.mc.getGameTime()
alya.util.chatInfo("Game time: " .. gameTime)
```

### getDayTime

```lua
alya.mc.getDayTime()
```

Returns the current day time in ticks.

**Returns:**
- `number` - The day time

**Example:**
```lua
local dayTime = alya.mc.getDayTime()
alya.util.chatInfo("Day time: " .. dayTime)
```

### isInGame

```lua
alya.mc.isInGame()
```

Returns whether the player is currently in a game.

**Returns:**
- `boolean` - `true` if the player is in a game, `false` otherwise

**Example:**
```lua
if alya.mc.isInGame() then
    alya.util.chatInfo("Player is in game")
else
    alya.util.chatInfo("Player is not in game")
end
```

## Mouse and Screen

### getMouseX

```lua
alya.mc.getMouseX()
```

Returns the current X position of the mouse cursor.

**Returns:**
- `number` - The mouse X position

**Example:**
```lua
local mouseX = alya.mc.getMouseX()
alya.util.chatInfo("Mouse X: " .. mouseX)
```

### getMouseY

```lua
alya.mc.getMouseY()
```

Returns the current Y position of the mouse cursor.

**Returns:**
- `number` - The mouse Y position

**Example:**
```lua
local mouseY = alya.mc.getMouseY()
alya.util.chatInfo("Mouse Y: " .. mouseY)
```

### getScreenWidth

```lua
alya.mc.getScreenWidth()
```

Returns the width of the game window.

**Returns:**
- `number` - The screen width

**Example:**
```lua
local width = alya.mc.getScreenWidth()
alya.util.chatInfo("Screen width: " .. width)
```

### getScreenHeight

```lua
alya.mc.getScreenHeight()
```

Returns the height of the game window.

**Returns:**
- `number` - The screen height

**Example:**
```lua
local height = alya.mc.getScreenHeight()
alya.util.chatInfo("Screen height: " .. height)
```