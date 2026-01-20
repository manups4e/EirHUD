---
title: Core API
parent: API Documentation
nav_order: 1
permalink: /api/core/
---

# Core API

The Core API provides global control over the entire HUD system.
It allows you to toggle visibility, manage performance modes, manipulate individual components, and manage their lifecycle.

## Accessing the API

The main entry point for all interactions is the `GetEirAPI` export.
This object not only contains global functions but also holds the API instances for every specific component.

```lua
local EirHUD = exports['eir_hud']:GetEirAPI()
```

---

## Global HUD Control

Methods to control the overall visibility and behavior of the interface.

### `ShowHUD()`
Makes the entire HUD visible. Triggers the `hudShown` event listener.
```lua
EirHUD.ShowHUD()
```

### `HideHUD()`
Hides the entire HUD. Triggers the `hudHidden` event listener.
```lua
EirHUD.HideHUD()
```

### `IsHUDVisible()`
Checks if the HUD is currently visible.
- **Returns:** `boolean`
```lua
if EirHUD.IsHUDVisible() then
    print("HUD is ON")
end
```

### `SetPerformanceMode(boolean)`
Toggles the optimization mode dynamically.
- `true`: Hybrid Threading at ~30FPS.
- `false`: Raw Performance at Max Client FPS, usually ~60FPS.
```lua
-- Enable optimization for low-end PC players
EirHUD.SetPerformanceMode(true)
```

### `GetPerformanceMode()`
Returns the current state of the performance mode.
- **Returns:** `boolean`

---

## Component Lifecycle & Visibility

These methods allow you to control **any** component using its numeric ID.
For almost all exports in this table, the first parameter is `componentID`: `integer`, See [Enums]({{ site.baseurl }}/api/enums#components-ids) for the full list of IDs.

| ID | Component |
| :--- | :--- |
| 1 | Compass |
| 2 | ReservedSlots |
| 3 | WorldMarkers |
| 4 | FixedInteraction |
| 5 | ServerLogo |
| 6 | ServerInfo |
| 7 | AudioVoice |
| 8 | AudioRadio |
| 9 | Notifications |
| 10 | StatusBars |
| 11 | VehicleDashboard |
| 12 | ProgressBar |
| 13 | EquippedWeapon |

### `EnableComponent(componentID)`
Initializes and constructs the component. Use this if a component was previously disabled/disposed or never initialized.
```lua
EirHUD.EnableComponent(1)
```

### `DisableComponent(componentID)`
Completely removes the component from memory/processing. Unlike `Hide`, this stops the component's update logic entirely.
```lua
EirHUD.DisableComponent(1)
```

### `ShowComponent(componentID)`
Forces an initialized component to be visible.
```lua
EirHUD.ShowComponent(1)
```

### `HideComponent(componentID)`
Forces an initialized component to be hidden (but keeps it in memory).
```lua
EirHUD.HideComponent(1)
```

### `ToggleComponent(componentID)`
Switches the visibility state of a component (Show -> Hide, or Hide -> Show).
```lua
EirHUD.ToggleComponent(1)
```

### `GetIsComponentVisible(componentID)`
Returns the intended visibility state of a component.
- **Returns:** `boolean`

### `GetComponentStatus(componentID)`
Returns the internal status ID of the component (e.g., OnScreen, OffScreen, Transitioning).
- **Returns:** `integer`

---

## Runtime Customization & Data

Methods to modify or retrieve the appearance and data of components on the fly.
*Note: These changes are temporary and reset on script restart.*

### `SetComponentPosition(componentID, x, y)`
Moves a component to a new normalized position.
- `x`, `y`: Screen coordinates (0.0 - 1.0).

```lua
-- Move Compass to center of screen
EirHUD.SetComponentPosition(1, 0.5, 0.5)
```

### `SetComponentSize(componentID, w, h)`
Resizes a component.
- `w`, `h`: Width and Height (0.0 - 1.0).

```lua
-- Make the Compass wider
EirHUD.SetComponentSize(1, 0.8, 0.15)
```

### `SetComponentColour(componentID, color)`
Updates the primary color of a component.
- `color`: An integer or hex value (Use `Config.Colors` helpers if available).

### `ResetComponentPositionFromOriginalConfigValues(componentID)`
Reverts the position and size of a component to the values defined in `Config.lua`.
```lua
EirHUD.ResetComponentPositionFromOriginalConfigValues(1)
```

### `GetComponentBoundsFromAnchor(componentID)`
Returns the calculated screen boundaries of the component based on its anchor point. Useful if you need to draw other UI elements relative to an HUD component.
- **Returns:** `table` `{ minX, minY, maxX, maxY }`

### `GetComponentInfo(componentID)`
Returns the full internal configuration table (`info`) of the component.
- **Returns:** `table`

---

## Component Accessors

The Core API object contains direct references to all component APIs.
You can access specific component methods directly through these properties or in case you can use the standalone exports to address the single components.

**Example:**
```lua
-- These two lines do exactly the same thing:
exports['eir_hud']:GetCompassAPI().ShowComponent(true) -- Standalone Export
exports['eir_hud']:GetEirAPI().Compass.ShowComponent(true) -- Core API Property
```

### Available Accessors

| Property | Description |
| :--- | :--- |
| `GetEirAPI().Compass` | Access the [**Compass API**]({{ site.baseurl }}/api/compass) |
| `GetEirAPI().ReservedSlots` | Access the [**Reserved Slots API**]({{ site.baseurl }}/api/reserved-slots) |
| `GetEirAPI().WorldMarkers` | Access the [**World Markers API**]({{ site.baseurl }}/api/world-markers) |
| `GetEirAPI().FixedInteraction` | Access the [**Fixed Interaction API**]({{ site.baseurl }}/api/fixed-interaction) |
| `GetEirAPI().ServerLogo` | Access the [**Server Logo API**]({{ site.baseurl }}/api/server-logo) |
| `GetEirAPI().ServerInfo` | Access the [**Server Info API**]({{ site.baseurl }}/api/server-info) |
| `GetEirAPI().AudioVoice` | Access the [**Audio Voice API**]({{ site.baseurl }}/api/audio-voice) |
| `GetEirAPI().AudioRadio` | Access the [**Audio Radio API**]({{ site.baseurl }}/api/audio-radio) |
| `GetEirAPI().Notifications` | Access the [**Notifications API**]({{ site.baseurl }}/api/notifications) |
| `GetEirAPI().StatusBars` | Access the [**Status Bars API**]({{ site.baseurl }}/api/status-bars) |
| `GetEirAPI().VehicleDashboard` | Access the [**Vehicle Dashboard API**]({{ site.baseurl }}/api/vehicle-dashboard) |
| `GetEirAPI().ProgressBar` | Access the [**Progress Bar API**]({{ site.baseurl }}/api/progress-bar) |
| `GetEirAPI().EquippedWeapon` | Access the [**Equipped Weapon API**]({{ site.baseurl }}/api/equipped-weapon) |