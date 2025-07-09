---
sidebar_position: 10
---

# Network API

The Network API provides functions to send packets and interact with the Minecraft network layer.

All functions in this API are accessible through the `alya.network` table.

## Packet Handling

### sendPacket

```lua
alya.network.sendPacket(packet)
```

Sends a packet to the server.

**Parameters:**
- `packet` (Packet) - The packet object to send

**Example:**
```lua
-- This example requires advanced knowledge of Minecraft's packet system
local packet = somePacketCreationFunction()
alya.network.sendPacket(packet)
```

### sendMovementPacket

```lua
alya.network.sendMovementPacket(x, y, z, yaw, pitch, onGround, changePosition)
```

Sends a player movement packet to the server.

**Parameters:**
- `x` (number) - The x coordinate
- `y` (number) - The y coordinate
- `z` (number) - The z coordinate
- `yaw` (number) [optional] - The yaw angle (defaults to player's current yaw)
- `pitch` (number) [optional] - The pitch angle (defaults to player's current pitch)
- `onGround` (boolean) [optional] - Whether the player is on the ground (defaults to player's current onGround state)
- `changePosition` (boolean) [optional] - Whether to change the player's position (defaults to true)

**Example:**
```lua
-- Send a movement packet to teleport the player
local pos = alya.mc.getPlayerPosition()
alya.network.sendMovementPacket(pos.x + 5, pos.y, pos.z, nil, nil, true)
```

### sendHandSwing

```lua
alya.network.sendHandSwing()
```

Sends a hand swing packet to the server.

**Example:**
```lua
-- Swing the player's hand without actually performing the swing animation locally
alya.network.sendHandSwing()
```

## Understanding Network Packets

Minecraft's networking system uses packets to communicate between the client and server. The Network API allows you to send custom packets, which can be useful for advanced scripting scenarios.

### Types of Packets

1. **Movement Packets**: Used to update the player's position and rotation
2. **Action Packets**: Used for player actions like hand swinging, block breaking, etc.
3. **Chat Packets**: Used for sending chat messages
4. **Interaction Packets**: Used for interacting with entities, blocks, etc.

### When to Use Network Packets

Network packets should be used with caution, as improper use can lead to disconnection or even bans on some servers. Here are some legitimate use cases:

- Optimizing movement for specific scenarios
- Creating custom animations or effects
- Implementing advanced PvP techniques
- Automating repetitive tasks

## Usage Examples

### Rapid Fire

```lua
function rapidFire(clicks)
    for i = 1, clicks do
        alya.network.sendHandSwing()
        alya.util.sleep(50) -- 50ms delay between clicks
    end
end

-- Usage: rapidFire(10) to send 10 clicks
```

### Smooth Movement

```lua
function smoothMove(targetX, targetY, targetZ, steps)
    if not alya.mc.isInGame() then return end
    
    local pos = alya.mc.getPlayerPosition()
    local startX, startY, startZ = pos.x, pos.y, pos.z
    
    for i = 1, steps do
        local progress = i / steps
        local x = alya.math.lerp(startX, targetX, progress)
        local y = alya.math.lerp(startY, targetY, progress)
        local z = alya.math.lerp(startZ, targetZ, progress)
        
        alya.network.sendMovementPacket(x, y, z)
        alya.util.sleep(10) -- Small delay between packets
    end
end

-- Usage: smoothMove(x, y, z, 20) to move in 20 steps
```

### Position Spoofing

```lua
function positionSpoof(offsetX, offsetZ, duration)
    if not alya.mc.isInGame() then return end
    
    local startTime = alya.util.currentTimeMillis()
    local originalPos = alya.mc.getPlayerPosition()
    
    -- Store original position
    local realX, realY, realZ = originalPos.x, originalPos.y, originalPos.z
    
    -- Send spoofed position
    alya.network.sendMovementPacket(realX + offsetX, realY, realZ + offsetZ)
    
    -- Wait for duration
    while alya.util.currentTimeMillis() - startTime < duration do
        alya.util.sleep(50)
    end
    
    -- Return to real position
    alya.network.sendMovementPacket(realX, realY, realZ)
end

-- Usage: positionSpoof(3, 0, 1000) to appear 3 blocks to the right for 1 second
```

## Security Considerations

When using the Network API, keep the following security considerations in mind:

1. **Anti-Cheat Systems**: Many servers have anti-cheat systems that detect unusual packet patterns
2. **Rate Limiting**: Sending too many packets too quickly can trigger rate limiting or disconnection
3. **Packet Validation**: Servers validate packets and may reject invalid ones
4. **Server Rules**: Some servers prohibit the use of certain packet manipulations

Always use the Network API responsibly and in accordance with server rules.