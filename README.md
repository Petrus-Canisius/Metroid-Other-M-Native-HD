# Metroid: Other M Native HD

I am working on an **independent native PC adaptation of *Metroid: Other M***, originally released for Nintendo Wii. The goal is to preserve the original game while making it practical to run on modern computers, with conventional PC controls and, eventually, optional visual improvements.

**Current status: an interactive Linux prototype exists, but it still runs below normal speed. There is no public playable release.**

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

## Latest progress — 2 November 2026

**A real SDL3 window now displays the game on Ubuntu using Vulkan.** The private prototype also outputs sound and reads a real Xbox Series X controller. A recorded controller session was replayed and compared against a no-input run: **Samus moved and fired in the training room** only when the controller inputs were present.

That is a significant gameplay milestone. However, **many other actions remain untested, the game is not yet at normal speed, and there is no downloadable build**.

| Area | Latest reported result |
| --- | --- |
| Native program | **32,282 translated PowerPC functions**; **753/753** translator tests passed |
| Linux display | **SDL3 desktop window with Vulkan**, resizing and fullscreen support; no user-facing Windows build |
| Real controller | Xbox controller input recorded and replayed; **movement and beam fire** demonstrated in the training room |
| Graphics | GPU-based Vulkan drawing with batching and GPU-side texture copies; **28,167 frames** unchanged against the established internal test baseline |
| Sound | Audio plays through the Linux output device; **156.7 seconds** matched a reference capture in the latest reported comparison |
| Repeatability | More than ten repeat runs produced identical images at the tested points; **7,686 GPU texture results** matched the CPU reference |
| Performance | Training-room benchmark improved from **0.369× to 0.576× normal game speed** (roughly 22 to 35 simulated frames/s on the tested 60 Hz sequence) |
| Windows | Cross-platform groundwork and Windows-specific code exist, but **the Windows version has not been compiled or tested** |

### What the performance numbers mean

The training-room sequence now averages **28.98 ms of processing per frame**, compared with **45.27 ms** before this optimization phase. To sustain the original speed on that 60 Hz test, the target is about **16.67 ms per frame**.

The gains came from moving certain image-to-texture conversions to the GPU, avoiding repeated vertex calculations, removing unused software work and dividing the final image conversion across CPU threads. The GPU is not the only limiting factor: CPU graphics preparation and synchronization still need work.

**A speed of 0.576× is not 57.6% completion of the project.** It is one benchmark result from one scene.

### What has not been validated

- A **full-speed, sustained gameplay session**.
- All Xbox controls: jump, Morph Ball, evasions, first-person aiming, missiles and concentration still need real gameplay verification.
- Physical Wii Remote **and DolphinBar** operation.
- Full-game progression, saved-game loading, and all areas or effects.
- The remaining Vulkan effects, including some fog, indirect texture effects, mipmaps and fade behavior. **29 fade frames still differ** from the software reference in the specific comparison.
- A compiled Windows build, 4K/ultrawide/HDR/VRR modes, or the planned anti-aliasing choices.

In particular, the **28,167 matching frames are an internal before/after regression check**, not proof that every frame matches Dolphin. At two memory checkpoints, **135 and 387 bytes still differ from Dolphin**.

## Next development priorities

1. **Reach 1.0× stable game speed** by improving CPU-side geometry and lighting, GPU copies and the remaining rendering bottlenecks.
2. **Complete the missing Wii graphics behavior** while continuing image-by-image comparisons.
3. **Test real gameplay with an Xbox controller**, including movement, shooting, jumping, Morph Ball, missiles, aiming and motion-based actions.
4. **Adapt all required Wii Remote gestures** for keyboard/mouse and conventional controllers while keeping optional real Wii Remote support in scope.
5. **Validate more scenes and game saves**, then pursue Windows builds and optional visual upgrades.

## Development evidence and publication

These results come from my **private Claude Code development reports dated 2 November 2026**. I have not independently rerun the underlying binaries for this public documentation update. I distinguish reported tests from features confirmed in a publicly available game.

I intend to publish a launcher through [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when it is ready. There is **no release date, public executable or established minimum PC specification**. The eventual setup is intended to use a player's **own local copy of the game**.

**This repository contains documentation only**, not Nintendo game files, translated game code, assets, private recordings, encryption keys or an executable. The project is unofficial and not affiliated with, endorsed by or approved by Nintendo. Open-source components adapted from Dolphin and other projects will need proper per-file licensing and corresponding-source compliance before any binary distribution. See the [FAQ](docs/FAQ.md#licensing).
