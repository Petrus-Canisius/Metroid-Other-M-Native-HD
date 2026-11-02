# Metroid: Other M Native HD

I am developing an **unofficial native PC adaptation of *Metroid: Other M***, the game originally released for Wii. The aim is to preserve the original adventure and make it run directly on a modern computer, with a more practical control scheme and optional graphics improvements.

This is an independent fan project, **not affiliated with Nintendo**. It is **in active development, not yet playable as a normal PC game, and not available for download**.

**English** | [Français](README.fr.md)

[Current progress](docs/BUILD_STATUS.md) · [Roadmap](docs/ROADMAP.md) · [Development history](docs/DEVELOPMENT_LOG.md) · [How it works](docs/ARCHITECTURE.md) · [FAQ](docs/FAQ.md)

## What I want to create

My goal is to give *Metroid: Other M* a genuine PC version rather than a packaged Wii emulator. The project translates the game's PowerPC instructions into code that a modern PC can run and rebuilds the Wii services the game expects.

The finished experience is intended to offer:

- **Native Windows and Linux versions**, with a dedicated launcher and local use of the player's own game copy.
- **Keyboard and mouse or conventional controllers**, including Xbox and PlayStation-style gamepads. Original Wii Remote movements, pointing and changes of orientation should be mapped to accessible PC controls so that a physical Wii Remote is **not required** to complete the game.
- **A flexible display**, starting with a faithful picture and eventually offering 1080p, 1440p and 4K, widescreen 16:9 and carefully adapted 21:9 where feasible.
- **Optional visual improvements**, including adjustable anti-aliasing (with an Off setting), HDR10 and variable refresh rate (G-SYNC/FreeSync where supported).
- **Respect for the original game**, including its pacing, gameplay, sound and atmosphere. Higher refresh rates are a goal only where they do not break the game.

These are **project objectives**, not features already available in a released build.

## Where development stands — 2 November 2026

**Samus now appears in the training area and responds to scripted controller inputs in the native development harness.** A controlled comparison showed that two runs produced the same frames until the first input was sent; afterwards, the input-driven run showed Samus moving while the no-input run did not.

This is an important step toward gameplay, but **it is not a playable PC build**. There is still **no normal game window or live keyboard/gamepad control**.

| Part of the project | Current result |
| --- | --- |
| Rebuilding the game | **32,282 translated functions**; **753/753 translator tests** passed in the latest reported session |
| Startup and navigation | Wii Remote emulation allows progress through warnings, menus, loading and the opening cinematic |
| First gameplay scene | Samus and the in-game display appear in the training area; movement caused by **scripted input** has been demonstrated |
| Graphics | A reference renderer produces genuine images and has matched Dolphin exactly on selected frames. An **early Vulkan GPU renderer** now works on the development PC |
| Sound | After aligning the recordings, **20.58 seconds** of active audio matched the Dolphin reference sample-for-sample |
| Reliability | A new emulated clock makes repeated native runs agree at the memory checkpoints tested; some differences with Dolphin remain |
| Windows | A cross-platform foundation and Windows-specific implementation have been written, but **the Windows version has not been compiled or tested** |

### What the Vulkan progress means

A first Vulkan rendering path is running on an NVIDIA RTX 4090. On the tested wrist-strap screen, **767 of 796 frames matched the reference exactly**; 29 fade-transition frames still differed. The GPU path still relies on some work from the slower software renderer. It is **not yet a complete, standalone, real-time rendering engine**.

The current images and gameplay experiments belong to the **private development environment**. I have deliberately not published game screenshots or Nintendo assets in this repository.

### What comes next

1. **Make Vulkan independent and fast enough** to display the game without relying on software rasterization.
2. **Open a real desktop window** and provide live keyboard/mouse and gamepad input.
3. **Adapt every essential Wii Remote action**—horizontal/vertical orientation, pointing, aiming, concentration and movement-based actions—to standard PC controls, with remapping options.
4. **Test a genuinely controllable training session**, with graphics, audio and responsive controls.
5. Continue checking visuals, sound and timing against Dolphin, then expand game coverage and prepare Windows support.

## How I check progress

I use **Dolphin as a reference for tests**, not as the final game engine. Matching certain images, audio samples and internal data helps verify the native implementation. I also run repeatable scripted-input tests to distinguish actual game responses from automatic animations.

The technical results above come from my **2 November 2026 Claude Code development report**. They have not been independently reproduced as part of this public documentation update. Passing tests and seeing Samus move under a script **does not prove that the entire game works**.

## Planned download and release

I intend to publish a launcher on this repository's [GitHub Releases page](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when it is ready. A future installation will require the player's **own copy of *Metroid: Other M***. I have not announced a release date or minimum PC specifications.

For the detailed progression and remaining limitations, see the [development log](docs/DEVELOPMENT_LOG.md), [roadmap](docs/ROADMAP.md) and [FAQ](docs/FAQ.md).

## Legal and distribution information

*Metroid: Other M Native HD* is an unofficial, independent fan project, not endorsed by Nintendo. Nintendo retains the rights to its games and trademarks.

**This public repository contains documentation only.** It does not provide a game image, translated game code, original assets, encryption keys, private game captures or a playable executable. Some private runtime components adapt code from open-source projects, including Dolphin; any future source or binary distribution must respect their applicable licences and corresponding-source obligations. See [licensing information](docs/FAQ.md#licensing).
