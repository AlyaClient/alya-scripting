---
sidebar_position: 2
---

# Minecraft API

The Minecraft API provides direct access to basic Minecraft game elements like the player, world, and entities.

All functions in this API are accessible through the `mc` table.

## Player Functions

### getPlayer

```lua
mc.getPlayer()
```

Returns the player object or `nil` if the player is not available.

**Returns:**
- `PlayerEntity` - The player object

**Example:**
```lua
local player = mc.getPlayer()
if player then
    util.chatInfo("Player name: " .. player:getName():getString())
end
```

### getPlayerPosition

```lua
mc.getPlayerPosition()
```

Returns the player's position as a table with x, y, and z coordinates.

**Returns:**
- `table` - A table with the following fields:
  - `x` - The x coordinate
  - `y` - The y coordinate
  - `z` - The z coordinate

**Example:**
```lua
local pos = mc.getPlayerPosition()
util.chatInfo("Position: " .. pos.x .. ", " .. pos.y .. ", " .. pos.z)
```

### getPlayerSpeed

```lua
mc.getPlayerSpeed()
```

Returns the player's current speed in blocks per second.

**Returns:**
- `number` - The player's speed

**Example:**
```lua
local speed = mc.getPlayerSpeed()
util.chatInfo("Current speed: " .. speed .. " blocks/s")
```

## World Functions

### getWorld

```lua
mc.getWorld()
```

Returns the world object or `nil` if the world is not available.

**Returns:**
- `World` - The world object

**Example:**
```lua
local world = mc.getWorld()
if world then
    util.chatInfo("World time: " .. world:getTime())
end
```

### getEntities

```lua
mc.getEntities()
```

Returns a list of all entities in the world.

**Returns:**
- `table` - A list of entity objects

**Example:**
```lua
local entities = mc.getEntities()
util.chatInfo("Number of entities: " .. #entities)
```

### getPlayers

```lua
mc.getPlayers()
```

Returns a list of all player entities in the world.

**Returns:**
- `table` - A list of player entity objects

**Example:**
```lua
local players = mc.getPlayers()
util.chatInfo("Number of players: " .. #players)
```

## Game Information

### getFPS

```lua
mc.getFPS()
```

Returns the current frames per second.

**Returns:**
- `number` - The current FPS

**Example:**
```lua
local fps = mc.getFPS()
util.chatInfo("Current FPS: " .. fps)
```

### getGameTime

```lua
mc.getGameTime()
```

Returns the current game time in ticks.

**Returns:**
- `number` - The game time

**Example:**
```lua
local gameTime = mc.getGameTime()
util.chatInfo("Game time: " .. gameTime)
```

### getDayTime

```lua
mc.getDayTime()
```

Returns the current day time in ticks.

**Returns:**
- `number` - The day time

**Example:**
```lua
local dayTime = mc.getDayTime()
util.chatInfo("Day time: " .. dayTime)
```

### isInGame

```lua
mc.isInGame()
```

Returns whether the player is currently in a game.

**Returns:**
- `boolean` - `true` if the player is in a game, `false` otherwise

**Example:**
```lua
if mc.isInGame() then
    util.chatInfo("Player is in game")
else
    util.chatInfo("Player is not in game")
end
```

## Mouse and Screen

### getMouseX

```lua
mc.getMouseX()
```

Returns the current X position of the mouse cursor.

**Returns:**
- `number` - The mouse X position

**Example:**
```lua
local mouseX = mc.getMouseX()
util.chatInfo("Mouse X: " .. mouseX)
```

### getMouseY

```lua
mc.getMouseY()
```

Returns the current Y position of the mouse cursor.

**Returns:**
- `number` - The mouse Y position

**Example:**
```lua
local mouseY = mc.getMouseY()
util.chatInfo("Mouse Y: " .. mouseY)
```

### getScreenWidth

```lua
mc.getScreenWidth()
```

Returns the width of the game window.

**Returns:**
- `number` - The screen width

**Example:**
```lua
local width = mc.getScreenWidth()
util.chatInfo("Screen width: " .. width)
```

### getScreenHeight

```lua
mc.getScreenHeight()
```

Returns the height of the game window.

**Returns:**
- `number` - The screen height

**Example:**
```lua
local height = mc.getScreenHeight()
util.chatInfo("Screen height: " .. height)
```