# Micro-8 Studio

**Code. Debug. Play.**

Micro-8 Studio est l'environnement de développement pour la console **Micro-8** (Franck Sauer) : éditeur,
analyseur statique du langage Lofi, débogueur (points d'arrêt, pas à pas), émulateur fidèle au matériel
(vidéo, son FM/synthé numérique, tuiles, sprites, raster, souris, manette, MIDI), et outils dédiés (tracker de
musique, éditeurs de sprites/tuiles/écrans/sons/images).

Ce dépôt distribue l'application **compilée** : il ne contient pas son code source, les ROM de la console ni le
code du FPGA, qui restent la propriété de Franck Sauer et sont fournis avec son accord, chiffrés à l'intérieur
de l'application.

## Téléchargement

Les binaires sont distribués via les [Releases GitHub](https://github.com/bsm3d/Micro-8/releases/tag/v1.0.0)
de ce dépôt (ce dossier ne contient que la documentation et les exemples).

| Fichier | Usage |
|---|---|
| [`Micro8Studio-1.0.0-Setup.exe`](https://github.com/bsm3d/Micro-8/releases/download/v1.0.0/Micro8Studio-1.0.0-Setup.exe) | Installeur Windows 64 bits (recommandé). Aucun droit administrateur requis. |
| [`Micro8Studio-1.0.0-win64-portable.zip`](https://github.com/bsm3d/Micro-8/releases/download/v1.0.0/Micro8Studio-1.0.0-win64-portable.zip) | Version portable : décompresser et lancer, sans installation. |

Aucune installation de Python ni d'autre dépendance n'est nécessaire : tout est inclus.

### Empreintes SHA-256

```
7b29a1f4def94c72cb14ffb567b0847a1a9ae225495bd79f5ac89a0f85d1134a  Micro8Studio-1.0.0-win64-portable.zip
6b26167de5abdaf09b5e6055b7a651d95a10d675fbfafc61314a2240e44576f6  Micro8Studio-1.0.0-Setup.exe
```

## Documentation

- [`docs/Studio_README_fr.md`](docs/Studio_README_fr.md) / [`docs/Studio_README_en.md`](docs/Studio_README_en.md)
  : le manuel de Micro-8 Studio (éditeur, débogueur, émulateur, outils).
- [`docs/Micro-8_User_Manual.md`](docs/Micro-8_User_Manual.md) : le manuel de référence de la console Micro-8
  (langage Lofi, API, matériel).
- [`docs/Tracker_README.md`](docs/Tracker_README.md) : le tracker de musique Micro-8 (TRACKER2.SRC), son
  format de projet et ses programmes Lofi.

## Exemples

Le dossier [`samples/micro8_tracker`](samples/micro8_tracker) contient le tracker de musique écrit en Lofi
(`TRACKER.SRC`, `TRACKER2.SRC`, `PLAYER.SRC`), la démo musicale `SIDRUSH.SRC` et son projet `SIDRUSH.M8S` : des
programmes source pour la console Micro-8, à ouvrir ou compiler dans Micro-8 Studio.

## Licence

© Benoit Saint-Moulin ([www.bsm3d.com](https://www.bsm3d.com)). Tous droits réservés.

Inclut [Nuked-OPN2](https://github.com/nukeykt/Nuked-OPN2) (LGPL 2.1, © Nuke.YKT) pour l'émulation exacte du
YM2612.

Les ROM et le firmware de la console Micro-8 sont la propriété de Franck Sauer, fournis avec son accord ; ils ne
sont jamais distribués en clair et ne sont déchiffrés qu'en mémoire, à l'intérieur de l'application.
