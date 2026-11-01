# Build status

[Home](../README.md) · [Development log](DEVELOPMENT_LOG.md)

**In Active Development · Not Playable Yet · No Public Release**

Evidence reviewed on 1 November 2026. This is a read only review of existing source, reports and logs. No compilation or test execution was started for this documentation update.

## Evidence levels

Generated means the translator emitted code. Compiled means that code produced native objects. Executed means a path ran in the harness. Validated means a specific comparison passed within stated limits. Playable requires a tested game experience. These levels are not interchangeable.

| Area | Evidence | Limit |
| --- | --- | --- |
| Latest translation | Completed log: 32,275 functions | Includes speculative function discovery. Does not prove complete game coverage |
| Translator suite | Existing run: 732 passed, 0 failed, 0 skipped | Translator tests, not 732 gameplay tests |
| Full native object compilation | Existing summary: 30,045 objects, 0 failures | Earlier generation. No equivalent complete result established for 32,275 functions |
| REL linking | Documented exact comparison with Dolphin, 8,017,604 bytes | Module linkage, not full execution correctness |
| Startup to first VI access | Existing MEM1 difference report: four bytes | Time base difference documented. No rendered frame is established |
| Interrupts and STM | Execution reports and runtime implementation | Early startup scope only |
| Linux fibers | Implementation present and harness reports one fiber | Inspected run reports zero thread switches. Multi-thread correctness is not established |
| NAND filesystem | Partial host file implementation present | Inspected execution log predates that implementation and stops at filesystem open |
| SYSCONF | Configuration service startup appears in the log | Successful SYSCONF file reading was not established by the inspected evidence |
| Wait loops | Analysis tool and scheduling integration present | Full coverage and correctness remain to be validated |
| Disc access | DI registers and IOS device skeleton present | IOS disc commands explicitly unimplemented |
| Graphics, audio and controls | Plans and architecture work | No verified playable integration |

## Unresolved observations

A later memory comparison at filesystem open reports **220 differing bytes**. Their causes are not resolved in the inspected evidence. The earlier first VI comparison must not be generalized to this later point.

The inspected boot log also reports unhandled special register writes defaulting to zero. Full CPU state fidelity is not established.

Some local summaries retain historical statements that conflict with newer sections or artifacts. This page uses the completed translation log for the latest generation count and keeps the older compilation result separate. Current filesystem source is newer than the inspected filesystem open run. Its existence is not proof of successful execution.

## Public build availability

This documentation repository contains no runtime source, generated translation, game data or binaries. There are no public build instructions or game releases. Windows remains a target. The inspected fiber implementation is Linux specific.
