# GRAVITY

The tutorial project of the second Micro-8 Studio tutorial (on the [website](https://www.micro-8-studio.com/gravity.html)): a
platform game in seven worlds, built on the physics and entity blocks of the Micro-8 library. Code & Music: Benoit (BSM) Saint-Moulin.

![GRAVITY](img/gravity.png)

Arrows walk (and fly, in space), `Z` or Up jumps, `Enter` starts, `Esc` quits. Take the energy cells of a room, avoid the
spikes and the patrolling drones, then reach the exit. Each world has its own gravity: earth, moon, heavy, ice, springs, a
storm that changes the world, and space with none.

| File | What it is |
|---|---|
| `GRAVITY.SRC` | the finished program, with the Micro-8 library blocks it uses |
| `GRAVITY.SPR`, `.PLA` | the sprites and their palette |
| `GRAVITY.TLS`, `GRAVITY.PAL` | the tiles and their palette |
| `GRAVITY1.LVL` ... `GRAVITY7.LVL` | the 25 x 15 maps of the seven worlds |
| `GRAVITY1.LAY` ... `GRAVITY7.LAY` | the layers of each map: what is solid, where the cells, spikes, exit, start, springs and drones are |
| `GRAVITY.ENT` | the entity table of the drones (Studio generates their code from it) |
| `GRAVITY.SCR` | the text of the title screen |
| `GRAVITY.BMP`, `GRAVITYW.BMP`, `GRAVITYB.PAL` | the pictures of the title (GRAVITY) and of the victory (YOU DID IT), and their palette |
| `GRAVITY.M8S` | the music |

**In Micro-8 Studio**: the project is in `Examples/OS/BSMCODE/GRAVITY`; open `GRAVITY.SRC` and press F5.
**On the machine**: copy the folder to the SD card, then `cd GRAVITY`, `compile GRAVITY.SRC` and `run GRAVITY.BIN`.
