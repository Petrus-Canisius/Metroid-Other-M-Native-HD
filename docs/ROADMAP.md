# Roadmap

[Home](../README.md) · [Status](BUILD_STATUS.md) · [Development log](DEVELOPMENT_LOG.md)

I group my development work by technical dependency in this roadmap. I have not assigned completion percentages or promised release dates.

| Phase | Scope | Position |
| --- | --- | --- |
| **1. Recompilation foundation** | DOL/REL analysis, relocation, function discovery and PowerPC-to-C++ translation | Early milestones verified; coverage continues |
| **2. Wii runtime** | Memory, interrupts, IOS, NAND, host fibers and event scheduling | Active development. SYSCONF read reported; scheduling still under validation |
| **3. Engine startup** | Complete IOS disc access, reliable thread changes and entry into the game engine | Not yet demonstrated |
| **4. Graphics, audio and gameplay** | GX renderer, Aurora/Dawn versus direct Vulkan evaluation, audio, controls, scenes and saves | Planned |
| **5. HD quality and performance** | HD graphics, resolution and refresh-rate testing, PC optimization, shader preparation and automated coverage | Planned; depends on a working game |
| **6. Installation and release** | Local import from a personal game copy, Windows/Linux builds, launcher, updater and GitHub Releases | Planned |

## Planned capabilities

The intended targets include Windows and Linux, keyboard and mouse, Xbox and PlayStation controllers, optional Wii Remote support using Mayflash DolphinBar, and output at 1080p, 1440p or 4K.

Refresh rates up to 144 FPS are exploratory. Game timing and behavior must be preserved. Shader and pipeline preparation depends on the selected rendering backend. CPU, GPU, RAM and VRAM budgets will require measurements on representative PCs, including modest hardware.

I intend to publish the launcher in this repository's [Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) section when ready. I develop the project in my spare time, and I have not set a release date.
