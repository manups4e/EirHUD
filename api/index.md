---
title: "API Documentation"
layout: default
permalink: /api/
has_children: true
nav_order: 4
---

# EirHUD API

The **EirHUD API** provides developers with full control over **every single component** of the interface.
Nothing is hidden: if it's on the screen, you can control it via code, if it's hidden you can toggle it. The API structure mirrors the configuration structure, making it intuitive to find the module you need.

The system is designed to be:
- **Zero-Latency:** Direct communication with the Scaleform engine.
- **Framework Agnostic:** While it auto-detects ESX/QB, you can override any data manually.
- **Priority-Based:** Modules like the Dashboard allow you to "force" states (e.g., check engine light) over game data.

## What’s Included

Every component listed below exposes its own set of methods:

### 🚗 Vehicle & Navigation
- **Vehicle Dashboard** – Full control over speed, RPM, fuel, and specific warning lights overrides.
- **Compass** – Manage dynamic blips, heading, and visibility.
- **World Markers** – Project 3D coordinates onto the screen (for pings, mission objectives).

### 🎒 Interaction & Items
- **Reserved Slots** – A dynamic Hotbar (up to 10 slots) for items, weapons, or actions.
- **Equipped Weapon** – Display the currently held weapon info.
- **Fixed Interaction** – A static panel for contextual keybinds (e.g., "Press E to interact").
- **Progress Bar** – Timed action bars for crafting or interactions.

### 📊 Status & Info
- **Status Bars** – Control Health, Armor, Hunger, Thirst, and Stress values.
- **Server Info** – Update text labels for Job, Cash, Bank, and ID.
- **Server Logo** – Control the server branding and its animations.
- **Notifications** – A complete feed system for advanced alerts.

### 🎙️ Audio
- **Audio Voice** – Control the voice proximity indicator.
- **Audio Radio** – Manage the list of active radio speakers.

## Architecture Overview

EirHUD utilizes an Object-Oriented structure. Every specific module inherits from a `BaseComponent` class, ensuring consistent behavior for visibility and updates, while adding its own unique API methods.
Components are contained in a main HUD class handled as a component on its own internally.

```mermaid
classDiagram
    direction TB

    %% 1. The Parent Class (Abstract)
    class BaseComponent {
        <<Abstract>>
        +New(id)
        +SetVisible(bool)
        +UpdateComponent(gameTimer)
    }

    %% 2. The Children (Specific Modules)
    class VehicleDashboard {
        +SetSymbol(id, color)
        +SetLightOverride(id, color)
    }

    class ProgressBar {
        +Start(duration, text, color)
        +Stop()
    }

    class Compass {
        +AddBlip(data)
    }

    %% 3. Inheritance Relationships (Derives From)
    BaseComponent <|-- VehicleDashboard : Inherit
    BaseComponent <|-- ProgressBar : Inherit
    BaseComponent <|-- Compass : Inherit

    %% 4. The API Access Point
    class EirHUD_Export {
        <<Global Export>>
        +GetEirAPI()
    }

    %% The API gives access to the instances
    EirHUD_Export ..> VehicleDashboard : Access Instance
    EirHUD_Export ..> ProgressBar : Access Instance
    EirHUD_Export ..> Compass : Access Instance
    
    %% Styling
    style BaseComponent fill:#444,stroke:#fff,stroke-width:2px,color:#fff
    style EirHUD_Export fill:#0f0,stroke:#333,stroke-width:2px,color:#000
```

## Quick Example

There are 2 ways to use the HUD, you can:
- retrieve the whole API object and target the specific modules by name.

```lua
-- 1. Get the Main API Object
local EirAPI = exports['eir_hud']:GetEirAPI()

-- 2. Example: Trigger a Progress Bar
EirAPI.ProgressBar.Start(5000, "Repairing Vehicle...", SColor.FromRandomValues())

-- 3. Example: Force an Icon in the Hotbar (ReservedSlots)
EirAPI.ReservedSlots.UpdateSlot(1, {
    txn = "bandage",
    amount = 5,
    enabled = true
})

-- 4. Example: Update Server Info manually
EirAPI.ServerInfo.UpdateLabel(1, "Job: Police - Chief")
```

- Retrieve the single components via their own Get exports

```lua
local compass = exports['eir_hud']:GetCompassAPI()
local slots = exports['eir_hud']:GetReservedSlotsAPI()
local info = exports['eir_hud']:GetServerInfoAPI()

compass.ShowComponent()
slots.UpdateSlot(1, { txn = "bandage", amount = 5, enabled = true })
info.UpdateLabel(1, "Job: Police - Chief")
```

Explore the sub-pages to see the specific functions for each component.