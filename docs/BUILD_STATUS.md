# Current development status

[Project overview](../README.md) · [Roadmap](ROADMAP.md) · [Development log](DEVELOPMENT_LOG.md)

**Updated: 2 November 2026 · Active development · No playable public build**

This page summarizes the latest **Claude Code report from my private project**. Its results have not been independently rerun for this GitHub documentation update. A feature that works in a scripted test is not necessarily ready for a player.

## At a glance

| Area | What is working or demonstrated | What remains |
| --- | --- | --- |
| Game reconstruction | **32,282 functions** translated; **753/753 automated translator tests** passed | Complete gameplay behavior and broader compatibility |
| Wii functionality | Startup, disc access, menus, sound processing, virtual Wii Remote and essential system services exercised | Remaining edge cases and hardware behaviors |
| Display and menus | Real game images; wrist-strap image matches Dolphin; OPTIONS, HOME and opening cinematic reached | Fully synchronized validation of every scene |
| **First gameplay interaction** | **Samus appears in the training area and moves in response to scripted inputs**; a run without those inputs stays still | **Live human control, gameplay validation against Dolphin, a playable window** |
| Sound | **20.58 seconds** of active audio identical to Dolphin after alignment | Longer-term sound and timing fidelity, audio output in a playable app |
| Internal timing | Two native executions now produced **identical 24 MiB memory snapshots at the measured points** | Remaining differences against Dolphin and timing-sensitive effects |
| Graphics acceleration | Initial **Vulkan rendering on an RTX 4090**; **767/796** tested wrist-strap frames matched the software reference exactly | Complete independent GPU renderer, correct fading, missing effects and real-time performance |
| Windows | Portable platform interface introduced; Windows implementation written | **No Windows compilation or execution has been completed** |
| Modern display options | Anti-aliasing levels, HD/4K, ultrawide, HDR10 and VRR are documented as goals | Not implemented as user features |

## What is new in the latest session

### 1. The game responds to controls

The native development program reaches the introduction and **a training scene with Samus, the HUD and subtitles**. To test whether the game actually responds, I compared two runs: one receiving scripted controller inputs and one without them. Both produced the same frames until the first test input at **450 seconds** of emulated time. Immediately afterwards the sequences diverged. In a later frame, Samus moved along a wall in the input-driven run, while she remained still in the other.

This is **evidence of input-driven movement in a game scene**, not yet proof that a person can play the game from start to finish. The exact meaning of each gameplay button still needs verification.

### 2. Timing has become more reproducible

An emulated Wii clock now tracks the simulated execution instead of simply following the computer's clock. Repeated native runs gave **zero differing bytes at the specific 24 MiB memory checkpoints tested**, where the previous comparison varied by 75 bytes. This makes failures and animations easier to investigate.

Against Dolphin, the remaining memory differences fell from **227 to 135 bytes** at checkpoint IPC #4 and from **810 to 387 bytes** at IPC #35. They have **not disappeared**. Some animated effects still have a slightly different phase.

### 3. The first Vulkan renderer runs on the GPU

The earlier software renderer remains the reference because it can be compared precisely with Dolphin. The new Vulkan implementation is an **early GPU-based version**.

On the wrist-strap screen, **767 of 796 tested frames** were identical pixel-for-pixel to that software reference. The **29 remaining frames** were part of a fade, where blending and dithering still need work. The current GPU path reuses some processing by the software renderer and submits work inefficiently. It **is not yet the final independent or real-time graphics engine**.

### 4. Audio and tests improved

The active audio matched Dolphin for **20.58 seconds** sample-for-sample after alignment, up from around 17.1 seconds. The first active sound in the reported run began at **23.360 seconds**, close to Dolphin's **23.415 seconds**. This is not full-game audio validation.

The translator test suite grew to **753 passing tests out of 753**. The translated function count remained **32,282**; sampled RVZ reads and the previously validated GX FIFO comparison remained correct.

### 5. Windows portability has begun

A shared platform layer now separates part of the Linux-specific code from the common game runtime. Windows-specific code has been written, but **has not been compiled**, because the development environment lacked an authorized Windows compiler toolchain. A tool download was blocked by the development environment's permissions; it was **not bypassed**. Windows readiness therefore remains **unverified**.

The planned anti-aliasing choices are also documented: Off, FXAA, SMAA and MSAA subject to graphics and hardware compatibility. **None are implemented yet.**

## What is actually playable?

**Nothing has yet been released or verified as a normal playable PC application.**

The private test harness can now display scenes, produce sound and move Samus in response to **automated inputs**. However, it still lacks a finished desktop window, real-time keyboard/gamepad controls, a fully independent high-performance renderer and proof of a stable interactive session.

It would be inaccurate to call this a playable beta. The first realistic target is an **Ubuntu training-room prototype with direct controls and a game window**.

## Current priorities

1. Render the game in a desktop window and finish the **standalone Vulkan graphics path**.
2. Provide live keyboard, mouse and gamepad input.
3. Adapt **all essential Wii Remote physical actions**—orientation, motion, pointing, first-person aiming and concentration—so a standard controller or keyboard/mouse can complete the game.
4. Verify a true human-controlled training-room session with graphics and audio.
5. Preserve reproducible tests, resolve reference differences, then validate the same portable codebase on Windows.
6. Add optional graphics improvements only after the main gameplay and renderer are reliable.

## Trust and publication limits

These observations are drawn from a private development report and have **not** been independently reproduced during this documentation update. Full-game compatibility, Linux real-time playability, Windows execution, physical controllers, HDR, ultrawide, anti-aliasing and variable refresh rate **are not validated features**.

This repository contains only public documentation. There is no public launcher, runtime binary, game image or Nintendo asset.
