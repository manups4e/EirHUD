---
title: Home
nav_order: 1
permalink: /
---

# EirHUD

![EirHUD Banner]({{ site.baseurl }}/images/banner.png)
(yes i asked gemini to make this 😽)


EirHUD is a **high-performance vector HUD system for FiveM** built entirely using native Scaleforms (Flash).
Unlike standard HTML/NUI interfaces, EirHUD renders **crisp, vector-based graphics** at any resolution (from 1080p up to 4K) without the performance overhead of a web renderer, this means that all CPU time in the resmon comes from the natives execution only with no NUI callbacks overhead.

- The script features a **Universal Bridge** that automatically detects your framework (**ESX, QB-Core, Qbox**) and adapts the logic accordingly.
{: .note}
> The framework files are fully editable and configurable to optimize the script execution.

- It includes a smart **Performance Mode** that separates heavy logic calculations from visual animations, ensuring smooth gameplay even on lower-end PCs.

{: .note}
> **Performance Transparency:** EirHUD typically runs between **0.05ms** and **0.08ms**. This baseline is due to **Native Scaleform Calls**, which are strictly monitored by FiveM's Resmon.
> Unlike HTML/NUI scripts—where the heavy rendering load is hidden in a separate CEF process (often consuming CPU without showing in Lua Resmon), `Eir HUD`'s usage is fully transparent.
>
> While NUI callbacks often suffer from **50ms - 250ms** latency due to JSON serialization, EirHUD's native communication is **instant**. The **Performance Mode** throttles non-essential updates to 30 FPS, saving ~0.04ms, but the baseline reflects the cost of having a **zero-latency** native interface.
---

## 📑 Documentation Structure

- [Getting Started](./getting-started.md) - Learn how to install the resource, load order, and framework requirements.
- [Configuration](./docs/configuration.md) - Deep dive into `Config.lua`, enabling components and Performance Mode.
- [API & Exports](./docs/api.md) - For Developers: learn how to use the **Override System** to control the HUD from your own scripts.
- [Components](./docs/components.md) - Detailed breakdown of the components such as: Compass, Dashboard, and Status.