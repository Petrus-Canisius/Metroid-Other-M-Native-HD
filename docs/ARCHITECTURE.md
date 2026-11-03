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

**Graphics and window.** A software GX renderer remains a fidelity reference. The native SDL3/Vulkan implementation uses GPU EFB copies, cached vertex and lighting computations, two ordered transformation workers, parallel triangle assembly and clipping, optimized TEV shaders, four-thread XFB conversion and bounded Vulkan image reuse. **Clang/LLD ThinLTO** now optimizes the renderer; **eight in-flight Vulkan submission slots** (formerly four) reduce unnecessary blocking in the tested workload. The latest private benchmark reports **1.013× average simulation speed** in the training room, after conditional alpha-test shader specialization. **Steady 1.0× across gameplay and whole-game fidelity remain unverified.**

**Sound.** Audio plays through the Linux audio device. A recent reported comparison found **156.7 seconds** identical to a reference recording in the tested sequence. That does not establish complete-game audio fidelity.

**Reliability.** Internal reference checks remained stable across the latest retained optimizations, including **28,167 unchanged frames**, **4,671 checked GPU copy results**, selected audio and IPC checkpoints. The existing **29 fade-frame** discrepancies and **135/387-byte Dolphin memory differences** remain open.

## Current performance architecture

This is a **native translated-game runtime** with multiple CPU stages feeding Vulkan rather than a conventional PC renderer. The key bottleneck is not simply arithmetic throughput: synchronization across **translated PowerPC CPU code → render thread → GPU → copy/readback stage → virtual Wii completion signals** determines how quickly the game resumes.

### Demonstrated improvements

- **EFB copy-to-texture on the GPU**, and reuse of transformed vertices and lighting calculations.
- **Two ordered transformation workers** and **parallel triangle assembly/clipping**, preserving command sequence. An earlier paired test demonstrated roughly **−0.54 ms/frame**; three workers performed worse.
- **Optimized TEV shaders** and bounded **Vulkan image reuse**, cutting allocations and related driver overhead.
- **ThinLTO with Clang/LLD** for renderer code, with a controlled **−0.58 ms/frame** average change over six paired comparisons. Experimental PGO was slower and is disabled.
- **Accurate EFB copy destination ranges**, removing **8,038 false texture/copy waits** without a demonstrated mean frame-time gain. It was retained as a correctness and dependency-precision fix (local commit `709434c`).
- **Conditional alpha-test shader specialization** selects a Vulkan fragment shader without `discard` only when the emulated alpha test is guaranteed to pass. A six-pair A/B comparison reported **−0.52 ms/frame** on average and **1.013×** average simulation speed in the training room, with matching tested reference frames and passing full non-regression tests. The additional TEV specialization A/B study was still underway at the end of the shared report.
- **Eight in-flight Vulkan submission slots**, up from four. Six paired A/B comparisons indicated **−1.28 ms/frame** on average; three further comparisons indicated **−1.20 ms/frame**. Six slots did not materially help and twelve consumed more memory for no clear benefit. Local commit `4d7cb38`.
- **Explicit floating-point contraction safeguards** after auditing 32,282 translated PowerPC functions and native runtime files. The translated code was byte-identical in the tested `-ffp-contract=off` comparison; the runtime arithmetic changed in four objects, but a targeted disc-time test found no change to computed emulated ticks (local commit `594ba1b`).

### What still limits the runtime

In the latest measured path, the trace attributes about **4.65 ms/frame** to GPU execution, **2.43 ms/frame** to copy-stage readback/encoding and **2.26 ms/frame** to delay before GPU execution. These measurements represent parts of the measured critical path and are not automatic potential savings.

A read-only NVIDIA GPU check found the development RTX 4090 in low-power **P8** for much of the test. The authorized PowerMizer comparison **could not be performed under Wayland/Xwayland**, because NV-CONTROL was unavailable; no driver policy was changed. The alpha-test shader improvement was obtained entirely in application code. The eventual runtime must not require NVIDIA PowerMizer or vendor-specific tuning.

The latest reported **1.013× average speed** is from the fixed training-room benchmark, not the whole game. The alpha-test specialization reportedly improved P99, but the shared excerpt did not provide its exact new value. The 60 Hz speed target is approximately **16.67 ms/frame**, and stable frame pacing across gameplay is not yet established. Shader pipeline cold-start can also cause long first-frame stutters: an isolated test measured **514 ms cold vs 82 ms warm**.

### Fidelity and hardware boundaries

- **28,167 internal regression frames** remain identical through retained changes; the latest relevant suite checked **4,671 GPU copy results** without differences.
- **753/753 translator tests, 3/3 input tests, selected 156.73 seconds of audio and IPC 4/35** matched their internal references.
- The Vulkan/software comparison still has **29 differing fade frames** and **98.9016% sample pixel agreement**; separate Dolphin memory comparisons still differ by **135/387 bytes**.
- The CPU instruction-set baseline is **x86-64-v3** (including AVX2), with no requirement for AVX-512 or a specific Intel CPU. Vulkan is intended to remain portable to compatible NVIDIA and AMD GPUs, but **multi-hardware benchmarks have not been performed**.
- RAM use rose from about **863 MB** to about **1,041 MB** with eight submission slots on the development PC; this is not a system-requirement claim.

Engine improvements target shared subsystems and should benefit multiple levels. **Representative scenes will still need testing**, but there is no plan to hand-optimize each room as an independent engine.

## What is still missing

The immediate goals are **reproducible profiling, stable 1.0× game speed**, correct remaining Vulkan effects, verified Xbox/keyboard controls and broad scene coverage.

The private PAL **R3OP** save has been recognized as a Wii SD export with three encrypted slots. Its advertised progression and ability to load in Dolphin or the native runtime have **not been verified**. It will be used only as a protected test fixture when appropriate.

A shared platform layer and Windows source files exist, but **Windows has not been compiled or tested**. An optional physical Wii Remote/DolphinBar mode and the planned 1080p/1440p/4K, ultrawide, anti-aliasing, HDR10 and VRR features are **not yet verified**.

## Public code and licences

**This public repository is documentation only.** The private build, Nintendo-derived translated code, personal game files, private recordings, disc/NAND data and captures are not published. Any future distribution must satisfy applicable third-party licences, especially where source code has been adapted from Dolphin, and the project's other legal obligations.
