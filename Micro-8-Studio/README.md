# Micro-8 Studio

**Code. Debug. Play.**

Micro-8 Studio is the development environment for the **Micro-8** console (Franck Sauer): editor, static
analyzer for the Lofi language, debugger (breakpoints, step), hardware-accurate emulator (video, FM sound and
digital synth, tiles, sprites, raster, mouse, gamepad, MIDI), and dedicated tools (music tracker, sprite / tile /
screen / sound / image editors).

Studio is dedicated to the Micro-8, and it can also serve as a development toolbox for other consoles and other
projects (music, image converter, sprites, tile maps, icons), so that one program replaces several tools:

- the **sprite editor** exports a PNG sheet with transparency and its JSON atlas (the TexturePacker and Aseprite
  format), the same sheet cut into sprites for Unity (`.png` + `.png.meta`), or a C header;
- the **tile map editor** exports the map as text, CSV, JSON for the Tiled editor (with the tiles as a PNG sheet) or a
  C header, and reads text, CSV and Tiled maps back;
- the **tracker** exports a song as WAV, WAV stems (one file a track), MIDI, note events (JSON, CSV) or a VGM of its FM
  tracks, with a guide to port it to another system;
- the **image converter** and the **Paint** tool write PNG, GIF, TGA, PCX, IFF, C64 Koala, ZX Spectrum, tiles for the NES,
  Game Boy, SNES, Mega Drive and Master System, and ICO icons in the official sizes; Paint has layers;
- sprite sheets and maps from Aseprite, Unity, Godot and Tiled can be imported;
- the **code editor** also opens Python, C, C++, assembly, JSON and text files, with their own colours;
- the **Assets panel** lists every file a program or a project uses and opens each one in its editor.

Studio embeds an **AI help system**: the AI Assistant answers from the Micro-8 manual and can work in Studio (writing always waits for
your OK). **No telemetry:** Studio sends nothing about you or your work, and everything stays on your computer, unless you choose a
remote AI service yourself, Mistral (under the GDPR) or Claude. Then each question sends the question, the manual sections found for
it, the recent conversation and the code of the open file, and, with the Studio and Web boxes ticked (the default), what the assistant
reads in Studio and the provider's web search. With a local model (llama.cpp) nothing leaves your computer.

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
3c019a89ca903e6f72e546e24933402e59ec5af43e8ea796da3479a6c3d5d324  Micro8Studio-1.3.0-win64-portable.zip
5a9802a7b9597590c655cb69cfaa21a6550c8e38964816ba191885db062f7994  Micro8Studio-1.3.0-Setup.exe
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
