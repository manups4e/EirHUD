---
title: Progress Bar API
parent: API Documentation
nav_order: 13
permalink: /api/progress-bar/
---

# Progress Bar API

![progress bar example]({{ site.baseurl }}/images/progress_bar.png)

The Progress Bar component is a dedicated UI element for "Timed Actions".
It is a standard GTA styled Bar typically used when the player performs an action that takes time to complete, such as **Repairing a Vehicle**, **Reviving a Player**, or **Crafting an Item**.



## ⚙️ Internal Architecture

### Thread Safety & Overlap Protection
This component is built to handle spam or conflicting calls gracefully using a **Job ID System**.
1.  When you call `Start()`, the component increments an internal `jobId` integer.
2.  It spawns a new thread for the animation loop, tied to that specific ID.
3.  If `Start()` is called again *before* the first one finishes, the ID increments again.
4.  The previous thread notices the ID mismatch (`self.jobId ~= currentJobId`) and terminates immediately.
**Result:** You never have to worry about manually stopping an old bar before starting a new one; the system auto-cancels the old one.

### Time-Based Interpolation
The progress calculation uses `GetGameTimer()` (Real Time) rather than frame counting.
- This ensures the duration is accurate (e.g., exactly 5000ms) regardless of the client's FPS.
- It guarantees smooth interpolation from 0% to 100%.

---

## Accessing the API

```lua
local Bar = exports['eir_hud']:GetEirAPI().ProgressBar
-- or
local Bar = exports['eir_hud']:GetProgressBarAPI()
```

---

## Usage

### `Start(duration, title, color)`
Begins a new timed action. The bar automatically appears, fills up, and then hides itself upon completion.

**Parameters:**
- `duration`: `integer` - Time in milliseconds.
- `title`: `string` - Text displayed on the bar.
- `color`: `integer` / `hex` - Color of the fill bar.

**Example:**
```lua
-- Start a 5-second repair action
Bar.Start(5000, "REPAIRING VEHICLE", SColor.HUD_Blue)
```

### `Stop()`
Interrupts the current action immediately.
- Stops the internal thread.
- Hides the component.
- **Note:** Use this if the player cancels the action (e.g., moves away or presses Cancel).

```lua
-- Player moved, cancel the bar
Bar.Stop()
```

### `SetProgress(title, percentage, color)`
**Advanced Use Only.**
Updates the bar manually without using the internal timer thread.
Useful if you want to bind the progress to an external variable (like file download progress or a specific sync variable) rather than time.

- `percentage`: `number` (0.0 - 100.0).

---

## Visibility & Positioning

### `ShowComponent()` / `HideComponent()` / `ToggleComponent()`
Standard visibility controls.
*Note: `Start()` calls Show automatically, and the internal loop calls Hide when finished.*

### `SetComponentPosition(x, y)` / `SetComponentSize(w, h)`
Standard positioning controls.

### `SetComponentColour(col)`
Updates the background/border tint.

---

## Data Retrieval

### `GetComponentInfo()`
Returns the base component info.
- **Returns:** [**`BaseComponentInfo`**]({{ site.baseurl }}/api/structures#basecomponentinfo)

### `GetBoundsFromAnchor()`
Calculates the exact screen boundaries.
- **Returns:** [**`BaseComponentData`**]({{ site.baseurl }}/api/structures#basecomponentdata-bounds)
