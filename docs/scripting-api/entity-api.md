---
sidebar_position: 12
---

# Entity API

The Entity API provides functions to interact with entities in the Minecraft world.

All functions in this API are accessible through the `alya.entity` table.

## Entity Finding

### getClosestPlayer

```lua
alya.entity.getClosestPlayer()
```

Gets the closest player to the local player.

**Returns:**
- `PlayerEntity` - The closest player entity, or `nil` if no players are found

**Example:**
```lua
local closestPlayer = alya.entity.getClosestPlayer()
if closestPlayer then
    local name = alya.entity.getEntityName(closestPlayer)
    alya.util.chatInfo("Closest player: " .. name)
end
```

### getClosestEntity

```lua
alya.entity.getClosestEntity(entityType)
```

Gets the closest entity of the specified type to the local player.

**Parameters:**
- `entityType` (string) - The type of entity to find (e.g., "zombie", "creeper")

**Returns:**
- `Entity` - The closest entity of the specified type, or `nil` if no matching entities are found

**Example:**
```lua
local closestZombie = alya.entity.getClosestEntity("zombie")
if closestZombie then
    local distance = alya.mc.getPlayer():distanceTo(closestZombie)
    alya.util.chatInfo("Closest zombie is " .. distance .. " blocks away")
end
```

### getPlayersInRange

```lua
alya.entity.getPlayersInRange(range)
```

Gets all players within the specified range of the local player.

**Parameters:**
- `range` (number) - The range in blocks

**Returns:**
- `table` - A list of player entities within the range

**Example:**
```lua
local players = alya.entity.getPlayersInRange(10)
alya.util.chatInfo("Players within 10 blocks: " .. #players)
for i, player in ipairs(players) do
    local name = alya.entity.getEntityName(player)
    alya.util.chatInfo(i .. ". " .. name)
end
```

### getEntitiesInRange

```lua
alya.entity.getEntitiesInRange(range, entityType)
```

Gets all entities of the specified type within the specified range of the local player.

**Parameters:**
- `range` (number) - The range in blocks
- `entityType` (string) - The type of entity to find (e.g., "zombie", "creeper")

**Returns:**
- `table` - A list of entities of the specified type within the range

**Example:**
```lua
local animals = alya.entity.getEntitiesInRange(20, "animal")
alya.util.chatInfo("Animals within 20 blocks: " .. #animals)
```

## Entity Information

### getEntityPosition

```lua
alya.entity.getEntityPosition(entity)
```

Gets the position of the specified entity.

**Parameters:**
- `entity` (Entity) - The entity object

**Returns:**
- `table` - A table with the following fields:
  - `x` - The x coordinate
  - `y` - The y coordinate
  - `z` - The z coordinate

**Example:**
```lua
local player = alya.entity.getClosestPlayer()
if player then
    local pos = alya.entity.getEntityPosition(player)
    alya.util.chatInfo("Player position: " .. pos.x .. ", " .. pos.y .. ", " .. pos.z)
end
```

### getEntityHealth

```lua
alya.entity.getEntityHealth(entity)
```

Gets the health of the specified entity.

**Parameters:**
- `entity` (Entity) - The entity object

**Returns:**
- `number` - The entity's health points

**Example:**
```lua
local player = alya.entity.getClosestPlayer()
if player then
    local health = alya.entity.getEntityHealth(player)
    alya.util.chatInfo("Player health: " .. health)
end
```

### getEntityMaxHealth

```lua
alya.entity.getEntityMaxHealth(entity)
```

Gets the maximum health of the specified entity.

**Parameters:**
- `entity` (Entity) - The entity object

**Returns:**
- `number` - The entity's maximum health points

**Example:**
```lua
local player = alya.entity.getClosestPlayer()
if player then
    local maxHealth = alya.entity.getEntityMaxHealth(player)
    local health = alya.entity.getEntityHealth(player)
    alya.util.chatInfo("Player health: " .. health .. "/" .. maxHealth)
end
```

### isEntityAlive

```lua
alya.entity.isEntityAlive(entity)
```

Checks if the specified entity is alive.

**Parameters:**
- `entity` (Entity) - The entity object

**Returns:**
- `boolean` - `true` if the entity is alive, `false` otherwise

**Example:**
```lua
local entity = alya.entity.getClosestEntity("zombie")
if entity and alya.entity.isEntityAlive(entity) then
    alya.util.chatInfo("Zombie is alive")
else
    alya.util.chatInfo("No living zombie found")
end
```

### getEntityName

```lua
alya.entity.getEntityName(entity)
```

Gets the name of the specified entity.

**Parameters:**
- `entity` (Entity) - The entity object

**Returns:**
- `string` - The entity's name

**Example:**
```lua
local player = alya.entity.getClosestPlayer()
if player then
    local name = alya.entity.getEntityName(player)
    alya.util.chatInfo("Player name: " .. name)
end
```

### getEntityType

```lua
alya.entity.getEntityType(entity)
```

Gets the type of the specified entity.

**Parameters:**
- `entity` (Entity) - The entity object

**Returns:**
- `string` - The entity's type

**Example:**
```lua
local entity = alya.mc.getEntities()[1]
if entity then
    local type = alya.entity.getEntityType(entity)
    alya.util.chatInfo("Entity type: " .. type)
end
```

## Usage Examples

### Entity Tracker

```lua
function trackEntities()
    -- Get all entities
    local entities = alya.mc.getEntities()
    local playerPos = alya.mc.getPlayerPosition()
    local trackedEntities = {}
    
    -- Filter and sort entities
    for _, entity in ipairs(entities) do
        local entityPos = alya.entity.getEntityPosition(entity)
        local distance = alya.math.distance(
            playerPos.x, playerPos.y, playerPos.z,
            entityPos.x, entityPos.y, entityPos.z
        )
        
        local type = alya.entity.getEntityType(entity)
        
        -- Only track certain entity types
        if type:find("zombie") or type:find("skeleton") or type:find("creeper") then
            table.insert(trackedEntities, {
                entity = entity,
                type = type,
                distance = distance
            })
        end
    end
    
    -- Sort by distance
    table.sort(trackedEntities, function(a, b)
        return a.distance < b.distance
    end)
    
    -- Display tracked entities
    alya.util.chatInfo("Tracked entities:")
    for i, data in ipairs(trackedEntities) do
        if i > 5 then break end -- Only show the 5 closest
        
        local health = alya.entity.getEntityHealth(data.entity)
        alya.util.chatInfo(i .. ". " .. data.type .. " - " .. 
            string.format("%.1f", data.distance) .. " blocks - " .. 
            string.format("%.1f", health) .. " HP")
    end
end
```

### Auto Target

```lua
function autoTarget()
    -- Find the closest hostile mob
    local hostiles = {}
    local entities = alya.mc.getEntities()
    local playerPos = alya.mc.getPlayerPosition()
    
    for _, entity in ipairs(entities) do
        local type = alya.entity.getEntityType(entity)
        
        -- Check if it's a hostile mob
        if type:find("zombie") or type:find("skeleton") or 
           type:find("creeper") or type:find("spider") then
            
            local entityPos = alya.entity.getEntityPosition(entity)
            local distance = alya.math.distance(
                playerPos.x, playerPos.y, playerPos.z,
                entityPos.x, entityPos.y, entityPos.z
            )
            
            if distance <= 20 and alya.entity.isEntityAlive(entity) then
                table.insert(hostiles, {
                    entity = entity,
                    distance = distance
                })
            end
        end
    end
    
    -- Sort by distance
    table.sort(hostiles, function(a, b)
        return a.distance < b.distance
    end)
    
    -- Target the closest hostile
    if #hostiles > 0 then
        local target = hostiles[1].entity
        local targetPos = alya.entity.getEntityPosition(target)
        
        -- Look at the target
        local angles = alya.math.angleTo(targetPos.x, targetPos.y + 1, targetPos.z)
        alya.player.setYaw(angles.yaw)
        alya.player.setPitch(angles.pitch)
        
        return target
    end
    
    return nil
end
```

### Entity ESP

```lua
function onRender2D()
    if not alya.mc.isInGame() then return end
    
    local screenWidth = alya.mc.getScreenWidth()
    local screenHeight = alya.mc.getScreenHeight()
    
    -- Get all players
    local players = alya.entity.getPlayersInRange(100)
    
    for _, player in ipairs(players) do
        -- Skip the local player
        if player ~= alya.mc.getPlayer() then
            local pos = alya.entity.getEntityPosition(player)
            local name = alya.entity.getEntityName(player)
            local health = alya.entity.getEntityHealth(player)
            local maxHealth = alya.entity.getEntityMaxHealth(player)
            
            -- Project 3D position to 2D screen coordinates (simplified)
            -- In a real implementation, you would need to use proper 3D to 2D projection
            -- This is just a conceptual example
            local screenX = screenWidth / 2
            local screenY = screenHeight / 2
            
            -- Draw player name and health
            alya.render.drawText(name, screenX, screenY, alya.render.color("RED"), true)
            alya.render.drawText(
                "HP: " .. string.format("%.1f", health) .. "/" .. maxHealth,
                screenX, screenY + 10, alya.render.color("GREEN"), true
            )
        end
    end
end
```

### Team Detection

```lua
function isTeammate(player)
    -- This is a simplified example. In a real implementation,
    -- you would need to check team information from the game
    
    -- Get player name
    local name = alya.entity.getEntityName(player)
    
    -- Check if the player is in your team list
    local teammates = {"Player1", "Player2", "Player3"}
    for _, teammateName in ipairs(teammates) do
        if name == teammateName then
            return true
        end
    end
    
    return false
end

function findEnemies()
    local players = alya.entity.getPlayersInRange(50)
    local enemies = {}
    
    for _, player in ipairs(players) do
        -- Skip the local player and teammates
        if player ~= alya.mc.getPlayer() and not isTeammate(player) then
            table.insert(enemies, player)
        end
    end
    
    return enemies
end
```