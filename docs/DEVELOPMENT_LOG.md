# Development log

[Home](../README.md) · [Status](BUILD_STATUS.md) · [Update procedure](MAINTENANCE.md)

This log summarizes reviewed milestones. It contains no raw game output, private data or automatic activity entries.

## 2026-11-01: Translation and early native startup

The latest completed translation log reports 32,275 functions. The accompanying translator run reports 732 passing tests, with no failures or skipped tests. A previous generation of 30,045 functions has a complete successful native object compilation report. The newer generation is not presented as fully compiled on that basis.

Native startup has been exercised through early Wii service initialization. Interrupt delivery, event scheduling and IOS STM communication are implemented and have execution evidence. MEM1 comparison at the first VI access differs in four time base bytes, as described in the local validation notes.

Further work includes Linux fibers, a partial NAND filesystem and wait loop analysis. The inspected filesystem open run reports one fiber and zero thread switches. A later memory comparison at that point has 220 differing bytes whose causes remain unresolved in the reviewed evidence. IOS disc commands are still unimplemented.

The rendering backend remains open. No native gameplay frame or playable build is verified.

## 2026-10-31: Executable analysis foundation

Initial DOL and REL parsing, relocation and integrity checks were documented. Analysis tools and tests established the foundation for function discovery and static translation.

## Evidence policy

Dates refer to the recorded milestone, not a promise of continuous publication. Each entry must distinguish implementation, execution and validation. Detailed evidence scope belongs in [build status](BUILD_STATUS.md). New entries follow the [maintenance procedure](MAINTENANCE.md).
