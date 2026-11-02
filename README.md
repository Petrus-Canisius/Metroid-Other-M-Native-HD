# Metroid: Other M Native HD

I am developing an independent project to run *Metroid: Other M* natively on PC through static recompilation of its Wii PowerPC code.

![In Active Development](https://img.shields.io/badge/status-In%20Active%20Development-blue)
![Not Playable Yet](https://img.shields.io/badge/gameplay-Not%20Playable%20Yet-orange)
![No Public Release](https://img.shields.io/badge/release-No%20Public%20Release-lightgrey)

**English** | [Français](README.fr.md)

[Architecture](docs/ARCHITECTURE.md) · [Build status](docs/BUILD_STATUS.md) · [Roadmap](docs/ROADMAP.md) · [Development log](docs/DEVELOPMENT_LOG.md) · [FAQ](docs/FAQ.md)

## Development status

**First native game images are now rendered.** My adapted software GX renderer produced the Wii Remote wrist-strap warning screen, with its **614,400-byte XFB image matching Dolphin byte-for-byte** under equivalent copy-filter settings. The native harness also reaches the **OPTIONS menu**, reacts to scripted Wii Remote input and runs the opening cinematic. **No gameplay sequence has been validated, and there is no playable release.**

| Area | Latest reported evidence |
| --- | --- |
| PowerPC translation | **32,282 functions** translated, including seven newly discovered indirect-entry functions; **732/732** translator tests passed |
| Wii runtime | Startup, IOS, NAND, direct RVZ reading, scheduling and Wii Remote emulation exercised |
| Graphics | Software GX renderer produces real image data; the wrist-strap XFB matched Dolphin on **614,400/614,400 bytes** |
| Menus and input | OPTIONS navigation, settings changes, HOME/IR pointer and START-to-intro transition observed **in native runs** |
| Audio | **17.09 seconds** of active output matched Dolphin sample-for-sample after synchronization |
| Next work | Deterministic Wii timing and repeatable menu validation, then GPU-accelerated rendering (direct Vulkan recommended, not yet implemented) |

This status reflects my **2 November 2026** Claude Code development report. The first image and some earlier introduction frames have reference matches, but Dolphin has **not yet reproduced every native menu interaction**. The reference software renderer is slow (approximately 28 FPS on the reported full-screen test) and is **not the planned final graphics backend**. I have not independently rerun these private tests for the documentation update. See [build status](docs/BUILD_STATUS.md).


## How it works

The toolchain translates Wii PowerPC instructions into C++ and compiles them for a host CPU. A dedicated runtime implements Wii memory handling, hardware interfaces and operating-system services.

This is **not a Dolphin launcher**. Dolphin serves as a reference for behavior and memory comparisons. A Dolphin-derived software renderer now produces the native reference images. **Direct Vulkan** is currently recommended for the future GPU-accelerated renderer, but that backend has not yet been implemented.

## Planned capabilities

These are development goals, not currently available features.

- **Platforms and input:** Windows and Linux, keyboard and mouse, Xbox and PlayStation controllers, and future Wii Remote support via Mayflash DolphinBar.
- **Visuals:** 16:9 widescreen and 21:9 ultrawide support, with a correctly adapted field of view rather than a stretched picture; 1080p, 1440p and 4K output, optional HD improvements, **optional anti-aliasing** (FXAA, SMAA and MSAA to evaluate according to the eventual GPU backend), and higher refresh rates up to 144 FPS where technically feasible. HDR10, VRR, G-SYNC and AMD FreeSync are also future, optional goals, with SDR fallback. None of these display features has been implemented or validated.
- **Performance:** efficient CPU, GPU, RAM and VRAM usage, support for modest PCs, shader preparation and automated regression testing.
- **Installation:** local import of a personal game copy, a dedicated launcher and a future update mechanism.

See the [roadmap](docs/ROADMAP.md) for dependencies and limitations.

## Launcher and downloads

I **will publish the planned launcher through this repository's [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases)** when it is ready. Players will need their own copy of *Metroid: Other M*. No download or release date is available yet.

I develop this project in my spare time. Progress may take time, but I intend to publish the launcher here.

## Follow development

I record significant milestones in my [development log](docs/DEVELOPMENT_LOG.md) and answer technical and distribution questions in the [FAQ](docs/FAQ.md). I review public updates manually using my [documentation maintenance procedure](docs/MAINTENANCE.md).

## Independence and rights

I run this unofficial fan project independently of Nintendo, without its endorsement or approval. Metroid and related names belong to their respective owners. This repository contains no Nintendo game code, game assets, disc images, private NAND data or encryption keys.

Third-party license considerations are covered in the [FAQ](docs/FAQ.md#licensing).
