# Micro-8 Studio

**Code. Debug. Play.**

Micro-8 Studio is the development environment for the **Micro-8** console (Franck Sauer): editor, static
analyzer for the Lofi language, debugger (breakpoints, step), hardware-accurate emulator (video, FM sound and
digital synth, tiles, sprites, raster, mouse, gamepad, MIDI), and dedicated tools (music tracker, sprite / tile /
screen / sound / image editors).

This repository distributes the **compiled** application.

## Download

Binaries are distributed as [GitHub Releases](https://github.com/bsm3d/Micro-8/releases/tag/v1.0.0) of this
repository (this folder only holds documentation).

| File | Use |
|---|---|
| [`Micro8Studio-1.0.0-Setup.exe`](https://github.com/bsm3d/Micro-8/releases/download/v1.0.0/Micro8Studio-1.0.0-Setup.exe) | Windows 64-bit installer (recommended). No admin rights required. |
| [`Micro8Studio-1.0.0-win64-portable.zip`](https://github.com/bsm3d/Micro-8/releases/download/v1.0.0/Micro8Studio-1.0.0-win64-portable.zip) | Portable version: unzip and run, no installation. |

No Python installation or other dependency is needed: everything is included.

### SHA-256 checksums

```
7b29a1f4def94c72cb14ffb567b0847a1a9ae225495bd79f5ac89a0f85d1134a  Micro8Studio-1.0.0-win64-portable.zip
6b26167de5abdaf09b5e6055b7a651d95a10d675fbfafc61314a2240e44576f6  Micro8Studio-1.0.0-Setup.exe
```

## Documentation

- [`docs/Studio_README_en.md`](docs/Studio_README_en.md): the Micro-8 Studio manual (editor, debugger, emulator,
  tools).
- [`docs/Micro-8_User_Manual.md`](docs/Micro-8_User_Manual.md): the reference manual for the Micro-8 console
  (Lofi language, API, hardware).
- [`docs/Tracker_README.md`](docs/Tracker_README.md): the Micro-8 music tracker bundled in Studio, its project
  format and programs.

The [`Tracker/`](../Tracker) and [`SidRush/`](../SidRush) folders of this repository hold the corresponding
Lofi sample programs.

## License

(c) Benoit Saint-Moulin ([www.bsm3d.com](https://www.bsm3d.com)). All rights reserved.

Includes [Nuked-OPN2](https://github.com/nukeykt/Nuked-OPN2) (LGPL 2.1, (c) Nuke.YKT) for exact YM2612
emulation.

The Micro-8 console's are the property of Franck Sauer.
