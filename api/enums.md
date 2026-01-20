---
title: Enums & Constants
parent: API Documentation
nav_order: 98
permalink: /api/enums/
---

# Enums & Constants

To interact with the API effectively, you often need to pass specific integer values (IDs) to represent components, icons, or status lights.
This page serves as a reference for all these constant values.

## Components IDs

Used in functions like `ShowComponent(id)`, `HideComponent(id)`, and `GetEirAPI().EnableComponent(id)`.

| ID | Constant Name | Description |
| :--- | :--- | :--- |
| `1` | `HUD_COMPASS` | The top navigation bar. |
| `2` | `HUD_RESERVED_SLOTS` | The items/weapons hotbar. |
| `3` | `HUD_WORLD_ELEMENTS` | 3D Markers projection layer. |
| `4` | `HUD_INTERACTION` | Fixed help text panel. |
| `5` | `HUD_SERVER_LOGO` | Server branding. |
| `6` | `HUD_SERVER_INFO` | Player stats (Cash, Job, ID). |
| `7` | `HUD_AUDIO_VOICE` | Voice proximity icon. |
| `8` | `HUD_AUDIO_RADIO` | Radio members list. |
| `9` | `HUD_NOTIFICATIONS` | The feed system. |
| `10` | `HUD_STATUS_BARS` | Health, Armor, and Needs. |
| `11` | `HUD_VEHICLE_DASHBOARD` | Speedometer and Cluster. |
| `12` | `HUD_PROGRESS_BAR` | Timed action bar. |
| `13` | `HUD_EQUIPPED_WEAPON` | Current weapon display. |

---

## Component Status

Returned by `GetComponentStatus(id)`. Represents the internal lifecycle state of a component.

| Value | State | Description |
| :--- | :--- | :--- |
| `-1` | `NOT_IN_LIST` | Invalid component ID. |
| `0` | `NOT_LOADED` | Initial state, not yet processed. |
| `1` | `STREAMING` | Assets are being requested. |
| `2` | `LOADING` | Loading into Scaleform. |
| `3` | `LOAD_IN_PROGRESS` | Initialization logic running. |
| `4` | `LOAD_COMPLETE` | Ready but not yet rendered. |
| `5` | `LOADED` | Fully initialized in memory. |
| `6` | `LOAD_ERROR` | Failed to load. |
| `7` | `ONSCREEN` | **Active and Visible.** |
| `8` | `HIDDEN` | Active but manually hidden. |
| `9` | `DEACTIVATED` | Removed from processing logic. |

---

## Vehicle Dashboard Symbols

Used in the Vehicle Dashboard API to force specific warning lights (e.g., `SetDashboardLightOverride(id, color)`).

| ID | Symbol Name | Description |
| :--- | :--- | :--- |
| `0` | `indicator_left` | Left Turn Signal |
| `1` | `indicator_right` | Right Turn Signal |
| `2` | `handbrake` | Handbrake / Brake Warning |
| `3` | `engine_light` | Check Engine Light |
| `4` | `abs_light` | ABS Warning |
| `5` | `petrol_light` | Low Fuel Warning |
| `6` | `temp_light` | High Temperature Warning |
| `7` | `oil_light` | Oil Pressure Warning |
| `8` | `oil_temp` | Oil Temperature |
| `9` | `headlights` | Low Beams |
| `10` | `full_beam` | High Beams |
| `11` | `battery_light` | Battery / Alternator Warning |
| `12` | `seatbelt` | Seatbelt Warning |
| `13` | `hood` | Open Hood (Bonnet) Indicator |
| `14` | `trunk` | Open Trunk (Boot) Indicator |
| `15` | `doors` | Open Doors Indicator |
| `16` | `towmode` | Tow/Haul Mode |

---

## Notification Icons

Used in the Notifications API to set the icon displayed next to the alert title.

| ID | Constant Name | Visual Description |
| :--- | :--- | :--- |
| `0` | `BLANK` | No Icon (Text only) |
| `1` | `MESSAGE` | Speech Bubble |
| `2` | `EMAIL` | Envelope |
| `3` | `NEW_CONTACT` | Silhouette with + symbol |
| `4` | `DRIVER` | Steering Wheel / Car |
| `5` | `HACKER` | Skull / Glitch Icon |
| `6` | `SHOOTER` | Crosshair / Gun |
| `7` | `INVITE` | Ticket / Invite Envelope |
| `8` | `RP` | RP Logo / Star |
| `9` | `CASH` | Dollar Sign (Green style) |
| `10` | `AP` | Arena Points / Trophy |
| `11` | `XP_ALT` | Experience / Rank Icon |
| `12` | `CASH_ALT` | Alternative Money Stack |

## Replay Notification Type
Based on this first parameter, the behaviour of the notification changes.

| ID |Type |
| :--- | :--- |
| `0` | `TYPE_DIRECTOR_RECORDING` |
| `1` | `TYPE_BUTTON_ICON` |
| `2` | `TYPE_ACTION_REPLAY` |

## Replay Notification Icon
These icons change based on [Notification Type]({{ site.baseurl }}/api/enums#replay-notification-type)
if type is `TYPE_DIRECTOR_RECORDING`:

| ID |Type |
| :--- | :--- |
| `0` | `RECORDING_BUFFER_ICON` |
| `1` | `RECORDING_START_STOP_ICON` |

if type is `TYPE_BUTTON_ICON` and the string icon parameter is empty:

| ID |Type |
| :--- | :--- |
| `0` | `UP` |
| `1` | `DOWN` |
| `2` | `LEFT` |
| `3` | `RIGHT` |
| `4` | `DPAD_UP` |
| `5` | `DPAD_DOWN` |
| `6` | `DPAD_LEFT` |
| `7` | `DPAD_RIGHT` |
| `8` | `DPAD_NONE` |
| `9` | `DPAD_ALL` |
| `10` | `DPAD_UPDOWN` |
| `11` | `DPAD_LEFTRIGHT` |
| `12` | `LSTICK_UP` |
| `13` | `LSTICK_DOWN` |
| `14` | `LSTICK_LEFT` |
| `15` | `LSTICK_RIGHT` |
| `16` | `LSTICK_NONE` |
| `17` | `LSTICK_ALL1` |
| `18` | `LSTICK_UPDOWN` |
| `19` | `LSTICK_LEFTRIGHT` |
| `20` | `LSTICK_ROTATE` |
| `21` | `RSTICK_UP` |
| `22` | `RSTICK_DOWN` |
| `23` | `RSTICK_LEFT` |
| `24` | `RSTICK_RIGHT` |
| `25` | `RSTICK_NONE` |
| `26` | `RSTICK_ALL` |
| `27` | `RSTICK_UPDOWN` |
| `28` | `RSTICK_LEFTRIGHT` |
| `29` | `RSTICK_ROTATE` |
| `30` | `BUTTON_A` |
| `31` | `BUTTON_B` |
| `32` | `BUTTON_X` |
| `33` | `BUTTON_Y` |
| `34` | `BUTTON_LB` |
| `35` | `BUTTON_LT` |
| `36` | `BUTTON_RB` |
| `37` | `BUTTON_RT` |
| `38` | `BUTTON_START` |
| `39` | `BUTTON_BACK` |
| `40` | `SIXAXIS_DRIVE` |
| `41` | `SIXAXIS_PITCH` |
| `42` | `SIXAXIS_RELOAD` |
| `43` | `SIXAXIS_ROLL` |
| `44` | `ICON_SPINNER` |