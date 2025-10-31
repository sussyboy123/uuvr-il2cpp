# Universal Unity VR for Modern IL2CPP Unity Games

> **Maintenance status:** This fork may be abandoned soon. I'm experimenting with getting UUVR running on modern IL2CPP Unity titles, as long as it doesn't become too much work.

This repository contains a lightly opinionated fork of [Raicuparta's Universal Unity VR (UUVR)](https://github.com/Raicuparta/UniversalUnityVR), tailored for Unity games that ship solely with contemporary IL2CPP runtimes. It keeps the original vision—turn any flat Unity game into a VR experience with minimal per-game code—while updating the bootstrap, XR shims, and deployment flow so they survive engine changes introduced in Unity 2021+.

## Why this fork exists

Modern IL2CPP builds regularly break the assumptions made by the legacy UUVR project. This fork aims to:

* Track Unity API and metadata changes that impact IL2CPP patching.
* Provide up-to-date XR loader shims (OpenXR, OpenVR, SteamVR) compatible with current runtimes.
* Smooth out the deployment pipeline so the mod remains simple to drop into new games.
* Offer a staging ground for experiments—features may land here before they are proposed upstream.

Because this work happens in spare time, expect rough edges and incomplete documentation. Feedback is appreciated, but long-term maintenance is not promised.

## Key systems

| Area | Description |
| --- | --- |
| **Plugin bootstrap** | `UuvrCore` is injected as a persistent host object that registers IL2CPP-safe behaviours, installs Harmony patches, and coordinates VR mode toggling inside the running game. |
| **Camera management** | `VrCameraManager` discovers every in-game camera, attaches VR-aware wrappers, and keeps stereoscopic rendering synchronised with the active headset pose. |
| **Runtime configuration** | `ModConfiguration` exposes BepInEx configuration entries for camera tracking, depth overrides, physics timing, UI handling, and per-title quirks. |
| **UI reprojection** | `VrUiManager` renders flat UIs to textures, then projects them as in-world canvases, overlays, or mirrored quads depending on configuration. |
| **Session resilience** | Core systems survive scene loads, rebuild themselves when destroyed, and align Unity physics timing with HMD refresh rates when requested. |

## Repository layout

* `Uuvr/` – Core managed mod that plugs into IL2CPP via BepInEx and orchestrates the VR runtime.
* `Uuvr.Patcher/` – Standalone patcher that copies managed assemblies, native plugins, and removes conflicting XR binaries before the game boots.
* `Uuvr.XR.Management/` – Drop-in replacement for Unity's XR Management package so XR loaders can start without the official dependency.
* `Uuvr.XR.OpenXR/` – Re-implementation of the OpenXR loader pipeline tailored for stripped IL2CPP players.
* `Uuvr.XR.OpenVR/` – OpenVR bootstrap including optional input-system registration for Valve runtimes.
* `Uuvr.SteamVR/` – Embedded SteamVR helper scripts for games compiled with those references.
* `Uuvr.XInput/` – Minimal Windows DLL stub that prevents missing XInput failures while VR hardware handles input.

## Building the mod

1. Install [BepInEx](https://docs.bepinex.dev/articles/user_guide/installation/index.html) into the target IL2CPP Unity game.
2. Clone this repository and open `UnityUniversalVr.sln` with Visual Studio 2022 or run `dotnet build` from the repo root.
3. Build in `Release` for your platform. The output lives in each project's `bin/Release` folder.
4. Copy the produced assemblies and native plugins into the appropriate directories of the target game. The `Uuvr.Patcher` project automates this layout if you prefer scripted deployment.

## Configuring in-game behaviour

Configuration is handled through BepInEx config files stored under `BepInEx/config/`. Key settings include:

* **Tracking and alignment** – control world-to-headset alignment, seated vs. standing offsets, and depth overrides.
* **UI presentation** – choose between overlay, curved, or world-space canvases for the flat HUD.
* **Performance** – tweak physics update cadence and enable headset-refresh synchronisation.
* **Input** – toggle experimental controller mappings or fall back to gamepad emulation.

Consult the generated config file after the first run for the full list of options and inline documentation.

## Troubleshooting tips

* Verify BepInEx is actually loading IL2CPP assemblies (`BepInEx/LogOutput.log` should mention `UniversalUnityVR`).
* If the game crashes on startup, remove pre-existing `Unity.XR.*` plugins—Unity can fail when multiple XR providers compete.
* Use `doorstop_config.ini` to enable the debugger and attach a managed debugger for deeper inspection.
* When in doubt, start with the `Uuvr.Patcher` output to ensure all required files are present.

## Contributing

Bug reports, minimal repro projects, and documentation fixes are all welcome. Pull requests are reviewed on a best-effort basis—if maintaining parity with upstream becomes too time-consuming, this fork may freeze in whatever state it currently occupies.

## License

```text
Rai Pal
Copyright (C) 2024  Raicuparta

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
```
