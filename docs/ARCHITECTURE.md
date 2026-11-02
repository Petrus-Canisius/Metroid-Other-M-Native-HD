# How Metroid: Other M Native HD works

[Project overview](../README.md) · [Current progress](BUILD_STATUS.md) · [Roadmap](ROADMAP.md)

## A native adaptation, not a bundled emulator

The Wii runs a PowerPC program and uses dedicated graphics, audio and operating-system services. A modern PC is very different. This project **translates the game program ahead of time** and provides the Wii functions it needs through a native PC runtime.

**Dolphin is a reference for compatibility tests**, not the final launcher. Some Dolphin open-source components have been adapted within the private implementation and remain subject to their respective licences.

```text
Player's own Wii game copy (kept locally)
               |
        Recompilation pipeline
               |
      Native PC game execution
               |
   Wii runtime, clock and memory
       /          |          \
  PC controls   Audio    GX graphics
       |          |          |
 Virtual remote  Output  Vulkan rendering
               |
        Linux window today
     Windows build in the future
```

## What already runs

**Game execution.** The latest reports show **32,282 functions** translated from the Wii program, with **753/753 translator tests passing**. The native program reaches the menus, opening cinematic and a training-room scene.

**Wii runtime.** Disc access, emulated time, memory, IOS-related services and a virtual Wii Remote provide the environment the game expects. This is not yet proven for every scene or hardware behavior.

**Player controls.** The Linux application uses SDL3 to accept real keyboard/mouse and gamepad input. In one physical Xbox Series X controller session, recorded inputs were fed back into the virtual Wii Remote. Comparing the replay with a no-input run confirmed **movement and beam firing by Samus**. Other actions have not yet been demonstrated. A future goal is to translate every required physical Wii Remote gesture to regular PC inputs and support an optional real Wii Remote through DolphinBar.

**Graphics and window.** A software GX renderer is retained to check fidelity. A **Vulkan GPU renderer** now draws into a real SDL3 window and handles batches of graphics commands. Recent optimizations avoid repeated vertex transformation, generate certain copy textures on the GPU and parallelize final-image conversion. These changes improved the training-room benchmark to **0.576× normal speed**, but more speed and several Wii effects are still needed.

**Sound.** Audio plays through the Linux audio device. A recent reported comparison found **156.7 seconds** identical to a reference recording in the tested sequence. That does not establish complete-game audio fidelity.

**Reliability.** Repeated native executions are reproducible at the tested points. **28,167 internal test frames** remained unchanged through optimization; **7,686 GPU-made textures** matched CPU decoding. This protects against regressions, but **does not mean all game frames match Dolphin**. Memory differences of **135 and 387 bytes** remain at two Dolphin comparison checkpoints.

## Why performance is still limited

A modern GPU is much faster than Wii-era graphics hardware, but the original game uses particular patterns of image copying, memory access and synchronization.

The current bottleneck is not simply raw GPU drawing power. The runtime still spends too much time preparing geometry and lighting calculations, processing image copies and waiting for dependent operations.

The latest training test averages **28.98 ms per frame**, with approximately **16.67 ms per frame** needed for its original 60 Hz pace. Work remains on the CPU-side graphics preparation and related dependencies. Improvements must preserve the Wii's observable order of operations.

## What is still missing

The next goals are **stable 1.0× speed**, correctly implemented outstanding graphics effects, full gamepad/keyboard mappings, physical Wii Remote testing and validation of more scenes and saves.

A shared platform foundation has been developed, but **Windows-specific code has not yet been compiled or tested**. Linux is currently the only platform with reported interactive execution.

Future 1080p/1440p/4K, 16:9/21:9, configurable anti-aliasing, HDR10 and VRR are **not yet verified features**. Smooth gameplay and fidelity come first.

## Public code and licences

**This public repository is documentation only.** The private build, Nintendo-derived translated code, personal game files, private recordings, disc/NAND data and captures are not published. Any future distribution must satisfy applicable third-party licences, especially where source code has been adapted from Dolphin, and the project's other legal obligations.
