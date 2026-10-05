# Micro-8

Projets pour le **Micro-8**, une machine 8 bits rétro avec son propre langage « lofi » proche du C.

| Dossier | Contenu |
|---|---|
| [`SidRush/`](SidRush) | **Sid Rush** : démo/cracktro façon C64 (basse FM, arpèges sur le synthé numérique, lead FM, drums DPCM) et son projet tracker `.M8S` |
| [`Tracker/`](Tracker) | Le séquenceur musical (`TRACKER.SRC`, `TRACKER2.SRC`), le lecteur `PLAYER.SRC` et la documentation du format |

## Sid Rush

Code, design et musique : Benoit Saint-Moulin.
Compiler `SidRush/SIDRUSH.SRC` sur la machine. Il charge `/tools/sounded/sounds/bass01.ins` et `lead02.ins` (fournis avec SOUNDED).
`SIDRUSH.M8S` (+ `.EXT.json`, `.NOTES.json`) sert uniquement au tracker et à `PLAYER.SRC`, pas à la démo.

## Notes

Les lignes de source ne doivent pas dépasser 159 caractères (limite de lecture de la machine) ; les fichiers sont en LF.
