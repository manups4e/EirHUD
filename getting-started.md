---
title: Getting Started
nav_order: 2
permalink: /getting-started/
---

# Getting Started
Here I'll show you the first steps on how to install **EirHUD**, configure the environment, and verify that everything is running smoothly.
When downloaded, the resource comes as a group folder  `[Eir]`. This contains 3 scripts:
- `eir_assets`: The scaleform with its own texture dictionary (they MUST always be together)
- `eir_hud`: The HUD itself, the main script which will automatically bind to your framework.
- `eir_test`: The test script with a ScaleformUI_Lua based menu to test, customize and learn about each HUD component.

First of all, please **do not rename any of these resources**. The resources names, especially `eir_hud` are used internally for exports and texture loading. Changing them might break the HUD or the API exports.

{: .warning}
Ensure that you are running a recent version of FiveM Server Artifacts (5848 or higher recommended) to support the latest Lua 5.4 features used in the optimization bridge.

## Requirements

- **FiveM Server** (Windows or Linux)
- **Voice System:** `pma-voice` (Required for the voice status indicator to work properly)
- **Framework (Optional):**
  - `es_extended` (ESX Legacy or v1)
  - `qb-core`
  - `qbx_core` (Qbox)
  - *Note: If no framework is found, the HUD runs in Standalone mode (Compass & Logo only). Everything else can be done via exports.*

## Installation

1. Copy the `[Eir]` folder into your FiveM `resources/` directory (or any sub-folder like `[scripts]`).
2. Add it to your `server.cfg`. **Crucial:** Pay attention to the start order.

```
# 1. Framework & Dependencies
ensure qb-core       # Or es_extended / qbx_core
ensure pma-voice     # Required for voice ranges
#### all the resources required ####

# 2. The HUD
ensure [Eir]
```

## Performances configuration.
- Performance Mode (Optional but Recommended): Open `config.lua` and verify the `PerformanceMode` setting.

```lua
Config.PerformanceMode = true
```
- True: Enables the smart frame-limiter. Heavy calculations and native-dependent animations (compass rotation / dashboard dials) run at 30FPS. (Saves ~0.04ms CPU)
- False: Runs everything at max FPS.

## Verification
Once the server is started, check your server console or client F8 console. EirHUD includes a smart bridge diagnostic.

You should see a message like this:

```
[eir_hud] Framework detected: Qbox
```
or
```
[eir_hud] Framework detected: ESX
```

{: .note }
>If you see Framework detected: Standalone but you are using ESX/QB, it means EirHUD started before your framework. Fix: Restart the server ensuring the load order in server.cfg is correct (Framework -> EirHUD).

{: .warning}
>Some framework scripts (example: `qb-hud`) don't have exports, so you'll need to manually adapt them to work with Eir and vice versa.

## Using the Exports (API)
Unlike older resources requiring file imports, EirHUD uses the native FiveM Exports system. You don't need to copy any API.lua file into your scripts.

Whenever you want to interact with the HUD (e.g., forcing a dashboard light), simply call the export directly from any resource:
```lua
-- Example: Force Check Engine Light (Red)
local dashboard = exports.eir_hud:GetVehicleDashboardAPI()
dashboard.SetSymbol(3, 2)
-- symbol ID 3: Check Engine
-- color 2: Red
```
[Add the link for "API Documentation" to the API folder.]: # 

Check the [API Documentation]({{ site.baseurl }}/api/index) for the full list of overrides and controls.