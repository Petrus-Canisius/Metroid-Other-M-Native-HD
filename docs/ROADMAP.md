# Roadmap

[Project overview](../README.md) · [Current progress](BUILD_STATUS.md) · [Development history](DEVELOPMENT_LOG.md)

My goal is a **faithful, fluent, fully controllable native PC version** of *Metroid: Other M*. The milestones below separate progress already observed in private tests from future goals. There is **no announced release date**.

## Development stages

| Stage | Purpose | Current status |
| --- | --- | --- |
| **1. Reconstruct the game program** | Translate PowerPC game code for PC execution | **32,282 functions** reported translated; **753/753** translator tests in the established suite |
| **2. Rebuild Wii services** | Disc, memory, timing, audio and virtual Wii Remote | Startup and selected paths run; known fidelity gaps remain |
| **3. Produce real images and sound** | Native graphics and audio | Demonstrated in the private Linux SDL3/Vulkan runtime |
| **4. Provide direct PC interaction** | Conventional input in a dedicated PC application | Xbox input, recorded movement and beam fire demonstrated; full mappings unverified |
| **5. Reach normal-speed gameplay** | Stable speed, frame pacing and responsive controls | **First fixed training-room benchmark above 1.0×: 1.013× average**, after conditional Vulkan alpha-test specialization; **steady all-scene 1.0× not demonstrated** |
| **6. Validate the full game** | Scenes, mechanics, progression, saves and longer sessions | Not demonstrated; representative scenarios and regression automation required |
| **7. Improve presentation** | HD/4K, 16:9/21:9, optional AA, HDR10 and VRR | Future goals, not completed player features |
| **8. Windows and distribution** | Portable build, test, launcher, release and compliance | No tested Windows build; no public playable release |

## Immediate priority: consolidate the first 1.0× milestone

The latest private engineering session reports **1.013× average simulation speed** on the fixed training-room benchmark after a conditionally specialized Vulkan alpha-test shader. The optimization removes fragment `discard` only when the emulated alpha test always passes. **Six alternating A/B comparisons** measured **−0.52 ms/frame on average** with improved P99 and unchanged tested reference frames. The full non-regression suite was reported passing.

Earlier, increasing Vulkan submission slots from four to eight produced a **−1.28 ms/frame** average gain in six paired measurements; renderer ThinLTO yielded **−0.58 ms/frame** in six paired comparisons. These are shared engine improvements, **not per-level patches**.

The temporary NVIDIA PowerMizer diagnostic **could not be performed** under Wayland/Xwayland because NV-CONTROL was unavailable; no driver policy was modified. A separate **TEV specialization** experiment is being measured in six A/B pairs, with **no reported outcome yet**.

Next engineering work:

1. Finish the **TEV specialization A/B run** and retain the change only if speed and fidelity both improve.
2. Confirm and extend the **1.013× training-room mean** into sustained normal-speed gameplay, including P95/P99 and sound/input stability, without manipulating simulation timing.
3. Expand automated tests across **representative scenes**: menus, exploration, battles, transitions, cinematics, and challenging effects.
4. Fix known GX/Vulkan, Dolphin-memory and cold shader-cache differences, preserving existing regression evidence.
5. Validate full keyboard/mouse and Xbox gameplay actions, protect and test save loading, and prepare Windows compilation and compatibility testing.

**A speed ratio is not the percentage of the project completed.** The nominal 60 Hz simulation target for the tested benchmark is approximately **16.67 ms/frame**. The first **1.013× average** result does not establish smooth presentation on every frame or full-game performance. **x86-64-v3** remains the declared CPU ISA baseline, with no complete minimum system specifications established.

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

After core speed and physical controls stabilize, validate menus, cinematics, rooms, battles, transitions, save loading, game progression, sound and visual effects in multiple scenes.

A **European PAL R3OP Wii SD save** has now been inspected in a private read-only analysis. Its `data.bin` export is **113,408 bytes**, with metadata and **three encrypted save slots**. The advertised **100% completion has not been verified**, and **neither Dolphin import nor native save loading has been tested**. It can become a private test fixture for later areas, but must not replace from-new-game testing.

Keep the original intact and never publish save contents, private NAND data or game assets.

## Release prerequisites

Before publishing a build:

- Demonstrate stable, correct gameplay over substantial portions of the game and ultimately full progression.
- Validate audio, display, input, saves and performance on Linux and Windows.
- Test conventional controller mappings and any claimed DolphinBar support.
- Ensure players use their **own legal game copy**; publish no Nintendo assets or disc images.
- Audit all third-party source licences and corresponding-source distribution obligations.

The [current progress page](BUILD_STATUS.md) provides the latest evidence and known limitations.
