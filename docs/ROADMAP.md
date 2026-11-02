# Roadmap

[Project overview](../README.md) · [Current progress](BUILD_STATUS.md) · [Development history](DEVELOPMENT_LOG.md)

The project is organized around reaching **a fully controllable native PC game** without compromising the original gameplay. There is no fixed release date or defensible overall completion percentage.

## From Wii game to a native PC experience

| Stage | Purpose | Current position |
| --- | --- | --- |
| **1. Rebuild the game program** | Convert the original Wii code into instructions a PC can execute | **32,282 functions translated; 753/753 translator tests passed** in the latest report |
| **2. Recreate Wii behavior** | Make the game's disc, sound, memory and virtual Wii Remote work inside the native program | Major startup and menu paths working in tests; some details still differ from the reference |
| **3. Make the game visible and audible** | Display real images, enter menus and play actual game audio | **Achieved in private tests**, with selected frames and sound samples matching Dolphin |
| **4. Reach real gameplay** | Display Samus and respond to game controls | **Scripted inputs move Samus in the training area**; live gameplay is not yet available |
| **5. Build the usable PC application** | Window, fast graphics, live keyboard/mouse and gamepad controls | **Next major milestone**; early Vulkan renderer exists but is not independent yet |
| **6. Make the experience better** | HD/4K, aspect ratios, optional anti-aliasing, HDR, VRR, optimizations and controller profiles | Planned, not implemented as player-ready options |
| **7. Support Windows and release** | Reuse the Linux code on Windows, add a launcher and local game import | Cross-platform groundwork begun; **Windows code not yet compiled or tested** |

## The next playable milestone

My immediate goal is a **Linux prototype where I can open a window, hear the game and control Samus myself**. To get there, development needs to:

1. Complete a standalone and more efficient **Vulkan** graphics renderer while keeping the slower software renderer for fidelity testing.
2. Show the game in a proper desktop window.
3. Read keyboard, mouse and gamepad inputs **live**, rather than only from a scripted sequence.
4. Map movement-based Wii Remote actions to ordinary PC controls, including horizontal/vertical orientation, pointer targeting, aiming, concentration and other gestures needed for progression.
5. Validate walking, camera response, actions, audio and stable execution in the training area.
6. Compare the resulting behavior with Dolphin and resolve important differences.

## Planned player experience

### Platforms and installation

- Linux and Windows 11 **native** versions, based on the same reusable code rather than two separate game ports.
- A simple launcher for graphics and input settings.
- Local setup using a player's **own copy of *Metroid: Other M***.
- No publicly downloadable game or announced release date.

### Controls

- Keyboard and mouse, conventional Xbox/PlayStation-style gamepads, optional physical Wii Remote support where practical.
- **No physical Wii Remote required:** pointing, movement gestures and orientation changes should be reproduced through configurable keyboard/mouse or controller actions.
- Remapping, pointer sensitivity, stick dead zones and accessible defaults for different keyboard layouts.
- These are **future interface requirements**; only scripted virtual Wii Remote tests are currently reported.

### Display and performance

- Faithful original visuals first, then 1080p, 1440p and 4K where feasible.
- Standard 16:9 and optionally 21:9 without stretching the image or breaking scripted scenes.
- **Configurable anti-aliasing**, including an Off option. FXAA and SMAA are candidates; MSAA support depends on the Vulkan renderer, and TAA remains exploratory.
- Optional HDR10 and VRR / G-SYNC / FreeSync for supported monitors and systems, with conventional SDR output retained.
- Smooth, stable performance; high-refresh targets such as 144 FPS are subject to preserving the game's original timing.

None of these enhanced display modes is currently validated or available to players. The existing Vulkan tests are about faithfully reproducing the Wii's original image, **not** about HDR, 4K or anti-aliasing.

## Quality and release requirements

Progress is checked through repeatable automated tests, comparisons against Dolphin and actual interactive gameplay trials. Selected matching screenshots and audio clips are not sufficient evidence for the full game.

Before any release, the project needs suitable game compatibility, controls, sound, graphics performance, installation/testing on Linux and Windows, and a review of third-party licence obligations. No game files or Nintendo assets are distributed from this repository.
