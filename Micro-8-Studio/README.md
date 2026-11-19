# Micro-8 Studio

**Code. Debug. Play.**

Micro-8 Studio is the development environment for the **Micro-8** console (Franck Sauer): editor, static
analyzer for the Lofi language, debugger (breakpoints, step), hardware-accurate emulator (video, FM sound and
digital synth, tiles, sprites, raster, mouse, gamepad, MIDI), and dedicated tools (music tracker, sprite / tile /
screen / sound / image editors).

This repository distributes the **compiled** application.

## Download

Binaries are distributed as [GitHub Releases](https://github.com/bsm3d/Micro-8/releases/tag/v1.3.0) of this
repository (this folder only holds documentation).

**Micro-8 Studio 1.3.0 targets the Micro-8 console's OS 0.9.** If your console still runs OS 0.8, use
[release v1.0.0](https://github.com/bsm3d/Micro-8/releases/tag/v1.0.0) instead; the two are not interchangeable
(the Lofi compiler and emulator follow each OS version's own rules, and a program compiled for one OS version is
not guaranteed to run on the other's hardware).

| File | Use |
|---|---|
| [`Micro8Studio-1.3.0-Setup.exe`](https://github.com/bsm3d/Micro-8/releases/download/v1.3.0/Micro8Studio-1.3.0-Setup.exe) | Windows 64-bit installer (recommended). No admin rights required. |
| [`Micro8Studio-1.3.0-win64-portable.zip`](https://github.com/bsm3d/Micro-8/releases/download/v1.3.0/Micro8Studio-1.3.0-win64-portable.zip) | Portable version: unzip and run, no installation. |

No Python installation or other dependency is needed: everything is included.

### SHA-256 checksums

```
a1e611615195d550ea92aaaf62fa172521d5a7c6a690767fb6c92bbf25e26d9c  Micro8Studio-1.3.0-win64-portable.zip
b0a91b5ca19e67e9c74a2cb3084d60b626a3024399afaabe7aba7b4993960ad6  Micro8Studio-1.3.0-Setup.exe
```

## Documentation

- [`docs/Studio_README_en.md`](docs/Studio_README_en.md) / [`docs/Studio_README_fr.md`](docs/Studio_README_fr.md):
  the Micro-8 Studio manual, in English and French: getting started, a complete tutorial (the DUNGEON HUNT game),
  then the reference for the editor, debugger, emulator and tools.

The Micro-8 User's Manual (the reference of the console: Lofi language, API, hardware) comes with Studio. For news about the
Micro-8, to follow its development or to buy one, go to the website of its designer, Franck Sauer:
[www.micro-8.com](https://www.micro-8.com).

The finished games of the two tutorials are in [`DungeonHunt/`](../DungeonHunt) and [`Gravity/`](../Gravity).

The [`Tracker/`](../Tracker) and [`SidRush/`](../SidRush) folders of this repository hold the corresponding
Lofi sample programs.

## License

© Benoit Saint-Moulin ([www.bsm3d.com](https://www.bsm3d.com)). All rights reserved. Free to use.

Includes [Nuked-OPN2](https://github.com/nukeykt/Nuked-OPN2) (LGPL 2.1, © Nuke.YKT) for exact YM2612
emulation.

The Micro-8 console is the property of Franck Sauer: [www.micro-8.com](https://www.micro-8.com).
