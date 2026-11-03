# Development log

[Home](../README.md) · [Status](BUILD_STATUS.md) · [Update procedure](MAINTENANCE.md)

I use this log to record significant milestones and the limits of the supporting evidence. I do not include raw game data or private diagnostic artifacts.

## 2026-11-03: First measured average above normal speed — 1.013× (training room)

**Milestone.** The private Linux native runtime first exceeded **1.0× average simulation speed** on the fixed training-room benchmark: **1.013×**, reported after **six alternating A/B pairs** of a Vulkan fragment-shader optimization. The new behavior provided **−0.52 ms/frame on average**, improved P99, and **unchanged internal reference frames**; the developer then reported that the full non-regression suite passed. No precise new P99 range or change-commit hash was included in the shared report. The earlier eight-slot Vulkan baseline had been about **0.978×** in a preceding controlled session.

**Why it helps.** Profiling showed the fragment/TEV shader was a large part of GPU render-pass time. A diagnostic removal of fragment `discard` everywhere lowered pass execution time, but this would not be a generally correct implementation. Instead, the runtime uses the emulated **alpha-test classification** (always pass / always fail / depends on value) to choose a shader **without `discard` only where the test necessarily passes**. Conditional paths preserve the required behavior. A deliberately trivial shader produced divergent reference frames, verifying that the reference check detects at least that incorrect variant.

**PowerMizer diagnostic could not run.** The host session uses Wayland, with no accessible X NV-CONTROL extension to read/change PowerMizer through `nvidia-settings`. The test was therefore **not performed**; the GPU remained at its observed **P8**, approximately **210 MHz core / 405 MHz memory**, during the initial diagnostic observation. No persistent or temporary NVIDIA policy change was made.

**Next experiment still running.** A separate **TEV shader specialization** using Vulkan specialization constants for stage counts/texture coordinates was prepared for six paired A/B runs. **There is no result in the shared excerpt**; it must not be counted as a retained performance improvement yet.

**Limits.** The 1.013× figure is a **mean speed in one training-room benchmark** and not proof of smooth 60 Hz pacing in every frame, gameplay beyond the tested scene, completed controls, full Wii/Dolphin fidelity, or a release-ready Windows/Linux game. This is a **shared-engine rendering change**, not a per-level patch. No game code, copyrighted assets, private save files or playable executable were published.

## 2026-11-03: Vulkan submission pipeline reaches ~0.978× in a measured training benchmark

**Retained performance result.** The latest private report increased in-flight Vulkan submission slots from **four to eight**, after new event tracing revealed that waiting to acquire a submission slot accounted for approximately **3.96 ms/frame** within a **5.66 ms/frame** render-thread wait/flush component. Six alternating A/B pairs found **−1.28 ms/frame** on average; three more pairs comparing the final binary with the pre-change reference confirmed **−1.20 ms/frame**. The training-room benchmark reached **16.8–17.2 ms/frame (~0.978×)**, with reported **P99 of 20.2–20.5 ms**. Six slots did not demonstrate improvement; twelve used more memory without a clear gain. Peak RAM increased from about **863 MB** to about **1,041 MB**. Local commit: **`4d7cb38`**.

**Earlier confirmed compiler optimization.** The renderer uses **Clang 23.1.4/LLD 23 ThinLTO**, not GCC's `-flto=auto`. Six paired A/B measurements showed **−0.58 ms/frame**, with mean training times **18.73 → 18.15 ms/frame** (reported **0.890× → 0.918×**). Experimental Clang PGO produced an average **+0.19 ms/frame** regression in the profiled training room and slight slowdowns in an unprofiled warning scene, so it was not activated. Local commits: **`b951371`**, **`606fcce`**.

**Correctness correction, not speed gain.** EFB copy destination ranges had been overestimated by a factor of **4–8**. Using the actual block-row geometry eliminated **8,038 false copy-before-texture waits** without a measurable mean speed improvement (**−0.04 ms/frame** over six pairs). The corrected bounds were checked against actual writes. This was retained for dependency accuracy, with a P99 improvement observed in five of six paired comparisons. Local commit: **`709434c`**.

**Floating-point audit.** Recompiling **32,282 translated PowerPC functions** with `-ffp-contract=off` left their generated machine code byte-identical in the tested build. Four native runtime objects changed, including disc and file time calculations. A targeted numerical test reported no final emulated-tick difference over **20 million simulated disk seeks**. The option was retained as a guard against unwanted host floating-point contractions, not as proof of full PowerPC equivalence. Local commit: **`594ba1b`**.

**Validation.** The latest retained code was reported to pass **28,167 internal frame checks, 753/753 translator tests, 3/3 input tests, selected 156.73 seconds of audio checks, IPC checkpoints 4 and 35**, and **4,671 GPU copy checks with zero differences**. These tests do not clear the pre-existing **29 Vulkan/software fade-frame differences** or **135/387-byte Dolphin memory differences**.

**Next measured bottleneck.** The trace now attributes about **4.65 ms/frame** to GPU execution, **2.43 ms/frame** to copy-stage readback/encoding, and **2.26 ms/frame** to delay before GPU execution in the studied synchronization path. Read-only NVIDIA telemetry observed the development RTX 4090 frequently at low-power **P8 (210 MHz core/405 MHz memory)**. The cause/effect is **not yet established**. A temporary, reversible A/B diagnostic has been authorized but **its outcome is not yet available**. Any application-side optimization should remain GPU-vendor-neutral.

**Limits.** The **~0.978×** number is a private, single-system training-room benchmark, **not sustained 1.0×, full-game compatibility or a public release**. The target CPU ISA is **x86-64-v3**. Cross-vendor Vulkan, Windows, complete controls, game saves and higher-resolution presentation remain future verification/development work. These results do not establish minimum PC requirements. No game assets or runtime binaries are published in this documentation repository.

## 2026-11-03: Earlier unchanged-code session — 0.897× observed (historical)

**Change / finding.** The most recent training-room benchmark completed **five runs at 0.879–0.917×**, with an observed average **0.897×** and average processing times between **18.19 and 18.97 ms/frame** (approximately **18.58 ms aggregate**). This is close to normal simulated game speed but **not yet demonstrated as sustained 0.90×, 0.95× or 1.0× gameplay**.

**Evidence and limits.** No new runtime optimization was retained in this session. The **same code** had produced **0.835–0.848×** in the previous session. The between-session improvement therefore **cannot be attributed to a code change**. CPU/GPU operating conditions, background activity, scheduling and cache state still need systematic monitoring. The controlled benchmark focuses on approximately 1,918 simulated frames in the training room (438–470 emulated seconds); these results do not demonstrate full-game performance.

**Rejected experiment.** Moving geometric **slope calculation** (previously within `make_record`) to the transformation workers maintained images, memory and triangle counts but worsened mean frame time by **~0.21 ms/frame** across three paired alternations. The extra data transfer was substantial (reported at **544 bytes/triangle**). The experimental code was **reverted**, and the previous binary hash was restored. Local documentation commit: `17897ac`. This was **not** a new speed improvement.

**Pipeline-cache experiment.** Isolated test caches gave pipeline creation time of **436 ms cold versus 1 ms warm**. The first frame took **514 ms cold versus 82 ms warm**; subsequent frames remained below 50 ms in both reported test cases. This supports cold Vulkan pipeline compilation as the cause of the **observed ~520 ms startup spike**, but does not prove all future scenes are stutter-free. No normal user shader caches were deleted.

**Correctness.** The cited regression runs retained **28,167 identical internal reference frames** and matching triangle counts. Because the experimental code was fully reverted, the most recent session did not rerun the full translator/audio/memory suite; the last full-suite results belong to the previously validated runtime. The known **29 fade frames** and **135/387-byte Dolphin memory differences** remain open. The original PAL save remained untouched and its claimed 100% progression was not verified.

**Next engineering focus.** Establish statistically meaningful **paired benchmarks**, profile the **GX vertex loader** and frequent native runtime helper calls, and investigate pipeline preparation without mixing cold-start and steady-state performance. The latest observed 18.58 ms frame mean remains approximately **1.9 ms** above the nominal **16.67 ms** budget for this 60 Hz test.

## 2026-11-03: Parallel triangle assembly and clipping — controlled improvement

**Change.** The two ordered vertex-transformation workers now also **assemble primitives, clip triangles and capture their data**, using thread-private intermediate state, while the render worker chiefly records Vulkan commands and aggregates counters. The old serial path remains available for controlled comparison. Local code commit: `24707d1`, documentation commit: `90e97c3`.

**Measured benefit.** Three paired same-binary A/B comparisons showed reductions of approximately **0.53, 0.48 and 0.61 ms/frame**, **~0.54 ms/frame on average**. This moved the training benchmark from around **20.30 ms/frame** to **19.67–19.98 ms/frame** in that session, corresponding to **0.835–0.848×**. Although close, **0.85× was not reproducibly reached in that session**.

**Checks.** The old and new modes produced matching **28,167 internal frames**, unchanged triangle counts (**98,418,297 input / 45,399,644 drawn** in the test), the same referenced audio and memory checkpoints, and **753/753 translator** and **3/3 input tests** passing. A new A/B comparison of **two versus three** transformation workers showed **three workers slower**; two remains the default. Approximately **857 MB RAM and 78 MB VRAM** were reported for the benchmark.

**Limits.** This improvement was measured only in the test sequence. The Vulkan/software comparison still reports **29 differing fade frames**. The private PAL save was not loaded. All results come from private Claude Code reports.

## 2026-11-03: GPU image reuse, threading safety and PAL save inspection

**Change.** The Vulkan path previously performed thousands of costly image memory allocations and frees. A **bounded reusable image pool** (up to 512 images) now recycles retired images by usage/size, checking that they are no longer in GPU use. Local commit: `8f19390`.

**Measurements.** The same-session benchmark improved from **22.20–22.22 ms/frame to 20.64–20.66 ms/frame**, or approximately **0.751× to 0.807–0.808×**. One paired measurement isolated **~1.3 ms/frame gain** from the image pool; the sampled driver-call proportion in the render thread fell from **16.7% to 0.8%**. RAM remained about **857 MB**, without a demonstrated memory increase. No broader minimum-hardware conclusion can be drawn.

**Other changes.** A shared normal-data cache was found to be accessed concurrently by rendering and vertex workers. The render path was changed to use the command's own data, eliminating the non-synchronized shared access (**`4bcf93d`**). This was kept as a **safety correction without measurable performance improvement**. The benchmark gained a top-five slowest-frame report (**`3846442`**), which identified a consistent **~85–88 ms first-frame startup cost**. The previous ~520 ms spike was not initially reproduced and was subsequently analyzed with isolated pipeline caches. Documentation commit: `9122b3b`.

**PAL save discovery.** A European **R3OP** Wii SD export (`data.bin`, **113,408 bytes**) was inspected **read-only**. Its metadata identified the Wii title and **three encrypted save slots**, plus system metadata. The original file's hash was recorded in private documentation and remained unchanged. **The claimed 100% completion, import into Dolphin and ability to load into the native runtime remain untested.** No save content, NAND data or private local path is included in this public log.

**Correctness.** The latest reported full suite continued to pass **753/753 translator**, **3/3 input**, matching internal reference frames and **156.73 seconds** of audio against a selected reference capture. Existing Dolphin discrepancies and missing graphics effects remain open.

## 2026-11-03: Two ordered transformation workers and optimized TEV shader

**Change.** Several iterative optimizations preceded the more recent Vulkan image-pool and triangle-assembly work:

- Reused a light's attenuation calculation for alpha and color, where the same value was previously recalculated (**`1db8166`**); paired results improved roughly **28.21 to 27.0 ms/frame**.
- Moved vertex transformation to **two ordered worker threads** (**`014d431`**), maintaining local copies of relevant XF/BP graphics state and checking primitive sequence numbers. Repeated comparisons favored **two workers over three**, with a reported **~22.24 ms/frame (0.750×)** configuration.
- Cached the last known ring-buffer minimum consumer position to avoid needless cache-line bouncing (**`4205464`**); paired results improved from roughly **22.22–22.54 to 21.73–21.86 ms/frame**.
- Optimized the actual fragment/TEV shader (**`fe1b28c`**), with a reported GPU-render stage reduction from **7.6 to 6.35 seconds** on the measured sequence, and a resulting **0.786–0.805×** benchmark range.
- Corrected alignment-area reads for image copies and improved thread diagnostics (**`4727370`**), as a fidelity change rather than a performance optimization.

**Rejected experiments.** Futex-style sleeping workers introduced a performance regression and exit problem; that approach was reverted. GPU encoding of EFB-to-RAM copies produced byte-identical output but **no net frame-time gain** and was removed. Further demand-driven synchronization and measurement-toggle changes were not beneficial.

**Accuracy and limitations.** The cited optimization tests retained reference frames, relevant texture-copy checks and audio/memory checkpoints. The runtime was **not** made feature-complete by these changes: **29 Vulkan/software fade frames** and known Dolphin memory deltas remain unresolved. A complete Vulkan validation-layer run was not available.

## 2026-11-02: Earlier Vulkan optimization: training room reaches 0.576×

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
