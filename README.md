# Metroid: Other M Native HD

I am working on an **independent native PC adaptation of *Metroid: Other M***, originally released for Nintendo Wii. The goal is to preserve the original game while making it practical to run on modern computers, with conventional PC controls and, eventually, optional visual improvements.

**Current status: the private Linux prototype has exceeded normal simulation speed in one measured training-room benchmark (1.013× average); full-game stability and compatibility remain unverified. There is no public playable release.**

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

## Latest progress — 3 November 2026: first training-room benchmark above 1.0×

**For the first time, the private native Linux prototype has exceeded normal simulation speed on the fixed training-room benchmark: a reported average of 1.013×.** The result follows a targeted Vulkan fragment-shader improvement and was measured in **six alternating A/B comparisons**, with a reported **0.52 ms/frame average gain** and improved P99. The **full internal regression suite passed** according to the latest Claude Code report.

**This is an important engineering milestone, not proof of full-speed gameplay throughout the game. There is no public playable release.**

The private SDL3/Vulkan prototype produces audio, accepts a physical Xbox Series X controller, and has previously demonstrated Samus moving and firing through recorded/replayed controller input. Other controls and full-game behavior remain to be validated.

| Area | Latest private development evidence |
| --- | --- |
| **Performance milestone** | **1.013× average simulation speed** in the measured training-room scenario, first reported result above 1.0× |
| **New Vulkan optimization** | Specialized alpha-test shader: omit fragment `discard` **only when the emulated alpha-test configuration guarantees every fragment passes**; **−0.52 ms/frame** across **six paired A/B runs**, with matching reference frames |
| Previous improvements | Eight in-flight Vulkan submission slots: **−1.28 ms/frame** in a six-pair comparison; renderer **ThinLTO: −0.58 ms/frame** in six pairs |
| Translation and tests | **32,282 translated PowerPC functions**, **753/753** translator tests in prior documented full suites; latest alpha-test regression suite reported passing |
| Graphics and sound | **28,167 internal reference frames** in the established regression suite; selected **156.73 s** audio reference; known discrepancies with Dolphin remain |
| Controls | Xbox controller recognition; movement and beam firing demonstrated in a controlled recorded-input replay, not complete gameplay verification |
| GPU driver diagnosis | NVIDIA PowerMizer adjustment **was not performed**: Wayland/Xwayland did not expose NV-CONTROL. GPU driver settings were left unchanged |
| In-progress experiment | **TEV shader specialization** using Vulkan specialization constants is undergoing **six paired A/B measurements**; **no outcome reported yet** |
| Portability | **x86-64-v3** CPU ISA requirement; Vulkan cross-vendor goal; **no tested Windows build or public release** |

### How the 1.013× milestone was reached

Profiling found that fragment/TEV work was a major part of GPU render-pass time. A diagnostic experiment removing alpha-test `discard` everywhere reduced measured render-pass cost, but was not suitable as a permanent implementation because it could change rendering. The retained implementation uses the emulator's **alpha-test classification** to select a shader variant **without `discard` only for the always-pass case**, retaining the correct path for conditional alpha tests.

The fixed-scene comparison used pipeline-cache prewarming and **six matched A/B pairs**. The developer reported **−0.52 ms/frame on average**, an average simulation speed of **1.013×**, improved P99 and unchanged regression frames; full non-regression tests subsequently passed. No exact new P99 range was included in the shared progress excerpt.

A trivial fragment shader was separately confirmed to **change reference frames**, showing that the graphical regression check can detect at least that kind of incorrect rendering.

### What this does — and does not — prove

At the tested 60 Hz simulation pace, normal speed corresponds to approximately **16.67 ms per frame**. Exceeding **1.0× on average in a controlled training-room test** does not establish smooth pacing on every frame, sustained real-controller gameplay, performance in demanding levels, or whole-game correctness. Known gaps include **29 differing fade frames** in a Vulkan/software comparison and **135/387-byte** differences against Dolphin memory references.

The optimization is part of the **shared Vulkan renderer**, not a per-level patch. Other areas still need representative automated testing, not an assumption that each must be hand-optimized.

### Immediate next work

Finish and evaluate the **TEV specialization A/B experiment** without claiming a result before measurement. Then establish a repeatable margin above 1.0×, improve P95/P99 and shader cold-start pacing, validate more gameplay scenes and controls, resolve remaining rendering/memory differences, and test saves and Windows. The attempted temporary PowerMizer test did **not** change the NVIDIA driver under Wayland.

Further technical detail: [build status](docs/BUILD_STATUS.md), [development log](docs/DEVELOPMENT_LOG.md), [roadmap](docs/ROADMAP.md).

## Development evidence and publication

These results come from my **private Claude Code development reports through 3 November 2026**. I have not independently rerun the underlying binaries for this public documentation update. I distinguish reported tests from features confirmed in a publicly available game.

I intend to publish a launcher through [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when it is ready. There is **no release date, public executable or established minimum PC specification**. The eventual setup is intended to use a player's **own local copy of the game**.

**This repository contains documentation only**, not Nintendo game files, translated game code, assets, private recordings, encryption keys or an executable. The project is unofficial and not affiliated with, endorsed by or approved by Nintendo. Open-source components adapted from Dolphin and other projects will need proper per-file licensing and corresponding-source compliance before any binary distribution. See the [FAQ](docs/FAQ.md#licensing).
