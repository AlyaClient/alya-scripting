---
sidebar_position: 6
---

# World API

The World API provides functions to interact with the Minecraft world, blocks, and environment.

All functions in this API are accessible through the `alya.world` table.

## Block Interaction

### getBlock

```lua
alya.world.getBlock(x, y, z)
```

Gets the block at the specified coordinates.

**Parameters:**
- `x` (number) - The x coordinate
- `y` (number) - The y coordinate
- `z` (number) - The z coordinate

**Returns:**
- `Block` - The block object at the specified coordinates, or `nil` if the world is not loaded

**Example:**
```lua
local block = alya.world.getBlock(0, 64, 0)
if block then
    alya.util.chatInfo("Block at (0, 64, 0): " .. block:toString())
end
```

### getBlockState

```lua
alya.world.getBlockState(x, y, z)
```

Gets the block state at the specified coordinates.

**Parameters:**
- `x` (number) - The x coordinate
- `y` (number) - The y coordinate
- `z` (number) - The z coordinate

**Returns:**
- `BlockState` - The block state at the specified coordinates, or `nil` if the world is not loaded

**Example:**
```lua
local blockState = alya.world.getBlockState(0, 64, 0)
if blockState then
    alya.util.chatInfo("Block state at (0, 64, 0): " .. blockState:toString())
end
```

### isAir

```lua
alya.world.isAir(x, y, z)
```

Checks if the block at the specified coordinates is air.

**Parameters:**
- `x` (number) - The x coordinate
- `y` (number) - The y coordinate
- `z` (number) - The z coordinate

**Returns:**
- `boolean` - `true` if the block is air, `false` otherwise

**Example:**
```lua
if alya.world.isAir(0, 64, 0) then
    alya.util.chatInfo("Block at (0, 64, 0) is air")
else
    alya.util.chatInfo("Block at (0, 64, 0) is not air")
end
```

### getLightLevel

```lua
alya.world.getLightLevel(x, y, z)
```

Gets the light level at the specified coordinates.

**Parameters:**
- `x` (number) - The x coordinate
- `y` (number) - The y coordinate
- `z` (number) - The z coordinate

**Returns:**
- `number` - The light level (0-15)

**Example:**
```lua
local lightLevel = alya.world.getLightLevel(0, 64, 0)
alya.util.chatInfo("Light level at (0, 64, 0): " .. lightLevel)
```

## Entity Interaction

### getEntitiesInRange

```lua
alya.world.getEntitiesInRange(x, y, z, range)
```

Gets all entities within the specified range of the given coordinates.

**Parameters:**
- `x` (number) - The x coordinate
- `y` (number) - The y coordinate
- `z` (number) - The z coordinate
- `range` (number) - The range to search within

**Returns:**
- `table` - A list of entity objects within the range

**Example:**
```lua
local pos = alya.mc.getPlayerPosition()
local entities = alya.world.getEntitiesInRange(pos.x, pos.y, pos.z, 5)
alya.util.chatInfo("Entities within 5 blocks: " .. #entities)
```

## Raycasting

### raycast

```lua
alya.world.raycast(distance)
```

Performs a raycast from the player's position in the direction they are looking.

**Parameters:**
- `distance` (number) [optional] - The maximum distance to raycast (default: 4.5)

**Returns:**
- `HitResult` - The result of the raycast, or `nil` if nothing was hit

**Example:**
```lua
local hit = alya.world.raycast(10)
if hit then
    alya.util.chatInfo("Hit type: " .. hit:getType():toString())
    if hit:getType():toString() == "BLOCK" then
        local pos = hit:getBlockPos()
        alya.util.chatInfo("Hit block at: " .. pos.x .. ", " .. pos.y .. ", " .. pos.z)
    elseif hit:getType():toString() == "ENTITY" then
        local entity = hit:getEntity()
        alya.util.chatInfo("Hit entity: " .. entity:getName():getString())
    end
end
```

## World Information

### getDimension

```lua
alya.world.getDimension()
```

Gets the current dimension.

**Returns:**
- `string` - The dimension identifier (e.g., "minecraft:overworld")

**Example:**
```lua
local dimension = alya.world.getDimension()
alya.util.chatInfo("Current dimension: " .. dimension)
```

### getWeather

```lua
alya.world.getWeather()
```

Gets information about the current weather.

**Returns:**
- `table` - A table with the following fields:
  - `raining` (boolean) - Whether it is currently raining
  - `thundering` (boolean) - Whether it is currently thundering
  - `rainGradient` (number) - The rain gradient (0.0 - 1.0)
  - `thunderGradient` (number) - The thunder gradient (0.0 - 1.0)

**Example:**
```lua
local weather = alya.world.getWeather()
alya.util.chatInfo("Weather:")
alya.util.chatInfo("- Raining: " .. tostring(weather.raining))
alya.util.chatInfo("- Thundering: " .. tostring(weather.thundering))
alya.util.chatInfo("- Rain Gradient: " .. weather.rainGradient)
alya.util.chatInfo("- Thunder Gradient: " .. weather.thunderGradient)
```

## Usage Examples

### Finding Safe Landing Spot

```lua
function findSafeLandingSpot()
    local pos = alya.mc.getPlayerPosition()
    local x, z = math.floor(pos.x), math.floor(pos.z)
    
    for y = math.floor(pos.y) - 1, 0, -1 do
        if not alya.world.isAir(x, y, z) then
            return x, y + 1, z
        end
    end
    
    return nil
end

local x, y, z = findSafeLandingSpot()
if x then
    alya.util.chatInfo("Safe landing spot found at: " .. x .. ", " .. y .. ", " .. z)
else
    alya.util.chatInfo("No safe landing spot found")
end
```

### Checking for Blocks Around Player

```lua
function checkSurroundings(radius)
    local pos = alya.mc.getPlayerPosition()
    local x, y, z = math.floor(pos.x), math.floor(pos.y), math.floor(pos.z)
    local blocks = {}
    
    for dx = -radius, radius do
        for dy = -radius, radius do
            for dz = -radius, radius do
                local block = alya.world.getBlock(x + dx, y + dy, z + dz)
                if block and not alya.world.isAir(x + dx, y + dy, z + dz) then
                    local name = block:toString()
                    blocks[name] = (blocks[name] or 0) + 1
                end
            end
        end
    end
    
    return blocks
end

local blocks = checkSurroundings(3)
alya.util.chatInfo("Blocks around you:")
for name, count in pairs(blocks) do
    alya.util.chatInfo("- " .. name .. ": " .. count)
end
```