# Development log

[Home](../README.md) · [Status](BUILD_STATUS.md) · [Update procedure](MAINTENANCE.md)

I use this log to record significant milestones and the limits of the supporting evidence. I do not include raw game data or private diagnostic artifacts.

## 2026-11-01: GXInit passed; DSP AX microcode is the next stop

My later Claude Code console report records native startup **passing GXInit** after implementing CPU-side GX gather-pipe, CP/PE registers, FIFO processing, command decoding and capture. At the 35th IPC call, **291 GX commands** and the **512 KiB FIFO** matched the Dolphin reference. An inspection tool supports private GX capture analysis. **No rendering stage is implemented**, so a command or frame submitted to the emulated GPU is not a picture on screen.

The reported updated toolchain retranslated **32,275 PowerPC functions** and passed **732/732 translator tests** after correcting direct HID0/HID1/HID2 register writes to preserve hardware semantics. Initial SPR values were compared against the reference. The NAND SYSCONF initialization output matched Dolphin byte-for-byte.

The native harness added a read-only RVZ reader, IOS disc timing and partition reads, filesystem directory and write support, Bluetooth HCI initialization without a connected Wiimote, and decrementer exception handling. **103 wait loops** are instrumented, including 48 with side-effect-free calls. A later boot run reported approximately **35,000 interrupts**, **96,982 drawing commands** and **1,290 frames submitted to the emulated GPU**. No frame was displayed.

A 24 MiB reference snapshot at the fourth IPC call after main-thread resumption had **223 differing bytes** in the most recent run. Across revisions the count ranged from 141 to 233. The known timing/FPU/interrupt-context classes explain the observations, but the discrepancies **remain unresolved**.

The reported current stop is at the **AX Wii audio DSP microcode load**. Audio processing is the next implementation priority. HDR10, VRR, G-SYNC, FreeSync and ultrawide support remain documentation-only goals.

**Evidence boundary:** these are results stated in the provided development console transcript. I did not rerun the binaries or independently compare private memory captures for the public update.

## 2026-11-01: NAND configuration and scheduling investigation (earlier milestone)

In a later console log from my project, I recorded native startup reaching and reading `/shared2/sys/SYSCONF` through a partial IOS filesystem proxy. The same log reports 31 delivered interrupts at that point.

I am using a wait-loop analysis tool to identify short memory-polling loops that depend on asynchronous hardware events. The tool reported **55 candidate loops**, including loops involved in DVD initialization. Safe-point integration was underway.

The remaining work includes matching the native and Dolphin IPC capture points, validating asynchronous event delivery, and implementing the IOS disc interface. Retranslation and a reference capture were still running at the end of the available log.

**Evidence boundary:** these observations come from my in-progress project log, not from a new independent reproduction. They do not establish a working disc path, validated thread switches, native rendering or playability.

## 2026-11-01: Translation and early native startup (earlier milestone)

The completed translation log reports **32,275 generated functions** and **732 passing translator tests**. An earlier generation has a successful native object compilation report for **30,045 functions**. The completed-build result must not be transferred to the newer generation.

Early Wii startup, interrupts, event scheduling and IOS communication have been exercised. At the first VI access, a previous MEM1 comparison with Dolphin differed in four time-base bytes.

An earlier filesystem IPC comparison reported 220 different memory bytes. Subsequent debugging found that the reference snapshots may have been taken at different points in the IPC sequence. The mismatch is therefore not yet a validated divergence.

## 2026-10-31: Executable analysis foundation

Initial DOL and REL parsing, relocation, integrity checks and function discovery established the basis for static translation.

## Evidence policy

Dates identify when an observation was reported. They do not imply a release schedule. The [build status](BUILD_STATUS.md) separates generated, compiled, executed and validated results. I review new entries carefully before publishing them.
