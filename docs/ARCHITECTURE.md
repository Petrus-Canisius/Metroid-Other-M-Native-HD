# How the project works

[Project overview](../README.md) · [Current progress](BUILD_STATUS.md) · [Roadmap](ROADMAP.md)

## Why this is a native port

*Metroid: Other M* was made for Wii hardware. PCs use a different processor and graphics environment. This project aims to **translate the game's program ahead of time** and provide the missing Wii functions directly inside a native PC application.

It is **not a Dolphin launcher**. Dolphin is used as a **reference** to check whether the PC version behaves like the original.

In simple terms:

```text
Personal copy of the Wii game (kept locally)
                |
       Analyze and translate
                |
      Native PC game program
                |
     Wii services and input
                |
  Graphics + sound + gameplay
                |
   Future Linux / Windows app
```

## What is working in the development environment

**Game program.** The latest report records **32,282 translated functions** and **753 passing translator tests**. The game executes far enough to display the introduction and a training scene.

**Wii services.** The runtime provides emulated versions of the system functions needed by the game, including disc reading, memory, sound, timekeeping and Wii Remote communication. These work on the tested paths, not necessarily across the entire game.

**Controls.** A virtual Wii Remote accepts scripted buttons and pointer/movement data. A controlled test showed Samus moving in the training area after an input was sent, whereas she remained still without it. Live player controls are the next step.

**Images.** A software renderer adapted from Dolphin generates images used as a trusted comparison. An **early Vulkan renderer** now uses the GPU to reproduce those images. It matched **767 of 796 frames** in one wrist-strap-screen test; some fade frames still differ. It is not a standalone high-speed renderer yet.

**Sound and timing.** The DSP audio implementation generated **20.58 seconds** of samples identical to the Dolphin reference after alignment. The newly simulated Wii clock makes two native runs produce identical memory at the test checkpoints; differences against Dolphin remain.

## What still needs to be built

The first user-facing prototype must combine all these pieces in a **Linux desktop window**, with responsive keyboard/mouse or gamepad controls, sound output and reliable graphics at the correct game speed.

An important input goal is to **replace mandatory Wii Remote gestures** with PC-friendly actions. For example, aiming with a pointer or switching how the Wii Remote is held should not require a physical motion controller. Actual mappings and gameplay behaviors need testing before they can be considered final.

The graphics renderer must eventually stop depending on the software reference for transformation/rasterization, handle the missing visual effects, and use the GPU efficiently.

## Two operating systems, one codebase

Linux is currently the main development system. A common platform layer has been introduced and Windows-specific code has been written, but the **Windows build has not yet been compiled or tested**. The aim is to reuse the same game translation and Wii logic on both systems, changing only the integration required by each OS.

## Planned visual enhancements

Once core gameplay is reliable, the Vulkan renderer should allow optional HD/4K output, widescreen/ultrawide presentation, adjustable anti-aliasing with an Off setting, and supported HDR/VRR display modes. These are **future goals**, not active features.

The original timing, composition and controls take precedence over increasing the frame rate or applying effects.

## Verification and release

The project uses repeatable controller scripts, reference comparisons with Dolphin, audio comparisons and memory tests. Only specific test cases have been validated so far; **there is no verified playable session or released game**.

All original Nintendo game data, private game captures, translated game code and binaries remain outside this documentation-only repository. Reused open-source code, especially Dolphin-derived components, must meet its licence and source-distribution requirements before any software is published.
