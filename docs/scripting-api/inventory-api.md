---
sidebar_position: 8
---

# Inventory API

The Inventory API provides functions to manage the player's inventory and items.

All functions in this API are accessible through the `alya.inventory` table.

## Item Access

### getHeldItem

```lua
alya.inventory.getHeldItem()
```

Gets the item currently held in the player's main hand.

**Returns:**
- `ItemStack` - The item stack in the player's main hand, or `nil` if the player is not available

**Example:**
```lua
local heldItem = alya.inventory.getHeldItem()
if heldItem and not heldItem:isEmpty() then
    alya.util.chatInfo("Holding: " .. heldItem:getItem():toString())
else
    alya.util.chatInfo("Not holding any item")
end
```

### getOffHandItem

```lua
alya.inventory.getOffHandItem()
```

Gets the item currently held in the player's off hand.

**Returns:**
- `ItemStack` - The item stack in the player's off hand, or `nil` if the player is not available

**Example:**
```lua
local offHandItem = alya.inventory.getOffHandItem()
if offHandItem and not offHandItem:isEmpty() then
    alya.util.chatInfo("Off hand: " .. offHandItem:getItem():toString())
else
    alya.util.chatInfo("Nothing in off hand")
end
```

### getSlot

```lua
alya.inventory.getSlot(slot)
```

Gets the item in the specified inventory slot.

**Parameters:**
- `slot` (number) - The inventory slot index (0-35)
  - Hotbar: 0-8
  - Main inventory: 9-35
  - Armor slots: 36-39 (boots, leggings, chestplate, helmet)
  - Off hand: 40

**Returns:**
- `ItemStack` - The item stack in the specified slot, or `nil` if the player is not available or the slot is invalid

**Example:**
```lua
-- Check what's in the first hotbar slot
local firstSlotItem = alya.inventory.getSlot(0)
if firstSlotItem and not firstSlotItem:isEmpty() then
    alya.util.chatInfo("First slot: " .. firstSlotItem:getItem():toString())
else
    alya.util.chatInfo("First slot is empty")
end

-- Check what armor the player is wearing
local helmet = alya.inventory.getSlot(39)
if helmet and not helmet:isEmpty() then
    alya.util.chatInfo("Helmet: " .. helmet:getItem():toString())
else
    alya.util.chatInfo("No helmet equipped")
end
```

## Hotbar Management

### getSelectedSlot

```lua
alya.inventory.getSelectedSlot()
```

Gets the currently selected hotbar slot index.

**Returns:**
- `number` - The selected hotbar slot index (0-8), or `0` if the player is not available

**Example:**
```lua
local selectedSlot = alya.inventory.getSelectedSlot()
alya.util.chatInfo("Selected slot: " .. selectedSlot)
```

### setSelectedSlot

```lua
alya.inventory.setSelectedSlot(slot)
```

Sets the selected hotbar slot.

**Parameters:**
- `slot` (number) - The hotbar slot index to select (0-8)

**Example:**
```lua
-- Select the first hotbar slot
alya.inventory.setSelectedSlot(0)

-- Select the last hotbar slot
alya.inventory.setSelectedSlot(8)
```

## Item Search

### findItem

```lua
alya.inventory.findItem(itemName)
```

Finds the first slot containing an item with the specified name.

**Parameters:**
- `itemName` (string) - The name of the item to find

**Returns:**
- `number` - The slot index containing the item, or `-1` if the item was not found

**Example:**
```lua
local diamondSwordSlot = alya.inventory.findItem("diamond_sword")
if diamondSwordSlot >= 0 then
    alya.util.chatInfo("Diamond sword found in slot " .. diamondSwordSlot)
    -- Select the slot if it's in the hotbar
    if diamondSwordSlot < 9 then
        alya.inventory.setSelectedSlot(diamondSwordSlot)
    end
else
    alya.util.chatInfo("No diamond sword found")
end
```

### getItemCount

```lua
alya.inventory.getItemCount(itemName)
```

Counts the total number of items with the specified name in the inventory.

**Parameters:**
- `itemName` (string) - The name of the item to count

**Returns:**
- `number` - The total count of the specified item

**Example:**
```lua
local arrowCount = alya.inventory.getItemCount("arrow")
alya.util.chatInfo("You have " .. arrowCount .. " arrows")

local enderPearlCount = alya.inventory.getItemCount("ender_pearl")
alya.util.chatInfo("You have " .. enderPearlCount .. " ender pearls")
```

## Working with ItemStack Objects

When you get an item using functions like `getHeldItem()`, `getOffHandItem()`, or `getSlot()`, you receive an `ItemStack` object. Here are some common methods you can use with these objects:

### ItemStack:isEmpty()

Checks if the item stack is empty.

**Example:**
```lua
local item = alya.inventory.getHeldItem()
if not item:isEmpty() then
    alya.util.chatInfo("Holding an item")
end
```

### ItemStack:getItem()

Gets the item type.

**Example:**
```lua
local item = alya.inventory.getHeldItem()
if not item:isEmpty() then
    alya.util.chatInfo("Item type: " .. item:getItem():toString())
end
```

### ItemStack:getCount()

Gets the number of items in the stack.

**Example:**
```lua
local item = alya.inventory.getHeldItem()
if not item:isEmpty() then
    alya.util.chatInfo("Stack size: " .. item:getCount())
end
```

### ItemStack:getDamage()

Gets the damage value of the item.

**Example:**
```lua
local item = alya.inventory.getHeldItem()
if not item:isEmpty() then
    alya.util.chatInfo("Item damage: " .. item:getDamage())
end
```

### ItemStack:getMaxDamage()

Gets the maximum damage value of the item.

**Example:**
```lua
local item = alya.inventory.getHeldItem()
if not item:isEmpty() then
    local maxDamage = item:getMaxDamage()
    if maxDamage > 0 then
        local damage = item:getDamage()
        local durabilityPercent = math.floor((1 - damage / maxDamage) * 100)
        alya.util.chatInfo("Durability: " .. durabilityPercent .. "%")
    end
end
```

## Usage Examples

### Auto-Tool Selection

```lua
function selectBestTool()
    -- Find tools in the hotbar
    local pickaxeSlot = alya.inventory.findItem("pickaxe")
    local axeSlot = alya.inventory.findItem("axe")
    local shovelSlot = alya.inventory.findItem("shovel")
    
    -- Perform a raycast to see what block we're looking at
    local hit = alya.world.raycast(5)
    if hit and hit:getType():toString() == "BLOCK" then
        local pos = hit:getBlockPos()
        local block = alya.world.getBlock(pos.x, pos.y, pos.z)
        local blockName = block:toString()
        
        -- Select the appropriate tool based on the block type
        if blockName:find("stone") or blockName:find("ore") then
            if pickaxeSlot >= 0 and pickaxeSlot < 9 then
                alya.inventory.setSelectedSlot(pickaxeSlot)
                return true
            end
        elseif blockName:find("log") or blockName:find("planks") then
            if axeSlot >= 0 and axeSlot < 9 then
                alya.inventory.setSelectedSlot(axeSlot)
                return true
            end
        elseif blockName:find("dirt") or blockName:find("grass") or blockName:find("sand") or blockName:find("gravel") then
            if shovelSlot >= 0 and shovelSlot < 9 then
                alya.inventory.setSelectedSlot(shovelSlot)
                return true
            end
        end
    end
    
    return false
end
```

### Inventory Scanner

```lua
function scanInventory()
    local items = {}
    
    -- Scan all inventory slots
    for slot = 0, 35 do
        local item = alya.inventory.getSlot(slot)
        if item and not item:isEmpty() then
            local itemName = item:getItem():toString()
            local count = item:getCount()
            
            if items[itemName] then
                items[itemName] = items[itemName] + count
            else
                items[itemName] = count
            end
        end
    end
    
    -- Display the results
    alya.util.chatInfo("Inventory contents:")
    for itemName, count in pairs(items) do
        alya.util.chatInfo("- " .. itemName .. ": " .. count)
    end
end
```

### Durability Warning

```lua
function checkToolDurability()
    local heldItem = alya.inventory.getHeldItem()
    if heldItem and not heldItem:isEmpty() then
        local maxDamage = heldItem:getMaxDamage()
        if maxDamage > 0 then
            local damage = heldItem:getDamage()
            local durabilityPercent = math.floor((1 - damage / maxDamage) * 100)
            
            if durabilityPercent < 10 then
                alya.util.chatError("Warning: Tool durability is only " .. durabilityPercent .. "%!")
            end
        end
    end
end
```