# Micro-8 Studio

**Code. Debug. Play.**

Micro-8 Studio is the development environment for the **Micro-8** console (Franck Sauer): editor, static
analyzer for the Lofi language, debugger (breakpoints, step), hardware-accurate emulator (video, FM sound and
digital synth, tiles, sprites, raster, mouse, gamepad, MIDI), and dedicated tools (music tracker, sprite / tile /
screen / sound / image editors).

This repository distributes the **compiled** application.

## Download

Binaries are distributed as [GitHub Releases](https://github.com/bsm3d/Micro-8/releases/tag/v1.1.0) of this
repository (this folder only holds documentation).

**Micro-8 Studio 1.1.0 targets the Micro-8 console's OS 0.9.** If your console still runs OS 0.8, use
[release v1.0.0](https://github.com/bsm3d/Micro-8/releases/tag/v1.0.0) instead; the two are not interchangeable
(the Lofi compiler and emulator follow each OS version's own rules, and a program compiled for one OS version is
not guaranteed to run on the other's hardware).

| File | Use |
|---|---|
| [`Micro8Studio-1.1.0-Setup.exe`](https://github.com/bsm3d/Micro-8/releases/download/v1.1.0/Micro8Studio-1.1.0-Setup.exe) | Windows 64-bit installer (recommended). No admin rights required. |
| [`Micro8Studio-1.1.0-win64-portable.zip`](https://github.com/bsm3d/Micro-8/releases/download/v1.1.0/Micro8Studio-1.1.0-win64-portable.zip) | Portable version: unzip and run, no installation. |

No Python installation or other dependency is needed: everything is included.

### SHA-256 checksums

```
42ed97580159bb8dd908133ef1f2bb9baf5e36b41818fa1055e620410f4f2cf3  Micro8Studio-1.1.0-win64-portable.zip
55a8cf98e3d8d47a43b73801f4408068fa22db4e64d90851fb8142cd98e7a053  Micro8Studio-1.1.0-Setup.exe
```

## Documentation

- [`docs/Studio_README_en.md`](docs/Studio_README_en.md) / [`docs/Studio_README_fr.md`](docs/Studio_README_fr.md):
  the Micro-8 Studio manual (editor, debugger, emulator, tools), in English and French.
- [`docs/Micro-8_User_Manual.md`](docs/Micro-8_User_Manual.md): the reference manual for the Micro-8 console
  (Lofi language, API, hardware).

The [`Tracker/`](../Tracker) and [`SidRush/`](../SidRush) folders of this repository hold the corresponding
Lofi sample programs.

## License

(c) Benoit Saint-Moulin ([www.bsm3d.com](https://www.bsm3d.com)). All rights reserved.

Includes [Nuked-OPN2](https://github.com/nukeykt/Nuked-OPN2) (LGPL 2.1, (c) Nuke.YKT) for exact YM2612
emulation.

The Micro-8 console's are the property of Franck Sauer.
