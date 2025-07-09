---
sidebar_position: 4
---

# Render API

The Render API provides functions for drawing text and shapes on the screen.

All functions in this API are accessible through the `alya.render` table.

## Colors

### color

```lua
alya.render.color(colorNameOrRGB)
```

Creates a color value that can be used with rendering functions.

**Parameters:**
- `colorNameOrRGB` (string or numbers) - Either a color name (e.g., "RED") or RGB(A) values

**Returns:**
- `number` - An integer color value

**Examples:**
```lua
-- Using a color name
local redColor = alya.render.color("RED")

-- Using RGB values (red, green, blue)
local customColor = alya.render.color(255, 100, 50)

-- Using RGBA values (red, green, blue, alpha)
local transparentBlue = alya.render.color(0, 0, 255, 128)
```

Available color names:
- "BLACK"
- "WHITE"
- "RED"
- "GREEN"
- "BLUE"
- "YELLOW"
- "CYAN"
- "MAGENTA"
- "ORANGE"
- "PURPLE"
- "GRAY"
- "DARK_RED"
- "DARK_GREEN"
- "DARK_BLUE"

## Drawing Text

### drawText

```lua
alya.render.drawText(text, x, y, color, shadow)
```

Draws text on the screen.

**Parameters:**
- `text` (string) - The text to draw
- `x` (number) - The x coordinate
- `y` (number) - The y coordinate
- `color` (number) [optional] - The color of the text (default: white)
- `shadow` (boolean) [optional] - Whether to draw a shadow behind the text (default: false)

**Example:**
```lua
-- Draw white text at coordinates (10, 10)
alya.render.drawText("Hello, World!", 10, 10)

-- Draw red text with shadow
alya.render.drawText("Warning!", 10, 30, alya.render.color("RED"), true)

-- Draw custom colored text
local purple = alya.render.color(200, 0, 255)
alya.render.drawText("Custom color", 10, 50, purple)
```

## Drawing Shapes

### drawRect

```lua
alya.render.drawRect(x, y, width, height, color)
```

Draws a filled rectangle on the screen.

**Parameters:**
- `x` (number) - The x coordinate of the top-left corner
- `y` (number) - The y coordinate of the top-left corner
- `width` (number) - The width of the rectangle
- `height` (number) - The height of the rectangle
- `color` (number) [optional] - The color of the rectangle (default: white)

**Example:**
```lua
-- Draw a white rectangle
alya.render.drawRect(10, 10, 100, 50)

-- Draw a red rectangle
alya.render.drawRect(10, 70, 100, 50, alya.render.color("RED"))

-- Draw a semi-transparent blue rectangle
local blueTransparent = alya.render.color(0, 0, 255, 128)
alya.render.drawRect(10, 130, 100, 50, blueTransparent)
```

## Text Measurements

### getStringWidth

```lua
alya.render.getStringWidth(text)
```

Returns the width of a string in pixels when rendered.

**Parameters:**
- `text` (string) - The text to measure

**Returns:**
- `number` - The width of the text in pixels

**Example:**
```lua
local text = "Hello, World!"
local width = alya.render.getStringWidth(text)
alya.util.chatInfo("Text width: " .. width .. " pixels")

-- Center text on screen
local screenWidth = alya.mc.getScreenWidth()
local textWidth = alya.render.getStringWidth(text)
local x = (screenWidth - textWidth) / 2
alya.render.drawText(text, x, 10, alya.render.color("WHITE"), true)
```

### getStringHeight

```lua
alya.render.getStringHeight()
```

Returns the height of a single line of text in pixels.

**Returns:**
- `number` - The height of text in pixels

**Example:**
```lua
local height = alya.render.getStringHeight()
alya.util.chatInfo("Text height: " .. height .. " pixels")

-- Draw multiple lines of text with proper spacing
local lines = {"Line 1", "Line 2", "Line 3"}
local lineHeight = alya.render.getStringHeight()
for i, line in ipairs(lines) do
    local y = 10 + (i-1) * (lineHeight + 2)
    alya.render.drawText(line, 10, y)
end
```

## Usage in Scripts

The Render API is most commonly used in the `onRender2D` function of scripts, which is called every frame when the 2D elements are being rendered:

```lua
function onRender2D()
    -- Draw a HUD element
    local screenWidth = alya.mc.getScreenWidth()
    local screenHeight = alya.mc.getScreenHeight()
    
    -- Background
    alya.render.drawRect(10, 10, 100, 50, alya.render.color(0, 0, 0, 128))
    
    -- Text
    alya.render.drawText("FPS: " .. alya.mc.getFPS(), 15, 15, alya.render.color("WHITE"), true)
    alya.render.drawText("X: " .. math.floor(alya.mc.getPlayerPosition().x), 15, 25, alya.render.color("WHITE"), true)
    alya.render.drawText("Y: " .. math.floor(alya.mc.getPlayerPosition().y), 15, 35, alya.render.color("WHITE"), true)
    alya.render.drawText("Z: " .. math.floor(alya.mc.getPlayerPosition().z), 15, 45, alya.render.color("WHITE"), true)
end
```