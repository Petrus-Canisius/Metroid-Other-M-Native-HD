# Frequently asked questions

[Project overview](../README.md) · [Current progress](BUILD_STATUS.md) · [Roadmap](ROADMAP.md)

## What is Metroid: Other M Native HD?

An independent fan effort to run the original Wii game **natively on a PC**, adapting the original program to modern hardware. The intention is to preserve the game's identity while offering practical PC controls and optional display improvements.

## Can I download or play it now?

**No.** There is no public launcher, demo or playable game build.

The private development environment now displays Samus in the training area and can move her using **scripted** inputs. However, a normal game window, real-time keyboard/gamepad controls and a stable human-controlled gameplay session have **not** been implemented or validated yet.

## Is it a Dolphin emulator packaged with a game?

**No.** The Wii program is translated for native PC execution, and the missing Wii services are supplied by a custom runtime. Dolphin is used extensively as a **reference for testing**, and parts of its open-source software renderer and other components have been adapted for use in development. This reuse carries licence obligations.

## What has actually been achieved?

The latest private development report, dated **2 November 2026**, records:

- **32,282 translated functions** and **753/753 translator tests** passing.
- Game startup, menus, intro, sound and a training scene working in scripted native tests.
- Real rendered images; the first wrist-strap-screen image matched Dolphin exactly.
- **Samus moving after scripted controller input**, confirmed by comparison with a no-input run.
- **20.58 seconds** of audio identical to Dolphin after synchronization.
- A more reproducible emulated Wii clock.
- An early **Vulkan renderer** on an RTX 4090, with **767/796** tested reference frames matching exactly.

These are **specific test results**, not evidence of a complete or playable game. The documentation update did not independently rerun the private tests.

## Can Samus already be controlled by a person?

**Not yet.** The current proof of movement uses preprogrammed inputs in the native development harness. Live keyboard, mouse and gamepad input, plus a game window, are part of the next development phase. Individual attacks and other gameplay actions still need to be identified and tested.

## Will I need a Wii Remote to play?

The intended answer is **no**. The future keyboard/mouse and conventional Xbox/PlayStation-style gamepad profiles should reproduce all essential Wii Remote actions, including pointing, aiming, changing the remote's orientation and movement-based interactions. Players should be able to complete the game without physical motion controls.

This is a **design requirement**, not a completed feature. Optional physical Wii Remote support may be investigated separately.

## What graphics improvements are planned?

The first objective is a faithful and efficient PC image. Later goals include **1080p, 1440p and 4K**, properly adapted **16:9 and 21:9** where possible, and optional **HDR10, VRR, G-SYNC and FreeSync** on supported systems.

**Adjustable anti-aliasing** is planned with a fully disabled mode. FXAA and SMAA are candidates; MSAA depends on the graphics pipeline, while TAA is only being studied. The exact presets and performance will be verified during future implementation.

These enhancements **are not available yet**. Higher refresh rates must not change the original game's speed or break its behavior.

## Does the Vulkan renderer work?

A **first experimental Vulkan path** works in the private development environment. It matched **767 of 796 frames** for one tested screen; some fade frames still differ. It is not a finished high-performance renderer: it still depends partly on software rendering and does not yet display the game in a usable desktop window.

## Is sound working?

In the latest report, the native audio matched Dolphin for **20.58 seconds** sample-for-sample after aligning the streams. This is a meaningful limited validation, not proof of correct sound throughout the entire game or working live speakers in a future PC application.

## Is Windows 11 supported?

**No, not yet.** Linux is the current development system. Shared platform code and initial Windows-specific functionality have been written, but the Windows code has **not been compiled or executed**. This is an explicit future target, not an available build.

## Will I need the original game?

The planned installer requires a **personal copy of *Metroid: Other M***. This repository does not distribute a disc image, translated game code, Nintendo assets, encryption keys or other original game files. The details of the future local import process remain to be finalized.

## When will it be released?

There is **no announced date**. I intend to use the project's [GitHub Releases page](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when a launcher is ready. Development progress is not a release promise.

## Can I build or contribute the game code from this repository?

Not currently. This public repository contains **documentation only**. The native game runtime and private tests are not distributed here. Documentation corrections are possible without publishing copyrighted game files or private captures.

## Licensing

This documentation does not grant rights to Nintendo software or assets. No blanket software license is assigned to this documentation repository by these pages.

The inspected WiiCompiled license is GNU GPL version 3. Its third party notices identify Aurora as MIT licensed. The local architecture record identifies DolRecomp as GPL version 3. Aurora and DolRecomp references do not mean their functionality is integrated or validated.

Dolphin's [licensing notice](https://github.com/dolphin-emu/dolphin/blob/master/COPYING) describes most original code as GPL version 2 or later, with per-file licenses and an aggregate compatible with GPL version 3. Local filesystem and disc runtime files explicitly identify Dolphin derived work. Using Dolphin as an external reference does not remove obligations for code actually ported from it.

My latest private documentation inventory reports additional Dolphin-derived AX DSP code and BSD-licensed zstd components. That inventory is an implementation report, not a completed legal assessment. Before any runtime source or binary distribution, review the exact revisions, per-file licenses, copyright notices and dependency inventory. Preserve required notices and license texts. Distribution of GPL covered binaries requires the corresponding source and other applicable GPL conditions to be fulfilled. This documentation review is not a completed distribution compliance audit. Consult the [GNU GPL version 3 text](https://www.gnu.org/licenses/gpl-3.0.html) for its terms.

The inspected architecture record excludes NWiiRecomp code because of additional noncommercial and royalty restrictions. No such code should be imported through public documentation work.

## Is Nintendo involved?

No. This is an independent project. It is not affiliated with, endorsed by, or approved by Nintendo.
