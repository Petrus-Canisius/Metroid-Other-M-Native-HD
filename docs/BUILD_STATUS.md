# Build status

[Home](../README.md) · [Development log](DEVELOPMENT_LOG.md)

**In Active Development · Not Playable Yet · No Public Release**

Evidence reviewed on **1 November 2026**. This update combines previously reviewed build reports with a later developer console log. The newer observations are explicitly marked as reported rather than independently reproduced. No compilation or runtime test was executed for this public documentation update.

## Evidence levels

**Generated** means translation output exists. **Compiled** means native objects were produced. **Executed** means a path ran in the harness. **Validated** means a specific comparison passed within stated limits. **Playable** requires a tested game experience. These levels are not interchangeable.

| Area | Evidence | Limitation |
| --- | --- | --- |
| PowerPC translation | Completed log: 32,275 generated functions | Some discovery is speculative; not complete-game proof |
| Translator suite | Existing report: 732 passed, 0 failed, 0 skipped | Translator tests, not gameplay tests |
| Native object compilation | Earlier completed report: 30,045 objects, no failures | No matching full-build report verified for 32,275 functions |
| REL linking | Previously documented comparison with Dolphin: 8,017,604 bytes | Module linkage does not prove runtime correctness |
| Early Wii startup | First VI-access MEM1 comparison previously differed by four time-base bytes | No actual gameplay frame was rendered |
| Interrupts and IPC | Early runs delivered external interrupts and exercised IOS communication | Wider scheduling and event-order correctness are unresolved |
| Linux fibers | Implementation and one fiber reported in earlier run | No verified thread switch in the cited earlier run |
| NAND / SYSCONF | Later 1 November developer log reports that `/shared2/sys/SYSCONF` was read and 31 interrupts were reached | Reported progress, not independently reproduced or fully reference-validated here |
| Wait-loop instrumentation | Developer log reports discovery of 55 short polling loops and integration of safe points | Retranslation and Dolphin comparison were still running at the log cutoff |
| IOS disc interface | DI investigation and a file-backed read strategy were being developed | Complete disc reads and timing were not demonstrated |
| Graphics, audio and gameplay | Architecture and product plans exist | No verified native gameplay rendering or playable build |

## Open validation questions

An earlier memory comparison around filesystem IPC reported **220 differing bytes**. Later debugging identified a problem in aligning the reference capture points. That comparison cannot establish a valid state divergence until both sides are captured at equivalent execution points.

A prior boot trace also reported unsupported PowerPC special-register writes. Full processor-state fidelity has not been established.

The recent log reports additional NAND and timing work, but its final operation was a concurrent retranslation and reference capture. This document does not infer those operations completed successfully.

## Public release status

The repository contains documentation and a fan-made banner, not runtime source, translated game code, Nintendo assets or binaries. There is no game release or public installer. The launcher is planned for [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when ready. Linux is the current development environment; Windows remains a target.
