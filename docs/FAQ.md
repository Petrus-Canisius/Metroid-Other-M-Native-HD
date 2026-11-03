# Frequently asked questions

[Project overview](../README.md) · [Current progress](BUILD_STATUS.md) · [Roadmap](ROADMAP.md) · [Development history](DEVELOPMENT_LOG.md)

## What is Metroid: Other M Native HD?

An independent effort to make the original Wii game run **natively on PC**. The Wii's PowerPC program is translated and supported by a PC runtime that recreates essential memory, graphics, audio and input behavior. This is an **unofficial project**, not a Nintendo product.

## How far has the recompilation progressed?

The latest private reports record **32,282 translated PowerPC functions**, with **753/753 translator tests** passing. This is only a part of the work; full-game behavior has not been validated.

## Can I play the game now?

**There is no public build or downloadable playable version.** A private Ubuntu prototype opens a real SDL3/Vulkan game window, plays audio and reads a physical Xbox controller. Some actions work, and a training-room benchmark has **exceeded 1.0× on average**, but **stable 1.0× across the game and full-game compatibility remain unverified**.

## Can Samus already be controlled?

In an earlier physical Xbox Series X controller session, the inputs were recorded and replayed against a no-input run; Samus **moved and fired** in the training room. **Jump, Morph Ball, dodge, first-person aiming, missiles, concentration and complete menus/controls are not yet verified.** Later optimization sessions did not rerun full hands-on gameplay.

## How fast is the native prototype?

The **latest private fixed training-room benchmark exceeded normal simulation speed for the first time: 1.013× average**. A Vulkan alpha-test shader specialization produced **−0.52 ms/frame** across **six paired A/B comparisons**, with improved P99, matching tested frames, and a subsequently reported passing full regression suite. The shader avoids fragment `discard` **only when the emulated alpha test is guaranteed always to pass**.

Earlier, eight simultaneous Vulkan submission slots improved frame time by **−1.28 ms/frame** in six A/B pairs (approximately **0.978×** in that session). Clang/LLD renderer ThinLTO had previously saved **−0.58 ms/frame** in six paired measurements.

The tested 60 Hz simulation budget is about **16.67 ms/frame**. **1.013× is a mean speed in one scene, not a claim that every frame is smooth or that all game areas run at full speed**. The performance ratio is not the percentage of work completed. No public playable build is available.

## What are the most important graphics and CPU improvements?

The engine incorporates cached vertex and lighting calculations, GPU texture copies, two ordered transformation workers, parallel triangle assembly/clipping, TEV shader optimizations and bounded Vulkan image reuse. Recent A/B-tested improvements include **renderer ThinLTO (~−0.58 ms/frame)**, **eight in-flight Vulkan submission slots (~−1.28 ms/frame)**, and **conditional alpha-test shader specialization (~−0.52 ms/frame)**.

The alpha-test change is a shared Vulkan renderer optimization rather than a training-room-specific workaround. An additional TEV specialization experiment was still running in the latest shared progress log, with **no result reported yet**.

A temporary NVIDIA PowerMizer test **could not run under Wayland/Xwayland** because NV-CONTROL was unavailable. No GPU driver setting was changed. The renderer should remain portable across compatible Vulkan GPUs.

## Why does the game sometimes stutter at startup?

An isolated shader/pipeline-cache experiment measured **514 ms for the first frame with an empty cache**, versus **82 ms with a warm cache**. Pipeline creation itself took **436 ms cold** versus **1 ms warm**. This explains a previously observed ~520 ms startup spike in that test. It **does not prove that the game has no other stutters**, and shader preparation remains a future optimization task.

## Are the graphics fully accurate?

Not yet. **28,167 frames** matched the established **internal regression baseline** through the reported optimization tests. Some **7,687 GPU-produced texture results** were byte-identical to the CPU reference in their tested scope.

In a separate Vulkan/software comparison, **29 fade frames** still differed, with an observed **98.9016% pixel agreement** for that sample. Some Wii features, including indirect texturing, fog, mipmaps and blend/dither ordering, remain incomplete. **Internal regression identity is not the same as exact matching against Dolphin**.

## Is the audio working?

The Linux prototype produces sound. The latest reported reference comparison found **156.73 seconds** matching a particular reference capture. This does not establish full-game synchronization or correct sound effects in every scene.

## Is the project based on Dolphin?

Dolphin is used as an **external correctness reference** and some of its **open-source components have been adapted** into the private native runtime. The objective remains a **native translated game with a dedicated runtime**, not a repackaged emulator. Reused code must comply with its relevant open-source licences.

## Will I need a Wii Remote or DolphinBar?

The intended release should be completable with **keyboard/mouse or a conventional Xbox/PlayStation-style controller**, by mapping the necessary Wii Remote orientation, pointer and motion actions to regular controls.

An **original Wii Remote with Mayflash DolphinBar** is an optional future goal. It has **not been implemented and tested** as a completed feature.

## Is a 100% PAL save available for tests?

A private **European R3OP Wii SD save export** has been identified. The `data.bin` file is **113,408 bytes**, and metadata indicates **three encrypted save slots**.

The advertised **100% completion has not been verified**; neither import into an isolated Dolphin profile nor native loading has been completed. It is intended to help test later scenes **once the core engine is sufficiently stable**. The original save remains private and untouched.

## Is all of Metroid: Other M working?

**No full-game compatibility claim is justified.** Native execution has reached menus, introduction and the training room, with only selected controls proven. Further rooms, bosses, cutscenes, inventory operations, saves, game progression and long gameplay sessions require validation.

## Is Windows 11 supported?

Not currently. The project has a portable framework and some Windows-specific source, but **no compiled or tested Windows build** has been reported.

## Will there be 1080p, 4K, ultrawide, HDR and anti-aliasing?

These are **future goals**, not current features: 1080p/1440p/4K, carefully adapted 16:9 and possibly 21:9, optional anti-aliasing (Off, FXAA, SMAA and MSAA if feasible), HDR10 and VRR/G-SYNC/FreeSync. Fidelity and stable normal-speed gameplay come first.

## What computer hardware will I need?

**Full minimum hardware requirements have not been determined.** The official CPU instruction-set baseline is **x86-64-v3** (including AVX2), but that alone does not define a tested performance requirement. Development measurements use an Intel i7-14700KF and RTX 4090, so compatibility or performance on other systems is unverified. The Vulkan renderer is intended to support compatible NVIDIA and AMD GPUs without requiring NVIDIA-specific settings.

## Will I need to own the game?

The eventual local setup is intended to use a player's **own copy of the Wii game**. This repository distributes **no game image, copyrighted assets, decrypted save, encryption keys or translated Nintendo game code**.

## Can I build or contribute to the runtime?

The **public repository contains documentation only**; the runtime is private. There is no downloadable build or public source checkout for the game. Public code release would need an explicit decision and a detailed open-source licence review.

## When will it be released?

There is **no release date**. [GitHub Releases](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) may be used for a future launcher if a lawful and sufficiently tested distribution becomes ready.

## How are the development claims checked?

The figures summarize **private Claude Code reports**, not independent verification of a public binary. Each statement is scoped: training-room speed is not full-game speed, an internal image regression is not whole-game Dolphin equivalence, and a selected audio capture is not all audio. Some experiments have reproducible **paired A/B code improvements**, while between-session clock and workload differences can still change absolute results. A read-only GPU diagnostic found low-power NVIDIA P8 behavior, but its causal effect has not yet been established. See [Current development status](BUILD_STATUS.md) for measured details and uncertainty.

## Licensing

This documentation does not grant rights to Nintendo software or assets. No blanket software license is assigned to this documentation repository by these pages.

The inspected WiiCompiled license is GNU GPL version 3. Its third party notices identify Aurora as MIT licensed. The local architecture record identifies DolRecomp as GPL version 3. Aurora and DolRecomp references do not mean their functionality is integrated or validated.

Dolphin's [licensing notice](https://github.com/dolphin-emu/dolphin/blob/master/COPYING) describes most original code as GPL version 2 or later, with per-file licenses and an aggregate compatible with GPL version 3. Local filesystem and disc runtime files explicitly identify Dolphin derived work. Using Dolphin as an external reference does not remove obligations for code actually ported from it.

My latest private documentation inventory reports additional Dolphin-derived AX DSP code and BSD-licensed zstd components. That inventory is an implementation report, not a completed legal assessment. Before any runtime source or binary distribution, review the exact revisions, per-file licenses, copyright notices and dependency inventory. Preserve required notices and license texts. Distribution of GPL covered binaries requires the corresponding source and other applicable GPL conditions to be fulfilled. This documentation review is not a completed distribution compliance audit. Consult the [GNU GPL version 3 text](https://www.gnu.org/licenses/gpl-3.0.html) for its terms.

The inspected architecture record excludes NWiiRecomp code because of additional noncommercial and royalty restrictions. No such code should be imported through public documentation work.

## Is Nintendo involved?

No. This is an independent project. It is not affiliated with, endorsed by, or approved by Nintendo.
