---
title: Configuration
nav_order: 3
permalink: /configuration/
---

# Configuration

The `Config.lua` file is the heart of EirHUD default customization.
This file acts as the blueprint: every component reads these default values when the script initializes.
While you can modify everything here, remember that many values can also be overridden dynamically via exports during runtime.

## Global Settings

These settings control the core behavior of the HUD.

```lua
Config.Debug = false 
-- Set to true to visualize bounding boxes around components.
-- Essential when repositioning elements to see their exact "safe zone" limits.

Config.PerformancesMode = 1
-- 1 (True): Enables the "Hybrid Threading" system. 
--    - Heavy logic runs at ~30 FPS to save CPU.
--    - Recommended for production servers.
-- 0 (False): Disables optimizations. Everything runs at max client FPS.
-- Note: This mode can also be toggled dynamically via exports.
```

---

## Layout System (Read First)

Every component in EirHUD uses a unified positioning system designed to be **resolution-independent**.
Instead of fixed pixels, we use **Screen Percentages (0.0 to 1.0)** to ensure consistency across 1080p, 1440p, and 4K displays.

### 1. Alignment
Defines the "Anchor Point" of the component on the screen.
- **Horizontal:** `L` (Left), `C` (Center), `R` (Right)
- **Vertical:** `T` (Top), `C` (Center), `B` (Bottom)

### 2. Size
Defined as `{ w = width, h = height }`.
- Values represent a percentage of the screen size.
- Example: `0.5` is 50% of the screen, while `0.05` is 5%.
- **Note:** The internal vector art scales proportionally to this bounding box. Drastically changing the aspect ratio (e.g., making a square component a wide rectangle) may distort the visuals.

### 3. Position
A `vector2(x, y)` offset relative to the **Alignment**.
- **Important:** Coordinates are normalized. `0.1` equals 10% of the screen axis.

{: .warning}
> **Coordinate System Logic:** > The Scaleform subsystem always considers **0.0 as Left/Top** and **1.0 as Right/Bottom**.
> This rule applies regardless of your alignment settings.
>
> **Example:** If you align a component to the **Bottom-Right**:
> * Positive values (`0.1`) will push it **off-screen** (further Right/Down).
> * To move it inwards (Up or Left), you must use **negative values** (e.g., `position = vector2(-0.05, -0.1)`).

## Component Structure

Inside `Config.Components`, you will find a table for each module (Compass, Dashboard, StatusBars, etc.).
While specific options vary (e.g., RPM gauge limits vs. Compass blip distance), they all share the standard layout properties defined above:

```lua
Config.Components.ExampleComponent = {
    Enabled = true,             -- Master toggle, if false component will be default hidden and will need to be shown via exports.
    alignment = { "C", "B" },   -- Anchor point, at that coordinate it will be the 0,0 position
    size = { w = 0.5, h = 0.1 },-- Size relative to screen regardless of the resolution
    position = vector2(0, -0.1) -- Offset regardless of the resolution.
    -- ... specific component settings ...
}
```

Check the `Config.lua` file directly for the full list of available options for each component.