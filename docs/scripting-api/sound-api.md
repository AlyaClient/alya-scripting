---
sidebar_position: 11
---

# Sound API

The Sound API provides functions to play sounds in the Minecraft game.

All functions in this API are accessible through the `sound` table.

## Playing Sounds

### playSound

```lua
sound.playSound(soundName, volume, pitch)
```

Plays a sound at the player's location.

**Parameters:**
- `soundName` (string) - The name of the sound to play (format: "namespace:sound_id")
- `volume` (number) [optional] - The volume of the sound (default: 1.0)
- `pitch` (number) [optional] - The pitch of the sound (default: 1.0)

**Example:**
```lua
-- Play a block breaking sound
sound.playSound("minecraft:block.stone.break")

-- Play a note block sound at higher pitch and volume
sound.playSound("minecraft:block.note_block.harp", 2.0, 1.5)
```

### playSoundAt

```lua
sound.playSoundAt(soundName, x, y, z, volume, pitch)
```

Plays a sound at the specified location.

**Parameters:**
- `soundName` (string) - The name of the sound to play (format: "namespace:sound_id")
- `x` (number) - The x coordinate
- `y` (number) - The y coordinate
- `z` (number) - The z coordinate
- `volume` (number) [optional] - The volume of the sound (default: 1.0)
- `pitch` (number) [optional] - The pitch of the sound (default: 1.0)

**Example:**
```lua
-- Play an explosion sound at coordinates (0, 64, 0)
sound.playSoundAt("minecraft:entity.generic.explode", 0, 64, 0)

-- Play a chest opening sound at a specific location with custom volume and pitch
local pos = mc.getPlayerPosition()
sound.playSoundAt("minecraft:block.chest.open", pos.x + 3, pos.y, pos.z, 0.5, 0.8)
```

## Common Minecraft Sounds

Here are some commonly used Minecraft sounds that you can use with the Sound API:

### Block Sounds
- `minecraft:block.stone.break` - Stone breaking
- `minecraft:block.wood.break` - Wood breaking
- `minecraft:block.grass.break` - Grass breaking
- `minecraft:block.glass.break` - Glass breaking
- `minecraft:block.anvil.land` - Anvil landing
- `minecraft:block.chest.open` - Chest opening
- `minecraft:block.chest.close` - Chest closing
- `minecraft:block.ender_chest.open` - Ender chest opening
- `minecraft:block.piston.extend` - Piston extending
- `minecraft:block.piston.contract` - Piston contracting

### Entity Sounds
- `minecraft:entity.player.attack.strong` - Strong attack
- `minecraft:entity.player.attack.weak` - Weak attack
- `minecraft:entity.player.hurt` - Player hurt
- `minecraft:entity.player.levelup` - Level up
- `minecraft:entity.experience_orb.pickup` - XP orb pickup
- `minecraft:entity.item.pickup` - Item pickup
- `minecraft:entity.generic.explode` - Explosion
- `minecraft:entity.lightning_bolt.thunder` - Thunder
- `minecraft:entity.arrow.hit` - Arrow hit
- `minecraft:entity.enderman.teleport` - Enderman teleport

### UI Sounds
- `minecraft:ui.button.click` - Button click
- `minecraft:ui.toast.in` - Toast notification in
- `minecraft:ui.toast.out` - Toast notification out

### Note Block Sounds
- `minecraft:block.note_block.harp` - Harp note
- `minecraft:block.note_block.bass` - Bass note
- `minecraft:block.note_block.snare` - Snare note
- `minecraft:block.note_block.hat` - Hat note
- `minecraft:block.note_block.bell` - Bell note
- `minecraft:block.note_block.chime` - Chime note
- `minecraft:block.note_block.guitar` - Guitar note
- `minecraft:block.note_block.xylophone` - Xylophone note
- `minecraft:block.note_block.iron_xylophone` - Iron xylophone note
- `minecraft:block.note_block.cow_bell` - Cow bell note
- `minecraft:block.note_block.didgeridoo` - Didgeridoo note
- `minecraft:block.note_block.bit` - Bit note
- `minecraft:block.note_block.banjo` - Banjo note
- `minecraft:block.note_block.pling` - Pling note

## Usage Examples

### Directional Sound Radar

```lua
function soundRadar()
    -- Get nearby players
    local players = player.getPlayersInRange(20)
    local playerPos = mc.getPlayerPosition()

    for _, p in ipairs(players) do
        local entityPos = entity.getEntityPosition(p)

        -- Calculate distance
        local distance = math.distance(
            playerPos.x, playerPos.y, playerPos.z,
            entityPos.x, entityPos.y, entityPos.z
        )

        -- Play sound at player's position with volume based on distance
        local volume = 1.0 - (distance / 20)
        if volume > 0 then
            sound.playSoundAt("minecraft:block.note_block.pling", 
                entityPos.x, entityPos.y, entityPos.z, 
                volume, 1.0)
        end
    end
end
```

### Musical Sequence

```lua
function playMelody()
    local notes = {
        { sound = "minecraft:block.note_block.harp", pitch = 0.5 },
        { sound = "minecraft:block.note_block.harp", pitch = 0.6 },
        { sound = "minecraft:block.note_block.harp", pitch = 0.7 },
        { sound = "minecraft:block.note_block.harp", pitch = 0.8 },
        { sound = "minecraft:block.note_block.harp", pitch = 0.9 },
        { sound = "minecraft:block.note_block.harp", pitch = 1.0 },
        { sound = "minecraft:block.note_block.harp", pitch = 1.1 },
        { sound = "minecraft:block.note_block.harp", pitch = 1.2 }
    }

    for _, note in ipairs(notes) do
        sound.playSound(note.sound, 1.0, note.pitch)
        util.sleep(200) -- 200ms delay between notes
    end
end
```

### Ambient Sound Environment

```lua
function createAmbience()
    -- Play ambient sounds at random intervals
    while true do
        -- Choose a random ambient sound
        local sounds = {
            "minecraft:ambient.cave",
            "minecraft:ambient.underwater.loop",
            "minecraft:ambient.underwater.loop.additions",
            "minecraft:ambient.underwater.loop.additions.rare",
            "minecraft:ambient.underwater.loop.additions.ultra_rare",
            "minecraft:ambient.underwater.enter"
        }

        local randomSound = sounds[util.randomInt(1, #sounds)]

        -- Play at a random position near the player
        local pos = mc.getPlayerPosition()
        local offsetX = util.randomDouble(-10, 10)
        local offsetY = util.randomDouble(-5, 5)
        local offsetZ = util.randomDouble(-10, 10)

        sound.playSoundAt(randomSound, 
            pos.x + offsetX, 
            pos.y + offsetY, 
            pos.z + offsetZ, 
            0.3, 1.0)

        -- Wait for a random time before playing the next sound
        local delay = util.randomInt(5000, 15000)
        util.sleep(delay)
    end
end
```
