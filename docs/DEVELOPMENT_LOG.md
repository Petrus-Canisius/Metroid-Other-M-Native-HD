# Development log

[Home](../README.md) · [Status](BUILD_STATUS.md) · [Update procedure](MAINTENANCE.md)

I use this log to record significant milestones and the limits of the supporting evidence. I do not include raw game data or private diagnostic artifacts.

## 2026-11-02: Further Vulkan optimization: training room reaches 0.576× (most recent report)

My latest Claude Code report records a substantial improvement in the **training-room performance benchmark**: **0.369× to 0.576× normal emulated speed**. The average frame processing time fell from **45.27 ms to 28.98 ms**, with P99 falling from **58.70 ms to 35.13 ms**. The benchmark covers approximately 1,918 simulated frames between 438 and 470 emulated seconds. This is a measured gain in one scene, **not a claim of stable full-speed gameplay**.

Four main improvements were implemented and committed:

- **GPU-side copy-to-texture** (`5a31680`). A Vulkan compute shader produces reused image textures directly on the GPU for matching source, format and dimensions, with memory-coherency checks and a fallback. The tested formats were RGB565 and RGBA8 including half-scale copies. Reported copy-related waits fell from **16.3 s to 2.1 s**.
- **Reuse of transformed vertices** (`0f4a848`). Repeated references to a vertex in one primitive no longer force repeated transformations. The measured vertex-transform stage fell from **30.9 s to 19.1 s**.
- **Remove an unused software clear in Vulkan mode** (`fb257f3`).
- **Parallel final-image conversion on four threads** (`f34e9ed`), preserving identical output.

The report also lists final documentation or validation commit `27cbcde`. Previously developed Vulkan batching, separate rendering/copy threads and in-flight submissions remain part of the optimized path.

**Correctness checks:** **28,167 frames** remained identical to the established internal baseline through the optimizations, **7,686 GPU copy-texture results** matched CPU decoding, and more than **ten** repeated runs produced matching images at the observed checkpoints. The translator tests remain **753/753** and the input-adaptation tests **3/3**. The runtime's previously reported audio comparison extends to **156.7 seconds** matching a reference capture. This report does not indicate new physical-controller tests or complete audio validation.

The Vulkan/software comparison still differs on **29 fade frames**. Indirect texturing, fog, mipmaps, dithering/blending order and constant destination alpha remain incomplete. **135 and 387 bytes** still differ from Dolphin at the two known memory checkpoints. Vulkan validation layers were not run in the optimization session because they were not installed.

**Next priority:** reduce the CPU-side cost of vertex lighting/transformation and remaining copy encoding, then optimize shaders only where profiling shows meaningful benefit. The report estimates that approximately **12.3 ms** must still be removed from the average 28.98 ms/frame to reach the **16.67 ms/frame** budget for normal speed in this benchmark. The player-facing window and Xbox input existed from earlier work, but were **not retested in this optimization session**.

**Evidence boundary:** all details come from the private Claude Code report; no independent execution or copyrighted game data was used for this public documentation update.

## 2026-11-02: First real Linux game window, Xbox controller and gameplay diagnostics (previous milestone)

An earlier Claude Code report records the **first real Linux application window** using SDL3 and Vulkan, with fullscreen switching, resizing and sound through the Ubuntu output device. A **physical Xbox Series X controller** was detected during a test session. The runtime logged **509 virtual Wii Remote state changes** derived from controller input, and generated a replayable input recording and timestamped diagnostics.

A controlled replay compared the recorded controller session with a run receiving no such inputs. Images were initially identical, then diverged after the recorded input became active. The replay showed **Samus moving and a beam shot**, while the no-input run left her stationary. This is evidence for **real-controller-originated movement and fire**, but not for all button mappings. Jump, Morph Ball, dodge, first-person aiming, missiles and concentration remained unverified; LB had no identified effect.

The display achieved high throughput on a simple warning screen but the **training room ran at only approximately 0.3× game speed**. The graphics path avoided software rasterization but still did substantial CPU geometry work and used some software image encoders. The diagnostic system supports a reportable F3 status view and local input logs / DTM replay. The report lists local commits **`5d3b4fd`** and **`1162d6f`**.

**Evidence boundary:** Claude used the logs and deterministic replay; he did not claim to watch the user's screen directly. This was a private interactive proof, **not a released or fully playable build**.

## 2026-11-02: Reproducible Wii timing, early Vulkan output and input-driven Samus movement (earlier milestone)

This session moved the native project beyond menus and cutscenes into its **first game scene with input-driven movement**. The private development harness reaches the training area, where Samus, the HUD and French subtitles are visible. Two test runs, one with controller inputs and one without, produced the same images until **450.05 seconds of emulated time**, just after the first scripted input. After that point, the images diverged; Samus moved along a wall in the input run and remained still in the control run. This establishes a response to scripted input in that scene. It **does not** prove that each action button is correctly mapped, that Dolphin reproduces the same gameplay or that a human can play the game in real time.

**Deterministic timing.** The project now uses a simulated Wii clock based on emulated execution, with a way to skip empty waiting periods. At the specific memory checkpoints tested, two native runs produced exactly the same **24 MiB memory contents**; under the earlier host-clock model, the repeat runs differed by 75 bytes. Compared against Dolphin, the outstanding differences fell to **135 bytes at IPC #4** (previously 227) and **387 bytes at IPC #35** (previously 810). Those differences are **still open**, and some time-sensitive scanline effects do not yet align with Dolphin.

**GPU progress.** An initial **Vulkan** graphics path was implemented and tested on an NVIDIA RTX 4090, using some transformed/rasterized data from the software renderer. For the reference wrist-strap screen, **767 of 796 frames** matched the software image exactly. **29 fade frames** still differed because blending/dithering is not yet fully reproduced. The Vulkan implementation is not standalone or optimized and does not yet provide a window for normal play; it is an early correctness milestone rather than a finished accelerated renderer.

**Audio and regression tests.** After aligning the output, **20.58 seconds** of active audio matched Dolphin sample-for-sample (up from about 17.1 seconds). The translator suite passed **753/753 tests**, including 21 new tests; **32,282 functions** remain translated. Sampled RVZ reads, the checked GX FIFO and the native repeatability checkpoints remained sound.

**Windows groundwork.** A common platform interface and initial Windows-specific implementation have been written. They have **not** been compiled or run on Windows. The developer environment prevented execution of a newly downloaded Windows toolchain; no permission bypass was used. A native Windows build is therefore unverified.

**Input and visuals planning.** Optional anti-aliasing (Off / FXAA / SMAA / possible MSAA, with TAA for later study) was documented, **not implemented**. The next input design requirement is to adapt essential Wii Remote orientation, IR pointing and physical gestures to configurable keyboard/mouse and conventional gamepad controls, so a physical Wii Remote will not be required. **Those live control mappings have not yet been implemented or tested.**

**Next milestone:** a standalone efficient Vulkan renderer, a Linux game window, live human controls and a reproducible training-room session with graphics and audio. Full gameplay, modern display upgrades and Windows execution remain future work.

**Evidence boundary:** this entry summarizes the 2 November 2026 Claude Code report. I have not independently run the private game binaries, inspected the images or tested Windows for this public update. No original Nintendo game data, rendered captures or private source code was published.

## 2026-11-02: First native rendered image and interactive OPTIONS menu

My latest Claude Code console report records the first **genuine software-rendered image from native GX commands**. I adapted Dolphin's software renderer behind the existing GX interface (vertex pipeline, TEV, textures, EFB/XFB copies). The Wii Remote wrist-strap warning XFB matched the Dolphin software renderer **byte-for-byte: 614,400/614,400 bytes**, after aligning copy-filter settings. A previously observed 68,439-byte discrepancy was traced to a reference configuration difference, not assumed to be a rendering-code error.

Several additional transition and animated introduction frames also matched exactly. The CRT-style scanline animation can differ in phase because the current emulated time still partly follows the host clock. The renderer achieved around **28 FPS** on the reported full-screen test and is a **reference-only implementation**, not the final accelerated renderer. **Direct Vulkan** is favored for future GPU acceleration but is not yet implemented.

Scripted Wii Remote inputs drive the native **OPTIONS menu**, where the cursor, language and subtitle values change. Pressing **1/START** reaches a loading screen and the opening cinematic. **HOME** opens the system menu and the IR pointer moves. The native harness also records and replays **DTM** input. Dolphin matched the wrist-strap screen and selected intro frames, but did **not** progress to OPTIONS in the lengthy software-renderer replay. Consequently, the native menu interactions are **not yet reference-validated**, and there is **no validated playable gameplay**.

A missing translated indirect-entry function at `0x8013F4C8` initially blocked progression after START. Analysis identified **seven** genuine extra function entries, bringing the translated count to **32,282**. The updated build passed **732/732 translator tests**. The REL image and GX FIFO checks retained their known exact matches; sampled RVZ reads remained exact. The latest memory comparisons still show **227 differing bytes at IPC #4** and **810 at IPC #35**. Audio matched Dolphin sample-for-sample for **17.09 seconds** after alignment.

**Next work:** a deterministic Wii clock for aligned input and frame comparisons, then a GPU-accelerated Vulkan renderer and a validated first controllable gameplay sequence.

**Evidence boundary:** these results come from my local Claude Code report. I did not rerun the runtime, inspect private screenshots or reproduce the frame and audio comparisons for this public update. No game captures or Nintendo data were uploaded.

## 2026-11-01: AX DSP processing passed; extended native run reaches watchdog

My subsequent Claude Code report states that the **AX Wii DSP microcode** is implemented at a high level, including command frames, audio voice processing, stereo DMA capture and scheduled DSP/ARAM interruptions. The implementation reuses Dolphin-derived code with notices retained. A new **context-switch correction** fixed a nonvolatile register guard that had overwritten registers after `OSLoadContext` restored a different fiber. The reported extended run completed **135,750 thread switches** and **73,903 audio command frames** before the configured watchdog cutoff, without a reported crash.

The native WAV capture contained **239 seconds of silence**. Dolphin was also silent for **250 seconds** in the same configuration with no connected Wii Remote. No voice was active, so this is only a **weak consistency check**. Actual audio mixing, sound quality and sample equivalence remain unvalidated. No image was rendered, and the game is **not playable**.

Regression checks reportedly retained **732/732 translator tests**, GX equivalence for **291 commands**, exact sampled RVZ reads and **223 differing bytes** at the 24 MiB / fourth IPC comparison. The 35th IPC memory difference was reported as **810 bytes**, versus 881 previously; these differences are **not resolved**.

The next dependency is the emulated **Wii Remote L2CAP/HID stack**, using scripted inputs for the Dolphin reference comparison. HDR10, VRR, G-SYNC, FreeSync and ultrawide support are still documentation-only goals.

**Evidence boundary:** all results come from the user-provided console transcript. I have not executed the build, compared its WAV files or independently validated the runtime in this documentation update.

## 2026-11-01: GXInit passed; DSP AX microcode is the next stop (earlier milestone)

My later Claude Code console report records native startup **passing GXInit** after implementing CPU-side GX gather-pipe, CP/PE registers, FIFO processing, command decoding and capture. At the 35th IPC call, **291 GX commands** and the **512 KiB FIFO** matched the Dolphin reference. An inspection tool supports private GX capture analysis. **No rendering stage is implemented**, so a command or frame submitted to the emulated GPU is not a picture on screen.

The reported updated toolchain retranslated **32,275 PowerPC functions** and passed **732/732 translator tests** after correcting direct HID0/HID1/HID2 register writes to preserve hardware semantics. Initial SPR values were compared against the reference. The NAND SYSCONF initialization output matched Dolphin byte-for-byte.

The native harness added a read-only RVZ reader, IOS disc timing and partition reads, filesystem directory and write support, Bluetooth HCI initialization without a connected Wiimote, and decrementer exception handling. **103 wait loops** are instrumented, including 48 with side-effect-free calls. A later boot run reported approximately **35,000 interrupts**, **96,982 drawing commands** and **1,290 frames submitted to the emulated GPU**. No frame was displayed.

A 24 MiB reference snapshot at the fourth IPC call after main-thread resumption had **223 differing bytes** in the most recent run. Across revisions the count ranged from 141 to 233. The known timing/FPU/interrupt-context classes explain the observations, but the discrepancies **remain unresolved**.

The reported current stop is at the **AX Wii audio DSP microcode load**. Audio processing is the next implementation priority. HDR10, VRR, G-SYNC, FreeSync and ultrawide support remain documentation-only goals.

**Evidence boundary:** these are results stated in the provided development console transcript. I did not rerun the binaries or independently compare private memory captures for the public update.

## 2026-11-01: NAND configuration and scheduling investigation (earlier milestone)

In a later console log from my project, I recorded native startup reaching and reading `/shared2/sys/SYSCONF` through a partial IOS filesystem proxy. The same log reports 31 delivered interrupts at that point.

I am using a wait-loop analysis tool to identify short memory-polling loops that depend on asynchronous hardware events. The tool reported **55 candidate loops**, including loops involved in DVD initialization. Safe-point integration was underway.

The remaining work includes matching the native and Dolphin IPC capture points, validating asynchronous event delivery, and implementing the IOS disc interface. Retranslation and a reference capture were still running at the end of the available log.

**Evidence boundary:** these observations come from my in-progress project log, not from a new independent reproduction. They do not establish a working disc path, validated thread switches, native rendering or playability.

## 2026-11-01: Translation and early native startup (earlier milestone)

The completed translation log reports **32,275 generated functions** and **732 passing translator tests**. An earlier generation has a successful native object compilation report for **30,045 functions**. The completed-build result must not be transferred to the newer generation.

Early Wii startup, interrupts, event scheduling and IOS communication have been exercised. At the first VI access, a previous MEM1 comparison with Dolphin differed in four time-base bytes.

An earlier filesystem IPC comparison reported 220 different memory bytes. Subsequent debugging found that the reference snapshots may have been taken at different points in the IPC sequence. The mismatch is therefore not yet a validated divergence.

## 2026-10-31: Executable analysis foundation

Initial DOL and REL parsing, relocation, integrity checks and function discovery established the basis for static translation.

## Evidence policy

Dates identify when an observation was reported. They do not imply a release schedule. The [build status](BUILD_STATUS.md) separates generated, compiled, executed and validated results. I review new entries carefully before publishing them.
