---
sidebar_position: 7
---

# Player API

The Player API provides functions to access and modify player properties and actions.

All functions in this API are accessible through the `player` table.

## Player Finding

### getPlayersInRange

```lua
player.getPlayersInRange(range)
```

Gets all players within the specified range of the local player.

**Parameters:**
- `range` (number) - The range in blocks

**Returns:**
- `table` - A list of player entities within the range

**Example:**
```lua
local players = player.getPlayersInRange(10)
util.chatInfo("Players within 10 blocks: " .. #players)
for i, p in ipairs(players) do
    local name = entity.getEntityName(p)
    util.chatInfo(i .. ". " .. name)
end
```

## Player Status

### getHealth

```lua
alya.player.getHealth()
```

Gets the player's current health.

**Returns:**
- `number` - The player's health points, or `0` if the player is not available

**Example:**
```lua
local health = alya.player.getHealth()
alya.util.chatInfo("Health: " .. health .. " / " .. alya.player.getMaxHealth())
```

### getMaxHealth

```lua
alya.player.getMaxHealth()
```

Gets the player's maximum health.

**Returns:**
- `number` - The player's maximum health points, or `0` if the player is not available

**Example:**
```lua
local maxHealth = alya.player.getMaxHealth()
alya.util.chatInfo("Max Health: " .. maxHealth)
```

### getFoodLevel

```lua
alya.player.getFoodLevel()
```

Gets the player's food level.

**Returns:**
- `number` - The player's food level (0-20), or `0` if the player is not available

**Example:**
```lua
local foodLevel = alya.player.getFoodLevel()
alya.util.chatInfo("Food Level: " .. foodLevel)
```

### getSaturation

```lua
alya.player.getSaturation()
```

Gets the player's saturation level.

**Returns:**
- `number` - The player's saturation level, or `0` if the player is not available

**Example:**
```lua
local saturation = alya.player.getSaturation()
alya.util.chatInfo("Saturation: " .. saturation)
```

### getExperience

```lua
alya.player.getExperience()
```

Gets the player's experience level.

**Returns:**
- `number` - The player's experience level, or `0` if the player is not available

**Example:**
```lua
local xpLevel = alya.player.getExperience()
alya.util.chatInfo("XP Level: " .. xpLevel)
```

## Movement and Position

### getVelocity

```lua
alya.player.getVelocity()
```

Gets the player's velocity.

**Returns:**
- `table` - A table with the following fields:
  - `x` - The x component of the velocity
  - `y` - The y component of the velocity
  - `z` - The z component of the velocity
- `nil` if the player is not available

**Example:**
```lua
local velocity = alya.player.getVelocity()
if velocity then
    alya.util.chatInfo("Velocity: " .. velocity.x .. ", " .. velocity.y .. ", " .. velocity.z)
end
```

### getYaw

```lua
alya.player.getYaw()
```

Gets the player's yaw (horizontal rotation).

**Returns:**
- `number` - The player's yaw in degrees, or `0` if the player is not available

**Example:**
```lua
local yaw = alya.player.getYaw()
alya.util.chatInfo("Yaw: " .. yaw)
```

### getPitch

```lua
alya.player.getPitch()
```

Gets the player's pitch (vertical rotation).

**Returns:**
- `number` - The player's pitch in degrees, or `0` if the player is not available

**Example:**
```lua
local pitch = alya.player.getPitch()
alya.util.chatInfo("Pitch: " .. pitch)
```

### isOnGround

```lua
alya.player.isOnGround()
```

Checks if the player is on the ground.

**Returns:**
- `boolean` - `true` if the player is on the ground, `false` otherwise

**Example:**
```lua
if alya.player.isOnGround() then
    alya.util.chatInfo("Player is on the ground")
else
    alya.util.chatInfo("Player is in the air")
end
```

### isTouchingWater

```lua
alya.player.isTouchingWater()
```

Checks if the player is touching water.

**Returns:**
- `boolean` - `true` if the player is touching water, `false` otherwise

**Example:**
```lua
if alya.player.isTouchingWater() then
    alya.util.chatInfo("Player is in water")
else
    alya.util.chatInfo("Player is not in water")
end
```

### isInLava

```lua
alya.player.isInLava()
```

Checks if the player is in lava.

**Returns:**
- `boolean` - `true` if the player is in lava, `false` otherwise

**Example:**
```lua
if alya.player.isInLava() then
    alya.util.chatInfo("Player is in lava!")
else
    alya.util.chatInfo("Player is not in lava")
end
```

## Player Abilities

### isCreative

```lua
alya.player.isCreative()
```

Checks if the player is in creative mode.

**Returns:**
- `boolean` - `true` if the player is in creative mode, `false` otherwise

**Example:**
```lua
if alya.player.isCreative() then
    alya.util.chatInfo("Player is in creative mode")
else
    alya.util.chatInfo("Player is not in creative mode")
end
```

### canFly

```lua
alya.player.canFly()
```

Checks if the player can fly.

**Returns:**
- `boolean` - `true` if the player can fly, `false` otherwise

**Example:**
```lua
if alya.player.canFly() then
    alya.util.chatInfo("Player can fly")
else
    alya.util.chatInfo("Player cannot fly")
end
```

## Player Actions

### setPosition

```lua
alya.player.setPosition(x, y, z)
```

Sets the player's position.

**Parameters:**
- `x` (number) - The x coordinate
- `y` (number) - The y coordinate
- `z` (number) - The z coordinate

**Example:**
```lua
-- Teleport the player 5 blocks up
local pos = alya.mc.getPlayerPosition()
alya.player.setPosition(pos.x, pos.y + 5, pos.z)
```

### setVelocity

```lua
alya.player.setVelocity(x, y, z)
```

Sets the player's velocity.

**Parameters:**
- `x` (number) - The x component of the velocity
- `y` (number) - The y component of the velocity
- `z` (number) - The z component of the velocity

**Example:**
```lua
-- Make the player jump higher
alya.player.setVelocity(0, 0.5, 0)

-- Launch the player forward
local yaw = alya.player.getYaw() * (math.pi / 180)
local speed = 1.0
alya.player.setVelocity(-math.sin(yaw) * speed, 0.5, math.cos(yaw) * speed)
```

### setYaw

```lua
alya.player.setYaw(yaw)
```

Sets the player's yaw (horizontal rotation).

**Parameters:**
- `yaw` (number) - The yaw in degrees

**Example:**
```lua
-- Make the player face north (0 degrees)
alya.player.setYaw(0)

-- Make the player face east (90 degrees)
alya.player.setYaw(90)
```

### setPitch

```lua
alya.player.setPitch(pitch)
```

Sets the player's pitch (vertical rotation).

**Parameters:**
- `pitch` (number) - The pitch in degrees

**Example:**
```lua
-- Make the player look straight ahead
alya.player.setPitch(0)

-- Make the player look up
alya.player.setPitch(-90)

-- Make the player look down
alya.player.setPitch(90)
```

### jump

```lua
alya.player.jump()
```

Makes the player jump.

**Example:**
```lua
alya.player.jump()
```

### swingHand

```lua
alya.player.swingHand()
```

Makes the player swing their main hand.

**Example:**
```lua
alya.player.swingHand()
```

## Usage Examples

### Auto-Jump

```lua
function onUpdate()
    if alya.mc.isInGame() and alya.player.isOnGround() then
        -- Check if there's a block in front of the player
        local pos = alya.mc.getPlayerPosition()
        local yaw = alya.player.getYaw() * (math.pi / 180)

        -- Calculate the position 1 block in front of the player
        local frontX = pos.x - math.sin(yaw)
        local frontZ = pos.z + math.cos(yaw)

        -- Check if there's a block at feet level
        if not alya.world.isAir(math.floor(frontX), math.floor(pos.y), math.floor(frontZ)) then
            alya.player.jump()
        end
    end
end
```

### Automatic Swimming

```lua
function onUpdate()
    if alya.mc.isInGame() and alya.player.isTouchingWater() then
        -- Get current velocity
        local vel = alya.player.getVelocity()

        -- Apply upward velocity to keep the player from sinking
        if vel.y < 0 then
            alya.player.setVelocity(vel.x, 0.1, vel.z)
        end
    end
end
```

### Look At Nearest Player

```lua
function lookAtNearestPlayer()
    local nearestPlayer = alya.entity.getClosestPlayer()
    if nearestPlayer then
        local playerPos = alya.mc.getPlayerPosition()
        local targetPos = alya.entity.getEntityPosition(nearestPlayer)

        -- Calculate angle to target
        local deltaX = targetPos.x - playerPos.x
        local deltaY = (targetPos.y + 1.62) - (playerPos.y + 1.62) -- Eye height
        local deltaZ = targetPos.z - playerPos.z

        local yaw = math.atan2(deltaZ, deltaX) * (180 / math.pi) - 90
        local dist = math.sqrt(deltaX * deltaX + deltaZ * deltaZ)
        local pitch = -math.atan2(deltaY, dist) * (180 / math.pi)

        -- Set player rotation
        alya.player.setYaw(yaw)
        alya.player.setPitch(pitch)
    end
end
```
