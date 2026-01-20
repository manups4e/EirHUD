---
title: World Markers API
parent: API Documentation
nav_order: 4
permalink: /api/world-markers/
---

# World Markers API

![markers]({{ site.baseurl }}/images/world_markers.gif)

The World Markers component allows you to project 2D UI elements onto 3D world coordinates.
Think of it as a system for **floating UI**: you can create health bars above players, mission objective markers, or interaction prompts that stick to specific locations in the game world.



## ⚙️ Internal Architecture

Rendering 2D elements that follow 3D objects requires heavy mathematical calculations (World-to-Screen projection) every frame. To ensure high performance, this component uses a **Smart Culling System**.

### 1. Active Element Rebuilding
Instead of blindly calculating screen positions for every single marker (which could be hundreds), the script runs a **Rebuild Loop** (every 1000ms or when triggered).
- It calculates the distance of every marker from the camera.
- It filters out markers that are beyond their `farScalingDistance`.
- It creates a lightweight list of only the **Active Elements** to be processed in the rendering loop.

### 2. Dynamic Scaling logic
The projection logic includes built-in scaling features handled natively:
- **Near/Far Scaling:** You can define a distance range (e.g., 0m to 500m). The marker will automatically scale down as it gets further away, creating a sense of depth.
- **Clamping:** You can choose if a marker should disappear when it goes off-screen (behind the camera) or if it should "stick" to the edge of the screen (like a damage indicator).

---

## Accessing the API

```lua
local Markers = exports['eir_hud']:GetEirAPI().WorldMarkers
-- or
local Markers = exports['eir_hud']:GetWorldMarkersAPI()
```

---

## Creating Markers

All `Add` functions return an **integer ID**. You **must** save this ID if you want to update or remove the marker later.

### `AddBlip(position, blipID, colour, scale)`
Adds a standard game blip icon at the specified coordinate.
- `position`: `vector3`
- `blipID`: `integer` (Game sprite ID)
- `colour`: `integer` (SColor)
- `scale`: `number` (Default: 100)

### `AddLabel(position, label, font, colour, scale)`
Adds floating text.
- `label`: `string`
- `font`: `string` (e.g., "$Font2")

### `AddProgressBar(position, width, height, isVertical, showContour, colour, scale)`
Adds a linear progress bar (useful for reloading/action timers).
- `width`, `height`: `integer` (Pixel dimensions)
- `isVertical`: `boolean`
- `showContour`: `boolean` (Black outline)

### `AddCircleBar(position, size, startAngle, endAngle, showContour, colour, scale)`
Adds a radial/circular progress bar (useful for health/stamina).
- `startAngle`, `endAngle`: `number` (Degrees)

### `AddTexture(position, txd, txn, w, h, colour, scale)`
Adds a custom image from a Texture Dictionary.
- `txd`, `txn`: `string` (Dictionary and Name)
- `w`, `h`: `integer` (Pixel dimensions)

### `AddWeapon(position, weaponHash, colour, scale)`
Adds a weapon icon derived from the game's weapon hash.

---

## Managing Markers

Once a marker is created, you use its **ID** to manipulate it.

### `UpdatePosition(id, position, offset)`
Moves a marker to a new 3D location.
- `position`: `vector3` (New world coords)
- `offset`: `vector3` (Optional offset from the position)

```lua
-- Example: Make a label follow a player
local playerPed = PlayerPedId()
local coords = GetEntityCoords(playerPed)
-- Update the marker to stay 1.0 unit above the player's head
Markers.UpdatePosition(myLabelId, coords, vector3(0, 0, 1.0))
```

### `RemoveElement(id)`
Deletes the marker and frees up memory.

---

## Visual Updates

These methods update the style or content of an existing marker.

### `UpdateColour(id, colour)`
Updates the tint color.

### `UpdateScale(id, scale, nearDist, farDist, minScale, maxScale)`
Configures the distance scaling logic.
- `scale`: Base scale multiplier.
- `nearDist` / `farDist`: Distance range for auto-scaling.
- `minScale` / `maxScale`: Clamping limits (0.0 - 1.0).

### `UpdateVisibilityFlags(id, showIfOutOfSight, keepScalingOutOfSight)`
- `showIfOutOfSight`: If `true`, the marker stays on screen edges when the target is behind the camera.

### Specific Updates
- `UpdateLabel(id, label, font, colour, scale)`
- `UpdateProgressBar(id, percentage, colour)`
- `UpdateCircleBar(id, percentage, colour)`
- `UpdateTexture(id, txd, txn, w, h, colour, scale)`

---

## Data Retrieval

### `GetComponentInfo()`
Returns the base component info.
- **Returns:** [**`BaseComponentInfo`**]({{ site.baseurl }}/api/structures#basecomponentinfo)

### `GetValuesRuntime()`
Returns the list of all currently tracked elements.