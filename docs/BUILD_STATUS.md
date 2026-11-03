# Current development status

[Project overview](../README.md) · [Roadmap](ROADMAP.md) · [Development log](DEVELOPMENT_LOG.md)

**Documentation updated: 3 November 2026 · Latest private reports: 3 November 2026 · Interactive Linux prototype · No public release**

This page summarizes **private Claude Code development reports**, not an independently reproduced public build.

## Summary

The private Ubuntu native runtime has an SDL3/Vulkan window, audio and physical Xbox input. Movement and firing have been demonstrated by a recorded controller replay. The **latest reported training-room benchmark has reached 1.013× average simulation speed**, the **first measured result above 1.0×**, following a conditionally specialized Vulkan alpha-test shader. **Sustained frame pacing, stable 1.0× in other scenes and complete gameplay remain unverified.** There is no public build.

The latest progress is drawn from **private Claude Code engineering reports of 3 November 2026**, not a publicly downloadable or independently rerun test binary. Results apply to the development machine and tested segment.

## Evidence and current limitations

| Area | Reported result | Important limitation |
| --- | --- | --- |
| PowerPC translation | **32,282 translated functions**, **753/753** translator tests | Does not imply full-game correctness |
| Native gameplay | Linux SDL3/Vulkan window, audio, physical Xbox input; recorded replay shows movement and firing | Further actions and longer gameplay untested |
| Fixed training benchmark | **1.013× average** after specialized alpha-test shader; **−0.52 ms/frame** across **6 A/B pairs**, P99 improved | First mean result over 1.0× in this scene; not verified on other levels or as steady frame pacing |
| Graphics | **28,167-frame established internal regression suite**; **4,671 GPU copies** checked in preceding suite; alpha-test candidate reported matching frames and full non-regression passing | **29 fade frames** still differ in Vulkan/software sample |
| Sample pixel comparison | **98.9016%** of pixels matched | Not full-game Dolphin equivalence |
| Memory | IPC checkpoints 4 and 35 match the project's baseline | Dolphin comparison still differs by **135/387 bytes** |
| Sound | **156.73 seconds** of tested reference audio unchanged | Not whole-game audio validation |
| Controls | **3/3** automated input tests | Jump, Morph Ball, dodge, first-person, missiles and concentration still need verification |
| Memory footprint | About **1,041 MB RAM** with eight slots (previous four-slot build ~863 MB) | Not a minimum-spec measurement |
| Save | Private European R3OP SD export, three encrypted save slots | Claimed 100% progress, Dolphin import and native reading unverified |
| Windows | Portability groundwork exists | No compiled/tested Windows build |

## Training-room benchmark: reproducible optimizations

Benchmark: **438–470 simulated seconds**, approximately 60 Hz; the nominal **1.0× target is 16.67 ms/frame**. Speed ratios reflect simulated timing in this fixed scenario, **not development-completion percentages**.

| Stage | Reported time/frame | Reported speed | Evidence |
| --- | ---: | ---: | --- |
| Early CPU/GPU path | 45.27 ms | 0.369× | Early baseline |
| GPU copies and geometry | 28.98 ms | 0.576× | Retained combined changes |
| Lighting and two transformation workers | ~22.24 ms | ~0.750× | Retained |
| TEV optimization | 20.7–21.3 ms | ~0.786–0.805× | Retained |
| Vulkan image recycling | 20.64–20.66 ms | ~0.807–0.808× | Paired improvement |
| Parallel triangle assembly/clipping | 19.67–19.98 ms | ~0.835–0.848× | About **−0.54 ms/frame** in paired tests |
| Earlier faster session, unchanged code | ~18.58 ms aggregate | ~0.897× | **Environment-dependent; no code gain claimed** |
| Clang/LLD renderer ThinLTO | 18.15 ms mean vs 18.73 ms without | ~0.918× vs ~0.890× | **−0.58 ms/frame**, faster in **6/6 paired A/B runs** |
| **Eight Vulkan in-flight submission slots** | **16.8–17.2 ms** | **~0.978×** | **−1.28 ms/frame** across six paired runs; separate three-pair check **−1.20 ms/frame** |
| **Conditional alpha-test shader specialization (latest)** | **Mean speed reported at 1.013×** | **1.013× mean** | **−0.52 ms/frame** across six paired A/B runs; better P99, matching tested frames |

### Why eight Vulkan submission slots matter

With four slots, the render thread frequently waited for a slot while GPU readback/copy work held previous slots. Event tracing identified roughly **seven synchronization events per frame** in the tested window, with **3.96 ms/frame of slot acquisition within 5.66 ms/frame of measured render-thread flushing/waiting**. Using **eight slots** allows more useful work in flight. Six slots showed no clear improvement; twelve used more memory without a clear speed gain.

Reported slot sweep: **4 slots: 18.0–18.8 ms, 863 MB RAM; 6: 18.1–18.7 ms, 954 MB; 8: 16.8–17.1 ms, 1,041 MB; 12: 16.7–17.2 ms, 1,232 MB**. Results were measured on the development machine, not on Intel/AMD and NVIDIA/AMD cross-vendor comparison systems. Local code commit **`4d7cb38`**.

### New milestone: 1.013× average in training-room benchmark (alpha-test shader specialization)

The latest private Claude Code session reports the **first mean simulation speed over 1.0×** on the fixed training-room benchmark: **1.013×**, from **six alternating A/B pairs** with an **average 0.52 ms/frame improvement** and better P99 versus the old alpha-test shader path. The updated candidate uses the emulated alpha-test result classification to select a **fragment-shader variant without `discard` only when the alpha-test condition always passes**; conditional tests still use the required path. This is a **general renderer change**, not a room-specific optimization.

A deliberately trivial fragment shader produced mismatching frames (demonstrating a useful negative control). The retained alpha-test specialization produced **identical tested frames** and the developer reported that the **full regression suite passed**. The shared report did not include exact new P99 values or a verifiable local commit hash for this change.

**Milestone boundary:** **1.013× is an average of the tested scene**, not a guarantee that every frame meets 16.67 ms, that the entire game runs at normal speed, or that all Dolphin-reference differences are resolved. No publicly playable build exists.

**PowerMizer diagnostic:** The NVIDIA driver setting could not be queried or changed using NV-CONTROL under the host's Wayland/Xwayland session. The test was not performed; GPU settings were left unchanged. The improvement is in the Vulkan application code, not a driver override.

**In progress:** A distinct TEV shader specialization experiment using Vulkan specialization constants has been built and a **six-pair A/B test was running** at the end of the report. Its outcome is **not yet known**. Do not count it as a proven optimization.

### Other retained corrections

- **ThinLTO** on the renderer under **Clang 23.1.4 / LLD 23**; experimental **PGO was rejected** after slower measured performance (~+0.19 ms/frame) and partial profile incompatibility. Local code/documentation commits **`b951371`**, **`606fcce`**.
- **Accurate EFB destination ranges** removed **8,038 unnecessary copy-before-texture waits** caused by 4–8× overestimation. No proven mean frame-time improvement (**−0.04 ms/frame** over six pairs); an accuracy/dependency fix, **`709434c`**.
- **Floating-point contraction audit**: 32,282 translated functions had byte-identical generated machine code with/without `-ffp-contract=off` in the tested Clang setup. Four native runtime object files did change; a targeted simulated-disc-timing test found no change in final emulated ticks across 20 million samples. The flag was retained as a guard against unwanted contraction, **`594ba1b`**.
- Previously retained: GPU EFB copy-to-texture, vertex and lighting reuse, two ordered transformation workers, parallel assembly/clipping, TEV shader optimization, bounded Vulkan image recycling and a threading-safety fix.

### Earlier bottleneck investigation (before alpha-test specialization)

The latest trace attributed approximately **4.65 ms/frame** to GPU execution, **2.43 ms/frame** to copy-stage readback/encoding and **2.26 ms/frame** to delay before GPU execution on the analyzed critical path. These are **trace-specific allocations, not independently additive frame-time savings**.

Read-only NVIDIA telemetry showed the development RTX 4090 in low-power **P8** for much of the benchmark (reported **210 MHz core / 405 MHz memory**). That observation **does not establish causality**. An authorized temporary PowerMizer test **could not be performed** because NV-CONTROL was unavailable under Wayland/Xwayland; the GPU configuration was left unchanged. The subsequent **alpha-test specialization** improved speed without driver changes. The public project must not require NVIDIA-only settings; Vulkan portability remains a goal.

### Correctness, measurement and stutters

The preceding eight-slot optimization passed **28,167 internal frame comparisons, 753/753 translator tests, 3/3 input tests, 156.73 seconds of selected audio checks, IPC checkpoints 4/35** and **4,671 checked GPU copies**. The newer alpha-test specialization was reported to preserve tested frames and to pass the full non-regression suite. This is regression protection, not proof that graphics perfectly match Dolphin. The **29 differing fade frames**, **98.9016%** sample pixel agreement and **135/387-byte Dolphin memory gaps** remain.

Frame-time tails matter: the current **P99 is approximately 20.2–20.5 ms**. An earlier isolated shader cache experiment found **514 ms for a cold first frame vs 82 ms warm**; more scene coverage and pipeline-cache testing are still needed. Host conditions can significantly affect frame-time averages, so new performance claims require paired A/B tests.

## CPU target and cross-platform constraints

**x86-64-v3** is the selected minimum CPU instruction-set target (including AVX2). The project avoids a required AVX-512 path, CPU-model-specific builds and dependencies on Intel hybrid-core scheduling. It is intended to work with compatible Intel and AMD processors and standard Vulkan backends on compatible NVIDIA and AMD GPUs, subject to actual testing.

This instruction-set decision is **not a complete minimum-system-specification announcement**. The development RTX 4090 and the tested RAM usage are not representative minimum requirements. No Windows build has yet been compiled or tested.

## PAL save: inspection completed, loading unverified

A locally obtained European **R3OP** save was inspected **read-only**. It is a Wii SD export (`data.bin`, **113,408 bytes**), with an identified title ID and **three encrypted save slots** (`save0.dat`, `save1.dat`, `save2.dat`) plus metadata. The original remained unmodified; its checksum was tracked privately.

**The advertised 100% completion has not been verified.** No Dolphin import or native save-load test has been completed. The local virtual NAND layout was identified as a possible future destination for **decrypted test copies**, but successful native reading is **not established**. The private original save, its extracted contents, paths and credentials are never published here.

## Next priorities

1. Finish the **six-pair TEV shader specialization A/B comparison** currently underway; report its result before claiming any further gain. The separate PowerMizer diagnostic was unavailable under Wayland, and no driver settings were changed.
2. Reduce **GPU execution, submission delay and copy-stage latency** without changing Wii synchronization semantics or making NVIDIA-specific driver settings a prerequisite.
3. **Consolidate and extend the first 1.013× average benchmark**, achieving sustainable 1.0× with acceptable P95/P99 and no audio desynchronization. The new result applies only to the fixed training benchmark.
4. Broaden representative scene, gameplay-input and graphical-fidelity tests rather than optimize every room individually; retain regression coverage.
5. Investigate shader cold-start stalls, the remaining **29 fade frames** and **135/387-byte** Dolphin differences.
6. Validate the protected PAL save, full game progression, and eventually Windows compatibility without publishing private or proprietary files.

## Publication boundary

**Documentation only.** No game assets, disc images, translated Nintendo code, private captures, encryption keys or playable binaries are published. Results are summarized from private development reports, not independently rerun for this GitHub update. A future distribution requires a rights and open-source licence compliance review.
