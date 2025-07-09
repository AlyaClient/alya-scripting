---
sidebar_position: 9
---

# Math API

The Math API provides mathematical utilities for calculations, particularly for 3D geometry and angles.

All functions in this API are accessible through the `alya.math` table.

## Distance Calculations

### distance

```lua
alya.math.distance(x1, y1, z1, x2, y2, z2)
```

Calculates the 3D distance between two points.

**Parameters:**
- `x1` (number) - The x coordinate of the first point
- `y1` (number) - The y coordinate of the first point
- `z1` (number) - The z coordinate of the first point
- `x2` (number) - The x coordinate of the second point
- `y2` (number) - The y coordinate of the second point
- `z2` (number) - The z coordinate of the second point

**Returns:**
- `number` - The distance between the two points

**Example:**
```lua
local playerPos = alya.mc.getPlayerPosition()
local distance = alya.math.distance(playerPos.x, playerPos.y, playerPos.z, 0, 64, 0)
alya.util.chatInfo("Distance to (0, 64, 0): " .. distance)
```

### distance2D

```lua
alya.math.distance2D(x1, z1, x2, z2)
```

Calculates the 2D distance between two points on the XZ plane (ignoring Y axis).

**Parameters:**
- `x1` (number) - The x coordinate of the first point
- `z1` (number) - The z coordinate of the first point
- `x2` (number) - The x coordinate of the second point
- `z2` (number) - The z coordinate of the second point

**Returns:**
- `number` - The 2D distance between the two points

**Example:**
```lua
local playerPos = alya.mc.getPlayerPosition()
local distance = alya.math.distance2D(playerPos.x, playerPos.z, 0, 0)
alya.util.chatInfo("Horizontal distance to (0, 0): " .. distance)
```

## Angle Utilities

### wrapAngle

```lua
alya.math.wrapAngle(angle)
```

Wraps an angle to the range [-180, 180).

**Parameters:**
- `angle` (number) - The angle to wrap in degrees

**Returns:**
- `number` - The wrapped angle

**Example:**
```lua
local wrappedAngle = alya.math.wrapAngle(370)
alya.util.chatInfo("370 degrees wrapped: " .. wrappedAngle) -- Should output 10
```

### angleTo

```lua
alya.math.angleTo(x, y, z)
```

Calculates the yaw and pitch angles needed to look at a specific point from the player's position.

**Parameters:**
- `x` (number) - The x coordinate of the target point
- `y` (number) - The y coordinate of the target point
- `z` (number) - The z coordinate of the target point

**Returns:**
- `table` - A table with the following fields:
  - `yaw` - The yaw angle in degrees
  - `pitch` - The pitch angle in degrees

**Example:**
```lua
-- Calculate angles to look at the origin (0, 0, 0)
local angles = alya.math.angleTo(0, 0, 0)
alya.util.chatInfo("Yaw: " .. angles.yaw .. ", Pitch: " .. angles.pitch)

-- Look at the calculated angles
alya.player.setYaw(angles.yaw)
alya.player.setPitch(angles.pitch)
```

## Interpolation and Clamping

### lerp

```lua
alya.math.lerp(start, endValue, factor)
```

Linearly interpolates between two values.

**Parameters:**
- `start` (number) - The starting value
- `endValue` (number) - The ending value
- `factor` (number) - The interpolation factor (0.0 - 1.0)

**Returns:**
- `number` - The interpolated value

**Example:**
```lua
-- Interpolate halfway between 10 and 20
local value = alya.math.lerp(10, 20, 0.5)
alya.util.chatInfo("Interpolated value: " .. value) -- Should output 15

-- Smooth movement
function smoothMove(targetX, targetY, targetZ, speed)
    local pos = alya.mc.getPlayerPosition()
    local newX = alya.math.lerp(pos.x, targetX, speed)
    local newY = alya.math.lerp(pos.y, targetY, speed)
    local newZ = alya.math.lerp(pos.z, targetZ, speed)
    alya.player.setPosition(newX, newY, newZ)
end
```

### clamp

```lua
alya.math.clamp(value, min, max)
```

Clamps a value between a minimum and maximum.

**Parameters:**
- `value` (number) - The value to clamp
- `min` (number) - The minimum allowed value
- `max` (number) - The maximum allowed value

**Returns:**
- `number` - The clamped value

**Example:**
```lua
-- Clamp a value between 0 and 100
local value = alya.math.clamp(-10, 0, 100)
alya.util.chatInfo("Clamped value: " .. value) -- Should output 0

-- Ensure a speed value is within reasonable bounds
local speed = alya.math.clamp(someCalculatedSpeed, 0.1, 2.0)
```

## Usage Examples

### Calculating Distance to Multiple Entities

```lua
function findNearbyEntities(range)
    local playerPos = alya.mc.getPlayerPosition()
    local entities = alya.mc.getEntities()
    local nearbyEntities = {}

    for _, entity in ipairs(entities) do
        local entityPos = alya.entity.getEntityPosition(entity)
        local distance = alya.math.distance(
            playerPos.x, playerPos.y, playerPos.z,
            entityPos.x, entityPos.y, entityPos.z
        )

        if distance <= range then
            table.insert(nearbyEntities, {
                entity = entity,
                distance = distance
            })
        end
    end

    -- Sort by distance
    table.sort(nearbyEntities, function(a, b)
        return a.distance < b.distance
    end)

    return nearbyEntities
end

local nearbyEntities = findNearbyEntities(10)
alya.util.chatInfo("Entities within 10 blocks: " .. #nearbyEntities)
for i, data in ipairs(nearbyEntities) do
    local name = alya.entity.getEntityName(data.entity)
    alya.util.chatInfo(i .. ". " .. name .. " (" .. string.format("%.2f", data.distance) .. " blocks)")
end
```

### Smooth Camera Movement

```lua
local targetYaw = 0
local targetPitch = 0
local smoothFactor = 0.1

function onUpdate()
    if alya.mc.isInGame() then
        -- Get current angles
        local currentYaw = alya.player.getYaw()
        local currentPitch = alya.player.getPitch()

        -- Calculate shortest path for yaw (handling the -180/180 boundary)
        local deltaYaw = targetYaw - currentYaw
        if deltaYaw > 180 then deltaYaw = deltaYaw - 360
        elseif deltaYaw < -180 then deltaYaw = deltaYaw + 360 end

        -- Interpolate angles
        local newYaw = currentYaw + deltaYaw * smoothFactor
        local newPitch = alya.math.lerp(currentPitch, targetPitch, smoothFactor)

        -- Clamp pitch to prevent flipping
        newPitch = alya.math.clamp(newPitch, -90, 90)

        -- Apply new angles
        alya.player.setYaw(newYaw)
        alya.player.setPitch(newPitch)
    end
end

-- Call this to set a new target to look at
function lookAt(x, y, z)
    local angles = alya.math.angleTo(x, y, z)
    targetYaw = angles.yaw
    targetPitch = angles.pitch
end
```

### Circular Movement Pattern

```lua
local centerX, centerY, centerZ = 0, 64, 0
local radius = 5
local height = 0
local angle = 0

function circleMovement()
    if alya.mc.isInGame() then
        -- Calculate new position on the circle
        local x = centerX + radius * math.cos(angle)
        local z = centerZ + radius * math.sin(angle)
        local y = centerY + height

        -- Move player to the new position
        alya.player.setPosition(x, y, z)

        -- Look at the center
        local angles = alya.math.angleTo(centerX, centerY, centerZ)
        alya.player.setYaw(angles.yaw)
        alya.player.setPitch(angles.pitch)

        -- Increment angle for next frame
        angle = angle + 0.05
        if angle > 2 * math.pi then
            angle = angle - 2 * math.pi
        end
    end
end
```
