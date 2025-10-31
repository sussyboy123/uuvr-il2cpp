# Universal Unity VR for Modern IL2CPP Unity Games

> **Maintenance status:** This fork may be abandoned soon. I'm experimenting with getting UUVR running on modern IL2CPP Unity titles, as long as it doesn't become too much work.

Universal Unity VR (UUVR) is a BepInEx-based mod that injects VR support into Unity games that ship as modern IL2CPP builds. This fork tracks recent Unity runtime changes and adapts the original project so that VR camera control, UI mirroring, and runtime configuration continue to function on newer IL2CPP releases.

## Project overview
* **Plugin bootstrap** – `UuvrCore` loads as a persistent host object that registers IL2CPP-safe behaviours, installs Harmony patches, and coordinates VR mode toggling within the target game session.
* **Camera management** – `VrCameraManager` discovers every in-game camera, attaches VR-aware wrappers, and keeps stereoscopic rendering synchronised with the user’s headset.
* **Configurable runtime** – `ModConfiguration` exposes BepInEx configuration entries to tune camera tracking, depth overrides, physics timing, UI handling, and other per-game quirks without modifying code.
* **In-game UI adaptation** – `VrUiManager` renders the flat-screen UI to textures, projects them into VR space, and swaps between overlay, in-world, or mirrored presentation based on the active configuration.
* **Session resilience** – The core systems persist across scene loads, automatically re-create themselves if destroyed, and align Unity physics timing with the HMD refresh rate when requested.

## Supporting projects
The repository bundles several auxiliary components that replicate Unity’s XR packages and platform shims often missing from IL2CPP-only builds:

* **`Uuvr.Patcher`** – Pre-game patcher that copies managed assemblies and native plugins, strips conflicting VR binaries, and can rewrite Unity asset metadata for legacy XR enablement.
* **`Uuvr.XR.Management`** – Re-implementation of Unity’s XR Management runtime objects so loaders can boot the chosen backend at runtime.
* **`Uuvr.XR.OpenXR`** – Custom OpenXR loader pipeline that creates XR subsystems, loads symbols, and prioritises features in the absence of Unity’s official package.
* **`Uuvr.XR.OpenVR`** – OpenVR loader with input layout registration and runtime initialisation for Valve’s platform.
* **`Uuvr.SteamVR`** – Bundled SteamVR helper scripts for projects expecting those APIs.
* **`Uuvr.XInput`** – Lightweight Windows DLL stub that provides deterministic controller state to prevent missing-DLL crashes.

## Getting started
1. Install BepInEx into your target Unity IL2CPP game (see the [BepInEx documentation](https://docs.bepinex.dev/articles/user_guide/installation/index.html)).
2. Build the solution (`UnityUniversalVr.sln`) using a recent version of Visual Studio or `dotnet build`.
3. Copy the produced assemblies and native plugins into the appropriate directories of the target game, mirroring how the `Uuvr.Patcher` project deploys them.
4. Launch the game and use the configured hotkeys to toggle VR mode and adjust runtime behaviour.

Because this fork is experimental, expect breaking changes and limited support. Contributions and issue reports are welcome, but please understand that long-term maintenance is not guaranteed.

## License

```
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
