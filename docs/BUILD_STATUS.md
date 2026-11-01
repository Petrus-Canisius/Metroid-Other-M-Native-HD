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
| AX Wii audio DSP | AX HLE command processing, audio DMA, deferred DSP/ARAM interrupts and WAV capture implemented; 73,903 command frames at 96 voice slots per frame reported | Captured audio is silent; no active voice or real-sound sample comparison yet |
| Thread switching across interrupts | Context restore bug involving nonvolatile register guard reportedly corrected; 135,750 switches recorded in extended run | Full scheduling fidelity not yet established |
| Extended native execution | AX microcode loading passed; boot ran to the harness watchdog limit without a reported native crash | Likely waiting for a disconnected Wii Remote; interactive gameplay not demonstrated |
| GPU submissions | Around 1,290 frames submitted to the emulated GPU in one reported run | **Zero confirmed rendered images**; no graphics rendering stage exists yet |
| Display enhancements | HDR10, VRR, G-SYNC, FreeSync, 16:9 and 21:9 recorded as optional goals | None implemented or validated |

## Audio validation boundary

In my latest development report, the native harness captured **239 seconds of silent audio**. Dolphin, with the same no-Wii-Remote configuration, was also silent for **250 seconds**. The two observations are consistent, but **no active voice was observed** and no sample-by-sample comparison of actual game audio was possible. I do not claim the mixer or audible sound output is validated. The DSP AX command path is exercised, not equivalent to proven audio fidelity.

The same report states that the corrected thread-resume path completed **135,750 switches** and processed **73,903 AX command frames**, with no crash before the configured watchdog cutoff. These numbers are reported from a local development run, not independently reproduced in this documentation update.

## Memory comparison boundaries

At the fourth IPC call following the main-thread resumption, the latest report again shows **223 different bytes in a 24 MiB MEM1 comparison**. Earlier observations of the same checkpoint varied between **141 and 233 bytes** across revisions. The differences were attributed to timestamps, FPU ownership/context and interrupt timing. Two executions of the same native binary reportedly differed only in timestamp fields (75 bytes).

These discrepancies are **classified but not resolved**. The host-clock-driven time model remains an identified source of nondeterminism. I do not claim full memory equivalence or complete scheduling fidelity.

At IPC #35, the GX command stream still matched Dolphin. A later report lists **810 different MEM1 bytes**, compared with 881 in an earlier run; these mismatches remain open. Full MEM1 snapshots captured at different points must not be presented as valid divergence measurements.

## Next technical step

**The AX microcode block has been passed.** The next development dependency is emulated **Wii Remote connectivity**, including L2CAP/HID and input reports, so the boot can progress beyond the apparent no-controller waiting state. Audible AX mixing still needs an active-voice reference test. Graphics rendering, controller hardware support and full runtime timing validation are outstanding.

## Public release status

I publish documentation and a fan-made banner here, not runtime source, translated game code, Nintendo assets, disc images, private NAND data or encryption keys. There is no playable release or launcher. I intend to publish a launcher through [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when it is ready. Linux is my current development environment, with Windows planned.
