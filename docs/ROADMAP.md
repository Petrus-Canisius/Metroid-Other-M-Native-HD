# Roadmap

[Home](../README.md) · [Status](BUILD_STATUS.md)

This roadmap describes intended work. Ordering follows technical dependencies and may change. There are no completion percentages or release dates.

| Stage | Work | Current position |
| --- | --- | --- |
| Executable analysis | Parse DOL/REL, establish relocation and discover functions | Initial milestones verified |
| Translation | Generate native code and validate translator behavior | Initial milestone verified, coverage work continues |
| Startup services | Interrupts, IOS, NAND, fibers and disc access | In progress |
| Engine execution | Reach the game engine with validated state | Pending |
| Graphics and audio | GX processing, backend evaluation, audio and scene validation | Planned |
| Playability | Controls, levels, cinematics, saves and campaign testing | Planned |
| Platform support | Linux and Windows implementation and testing | Linux development underway, Windows planned |
| HD presentation | 1080p, 1440p, 4K and graphics improvements | Planned |
| High refresh | Evaluate rates up to 144 FPS, including interpolation | Feasibility dependent |
| Input support | Keyboard, mouse, Xbox and PlayStation controllers | Planned |
| Wii Remote | Mayflash DolphinBar integration | Future goal |
| Performance | CPU, GPU, RAM and VRAM profiling, modest PC testing | Planned validation program |
| Shader preparation | Precompile shaders and pipelines, evaluate caches | Depends on rendering backend |
| Graphics testing | Automated comparisons with reference frames | Depends on verified native frames |
| Installation | Import a personal game copy locally | Planned |
| Delivery | Launcher and update system | Planned |

Performance targets require measurements on representative hardware. No minimum specification is promised. Higher refresh rates must preserve game timing and behavior before they can be offered as supported modes.
