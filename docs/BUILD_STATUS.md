# Build status

[Home](../README.md) · [Development log](DEVELOPMENT_LOG.md) · [Roadmap](ROADMAP.md)

**In Active Development · Not Playable Yet · No Public Release**

I updated this page from my **1 November 2026 Claude Code console report**. Its reported builds and comparisons have not been independently reproduced for this documentation update. I have not performed a new runtime test here.

## Evidence levels

**Generated** means translation output exists. **Compiled** means native objects were built. **Executed** means code ran in the native development harness. **Validated** means a specific test or comparison passed within its stated limits. **Playable** requires a tested interactive game session. These stages are not interchangeable.

| Area | Latest reported result | Limit |
| --- | --- | --- |
| PowerPC translation | 32,275 functions retranslated; translator unit tests 732/732 | Translation coverage alone does not prove correct gameplay |
| Earlier compilation | 30,045 functions successfully compiled in an earlier generation | This historical milestone is separate from the later full retranslation |
| Native scheduling and SPR | Thread-switch comparisons, Dolphin-derived initial SPR values, HID0 translation fix | Timing and floating-point context differences remain |
| Graphics command processor | **GXInit passed**. 291 GX commands decoded and matched; 512 KiB FIFO compared byte-for-byte with Dolphin at IPC #35 | GX command processing is not graphical rendering |
| GX capture | Binary command capture and inspection tools implemented | Private captures are not published |
| Wait loops and interrupts | 103 polling loops instrumented, including 48 with side-effect-free calls; around 35,000 interrupts during a later boot run | Scheduling fidelity is not yet fully established |
| IOS filesystem / NAND | Permissions, directory handling, metadata and writes exercised; startup SYSCONF matched Dolphin byte-for-byte | Wider filesystem behavior not comprehensively validated |
| IOS disc / RVZ | Integrated read-only RVZ reader; DOL, FST and sampled reads from a 763 MiB file matched; disc timing model and partition reads exercised | All discs, offsets and timing cases have not been exhaustively verified |
| Bluetooth | HCI controller initialized with five registered virtual remotes and none connected; native startup progressed past initialization | Complete Wii Remote L2CAP/HID connection and physical controller support are not available |
| Game startup | **Past GXInit and onward to DSP AX microcode loading**, the current stop | No native audio output or playable session |
| GPU submissions | Around 1,290 frames submitted to the emulated GPU in one reported run | **Zero confirmed rendered images**; no graphics rendering stage exists yet |
| Display enhancements | HDR10, VRR, G-SYNC, FreeSync, 16:9 and 21:9 recorded as optional goals | None implemented or validated |

## Memory comparison boundaries

At the fourth IPC call following the main-thread resumption, my most recent report shows **223 different bytes in a 24 MiB MEM1 comparison**. Earlier observations of the same checkpoint varied between **141 and 233 bytes** across revisions. The differences were attributed to timestamps, FPU ownership/context and interrupt timing. Two executions of the same native binary reportedly differed only in timestamp fields (75 bytes).

These discrepancies are **classified but not resolved**. The host-clock-driven time model remains an identified source of nondeterminism. I do not claim full memory equivalence or complete scheduling fidelity.

At IPC #35, the GX commands and FIFO matched Dolphin. Full MEM1 snapshots captured at different points must not be presented as valid divergence measurements.

## Next technical step

The reported current stop is **loading the Wii AX audio microcode into the DSP**. My next engineering task is to implement and compare the necessary audio DSP/AX behavior, initially into a verifiable buffer without requiring actual sound output. Wii Remote connectivity and continued timing validation remain separate tasks.

## Public release status

I publish documentation and a fan-made banner here, not runtime source, translated game code, Nintendo assets, disc images, private NAND data or encryption keys. There is no playable release or launcher. I intend to publish a launcher through [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when it is ready. Linux is my current development environment, with Windows planned.
