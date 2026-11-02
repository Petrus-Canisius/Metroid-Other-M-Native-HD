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
| **5. Reach normal-speed gameplay** | Stable rendering, audio, responsive controls and complete training actions | **In progress:** latest unchanged-code run averaged **0.897×** in one training benchmark; host conditions vary and **stable 1.0× is not reached** |
| **6. Validate the full game** | All scenes, mechanics, saves, controller functions and long sessions | Not demonstrated; additional scenarios needed |
| **7. Improve presentation** | 1080p/1440p/4K, 16:9/21:9, optional AA, HDR10 and VRR | Planned, not player-ready |
| **8. Windows and distribution** | Compile and test Windows, install using a player's copy, launcher and Releases | Windows source groundwork only; no public build |

## Immediate priority: establish reliable measurement and full-speed gameplay

The Linux prototype already has a window, audio and physical Xbox-originated inputs. **The biggest remaining milestone is demonstrably stable, normal-speed gameplay.**

**Performance history:** early tests measured **0.369×**, then **0.576×**; retained code optimizations led to **0.807–0.808×** and **0.835–0.848×** in later controlled sessions. The latest run of the *same code*, under different host conditions, measured **0.879–0.917×**, **0.897× average** across five executions. **No new code optimization caused that latest between-session rise.**

Next engineering work:

1. **Stabilize benchmark conditions**: quantify CPU/GPU frequency, background load, scheduling, shader/pipeline caches and variability; use paired A/B comparisons.
2. **Profile and specialize high-cost vertex formats** in the GX vertex loader, with a fully correct generic fallback.
3. **Profile the translated PowerPC runtime helper overhead**, preserving calling-convention correctness and register state.
4. **Reduce critical-path Vulkan work** and prepare known pipelines earlier, if measured worthwhile.
5. Demonstrate **0.90×, 0.95× and sustained 1.0×** without skipped work, inaccurate timing or significant frame pacing problems.
6. Re-test **real physical Xbox gameplay** and validate all essential actions, then broaden checks to other scenes and saves.

The reported frame-time budget for 1.0× on the tested 60 Hz training sequence is **16.67 ms**; the last run averaged around **18.58 ms**. These measurements **do not represent the percentage of project completion**. The lightweight warning screen may run much faster, but is not representative of gameplay.

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
