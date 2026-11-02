# Frequently asked questions

[Project overview](../README.md) · [Current progress](BUILD_STATUS.md) · [Roadmap](ROADMAP.md)

## What is Metroid: Other M Native HD?

An independent project to run the original Wii game *Metroid: Other M* **natively on Linux and, eventually, Windows**. The game program is translated for PC execution, and the required Wii hardware/software behavior is reproduced by a dedicated runtime.

## Can I play it already?

**Not as a normal, full-speed PC game.** A **private interactive prototype** runs in a real Ubuntu window using SDL3 and Vulkan, with sound and Xbox controller input. However, the training-room benchmark currently runs at around **0.576× normal speed**. Many gameplay actions and the complete game have not yet been validated.

**There is no public executable, demo or release.**

## Is this just Dolphin with a launcher?

No. The goal is ahead-of-time translation of the Wii game's program to native PC code, alongside a Wii-compatible runtime. **Dolphin is used as a comparison reference**. The private project also adapts certain open-source components from Dolphin, which carries licensing obligations.

## Can Samus be controlled by a real gamepad?

A physical **Xbox Series X controller** was recognized in a private Ubuntu test. Its inputs were recorded, then replayed and compared with an identical no-input run. The result showed **Samus moving and firing in the training area**.

That is more than a scripted-only demonstration, but it is **not a complete validation of gamepad gameplay**. Jumping, Morph Ball, dodge, first-person aiming, missiles and concentration still need physical testing. The latest performance optimization session did not rerun that gamepad/window test.

## Will I need a Wii Remote or DolphinBar?

The intended finished game should be **fully playable with keyboard/mouse or a conventional Xbox/PlayStation-style gamepad**, without mandatory physical gestures. The plan is to convert Wii Remote orientation, movement and infrared pointing into PC-friendly control actions.

Support for a **real Wii Remote with Mayflash DolphinBar** remains a separate optional goal. Physical DolphinBar operation is **not yet implemented and validated**.

## Is Vulkan working, and what performance does it achieve?

Yes, an **experimental Vulkan backend renders into a real Linux desktop window**. It is increasingly GPU-driven, including GPU-side copy-to-texture conversions, but substantial CPU-side graphics work and missing Wii effects remain.

On the reported training-room benchmark, speed improved from **0.369× to 0.576×** normal game speed, and average processing time improved from **45.27 ms to 28.98 ms per frame**. The next objective is **stable 1.0×**, not simply more frames shown at an incorrect simulation speed.

## Have the graphics been verified?

On the latest internal optimization tests, **28,167 frames stayed identical to their established internal baseline**, and **7,686 GPU-produced copy textures** matched CPU decoding. This helps catch optimization regressions.

In a separate Vulkan/software renderer comparison, **29 fade frames still differ**. Other effects including indirect texturing, fog, mipmaps and some blending behavior are incomplete. Matching an internal baseline is **not the same as proving every frame equals Dolphin**.

## What about audio?

Sound plays through the Linux audio output. The latest private report says **156.7 seconds matched a reference audio capture** in the tested context. The tests do not establish that all of the game's music, effects and timing work perfectly in every scene.

## Does the entire game work?

**That has not been established.** The reported native runs reach the menus, intro and a training room. Later areas, save loading, progression, all abilities, and long interactive gameplay sessions still need testing.

A European PAL save advertised as 100% complete has been identified as a possible **future private test fixture**. Its actual contents, compatibility and native import have not yet been validated.

## Will it work on Windows 11?

Windows is a major target, but **not supported today**. A platform abstraction and some Windows-specific code have been written, but there is no reported Windows compilation or working Windows build.

## Will there be HD, 4K, ultrawide or HDR support?

These remain **planned, optional features**. The roadmap includes faithful 16:9, possible 21:9 without stretching, 1080p/1440p/4K, adjustable anti-aliasing (Off; FXAA/SMAA candidates; MSAA subject to feasibility), and optional HDR10/VRR/G-SYNC/FreeSync.

None has been released or validated as a user-facing feature. Preserving game speed, picture quality and compatibility comes first.

## What hardware will be required?

**No minimum system requirements have been measured and confirmed.** The private development benchmarks use a powerful RTX 4090 system, but that does not establish the requirements of a finished optimized build.

## Will I need the original game?

The planned local setup is designed around a player's **own copy of *Metroid: Other M***. No Wii game image, translated Nintendo code, assets, keys, private NAND data or saved-game files are distributed here.

## When will it be released?

There is **no announced release date**. I intend to use the project's [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when a launcher is ready. There is no public downloadable game today.

## Can I build the game or contribute code here?

This is currently a **documentation-only repository**. The private native runtime and its test fixtures are not published. Code build/contribution instructions would require a separate publication decision and a full licensing review.

## How trustworthy are the progress figures?

They summarize **private Claude Code development reports dated 2 November 2026**. They have not been independently reproduced as part of these public documentation edits. Different figures refer to different scopes: speed in one training scene, audio for a particular reference recording, and image equality for specific regression checks. See [current status](BUILD_STATUS.md).

## Licensing

This documentation does not grant rights to Nintendo software or assets. No blanket software license is assigned to this documentation repository by these pages.

The inspected WiiCompiled license is GNU GPL version 3. Its third party notices identify Aurora as MIT licensed. The local architecture record identifies DolRecomp as GPL version 3. Aurora and DolRecomp references do not mean their functionality is integrated or validated.

Dolphin's [licensing notice](https://github.com/dolphin-emu/dolphin/blob/master/COPYING) describes most original code as GPL version 2 or later, with per-file licenses and an aggregate compatible with GPL version 3. Local filesystem and disc runtime files explicitly identify Dolphin derived work. Using Dolphin as an external reference does not remove obligations for code actually ported from it.

My latest private documentation inventory reports additional Dolphin-derived AX DSP code and BSD-licensed zstd components. That inventory is an implementation report, not a completed legal assessment. Before any runtime source or binary distribution, review the exact revisions, per-file licenses, copyright notices and dependency inventory. Preserve required notices and license texts. Distribution of GPL covered binaries requires the corresponding source and other applicable GPL conditions to be fulfilled. This documentation review is not a completed distribution compliance audit. Consult the [GNU GPL version 3 text](https://www.gnu.org/licenses/gpl-3.0.html) for its terms.

The inspected architecture record excludes NWiiRecomp code because of additional noncommercial and royalty restrictions. No such code should be imported through public documentation work.

## Is Nintendo involved?

No. This is an independent project. It is not affiliated with, endorsed by, or approved by Nintendo.
