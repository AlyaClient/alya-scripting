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
player.getHealth()
```

Gets the player's current health.

**Returns:**
- `number` - The player's health points, or `0` if the player is not available

**Example:**
```lua
local health = player.getHealth()
util.chatInfo("Health: " .. health .. " / " .. player.getMaxHealth())
```

### getMaxHealth

```lua
player.getMaxHealth()
```

Gets the player's maximum health.

**Returns:**
- `number` - The player's maximum health points, or `0` if the player is not available

**Example:**
```lua
local maxHealth = player.getMaxHealth()
util.chatInfo("Max Health: " .. maxHealth)
```

### getFoodLevel

```lua
player.getFoodLevel()
```

Gets the player's food level.

**Returns:**
- `number` - The player's food level (0-20), or `0` if the player is not available

**Example:**
```lua
local foodLevel = player.getFoodLevel()
util.chatInfo("Food Level: " .. foodLevel)
```

### getSaturation

```lua
player.getSaturation()
```

Gets the player's saturation level.

**Returns:**
- `number` - The player's saturation level, or `0` if the player is not available

**Example:**
```lua
local saturation = player.getSaturation()
util.chatInfo("Saturation: " .. saturation)
```

### getExperience

```lua
player.getExperience()
```

Gets the player's experience level.

**Returns:**
- `number` - The player's experience level, or `0` if the player is not available

**Example:**
```lua
local xpLevel = player.getExperience()
util.chatInfo("XP Level: " .. xpLevel)
```

## Movement and Position

### getVelocity

```lua
player.getVelocity()
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
local velocity = player.getVelocity()
if velocity then
    util.chatInfo("Velocity: " .. velocity.x .. ", " .. velocity.y .. ", " .. velocity.z)
end
```

### getYaw

```lua
player.getYaw()
```

Gets the player's yaw (horizontal rotation).

**Returns:**
- `number` - The player's yaw in degrees, or `0` if the player is not available

**Example:**
```lua
local yaw = player.getYaw()
util.chatInfo("Yaw: " .. yaw)
```

### getPitch

```lua
player.getPitch()
```

Gets the player's pitch (vertical rotation).

**Returns:**
- `number` - The player's pitch in degrees, or `0` if the player is not available

**Example:**
```lua
local pitch = player.getPitch()
util.chatInfo("Pitch: " .. pitch)
```

### isOnGround

```lua
player.isOnGround()
```

Checks if the player is on the ground.

**Returns:**
- `boolean` - `true` if the player is on the ground, `false` otherwise

**Example:**
```lua
if player.isOnGround() then
    util.chatInfo("Player is on the ground")
else
    util.chatInfo("Player is in the air")
end
```

### isTouchingWater

```lua
player.isTouchingWater()
```

Checks if the player is touching water.

**Returns:**
- `boolean` - `true` if the player is touching water, `false` otherwise

**Example:**
```lua
if player.isTouchingWater() then
    util.chatInfo("Player is in water")
else
    util.chatInfo("Player is not in water")
end
```

### isInLava

```lua
player.isInLava()
```

Checks if the player is in lava.

**Returns:**
- `boolean` - `true` if the player is in lava, `false` otherwise

**Example:**
```lua
if player.isInLava() then
    util.chatInfo("Player is in lava!")
else
    util.chatInfo("Player is not in lava")
end
```

## Player Abilities

### isCreative

```lua
player.isCreative()
```

Checks if the player is in creative mode.

**Returns:**
- `boolean` - `true` if the player is in creative mode, `false` otherwise

**Example:**
```lua
if player.isCreative() then
    util.chatInfo("Player is in creative mode")
else
    util.chatInfo("Player is not in creative mode")
end
```

### canFly

```lua
player.canFly()
```

Checks if the player can fly.

**Returns:**
- `boolean` - `true` if the player can fly, `false` otherwise

**Example:**
```lua
if player.canFly() then
    util.chatInfo("Player can fly")
else
    util.chatInfo("Player cannot fly")
end
```

## Player Actions

### setPosition

```lua
player.setPosition(x, y, z)
```

Sets the player's position.

**Parameters:**
- `x` (number) - The x coordinate
- `y` (number) - The y coordinate
- `z` (number) - The z coordinate

**Example:**
```lua
-- Teleport the player 5 blocks up
local pos = mc.getPlayerPosition()
player.setPosition(pos.x, pos.y + 5, pos.z)
```

### setVelocity

```lua
player.setVelocity(x, y, z)
```

Sets the player's velocity.

**Parameters:**
- `x` (number) - The x component of the velocity
- `y` (number) - The y component of the velocity
- `z` (number) - The z component of the velocity

**Example:**
```lua
-- Make the player jump higher
player.setVelocity(0, 0.5, 0)

-- Launch the player forward
local yaw = player.getYaw() * (math.pi / 180)
local speed = 1.0
player.setVelocity(-math.sin(yaw) * speed, 0.5, math.cos(yaw) * speed)
```

### setYaw

```lua
player.setYaw(yaw)
```

Sets the player's yaw (horizontal rotation).

**Parameters:**
- `yaw` (number) - The yaw in degrees

**Example:**
```lua
-- Make the player face north (0 degrees)
player.setYaw(0)

-- Make the player face east (90 degrees)
player.setYaw(90)
```

### setPitch

```lua
player.setPitch(pitch)
```

Sets the player's pitch (vertical rotation).

**Parameters:**
- `pitch` (number) - The pitch in degrees

**Example:**
```lua
-- Make the player look straight ahead
player.setPitch(0)

-- Make the player look up
player.setPitch(-90)

-- Make the player look down
player.setPitch(90)
```

### jump

```lua
player.jump()
```

Makes the player jump.

**Example:**
```lua
player.jump()
```

### swingHand

```lua
player.swingHand()
```

Makes the player swing their main hand.

**Example:**
```lua
player.swingHand()
```

## Usage Examples

### Auto-Jump

```lua
function onUpdate()
    if mc.isInGame() and player.isOnGround() then
        -- Check if there's a block in front of the player
        local pos = mc.getPlayerPosition()
        local yaw = player.getYaw() * (math.pi / 180)

        -- Calculate the position 1 block in front of the player
        local frontX = pos.x - math.sin(yaw)
        local frontZ = pos.z + math.cos(yaw)

        -- Check if there's a block at feet level
        if not world.isAir(math.floor(frontX), math.floor(pos.y), math.floor(frontZ)) then
            player.jump()
        end
    end
end
```

### Automatic Swimming

```lua
function onUpdate()
    if mc.isInGame() and player.isTouchingWater() then
        -- Get current velocity
        local vel = player.getVelocity()

        -- Apply upward velocity to keep the player from sinking
        if vel.y < 0 then
            player.setVelocity(vel.x, 0.1, vel.z)
        end
    end
end
```

### Look At Nearest Player

```lua
function lookAtNearestPlayer()
    local nearestPlayer = entity.getClosestPlayer()
    if nearestPlayer then
        local playerPos = mc.getPlayerPosition()
        local targetPos = entity.getEntityPosition(nearestPlayer)

        -- Calculate angle to target
        local deltaX = targetPos.x - playerPos.x
        local deltaY = (targetPos.y + 1.62) - (playerPos.y + 1.62) -- Eye height
        local deltaZ = targetPos.z - playerPos.z

        local yaw = math.atan2(deltaZ, deltaX) * (180 / math.pi) - 90
        local dist = math.sqrt(deltaX * deltaX + deltaZ * deltaZ)
        local pitch = -math.atan2(deltaY, dist) * (180 / math.pi)

        -- Set player rotation
        player.setYaw(yaw)
        player.setPitch(pitch)
    end
end
```
