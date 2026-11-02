# Development log

[Home](../README.md) · [Status](BUILD_STATUS.md) · [Update procedure](MAINTENANCE.md)

I use this log to record significant milestones and the limits of the supporting evidence. I do not include raw game data or private diagnostic artifacts.

## 2026-11-02: Reproducible Wii timing, early Vulkan output and input-driven Samus movement (latest milestone)

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
