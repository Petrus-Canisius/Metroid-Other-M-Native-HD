# Metroid: Other M Native HD

An independent, experimental project to run Metroid: Other M natively on PC through static recompilation of its Wii PowerPC code.

![In Active Development](https://img.shields.io/badge/status-In%20Active%20Development-blue)
![Not Playable Yet](https://img.shields.io/badge/gameplay-Not%20Playable%20Yet-orange)
![No Public Release](https://img.shields.io/badge/release-No%20Public%20Release-lightgrey)

**English** | [Français](README.fr.md)

[Architecture](docs/ARCHITECTURE.md) · [Build status](docs/BUILD_STATUS.md) · [Roadmap](docs/ROADMAP.md) · [Development log](docs/DEVELOPMENT_LOG.md) · [FAQ](docs/FAQ.md)

## Current status

**In Active Development · Not Playable Yet · No Public Release**

Early startup code runs natively in a development harness. There is no playable build or public download. This repository documents the project. It does not contain the development source or game data.

Evidence reviewed on **1 November 2026**:

| Area | Verified position |
| --- | --- |
| PowerPC translation | Latest inspected log reports 32,275 translated functions |
| Translator tests | Latest inspected run reports 732 passed, none failed or skipped |
| Native compilation | Earlier generation: 30,045 functions compiled successfully |
| Startup validation | MEM1 compared with Dolphin at the first VI access, with four time-base bytes differing |
| Interrupts and IOS | Early interrupt delivery and STM communication exercised |
| Threads and NAND | Partial implementations present, broader validation pending |
| Disc access | IOS disc commands remain unimplemented in the inspected code |
| Rendering and gameplay | No verified native game rendering or playable build |

These are separate milestones. Translation counts do not measure game completion. See [build status](docs/BUILD_STATUS.md) for evidence limits and unresolved differences.

## How native recompilation works

The development pipeline translates PowerPC instructions into C++, then compiles that code for the host CPU. A dedicated runtime supplies Wii services, memory handling and hardware interfaces.

The project is not a Dolphin launcher. Dolphin serves as a development reference for comparing behavior and memory. Native execution still needs Wii hardware compatibility work, including graphics, audio, storage and input.

The current technical foundation includes C++20 analysis tools, DOL and REL parsing, static relocation, an adapted WiiCompiled translator, and an Other M runtime. The graphics backend remains undecided.

## Planned capabilities

The following are **goals**, not available features:

- Native execution on Windows and Linux.
- 1080p, 1440p and 4K output, with HD graphics improvements.
- Higher refresh rates up to 144 FPS where technically feasible.
- Keyboard and mouse, Xbox controllers and PlayStation controllers.
- Future Wii Remote support through the Mayflash DolphinBar.
- CPU, GPU, RAM and VRAM optimization, including testing on modest PCs.
- Shader precompilation and automated graphics comparisons.
- Installation using a personal copy of the game, a launcher and updates.

The [roadmap](docs/ROADMAP.md) explains dependencies. No release date or minimum hardware requirements have been established.

## Follow development

The [development log](docs/DEVELOPMENT_LOG.md) records verified milestones. The [FAQ](docs/FAQ.md) covers downloads, game copies and contributions. Public updates are reviewed manually using the [maintenance procedure](docs/MAINTENANCE.md).

## Independence and rights

This project is not affiliated with, endorsed by, or approved by Nintendo. Metroid and associated names belong to their respective owners. No Nintendo game code, assets, disc images, keys or private NAND data are distributed here.

Third party licensing and the conditions for any future source distribution are described in the [FAQ](docs/FAQ.md#licensing).
