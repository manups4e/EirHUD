---
title: Audio Voice API
parent: API Documentation
nav_order: 8
permalink: /api/audio-voice/
---

# Audio Voice API

![voice example]({{ site.baseurl }}/images/audio_voice.png)

The Audio Voice component is a minimalist proximity indicator.
It visualizes the player's current voice range (e.g., Whisper, Normal, Shout) and talking state. It is designed to be hooked into voice resources like **pma-voice** or **SaltyChat**.

## ⚙️ Internal Architecture

### Passive State Machine
Unlike the Compass or World Markers, this component has **zero CPU overhead** when idle.
It does not run any update loops. It simply waits for an external command (`SetProximityLevel`) to update the Scaleform state. This ensures that your voice indicator consumes resources only when the player actually changes their voice range or speaks.

---

## Accessing the API

```lua
local Voice = exports['eir_hud']:GetEirAPI().AudioVoice
-- or
local Voice = exports['eir_hud']:GetAudioVoiceAPI()
```

---

## Voice Management

### `SetProximityLevel(level, color)`
Updates the visual state of the voice icon.
Typically called inside your voice resource's event loop when the range changes.

**Parameters:**
- `level`: `integer` - The visual state ID.
  - `1`: Mute (No range)
  - `2`: Whisper (Low range)
  - `3`: Normal (Medium range)
  - `4`: Shout (High range)
- `color`: `SColor` - Optional color override for the icon (Default white).

**Example Integration (pma-voice):**
```lua
-- Inside your client-side voice script
AddEventHandler('pma-voice:setTalkingMode', function(mode)
    -- Map pma-voice modes (1, 2, 3) to EirHUD
    -- we add one because EirHUD level 1 is muted and pma-voice has no mute level apparently.
    exports['eir_hud']:GetAudioVoiceAPI().SetProximityLevel(mode + 1, SColor.HUD_White)
end)
```

---

## Visibility & Positioning

### `ShowComponent()` / `HideComponent()` / `ToggleComponent()`
Standard visibility controls.

### `SetComponentPosition(x, y)` / `SetComponentSize(w, h)`
Standard positioning controls.

### `SetComponentColour(col)`
Updates the default tint color of the icon.

---

## Data Retrieval

### `GetComponentInfo()`
Returns the base component info.
- **Returns:** [**`BaseComponentInfo`**]({{ site.baseurl }}/api/structures#basecomponentinfo)

### `GetBoundsFromAnchor()`
Calculates the exact screen boundaries of the icon.
- **Returns:** [**`BaseComponentData`**]({{ site.baseurl }}/api/structures#basecomponentdata-bounds)
