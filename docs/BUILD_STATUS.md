# Build status

[Home](../README.md) · [Development log](DEVELOPMENT_LOG.md) · [Roadmap](ROADMAP.md)

**In Active Development · Native Images Rendered · Not Playable Yet · No Public Release**

I updated this page from my **2 November 2026 Claude Code console report**. The technical tests below are **reported from the private development workspace**, not independently rerun during this public documentation update.

## Evidence levels

**Translated** means host-target code was generated. **Compiled** means host binaries or objects were built. **Executed** means a path ran in the native harness. **Validated** means a specific reference comparison passed under stated conditions. **Playable** requires a tested interactive gameplay sequence. These are separate levels.

## Latest results

| Area | Reported result | Validation boundary |
| --- | --- | --- |
| PowerPC translation | **32,282 functions** translated, including **7 newly identified indirect-entry functions**; **732/732** translator tests passed | Translation is not proof of complete game correctness |
| Earlier object build | 30,045 objects compiled in a previous generation | Historical result, not the current 32,282-function build count |
| GX / FIFO | Reference GX stream retained; FIFO at IPC #35 matches Dolphin across **524,256 bytes** | CPU-side command processing; independent of screen output |
| Software GX renderer | Dolphin-derived software pipeline adapted to the native runtime: vertices, rasterization, TEV, textures, EFB copies and XFB export | Reference renderer only, about **28 FPS** in the reported full-screen test |
| **First rendered game image** | Wii Remote wrist-strap warning: **614,400 of 614,400 XFB bytes identical to Dolphin**, with equivalent copy-filter settings | Exact match of that XFB image, **not** a validation of all scenes |
| Additional frames | Black transition and some animated intro frames matched Dolphin's XFB exactly | A scanline animation differs in phase due to timing |
| Wii Remote input | Scripted input and DTM recording/replay; OPTIONS navigation, settings changes, HOME/IR pointer and START-to-intro observed natively | Full menu interactions not yet reproduced against Dolphin |
| Audio DSP AX | After synchronization, **17.09 seconds** of active audio matched Dolphin sample-for-sample | Later audio differs due to a suspected timing mismatch, not yet proven |
| Disc and filesystem | Exact sampled RVZ reads, IOS/NAND and Bluetooth paths retained | Full-game and all-hardware behavior not demonstrated |
| Extended run after START | Loading screen then opening cinematic ran natively, with audio and EFB copies | No validated controllable gameplay sequence |
| Planned display technology | Vulkan accelerated renderer, 1080p/1440p/4K, 16:9/21:9, HDR10, VRR, G-SYNC and FreeSync | **Not implemented or validated**; advanced modes remain optional |

## Menu and image validation detail

The first wrist-strap screen is a genuine game image produced from the native GX command stream. The XFB result matched Dolphin byte-for-byte (614,400 bytes). A first difference of 68,439 bytes was investigated and resolved by aligning Dolphin's **DisableCopyFilter** setting to the intended native copy-filter behavior. Several additional frames matched exactly.

The native harness reached the **OPTIONS menu**. Scripted input changed language, subtitles and cursor position. **1/START** advanced to loading and the opening cinematic. **HOME** opened the system menu with an IR pointer. These latter interactions were **observed natively**, but Dolphin's slow software-renderer replay did not reach the same OPTIONS point for full comparison. This is not a claim of reference-validated gameplay.

The software renderer is a diagnostic reference. The current **direct Vulkan** approach is recommended as a future GPU backend, **not yet implemented**. The current software reference is too slow for the intended product.

## Runtime consistency and open issues

- **IPC #4, 24 MiB MEM1:** **227 bytes differ**, within previously investigated classes (timestamps, lazy FPU ownership and interrupt timing), but the mismatches **remain unresolved**.
- **IPC #35:** **810 bytes differ**, also still open.
- **GX FIFO at IPC #35:** identical reference bytes, a separate and successful comparison.
- **Audio:** 17.09 seconds identical after aligning the first nonzero sample. Subsequent divergence is suspected to arise from differing event times. The hypothesis has **not been demonstrated**.
- **Wii clock:** some simulation still depends on host elapsed time, so animation phases and input timing may differ between native and Dolphin.
- **GPU reads:** bounding-box and pixel-engine counter register behavior is incomplete and explicitly stops the test if used.
- **Other Wii Remote modes and extensions:** not fully ported.

Seven additional function entry points were detected and translated after a native indirect jump to `0x8013F4C8` prevented progression after START. The resulting **32,282** translated functions passed the reported regression suite.

## Next engineering milestone

My next priority is a **deterministic emulated Wii clock**, followed by synchronized native/Dolphin DTM playback and frame-by-frame menu comparisons. A **Vulkan GPU renderer** can then be built behind the existing graphics interface while keeping the software renderer as a fidelity reference. The first validated controllable gameplay sequence has not yet been reached.

## Publication boundary

This is a **documentation-only** repository. I do not publish translated Nintendo game code, game images or captures, private NAND/disc data, keys, source or binaries from the private runtime. The launcher is planned for [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases), but no public download or release date exists. Linux is the current development environment; Windows remains a target.

The software renderer includes Dolphin-derived components. Any future public binary/source distribution will require a comprehensive GPL and per-file license review.
