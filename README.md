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

## Latest progress — reports through 2 November 2026

**The private Ubuntu prototype already opens a real SDL3/Vulkan window, outputs audio and accepts a physical Xbox Series X controller.** A recorded controller session was replayed against a no-input run and showed Samus **moving and firing** in the training room. This is an interactive prototype, **not a full-speed or publicly released PC game**.

The recent engineering effort has focused on **accurate, repeatable performance optimization**, not adding headline features:

| Area | Latest reported evidence |
| --- | --- |
| Recompilation | **32,282 PowerPC functions** translated; **753/753 translator tests** passed |
| Linux experience | SDL3/Vulkan window, audio output and real-controller input; movement and beam fire demonstrated in a controlled replay |
| Rendering | GPU copy-to-texture, optimized TEV shader, two transformation workers, parallel triangle assembly/clipping and bounded Vulkan image reuse |
| Performance | Most recent unchanged-code session: **0.879–0.917× speed (0.897× average across five runs)** in the training-room benchmark; **18.19–18.97 ms/frame** |
| Controlled code improvement | Earlier paired A/B testing demonstrated a **~0.54 ms/frame** gain from parallel triangle assembly; stable results in that session were **0.835–0.848×** |
| Accuracy | **28,167 internal regression frames** unchanged; graphics and game timing still have known differences from Dolphin |
| Audio | **156.73 seconds** matched a tested reference capture; not whole-game validation |
| Memory | Approximately **857 MB RAM** and **78 MB VRAM** in the recent benchmark; not minimum requirements |
| PAL save | European **R3OP** Wii SD export identified, with **three save slots**; contents are encrypted, “100%” progress and native loading unverified |
| Windows | Cross-platform scaffolding exists; **Windows has not been compiled or tested** |

### Why the latest speed must be interpreted carefully

The last session **did not retain a new performance optimization**. Moving geometric slope calculation to the transformation workers was tested with alternating A/B runs, produced correct frames, but was **slightly slower (~0.21 ms/frame on average)** and was reverted.

The same existing code benchmarked substantially faster in the most recent session than the previous one. Thus, **the 0.897× average is an observed result under those machine conditions, not a new code-attributable improvement**. The next development task is to characterize between-session variability and optimize the vertex loader and native runtime overhead.

For the tested 60 Hz training sequence, normal speed would require around **16.67 ms/frame**, compared with approximately **18.58 ms/frame** averaged across the most recent runs. **0.90×, 0.95× and stable 1.0× remain unverified milestones.** A speed multiplier is not a project-completion percentage.

### Better understanding of startup stutters

Isolated cache tests measured approximately **436 ms to create pipelines with an empty cache**, versus **1 ms with a warm cache**. The first rendered frame took **514 ms cold**, versus **82 ms warm**. This explains a previously observed ~520 ms startup spike in the tested case; it does **not** prove every gameplay stutter is gone. Pipeline preparation and cache strategy are future work.

### What still needs work

- Sustained **1.0×** simulation speed, predictable frame pacing and tests in multiple scenes.
- Missing GX/Vulkan effects, including certain fog, indirect-texture, mipmap and blending behavior; **29 fade frames remain different** in the software comparison.
- Unresolved Dolphin memory differences of **135 and 387 bytes** at two observed checkpoints.
- Gameplay validation for jump, Morph Ball, dodge, first-person aiming, missiles, concentration and other actions.
- The actual contents and 100%-completion claim of the PAL save; **no Dolphin import or native save loading has been demonstrated**.
- Physical Wii Remote/DolphinBar, a tested Windows build and optional HD/HDR/VRR/anti-aliasing modes.

### Immediate priorities

1. Establish **repeatable performance measurements**: the same binary has varied by more than a millisecond per frame between sessions.
2. Profile and streamline the **vertex loader** and overhead around the recompiled PowerPC code.
3. Reduce remaining Vulkan and synchronization costs while preserving exact output.
4. Prepare GPU pipelines more predictably to avoid cold-cache startup stalls.
5. Validate full-speed real-controller gameplay, then broaden scene and save-game coverage.

For measurement methodology, optimization history, retained versus rejected experiments and remaining known failures, see the [development status](docs/BUILD_STATUS.md) and [development history](docs/DEVELOPMENT_LOG.md).

## Development evidence and publication

These results come from my **private Claude Code development reports dated 2 November 2026**. I have not independently rerun the underlying binaries for this public documentation update. I distinguish reported tests from features confirmed in a publicly available game.

I intend to publish a launcher through [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when it is ready. There is **no release date, public executable or established minimum PC specification**. The eventual setup is intended to use a player's **own local copy of the game**.

**This repository contains documentation only**, not Nintendo game files, translated game code, assets, private recordings, encryption keys or an executable. The project is unofficial and not affiliated with, endorsed by or approved by Nintendo. Open-source components adapted from Dolphin and other projects will need proper per-file licensing and corresponding-source compliance before any binary distribution. See the [FAQ](docs/FAQ.md#licensing).
