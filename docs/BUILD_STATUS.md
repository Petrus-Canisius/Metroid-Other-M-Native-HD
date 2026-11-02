# Current development status

[Project overview](../README.md) · [Roadmap](ROADMAP.md) · [Development log](DEVELOPMENT_LOG.md)

**Documentation updated: 3 November 2026 · Latest private reports: 2 November 2026 · Interactive Linux prototype · No public release**

This page summarizes **private Claude Code development reports**, not an independently reproduced public build.

## Summary

An **interactive Linux SDL3/Vulkan prototype** displays native game images, outputs sound and accepts a **physical Xbox Series X controller**. Recorded controller input was replayed and compared with a no-input run; **Samus moved and fired** in the training room. This does not validate full gameplay or a normal-speed public application.

The latest five runs of the unchanged code achieved **0.879–0.917× normal simulation speed**, with a reported **0.897× average** on the training-room benchmark. This is **not attributable to a new code optimization**: the same code had measured approximately **0.835–0.848×** in the prior session. Benchmarks depend on host load, clock behavior and cache state, and need a more tightly controlled measurement protocol.

## Development evidence and limits

| Area | Demonstrated in private reports | Open limit |
| --- | --- | --- |
| Game translation | **32,282 PowerPC functions** translated; **753/753** translator tests pass | Whole-game execution and correctness unproven |
| Wii services | Boot and menu paths, IOS/NAND services, memory and clock simulation, virtual Wii Remote | Some reference discrepancies and untested functionality |
| First gameplay | Samus visible in training room; physical Xbox-originated input replay causes **movement and firing** | Jump, Morph Ball, dodge, first-person, missiles, concentration not yet validated |
| Linux graphics | SDL3 window, Vulkan rendering; several GPU and CPU optimizations retained | Missing GX effects; below normal speed |
| Input suite | **3/3** automated input tests pass | Not proof of all in-game control mappings |
| GPU copies | **7,687** GPU-generated textures verified against CPU decoding in the reported suite | Only exercised formats and scenes |
| Frame regressions | **28,167 reference frames** remained unchanged through tested optimization paths | Internal equality is **not** equivalent to all-frame equality with Dolphin |
| Audio | **156.73 seconds** matched a selected reference capture | Not full-game audio coverage |
| Memory checkpoints | Native runs repeat deterministically at selected points | **135 and 387 bytes** still differ against Dolphin at two checkpoints |
| Memory footprint | About **857 MB RAM / 78 MB VRAM** in latest measured session | Single development system; no verified minimum specifications |
| European save | Wii SD export identified as title **R3OP**, with **three encrypted slots** | Completion percentage, Dolphin import and native loading unverified |
| Windows | Some portable and Windows-specific foundations written | **No Windows compilation or execution** |

## Training-room performance: historical measurements

Reported benchmark window: **438–470 seconds of simulated game time**, approximately 1,918 simulated frames. Timing statistics are **per-frame processing time**, not proof of stable presentation. Results across sessions are affected by host conditions.

| Stage | Mean frame time (approx.) | Simulation speed | Evidence/qualification |
| --- | ---: | ---: | --- |
| Early CPU/GPU path | 45.27 ms | 0.369× | Starting point before major optimization |
| GPU copy and geometry optimizations | 28.98 ms | 0.576× | GPU texture copies, vertex reuse, software-clear removal, parallel XFB conversion |
| Lighting reuse and parallel geometry | 22.24 ms | 0.750× | Reusing light attenuation and two ordered transformation workers |
| Optimized TEV shader and synchronization | 20.7–21.3 ms | 0.786–0.805× | Shader simplification preserving output |
| Vulkan image recycling | 20.64–20.66 ms | 0.807–0.808× | Paired test: **~1.3 ms** frame-time gain from reusing GPU images |
| Parallel triangle assembly/clipping | 19.67–19.98 ms | 0.835–0.848× | Paired A/B/A/B/A/B: **~0.54 ms** gain vs same-binary sequential mode |
| **Most recent session, same code** | **18.19–18.97 ms; ~18.58 ms aggregate** | **0.879–0.917×; 0.897× mean** | **No new optimization retained.** Improvement between sessions cannot be attributed to code |

The 60 Hz test has a nominal **16.67 ms/frame** budget for **1.0×**. The most recent observed aggregate is approximately **1.9 ms/frame** away, but this estimate is conditional on the latest host environment. **0.90× reproducible, 0.95× and stable 1.0× are not demonstrated.** A simulation-speed percentage is **not** a project-completion percentage.

### Retained engineering changes

- **GPU-based EFB copy-to-texture** and reference-verified encodings, reducing redundant CPU/GPU round trips.
- **Vertex reuse** and **lighting attenuation reuse**, avoiding repeated calculations per primitive/light.
- **Two transformation threads** with strict primitive ordering; 3 threads were repeatedly slower.
- **Less producer-consumer cache contention**, including caching an already-known minimum ring-buffer tail.
- **TEV shader optimization**, reducing dynamic indexing and unnecessary fragment work.
- **Bounded Vulkan image recycling** (`8f19390`, up to 512 reusable images), reducing repeated driver allocations. Related sample share in the render thread fell from **16.7% to 0.8%**.
- **Correction of a concurrently accessed normal cache** (`4bcf93d`), a stability fix without a measurable speed gain.
- **Parallel triangle assembly, clipping and capture** (`24707d1`), moving work off the render thread while preserving ordering and triangle counts.

### Rejected experiments and measurement discipline

Several plausible changes were tested and reverted when they did not improve overall speed, including futex-based worker sleeps, GPU encoding of EFB-to-RAM copies, demand-driven synchronization and extra transformation threads.

Most recently, moving **slope calculation in `make_record`** to transformation threads produced matching images and triangle counts but **~0.21 ms/frame average slowdown** across three alternating A/B comparisons. The experiment was rolled back; **`17897ac` documents the negative result**, not a new runtime optimization.

Because the **same native binary** was measured at roughly 19.7–20.0 ms/frame in one session and around 18.6 ms in another, future experiments must control host load, CPU/GPU clock behavior, GPU pipeline cache state and background activity. The private reports explicitly avoid attributing between-session differences to code without paired measurements.

### Pipeline-cache and stutter investigation

A controlled, isolated empty-cache/warm-cache experiment found:

| State | Pipeline creation | First rendered frame | Subsequent frames in that test |
| --- | ---: | ---: | --- |
| **Cold** | 436 ms | 514 ms | None above 50 ms |
| **Warm** | 1 ms | 82 ms | None above 50 ms |

The tested ~520 ms startup spike was due to **cold Vulkan pipeline compilation**. This does not prove every later game scene is stutter-free. Preparing known pipelines ahead of time and maintaining a suitable cache are possible future improvements. The cold-cache experiments were isolated from the developer's usual caches.

### Graphics, audio, determinism and validation

- **28,167 internal regression frames** remained identical in the cited test sequences, including the experiments that were reverted.
- **7,687 GPU-produced copy textures** previously matched CPU decoding. This is format-/scene-scoped evidence.
- The Vulkan/software comparison continues to report **98.9016% pixel agreement** in its tested sample, with **29 differing fade frames**. Indirect textures, fog, mipmaps and parts of blending/dithering still need implementation or correction.
- Native memory snapshots match their previous baseline, while **Dolphin differences of 135 and 387 bytes** remain unresolved.
- The previously validated **156.73 seconds of audio**, **753/753 translator tests** and **3/3 input tests** remain the latest reported full-suite results. The newest slope-only session did **not** rerun the full suite after its complete rollback; it used the same previously validated runtime binary.
- No Vulkan validation-layer run has been reported because the required layer was not installed.

## PAL save: inspection completed, loading unverified

A locally obtained European **R3OP** save was inspected **read-only**. It is a Wii SD export (`data.bin`, **113,408 bytes**), with an identified title ID and **three encrypted save slots** (`save0.dat`, `save1.dat`, `save2.dat`) plus metadata. The original remained unmodified; its checksum was tracked privately.

**The advertised 100% completion has not been verified.** No Dolphin import or native save-load test has been completed. The local virtual NAND layout was identified as a possible future destination for **decrypted test copies**, but successful native reading is **not established**. The private original save, its extracted contents, paths and credentials are never published here.

## Next priorities

1. **Measurement science:** quantify host and between-session variability with paired A/B tests, controlled cache state and CPU/GPU load observations.
2. **Vertex loader:** profile dominant GX formats, memory accesses and decoding; test targeted specialization with a generic fallback.
3. **Runtime overhead:** profile PowerPC helper transitions; preserve the floating-point register safeguards unless the ABI requirements are proved.
4. **Vulkan and startup:** reduce remaining critical-path cost and investigate proactive pipeline creation without moving stutters into gameplay.
5. Achieve and verify **0.90×, 0.95× and sustained 1.0×**; then validate longer sessions, real-controller actions and more scenes.
6. Use the **PAL save only as a protected, private test fixture** once loading is safe and performance sufficiently stable.

## Publication boundary

**Documentation only.** No game assets, disc images, translated Nintendo code, private captures, encryption keys or playable binaries are published. Results are summarized from private development reports, not independently rerun for this GitHub update. A future distribution requires a rights and open-source licence compliance review.
