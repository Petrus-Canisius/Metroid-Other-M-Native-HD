# Roadmap

[Project overview](../README.md) · [Current progress](BUILD_STATUS.md) · [Development history](DEVELOPMENT_LOG.md)

My goal is a **faithful, fluent, fully controllable native PC version** of *Metroid: Other M*. The milestones below separate progress already observed in private tests from future goals. There is **no announced release date**.

## Development stages

| Stage | Purpose | Current status |
| --- | --- | --- |
| **1. Reconstruct the game program** | Translate PowerPC game code for PC execution | **32,282 functions** reported translated; **753/753** translator tests passing |
| **2. Rebuild Wii services** | Disc, memory, timing, audio and virtual Wii Remote | Startup and selected paths run; known fidelity gaps remain |
| **3. Produce real images and sound** | Native graphics and audio | Demonstrated in the private Linux SDL3/Vulkan runtime |
| **4. Provide direct PC interaction** | Conventional input via a dedicated PC application | Physical Xbox input, recorded movement and beam fire demonstrated; full mappings unverified |
| **5. Reach normal-speed gameplay** | Stable speed, frame pacing and responsive controls | **~0.978× in one training-room benchmark (16.8–17.2 ms/frame)**; **1.0× sustained not demonstrated** |
| **6. Validate the full game** | Scenes, mechanics, progression, saves and longer sessions | Not demonstrated; representative scenarios and regression automation required |
| **7. Improve presentation** | HD/4K, 16:9/21:9, optional AA, HDR10 and VRR | Future goals, not completed player features |
| **8. Windows and distribution** | Portable build, test, launcher, release and compliance | No tested Windows build; no public playable release |

## Immediate priority: move from a fast benchmark to robust normal-speed gameplay

The latest private A/B results report a **~0.978×** training-room benchmark, after increasing in-flight Vulkan submissions from four to eight. **Six alternating pairs found ~−1.28 ms/frame improvement**, confirmed by three further pairs (~−1.20 ms/frame). ThinLTO delivered an earlier ~−0.58 ms/frame improvement; PGO slowed the scene and was rejected. A corrected EFB memory-range calculation eliminated false waits without proven mean performance gain.

The next optimization experiment is to test whether the development RTX 4090's low-power P8 state affects measured GPU execution latency. The test is **temporary, reversible and diagnostic only**; **results are not yet available** and a player-facing NVIDIA setting is not an acceptable dependency.

Next engineering work:

1. Reproduce and validate **sustained 1.0×** in the fixed training-room benchmark (about **16.67 ms/frame** for the tested 60 Hz pace) with robust P95/P99, correct audio and accurate simulated timing.
2. Profile and reduce GPU scheduling, submission and copy-stage critical-path costs with **cross-vendor Vulkan** approaches.
3. Expand automated tests across **representative workloads**: menus, exploration, battles, transitions, cutscenes and heavier effects. Global engine improvements should apply across areas; each room should not need a bespoke optimization.
4. Resolve known graphics/memory discrepancies against references and reduce shader cold-start stalls.
5. Test full keyboard/mouse and Xbox controls, native save reading and longer progression, then prepare a Windows build.

**Performance speed is not the project's percentage of completion.** The latest measured benchmark does not establish whole-game performance or a public release date. **x86-64-v3** is the chosen CPU ISA baseline; no full minimum hardware requirements have yet been measured.

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
