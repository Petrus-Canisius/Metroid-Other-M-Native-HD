# Roadmap

[Home](../README.md) · [Status](BUILD_STATUS.md) · [Development log](DEVELOPMENT_LOG.md)

I organize development by technical dependencies, without assigning overall completion percentages or release dates.

| Phase | Scope | Current position |
| --- | --- | --- |
| **1. Recompilation foundation** | DOL/REL analysis, static relocation, function discovery and PowerPC-to-C++ translation | 32,275 functions retranslated; 732 translator tests passed in the reported run. Coverage and correctness remain ongoing |
| **2. Wii runtime** | Memory, SPR, interrupts, IOS, NAND, fibers, Bluetooth and disc services | Active; significant paths exercised. Memory and timing mismatches remain under investigation |
| **3. Engine startup** | Reliable threading, IOS disc reads, GX command processing, DSP initialization and entry into further engine execution | **GXInit passed**; disc and Bluetooth initialization exercised; currently stopped at AX DSP microcode loading |
| **4. Graphics, audio and gameplay** | DSP AX handling, actual GX rendering, audio output, input, scenes and saves | DSP AX is the next task. GPU command submissions are observed, but no pixels are rendered |
| **5. HD quality and performance** | HDR10, VRR, 16:9 and 21:9, HD/4K output, frame pacing, shader preparation, optimization and regression coverage | Planned only. All advanced display modes must be optional with SDR fallback |
| **6. Installation and release** | Local import from personal game copy, Windows/Linux builds, launcher, updates and GitHub Releases | Planned; no public release |

## Technical sequence

1. Implement the AX audio DSP behavior sufficiently to advance the native boot, starting with a testable audio buffer.
2. Continue Wii Remote L2CAP/HID, exceptional-case and disc timing work as dependencies require.
3. Build and validate a native graphics rendering stage. First achieve reliable SDR output.
4. Design and validate widescreen/ultrawide, HDR and adaptive synchronization through the selected backend. Test VRR behavior on actual hardware.
5. Complete a usable gameplay path before packaging and releasing the launcher.

## Planned capabilities

I target Windows and Linux, keyboard and mouse, Xbox and PlayStation controllers and optional Wii Remote connectivity via Mayflash DolphinBar. Display goals include 1080p, 1440p, 4K, 16:9 and 21:9. HDR10, VRR, NVIDIA G-SYNC and AMD FreeSync are **optional plans**, dependent on supported GPUs, drivers, monitors and operating systems. Standard SDR 60 Hz 16:9 output must remain available.

Higher frame rates, possibly up to 144 FPS, require game timing validation and may not be feasible for every scene. Hardware requirements and release dates remain undetermined.

I plan to publish the launcher through this repository's [Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when ready. I develop this project in my spare time.
