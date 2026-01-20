---
title: Data Structures
parent: API Documentation
nav_order: 99
permalink: /api/structures/
---

# Data Structures

Many API functions return complex objects containing detailed information about the component's state or position.
This page details the structure of these return values.

## BaseComponentInfo

Returned by `GetComponentInfo(id)`.
Contains the live configuration and state of a component.

| Field | Type | Description |
| :--- | :--- | :--- |
| `EnumId` | `integer` | Unique ID of the component. |
| `Status` | `integer` | Current internal status (e.g., OnScreen, Hidden). |
| `IntendedVisibleState` | `boolean` | `true` if the component *should* be visible, regardless of current status. |
| `Alignment` | `table` | `{"H", "V"}` alignment keys (e.g., `{"R", "B"}`). |
| `Position` | `vector2` | The normalized (0.0 - 1.0) position offset. |
| `Size` | `table` | `{ w = width, h = height }` in screen percentage. |
| `Colour` | `integer` | The current primary color (SColor format). |
| `Depth` | `integer` | Rendering depth layer (Z-index). |

---

## BaseComponentData (Bounds)

Returned by `GetComponentBoundsFromAnchor(id)`.
This powerful structure gives you the exact calculated coordinates of the component in different spaces (Screen, Resolution, Scaleform).

### Core Fields

| Field | Type | Description |
| :--- | :--- | :--- |
| `Orientation` | `string` | The alignment string (e.g., "RT" for Right-Top). |
| `ScreenSize` | `table` | `{ Width, Height }` in normalized screen space (0.0 - 1.0). |
| `ResolutionSize` | `table` | `{ Width, Height }` in actual pixels (e.g., 1920x1080). |
| `ScaleformSize` | `table` | `{ Width, Height }` in scaleform screen space pixels (e.g., 1280x720). |
| `ScreenCoords` | `ComponentPosition` | Bounding box in normalized screen space. |
| `ResolutionCoords` | `ComponentPosition` | Bounding box in pixel coordinates. |
| `ScaleformCoords` | `ComponentPosition` | Bounding box in scaleform screen space (e.g., 1280x720). |

### ComponentPosition Object

Used inside `BaseComponentData` to describe a rectangular area.

| Field | Type | Description |
| :--- | :--- | :--- |
| `TopLeft` | `vector2` | Coordinates of the top-left corner. |
| `TopRight` | `vector2` | Coordinates of the top-right corner. |
| `BottomLeft` | `vector2` | Coordinates of the bottom-left corner. |
| `BottomRight` | `vector2` | Coordinates of the bottom-right corner. |
| `Center` | `vector2` | Coordinates of the center point. |

### Example Usage

If you want to draw a red rectangle precisely where the Compass is currently rendered:

```lua
local bounds = exports['eir_hud']:GetEirAPI().GetComponentBoundsFromAnchor(1) -- 1 = Compass

-- Use ResolutionCoords for pixel-perfect drawing with DrawRect or similar natives
-- Note: GTA natives often use 0.5 centered coordinates, so you might use ScreenCoords instead.

local center = bounds.ScreenCoords.Center
local width = bounds.ScreenSize.Width
local height = bounds.ScreenSize.Height

-- Draw a rectangle over the compass
DrawRect(center.x, center.y, width, height, 255, 0, 0, 100)
```