# Roadmap

[Project overview](../README.md) · [Current progress](BUILD_STATUS.md) · [Development history](DEVELOPMENT_LOG.md)

My goal is a **faithful, fluent, fully controllable native PC version** of *Metroid: Other M*. The milestones below separate progress already observed in private tests from future goals. There is **no announced release date**.

## Development stages

| Stage | Purpose | Current status |
| --- | --- | --- |
| **1. Reconstruct the game program** | Translate PowerPC game code for execution on PC | **32,282 functions** translated and **753/753 tests** passing in reported suite |
| **2. Rebuild Wii services** | Disc, memory, timing, audio and virtual Wii Remote behavior | Startup and major paths working; open fidelity differences remain |
| **3. Produce real images and sound** | Menus, introduction and game graphics/audio | **Demonstrated in the private native harness** |
| **4. Provide direct PC interaction** | Linux window and physical gamepad or keyboard input | **SDL3 window and real Xbox input demonstrated**; movement and beam fire confirmed through replay |
| **5. Reach normal-speed gameplay** | Stable rendering, audio, responsive controls and complete training actions | **In progress:** training benchmark at **0.576× normal speed** |
| **6. Validate the full game** | All scenes, mechanics, saves, controller functions and long sessions | Not demonstrated; additional scenarios needed |
| **7. Improve presentation** | 1080p/1440p/4K, 16:9/21:9, optional AA, HDR10 and VRR | Planned, not player-ready |
| **8. Windows and distribution** | Compile and test Windows, install using a player's copy, launcher and Releases | Windows source groundwork only; no public build |

## Immediate priority: finish the first fluid Linux gameplay prototype

The development environment already has a window, sound and input. The main missing capability is **reliable real-time speed**.

The next engineering work therefore targets:

1. Measure the latest rendering bottlenecks using reproducible training-room tests.
2. Reduce CPU-side vertex processing and lighting overhead, with safe parallelism where appropriate.
3. Reduce remaining copy/encoding and memory synchronization costs.
4. Preserve byte-identical internal frames, reference audio and deterministic test behavior.
5. Reach **1.0× game speed** and acceptable frame pacing, then verify a real interactive session.
6. Complete and test Xbox actions: jump, Morph Ball, dodge, first-person view, missiles and concentration.

Measured speed increased from **0.369× to 0.576×** in the latest optimization phase. This is **benchmark speed, not project completion**. Normal speed still needs proof in multiple scenes, not just one training segment.

## Making Wii controls work on PC

The finished input system is intended to offer:

- **Keyboard and mouse**, including pointer/first-person aiming without Wii infrared equipment.
- **Xbox and PlayStation-style controllers**, with accessible mappings for Wii Remote orientation, gestures and other essential actions.
- **Optional original Wii Remote and Mayflash DolphinBar** support for those who want physical Wii controls.
- Remapping, sensitivity, dead zones and user-selectable control profiles.

Only **movement and beam firing with recorded physical Xbox input** have been demonstrated in a game scene so far. Other gameplay actions and DolphinBar support are **not validated**. All live input sources are intended to feed the same virtual Wii Remote behavior, without patching game logic to fake actions.

## Visuals and performance after core gameplay

My priorities are fidelity and correct timing before optional enhancements:

- Faithful original image, then **1080p/1440p/4K** output.
- **16:9** and potentially **21:9** with correct framing rather than stretched imagery.
- Switchable **anti-aliasing**: Off, candidate FXAA/SMAA, MSAA only if supported by the rendering design; TAA remains exploratory.
- Optional **HDR10, VRR, G-SYNC and FreeSync**, with standard SDR fallback.
- Higher refresh rates only if they do not accelerate the game's simulation or break animations.
- Efficient CPU/GPU/RAM/VRAM use across varied hardware, with measured minimum requirements rather than assumptions.

These display enhancements have **not** been implemented and validated as player features.

## Full-game verification and saves

After initial speed and controls stabilize, tests should include different rooms, cinematics, fights, saves, loading, transitions and equipment behavior.

A separately obtained European PAL **R3OP** save has been proposed for private compatibility testing. It can potentially help reach later content faster, but **its contents and compatibility with the native runtime have not yet been confirmed**. Never overwrite an original save or distribute private game-save data.

## Release prerequisites

Before publishing a build:

- Demonstrate stable, correct gameplay over substantial portions of the game and ultimately full progression.
- Validate audio, display, input, saves and performance on Linux and Windows.
- Test conventional controller mappings and any claimed DolphinBar support.
- Ensure players use their **own legal game copy**; publish no Nintendo assets or disc images.
- Audit all third-party source licences and corresponding-source distribution obligations.

The [current progress page](BUILD_STATUS.md) provides the latest evidence and known limitations.
