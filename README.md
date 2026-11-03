# Metroid: Other M Native HD

I am working on an **independent native PC adaptation of *Metroid: Other M***, originally released for Nintendo Wii. The goal is to preserve the original game while making it practical to run on modern computers, with conventional PC controls and, eventually, optional visual improvements.

**Current status: an interactive private Linux prototype benchmarks near normal speed (~0.978× in one training-room test); full-game stability and compatibility are unverified. There is no public playable release.**

**English** | [Français](README.fr.md)

[Latest progress](docs/BUILD_STATUS.md) · [Roadmap](docs/ROADMAP.md) · [Development history](docs/DEVELOPMENT_LOG.md) · [How the project works](docs/ARCHITECTURE.md) · [FAQ](docs/FAQ.md)

## The project in brief

This is **not a repackaged Dolphin emulator**. The development process translates the Wii game's PowerPC program into code that can run on a PC, while rebuilding the Wii services, input, audio and graphics behavior it depends on. Dolphin remains an important **reference for checking correctness**, and some open-source Dolphin components have been adapted in the private runtime.

My intended PC version will offer:

- **Linux and Windows** native applications and a dedicated launcher.
- **Keyboard and mouse or Xbox/PlayStation-style gamepads**, including adaptations for Wii Remote gestures, pointing and the switch between horizontal and vertical use. A physical Wii Remote should not be required to finish the game.
- **Optional original-style Wii Remote controls**, including the goal of supporting a real remote with a Mayflash DolphinBar.
- **Faithful graphics first**, then optional 1080p/1440p/4K output, carefully adapted 16:9 and 21:9, adjustable anti-aliasing, HDR10 and VRR where feasible.
- **Smooth performance and accurate gameplay**, without making the game's speed depend on the monitor's refresh rate.

These are **future product goals**, not a list of completed features.

## Latest progress — 3 November 2026

**The private Ubuntu prototype opens an SDL3/Vulkan window, produces audio and accepts physical Xbox Series X controller input.** Movement and beam firing have been demonstrated in a recorded and replayed training-room session. Full control coverage and full-game compatibility remain unverified.

The **latest private development report** records a measured **~0.978× simulation speed** in the fixed training-room benchmark (**approximately 16.8–17.2 ms/frame**) following a reproducible improvement to Vulkan submission scheduling. This is **not yet a demonstrated stable 1.0× across the game**, and there is **no public playable release**.

| Area | Latest evidence from private reports |
| --- | --- |
| Native translation | **32,282 PowerPC functions** translated; **753/753** translator tests passing |
| Current game experience | Linux SDL3/Vulkan window, audio and physical Xbox input; controlled replay demonstrates movement and beam firing |
| Render performance | **Eight Vulkan submissions in flight**, up from four: approximately **−1.28 ms/frame** over six alternating A/B pairs; three further pairs confirm about **−1.20 ms/frame** against the earlier reference |
| Build optimizations | **Clang/LLD ThinLTO** on the renderer; previously about **−0.58 ms/frame** over six matched pairs; experimental PGO was rejected because it slowed the tested scene |
| Memory tradeoff | Eight submission slots use about **1,041 MB RAM**, versus **863 MB** with four slots on the development system |
| Fidelity | **28,167 internal regression frames** unchanged; **753/753** translator and **3/3** input tests; **156.73 seconds** of reference audio unchanged |
| GPU copies | **4,671 checked copies**, none differing in the most recent relevant validation |
| Remaining reference differences | **29** differing fade frames in the Vulkan/software sample, and **135/387-byte** memory differences against Dolphin at two checkpoints |
| PAL save | Private European **R3OP** export recognized with three encrypted slots; purported 100% completion, Dolphin import and native loading **not verified** |
| Windows | Cross-platform groundwork exists; **no Windows binary compiled or tested** |

### How to interpret the new speed

The 60 Hz benchmark runs the training-room segment at **438–470 seconds of simulated time**. Normal-speed simulation corresponds to approximately **16.67 ms/frame** in this test. The latest **0.978×** is a benchmark result, **not a completion percentage**, a guarantee for other scenes, or proof of smooth frame pacing. The reported **P99 is around 20.2–20.5 ms**, so occasional slower frames remain.

The improvement is attributable to a **measured code change**: increasing in-flight Vulkan submission slots from four to eight. Six slots showed no reproducible gain, while twelve did not justify their higher memory usage. A separate fix removed **8,038 false EFB copy/texture waits** by correcting overestimated memory ranges; it improved dependency accuracy but **did not demonstrate a mean speed gain**.

Earlier ThinLTO testing also delivered a controlled gain. PGO did not. The translated PowerPC functions' machine code was unchanged in the tested `-ffp-contract=off` comparison; the runtime now explicitly disables unwanted floating-point contraction where appropriate. The CPU instruction-set baseline is **x86-64-v3**; this does **not** establish complete minimum system requirements.

### Next bottlenecks and work

The trace now highlights roughly **4.65 ms/frame of GPU execution**, **2.43 ms/frame of copy-stage readback/encoding** and **2.26 ms/frame of pre-GPU delay** along the profiled synchronization path. A read-only observation found the development RTX 4090 in its low-power **P8** state for much of the benchmark. **Whether that causes the slowdown is not established**; a temporary, reversible diagnostic has been authorized, but no result is reported here. The shipped runtime must **not** require an NVIDIA-specific driver setting.

Next: validate sustained **1.0×** and frame pacing, profile the remaining GPU/copy path under normal driver settings, broaden gameplay-scene regression coverage, verify all controller actions and saves, resolve the remaining graphical and Dolphin-reference differences, and eventually test Windows. Shader-pipeline compilation still causes a measured cold-start delay in a separate test. Reusable engine changes should benefit many zones; testing other scenes does not mean hand-optimizing every level.

Details and qualifications are in [current status](docs/BUILD_STATUS.md), [development history](docs/DEVELOPMENT_LOG.md) and the [roadmap](docs/ROADMAP.md).

## Development evidence and publication

These results come from my **private Claude Code development reports through 3 November 2026**. I have not independently rerun the underlying binaries for this public documentation update. I distinguish reported tests from features confirmed in a publicly available game.

I intend to publish a launcher through [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when it is ready. There is **no release date, public executable or established minimum PC specification**. The eventual setup is intended to use a player's **own local copy of the game**.

**This repository contains documentation only**, not Nintendo game files, translated game code, assets, private recordings, encryption keys or an executable. The project is unofficial and not affiliated with, endorsed by or approved by Nintendo. Open-source components adapted from Dolphin and other projects will need proper per-file licensing and corresponding-source compliance before any binary distribution. See the [FAQ](docs/FAQ.md#licensing).
