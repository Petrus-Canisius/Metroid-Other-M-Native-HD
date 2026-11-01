# Metroid: Other M Native HD

![Fan-made Metroid: Other M Native HD project banner](assets/metroid-other-m-native-hd-banner.webp)

*Unofficial fan artwork. Not an in-game screenshot.*

I am developing an independent project to run *Metroid: Other M* natively on PC through static recompilation of its Wii PowerPC code.

![In Active Development](https://img.shields.io/badge/status-In%20Active%20Development-blue)
![Not Playable Yet](https://img.shields.io/badge/gameplay-Not%20Playable%20Yet-orange)
![No Public Release](https://img.shields.io/badge/release-No%20Public%20Release-lightgrey)

**English** | [Français](README.fr.md)

[Architecture](docs/ARCHITECTURE.md) · [Build status](docs/BUILD_STATUS.md) · [Roadmap](docs/ROADMAP.md) · [Development log](docs/DEVELOPMENT_LOG.md) · [FAQ](docs/FAQ.md)

## Development status

Native startup has now **passed AX audio DSP initialization** and continues to the development harness's watchdog limit without a reported crash. I have not produced a visible gameplay frame, audible game audio or a playable build.

| Area | Latest documented evidence |
| --- | --- |
| PowerPC translation | 32,275 functions retranslated; translator suite 732/732 passed in the reported run |
| Native compilation | Earlier 30,045-function build completed; newer 32,275-function generation reported rebuilt |
| Wii runtime | Thread switching, special registers, interrupts, IOS, NAND, disc and Bluetooth startup exercised |
| Graphics command processor | GXInit passed; 291 GX commands and 512 KiB FIFO matched Dolphin at the 35th IPC call |
| Disc / RVZ | Integrated read-only RVZ reader; DOL, FST and sampled large-file reads matched reference data |
| AX DSP audio | AX microcode processing, audio DMA, interrupts and WAV capture implemented; 73,903 command frames and 135,750 thread switches reported in a longer run | 
| Latest execution | No reported crash up to the harness watchdog. The game may be waiting for an unconnected Wii Remote; **no image rendered and no audible game sound** |

My latest **1 November 2026** console report describes these milestones. I have not independently reproduced the runtime tests for this public documentation update. The native audio capture was silent over 239 seconds, and a 250-second Dolphin comparison was also silent without a connected Wii Remote. With no active voices, this is **not** a validation of audible mixing. Submitted GPU frames are not rendered gameplay images. The game is still not playable. See [build status](docs/BUILD_STATUS.md).

## How it works

The toolchain translates Wii PowerPC instructions into C++ and compiles them for a host CPU. A dedicated runtime implements Wii memory handling, hardware interfaces and operating-system services.

This is **not a Dolphin launcher**. Dolphin serves as a reference for behavior and memory comparisons. The graphics backend remains undecided between Aurora/Dawn, direct Vulkan and a possible Direct3D 12 path.

## Planned capabilities

These are development goals, not currently available features.

- **Platforms and input:** Windows and Linux, keyboard and mouse, Xbox and PlayStation controllers, and future Wii Remote support via Mayflash DolphinBar.
- **Visuals:** 16:9 widescreen and 21:9 ultrawide support, with a correctly adapted field of view rather than a stretched picture; 1080p, 1440p and 4K output, optional HD improvements and higher refresh rates up to 144 FPS where technically feasible. HDR10, VRR, G-SYNC and AMD FreeSync are also future, optional goals, with SDR fallback. None of these display features has been implemented or validated.
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
