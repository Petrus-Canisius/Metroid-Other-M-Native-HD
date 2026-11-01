# FAQ

[Home](../README.md) · [Roadmap](ROADMAP.md)

## Can I play it?

No. The project is not playable yet. There is no public game release or download.

## Is this a Dolphin launcher?

No. The project translates Wii PowerPC code ahead of time and compiles it for a host CPU. Dolphin is used as a development reference. The native runtime still supplies Wii hardware and service compatibility.

## Does translating 32,275 functions mean the game works?

No. Translation, compilation, execution and validation are separate milestones. Only a small startup path has the documented runtime comparisons. Some function discovery is speculative.

## Will I need the original game?

The installation goal requires a personal copy of the game. This repository provides no disc images, extracted files, Nintendo assets, private NAND data or keys. A future installer would process required inputs locally. Its design and distribution conditions still need review.

## Which platforms and features are supported?

My goal is to run *Metroid: Other M* natively on Windows and Linux. I'm currently developing and testing the native runtime on Linux. I plan to support 16:9 widescreen and 21:9 ultrawide displays with properly adapted visuals rather than stretching the image. Windows support, HD resolutions, higher frame rates, and keyboard, mouse and controller support are also planned. None of these display or gameplay features has been validated yet, and I haven't established minimum PC requirements. See my [roadmap](ROADMAP.md).

## When will it release?

I have not announced a release date. I develop this project in my spare time, so progress may take time. I intend to publish the launcher on this repository's [Releases page](https://github.com/Petrus-Canisius/Metroid-Other-M-Native-HD/releases) when it is ready. I have not published a launcher or playable build yet.

## Can I build or contribute code here?

I currently publish only the project's documentation here. I have not published the development source. Documentation corrections can identify the claim and supporting evidence without attaching private game files. Code contribution instructions will need a separate source publication decision.

## Licensing

This documentation does not grant rights to Nintendo software or assets. No blanket software license is assigned to this documentation repository by these pages.

The inspected WiiCompiled license is GNU GPL version 3. Its third party notices identify Aurora as MIT licensed. The local architecture record identifies DolRecomp as GPL version 3. Aurora and DolRecomp references do not mean their functionality is integrated or validated.

Dolphin's [licensing notice](https://github.com/dolphin-emu/dolphin/blob/master/COPYING) describes most original code as GPL version 2 or later, with per-file licenses and an aggregate compatible with GPL version 3. Local filesystem and disc runtime files explicitly identify Dolphin derived work. Using Dolphin as an external reference does not remove obligations for code actually ported from it.

Before any runtime source or binary distribution, review the exact revisions, per-file licenses, copyright notices and dependency inventory. Preserve required notices and license texts. Distribution of GPL covered binaries requires the corresponding source and other applicable GPL conditions to be fulfilled. This documentation review is not a completed distribution compliance audit. Consult the [GNU GPL version 3 text](https://www.gnu.org/licenses/gpl-3.0.html) for its terms.

The inspected architecture record excludes NWiiRecomp code because of additional noncommercial and royalty restrictions. No such code should be imported through public documentation work.

## Is Nintendo involved?

No. This is an independent project. It is not affiliated with, endorsed by, or approved by Nintendo.
