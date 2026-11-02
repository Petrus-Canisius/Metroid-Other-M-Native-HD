# Roadmap

[Home](../README.md) · [Status](BUILD_STATUS.md) · [Development log](DEVELOPMENT_LOG.md)

I organize development around technical dependencies and confirmed tests. I have not assigned an overall completion percentage or a release date.

| Phase | Scope | Latest position |
| --- | --- | --- |
| **1. Recompilation foundation** | DOL/REL analysis, relocation, function discovery, PowerPC-to-C++ translation | **32,282** functions translated after seven new indirect entries; **732/732** translator tests passed in the latest report |
| **2. Wii runtime** | Memory, SPR, interrupts, IOS, NAND, fibers, RVZ, Bluetooth and Wii Remote | Major startup paths exercised; timing, memory and peripheral edge cases still open |
| **3. Engine startup** | GXInit, DSP AX, disc access, menu progression | GXInit and AX DSP passed; native execution reaches OPTIONS, loading and opening cinematic |
| **4. Graphics, audio and gameplay** | GX software reference, GPU renderer, audio, controllers, menus, scenes and saves | **First real image matched Dolphin byte-for-byte**; audio partly matched; OPTIONS responds natively. **No validated gameplay yet** |
| **5. HD quality and performance** | Accelerated rendering, widescreen/ultrawide, HDR10, VRR, HD/4K, frame pacing, optimization | Direct Vulkan recommended but not yet implemented; HDR/VRR and other enhancements remain planned and optional |
| **6. Installation and release** | Local personal game import, Windows/Linux builds, launcher, updates and GitHub Releases | Planned only; no publicly downloadable game |

## Next engineering milestones

1. **Deterministic Wii time:** decouple simulated time from host wall clock, align asynchronous events and keep rendering cost out of emulated time.
2. **Reproducible menu comparisons:** replay scripted input / DTM in both native and Dolphin, then compare corresponding XFB frames and menu state. Current native OPTIONS/START/HOME results have not all been matched in Dolphin.
3. **Direct Vulkan GPU backend:** preserve the software renderer as an oracle, implement native accelerated image output incrementally and compare against the reference.
4. **First controllable gameplay:** move beyond menus and cinematic to a tested sequence involving Samus and actual game controls.
5. **Modern visual options:** consider 16:9 and 21:9 framing, HD/4K, optional HDR10 and VRR/G-SYNC/FreeSync when the renderer is stable and hardware validation is possible.
6. **Distribution:** test Windows/Linux portability and prepare the launcher only once a robust game path is demonstrated.

## Technical limits

The current software reference renderer is relatively slow (approximately 28 FPS on the reported test). The first XFB screen matched Dolphin exactly, but a handful of frames do not establish whole-game graphics fidelity. Audio was identical for 17.09 seconds after synchronization, then diverged in a way not yet explained. IPC #4 and #35 memory differences remain open.

The targeted platforms are Windows and Linux. Planned input includes keyboard, mouse, Xbox/PlayStation controllers and optional physical Wii Remote support via Mayflash DolphinBar. Existing **scripted Wii Remote emulation in the development harness** is not equivalent to validated physical controller support.

High refresh rates up to 144 FPS remain exploratory and must not change original game timing. Standard SDR 60 Hz 16:9 must remain available.

I intend to publish the launcher through this repository's [Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when ready. I develop the project in my spare time and have not set a release date.
