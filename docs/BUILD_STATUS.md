# Build status

[Home](../README.md) · [Development log](DEVELOPMENT_LOG.md)

**In Active Development · Not Playable Yet · No Public Release**

I reviewed the available evidence on **1 November 2026**, combining earlier build reports with a more recent console log from my project. I identify the newer observations as reported rather than independently reproduced. I did not run a new build or runtime test for this documentation update.

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
| NAND / SYSCONF | My later 1 November project log reports reading `/shared2/sys/SYSCONF` and reaching 31 interrupts | Reported progress, not independently reproduced or fully reference-validated here |
| Wait-loop instrumentation | My project log reports the discovery of 55 short polling loops and work on safe-point integration | Retranslation and Dolphin comparison were still running at the log cutoff |
| IOS disc interface | DI investigation and a file-backed read strategy were being developed | Complete disc reads and timing were not demonstrated |
| Graphics, audio and gameplay | Architecture and product plans exist | No verified native gameplay rendering or playable build |

## Open validation questions

An earlier memory comparison around filesystem IPC reported **220 differing bytes**. Later debugging identified a problem in aligning the reference capture points. That comparison cannot establish a valid state divergence until both sides are captured at equivalent execution points.

A prior boot trace also reported unsupported PowerPC special-register writes. Full processor-state fidelity has not been established.

My recent log records further NAND and timing work, but ends while retranslation and reference capture are in progress. I do not claim those operations finished successfully.

## Public release status

I publish documentation and a fan-made banner here, not runtime source, translated game code, Nintendo assets or binaries. I have not released a game build or installer. I plan to publish the launcher through [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when ready. Linux is the current development environment; Windows remains a target.
