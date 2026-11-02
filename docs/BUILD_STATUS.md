# Current development status

[Project overview](../README.md) · [Roadmap](ROADMAP.md) · [Development log](DEVELOPMENT_LOG.md)

**Last reported progress: 2 November 2026 · Interactive Linux prototype · Below normal game speed · No public release**

This page summarizes **private Claude Code development reports**, not an independently reproduced public build.

## Summary

A **real Linux SDL3 game window** now presents Vulkan-rendered images and sound. A physical Xbox Series X controller was recognized in a recorded session. Replaying that session showed **Samus moving and firing in the training room**, compared with a no-input control run.

The immediate obstacle is **performance**: the reported training-room run improved to **0.576×** its intended speed, but is **not yet fluid or ready for general play**.

## Verified or observed within the reported test scope

| Component | Latest reported evidence | Important limit |
| --- | --- | --- |
| Translated game program | **32,282 PowerPC functions**; **753/753 translator tests** passed | Full-game correctness is not established by translation |
| Original game startup | Warning, logo, OPTIONS, loading, opening cinematic and training room reached | Other scenes and transitions remain untested |
| Linux graphics | SDL3 window, resize, fullscreen; Vulkan renders frames; batching and GPU-generated copy textures | Missing Wii effects and insufficient full-speed performance |
| Controller input | A **real Xbox Series X controller** recognized; inputs logged and replayed; Samus **moves and fires** in training area | Jump, Morph Ball, dodge, aiming, missiles and concentration are not validated |
| Input diagnostics | Optional diagnostics, F3 status information, timestamped input log and replayable DTM data | Logs prove event transmission, but each gameplay action needs separate reaction checks |
| Audio | Playback to the Linux audio device; **156.7 seconds** matched a reference recording in latest reported comparison | Not proof that audio stays correct throughout the entire game |
| Repeatability | Over **10 runs** with matching images; **28,167 frames** unchanged in the optimization regression test | This is an internal reference comparison, not all-frame Dolphin equivalence |
| GPU texture copies | **7,686 texture results** compared against CPU decoding with no mismatches | Limited to formats and scenes exercised |
| Wii/Dolphin comparison | Sampled RVZ/GX FIFO comparisons remain valid; known memory differences are **135 bytes** at IPC #4 and **387 bytes** at IPC #35 | Memory discrepancies remain unresolved |
| Windows | Shared platform groundwork and Windows-specific code written | **Not compiled or tested on Windows** |

## Performance: training-room benchmark

The reported test uses the deterministic training-room sequence from **438 to 470 emulated seconds**, approximately 1,918 simulated frames.

| Optimization stage | Mean frame time | P99 | Emulation speed |
| --- | ---: | ---: | ---: |
| Before recent optimization phase | **45.27 ms** | **58.70 ms** | **0.369×** |
| GPU-side copy-to-texture | 38.0 ms | 45.4 ms | 0.438× |
| Reuse of transformed vertices | 31.3–32.5 ms | 39.5 ms | 0.514–0.533× |
| Removed redundant software clear | 30.24 ms | 35.61 ms | 0.552× |
| Parallel final-frame conversion | **28.98 ms** | **35.13 ms** (P95: 33.14 ms) | **0.576×** |

For this sequence, normal speed corresponds to about **16.67 ms per frame** (60 frames per emulated second). The remaining average gap is approximately **12.3 ms per frame**. The latest number is roughly a **56% throughput improvement** over the 0.369× starting point, **not an overall project-completion percentage**.

### What made it faster

- **GPU copy-to-texture:** a compute shader prepares textures used immediately by the game, avoiding many costly CPU/GPU round trips. The reported wait for those texture copies fell from **16.3 to 2.1 seconds** over the benchmark.
- **Vertex reuse:** a vertex referenced repeatedly by a primitive is transformed once instead of being recalculated for each triangle-strip occurrence. The reported vertex-processing total fell from **30.9 to 19.1 seconds**.
- **Removed software-only image clear:** the Vulkan path no longer performs an unused clear operation.
- **Four-thread final-image conversion:** distributes independent image lines while preserving byte-identical output.
- **Multi-stage Vulkan pipeline:** rendering and copy operations are separated where safe, preserving the observable ordering required by the Wii game.

The reported **GPU work is approximately 6 ms per frame**, while geometry preparation and synchronization still dominate the overall frame time. Simply buying a faster GPU would not necessarily resolve this.

### Graphical accuracy

The optimization checks retained **28,167 identical frames against the established internal baseline** and **7,686 identical GPU-produced texture results compared with CPU decoding**.

In a separate Vulkan/software comparison, **29 fade-transition frames still differ**; this was a known issue before the optimization phase. Additional implementation work is needed for **indirect texture effects, fog, mipmaps, blending/dithering order and constant destination alpha**.

These figures must not be interpreted as proof of complete image equivalence with Dolphin across the game.

### Audio, stability and timing

- **156.7 seconds** of audio were reported identical to a reference capture; the report does not establish full-game audio correctness.
- More than **ten** runs produced identical images at the tested points.
- Memory snapshots from repeated native runs matched at the selected checkpoints, but **Dolphin comparison differences (135/387 bytes) remain open**.
- The final report states that the application exited cleanly during its repeated tests.
- Vulkan validation layers were **not run**, as they were not installed in that environment.
- The most recent optimization session **did not repeat live window/gamepad testing**. Those features were exercised in an earlier reported session.

## Current priorities

1. Profile the current path again; reduce CPU-side vertex/lighting costs and remaining image-copy overhead without breaking determinism.
2. Achieve **1.0× normal speed in the training room**, followed by frame-time stability checks.
3. Verify missing GX features and compare across further scenes.
4. Repeat **hands-on Xbox gameplay tests** once performance is reliable, including actions beyond movement and firing.
5. Support the required Wii Remote gestures through keyboard/mouse and gamepad, and eventually test a **physical Wii Remote with DolphinBar**.
6. Expand scene and save-game validation; compile and test Windows when the supported toolchain is available.

A reported full or 100% PAL save may become a useful private test fixture, but its contents, import and behavior have **not yet been verified** and are not part of the validated results above.

## Publication boundary

**Documentation only.** No game assets, disc images, translated Nintendo code, private captures, encryption keys or playable binaries are published. Results are summarized from private development reports, not independently rerun for this GitHub update. A future distribution requires a rights and open-source licence compliance review.
