# How Metroid: Other M Native HD works

[Project overview](../README.md) · [Current progress](BUILD_STATUS.md) · [Roadmap](ROADMAP.md)

## A native adaptation, not a bundled emulator

The Wii runs a PowerPC program and uses dedicated graphics, audio and operating-system services. A modern PC is very different. This project **translates the game program ahead of time** and provides the Wii functions it needs through a native PC runtime.

**Dolphin is a reference for compatibility tests**, not the final launcher. Some Dolphin open-source components have been adapted within the private implementation and remain subject to their respective licences.

```text
Player's own Wii game copy (kept locally)
               |
        Recompilation pipeline
               |
      Native PC game execution
               |
   Wii runtime, clock and memory
       /          |          \
  PC controls   Audio    GX graphics
       |          |          |
 Virtual remote  Output  Vulkan rendering
               |
        Linux window today
     Windows build in the future
```

## What already runs

**Game execution.** The latest reports show **32,282 functions** translated from the Wii program, with **753/753 translator tests passing**. The native program reaches the menus, opening cinematic and a training-room scene.

**Wii runtime.** Disc access, emulated time, memory, IOS-related services and a virtual Wii Remote provide the environment the game expects. This is not yet proven for every scene or hardware behavior.

**Player controls.** The Linux application uses SDL3 to accept real keyboard/mouse and gamepad input. In one physical Xbox Series X controller session, recorded inputs were fed back into the virtual Wii Remote. Comparing the replay with a no-input run confirmed **movement and beam firing by Samus**. Other actions have not yet been demonstrated. A future goal is to translate every required physical Wii Remote gesture to regular PC inputs and support an optional real Wii Remote through DolphinBar.

**Graphics and window.** A software GX renderer remains the fidelity reference. A **Vulkan GPU renderer** draws into a real SDL3 window and records GX-derived graphics commands. The private runtime uses **GPU copy-to-texture, cached vertex transformations and light attenuation, two ordered transformation workers, parallel triangle assembly/clipping, an optimized TEV shader, four-thread XFB conversion and bounded Vulkan image recycling**. The most recent unchanged-code benchmark averaged **0.897×** in the training room, under host conditions measurably different from earlier sessions; **stable 1.0× is not validated**. Several Wii-specific effects are incomplete.

**Sound.** Audio plays through the Linux audio device. A recent reported comparison found **156.7 seconds** identical to a reference recording in the tested sequence. That does not establish complete-game audio fidelity.

**Reliability.** Repeated native executions are reproducible at the tested points. **28,167 internal test frames** remained unchanged through optimization; **7,686 GPU-made textures** matched CPU decoding. This protects against regressions, but **does not mean all game frames match Dolphin**. Memory differences of **135 and 387 bytes** remain at two Dolphin comparison checkpoints.

## Why performance is still limited

The Wii game depends on specialized GPU memory, EFB/XFB copying, graphics state transitions and CPU/GPU synchronization. Translating its PowerPC code does not automatically optimize those dependencies for modern multicore CPUs and GPUs.

### What has already improved

- **GPU texture copies** avoid some CPU/GPU data round trips.
- **Caching vertex and lighting calculations** removes duplicate work.
- **Two ordered transformation workers** and **parallel triangle assembly/clipping** shift geometry preparation away from the render thread. A controlled A/B test showed about **0.54 ms/frame** saved in the same binary; adding a third worker was slower.
- **Optimized TEV shaders** reduce some per-fragment and pipeline work.
- **Vulkan image recycling** reduces thousands of driver allocations. The image cache is bounded and only reuses retired resources.
- A separate fix eliminated a shared normal-cache access across threads, improving safety without a measurable speed gain.

### Why there is still a gap

Across five runs in the most recent session, the same code measured **0.879–0.917× speed (0.897× mean)**, about **18.58 ms/frame** in the 60 Hz training benchmark versus **16.67 ms/frame** at normal speed. **Previous sessions measured slower with no code difference**, so performance must be tested under controlled host and cache conditions before attributing small gains.

Some remaining costs are distributed between **the GX vertex loader, native runtime helper calls, Vulkan command recording, GPU rendering and required synchronizations**. Moving geometric slope data to worker threads was tested and **reverted after a ~0.21 ms/frame average slowdown** caused by extra data movement and worker load.

### Shader pipeline startup

Isolated tests measured **436 ms** for cold pipeline creation and **1 ms** warm; the first frame took **514 ms cold / 82 ms warm**. This explains the measured first-startup spike, but does not show that all gameplay stutters are solved. Proactive pipeline preparation is a candidate, subject to real measurements and memory cost.

### Correctness boundaries

**28,167 internal regression frames** remained identical across the reported test paths; **7,687 GPU-produced textures** matched a CPU decoder in the exercised cases. These are **not full-game Dolphin equivalence checks**. The software/Vulkan comparison still has **29 differing fade frames**, and memory at two checkpoints differs from Dolphin by **135/387 bytes**. An audio reference segment of **156.73 seconds** matched; full-game audio fidelity remains to be tested.

## What is still missing

The immediate goals are **reproducible profiling, stable 1.0× game speed**, correct remaining Vulkan effects, verified Xbox/keyboard controls and broad scene coverage.

The private PAL **R3OP** save has been recognized as a Wii SD export with three encrypted slots. Its advertised progression and ability to load in Dolphin or the native runtime have **not been verified**. It will be used only as a protected test fixture when appropriate.

A shared platform layer and Windows source files exist, but **Windows has not been compiled or tested**. An optional physical Wii Remote/DolphinBar mode and the planned 1080p/1440p/4K, ultrawide, anti-aliasing, HDR10 and VRR features are **not yet verified**.

## Public code and licences

**This public repository is documentation only.** The private build, Nintendo-derived translated code, personal game files, private recordings, disc/NAND data and captures are not published. Any future distribution must satisfy applicable third-party licences, especially where source code has been adapted from Dolphin, and the project's other legal obligations.
