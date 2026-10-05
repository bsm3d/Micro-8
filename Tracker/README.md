# Micro-8 Tracker

Un tracker de patterns pour la machine **Micro-8**, écrit en **Lofi**, et son lecteur. Il remplace l'ancien tracker
JavaScript, qui n'est plus maintenu.

## Les programmes

| Fichier | Rôle |
|---|---|
| `TRACKER.SRC` | **0.1, prototype** : 8 pistes, patterns de 16 à 256 pas, un pattern joue en boucle (pas de morceau), écrans PATTERN, SETUP, FILE, HELP, ABOUT |
| `TRACKER2.SRC` | **0.2, le tracker complet** : morceau (SONG), muet/solo, entrée MIDI, oscilloscopes, 5 écrans (PATTERN, SONG, SETUP, FILE, HELP), grille qui défile pendant la lecture, nom de l'auteur et site www.bsm3d.com en bas de l'écran. Les 10 instruments par défaut sont chargés depuis `/tools/sounded/sounds`. Compilé sur la machine : 84 % de la zone de code |
| `PLAYER.SRC` | le lecteur seul, sans édition : à copier dans un jeu ou une démo pour rejouer un projet |
| `SIDRUSH.SRC` | **SID RUSH** : démo musicale originale façon C64 et cracktro (basse FM au galop, arpèges à la fréquence d'image, mélodie à vibrato, batterie DPCM, barre raster « copper », titre arc-en-ciel, étoiles en parallaxe, crédits « Code, Design, Music ») ; générée par `micro8_studio/dev/make_sidrush.py`. Arpège : un `synNoteOn` par image (le firmware écrit les deux octets de fréquence d'un coup), sans `poke` de registres |
| `SIDRUSH.M8S` (+ `.EXT.json`, `.NOTES.json` pour Studio) | la musique de SID RUSH en projet tracker, un seul fichier à ouvrir dans le tracker de Micro-8 Studio, TRACKER2 (FILE : `R` SID RUSH, `L`) ou PLAYER (le choisir dans sa liste) : basse FM1 (BASS01), mélodie FM2 (LEAD02), batterie DRM, arpège SYN, un motif par mesure (17), 34 positions, tempo 150. La machine joue l'arpège une note par pas ; avec les FX Studio actifs (codes `Axy` et vibrato `Vxy`, lus seulement par Studio), Studio rejoue l'arpège rapide de la démo. Généré par `micro8_lofi/dev/make_sidrush_tracker.py` depuis les mêmes données que la démo |

Les programmes lisent et écrivent les **mêmes fichiers** : un projet fait avec l'un s'ouvre dans les autres, et
dans Micro-8 Studio.

## État sur la vraie machine

- **Synthé numérique (d'après le FPGA)** : une enveloppe ou un son DPCM ne repart que sur un front montant du
  déclenchement. Les trois programmes font donc `synNoteOff` puis `synNoteOn` à chaque note des pistes DRM et SYN,
  comme l'exemple DRUMBOX de la carte SD ; sans cela, sur la machine, une note répétée sans note off ne se rejouait
  pas.

- **Compilation.** TRACKER.SRC a été compilé sur la machine : arrêt ligne 190, `Unsupported operand type` sur `>>`,
  puis TRACKER3.SRC vers la ligne 297, sur `)`. Cause trouvée : le compilateur de la machine ne décale (`<<`, `>>`)
  que d'un `byte`. `v>>(b%8)` avec `b` en `int` est refusé ; aucun des 73 programmes de la carte SD ne décale d'autre
  chose qu'un `byte`. Corrigé dans les trois programmes (`byte s=(byte)(b%8); v>>s`). Micro-8 Studio signale
  maintenant cette erreur avant la machine. **À recompiler.**
- **Fins de ligne.** Deuxième essai : TRACKER.SRC refusé ligne 7 (puis 9), SIDRUSH.SRC ligne 8, `at ','.
  Expect expression.`, chaque fois sur la première ligne vide du fichier. Ces sources étaient en fins de ligne
  Windows (CRLF) ; les 73 programmes de la carte SD sont tous en LF. Le compilateur de la machine ne saute pas
  le CR (0x0D est le saut de ligne de son écran) : caché dans les commentaires, il casse la première ligne qui
  n'en est pas une. Toutes les sources (tracker, `lib/`) sont passées en LF, Studio enregistre toujours en LF
  et son analyseur refuse un CR comme la machine. Les lignes font aussi 90 caractères au plus. **À recompiler.**
- **Ligne de 159 caractères (2026-10-05).** la première version de SID RUSH (alors appelée SID RUSH) ne se lançait pas alors que l'ancien SIDRUSH oui : la ligne du tableau `rainbow` faisait exactement 159 caractères, la limite de lecture de la machine (le retour à la ligne compte probablement, le `;` final était perdu). Règle : **garder les lignes bien en dessous de 159 caractères (viser 130 au plus)** et couper les grands tableaux sur plusieurs lignes ; l'analyseur de Studio (`source_rules`) ne signale que > 159. Arpège (tick à chaque note, constaté sur la machine malgré un seul `synNoteOn` par image) : `synNoteOff(1)` en fin d'image juste avant `vSync()` et relâchement d'enveloppe rapide (`synEnv(1,0,5,9,15)`), pour que le changement de hauteur se fasse porte fermée. Même passage : l'arpège de l'ancien SIDRUSH passait de `poke` des registres à un `synNoteOn` par image (glitch), et `newChord` ne déclenche plus la première note (`arpFrame` la joue : un seul déclenchement).
- **OS de la machine.** Troisième essai : `getKeyDwn()` inconnu ligne 332 avec l'OS 0.1.x. Après mise à jour
  en 0.8.0 (`flash`), arrêt ligne 1561 sur `Type mismatch` : `memSet(..., 384*n, ...)` avec `n` en `int`, alors que
  la taille est un `nat`. La machine refuse tout argument d'API d'un type plus haut que celui du manuel (pas de
  conversion implicite vers le bas). Corrigé par des casts dans TRACKER, TRACKER3 et PLAYER (5 appels) ; l'analyseur
  de Studio vérifie maintenant ces types (table tirée du manuel, aucun faux positif sur les 73 programmes de la
  carte). **À recompiler.**
- **`break` et `continue` fuient la pile (2026-10-05).** `TRACKER2` démarrait, dessinait son écran, puis ne réagissait plus : les touches arrivaient dans la console (« Error ln 1, at '-' »). Cause lue dans le firmware (`breakStatement`, `continueStatement`, `patchLoopJumps`) : un `break` n'émet qu'un saut, les variables locales déclarées dans la boucle ne sont pas retirées de la pile de valeurs (256 entrées). Mes blocs `while(true) { ... break; }`, qui remplaçaient des fonctions, en laissaient des dizaines à chaque tour de la boucle principale. Ils sont réécrits sans `break` (drapeau `done`), et l'analyseur de Studio refuse maintenant un `break` ou `continue` qui laisse des locales dans une boucle imbriquée dans un `while`.
- **Taille du code** (estimée d'après la source, modèle ajusté sur les 74 `.BIN` de la carte SD, à 5 % près) :
  TRACKER.SRC environ 15 Ko, **TRACKER3.SRC environ 29 Ko sur 32**, PLAYER.SRC environ 3 Ko. Si `compile` refuse
  TRACKER3.SRC faute de place : `data 0xC000` avant de compiler donne 36 Ko de code (le tracker n'utilise qu'environ
  2,8 Ko de données). Ce réglage reste en EEPROM ; `data 0xB000` remet le partage par défaut.
- **Limites du manuel** (6.4.5), toutes loin du maximum : table des symboles environ 3,7 Ko sur 11, 16 niveaux
  d'appel sur 64, 20 valeurs de pile au plus par fonction sur 256.
- Contrôles sur le PC : l'analyseur de Micro-8 Studio (dans l'éditeur, ou `python micro8_lofi/lofic.py FICHIER.SRC`)
  applique les règles de la machine et estime la taille du code.

## Fichiers d'un projet

Un projet est **un seul fichier `NOM.M8S`, compressé comme sur C64** : les pistes sont des listes d'événements, et
une piste déjà écrite ailleurs, même transposée, n'est qu'une référence de 4 octets (comme les patterns transposés de
Future Composer).

| Partie | Contenu |
|---|---|
| en-tête | `M8S1` |
| morceau | 16 octets (un bit par octet non nul des 128 premiers réglages) puis ces octets ; les numéros de bloc du morceau (autant que sa longueur) ; `0`, ou `1` suivi des transpositions du morceau |
| instruments | 2 octets (un bit par emplacement FM enregistré) puis 54 paramètres par instrument |
| patterns | pour chaque bloc non vide : son numéro (0-63), un octet (un bit par piste non vide), puis pour chaque piste non vide soit `254`, la piste déjà écrite qu'elle répète (bloc × 8 + piste, 2 octets) et la transposition (+128), soit le nombre d'événements puis chacun : pas (0-15) + 16 si l'instrument change + 32 si la vélocité change, la note, puis l'instrument et la vélocité s'ils changent |
| fin | `255` |

Tailles : SID RUSH (17 patterns, 34 positions, 2 instruments) fait **782 octets** écrit par Studio, 1 220 enregistré
par TRACKER3 (qui garde ses 10 instruments) ; 25 500 en format brut. Sur la machine (mode X1 de l'émulateur) :
chargement 0,14 s, enregistrement 0,6 s (le tracker cherche les pistes répétées).

- **Un seul fichier** : TRACKER, TRACKER3, PLAYER, le bloc `song` de la bibliothèque M8LIB et Micro-8 Studio
  écrivent et lisent `NOM.M8S`. Le fichier est effacé puis réécrit à chaque sauvegarde (`fOpen` ne raccourcit pas
  un fichier existant).
- **Instruments.** Un emplacement tout à zéro garde l'instrument de la machine. Micro-8 Studio ne réécrit que les
  emplacements qu'il connaît.
- **Longueurs** : octets 84 à 91, un bit par bloc (1 = le bloc prolonge le précédent).
- **Pistes muettes** : octets 42 à 49 (TRACKER, TRACKER3). PLAYER les respecte.
- **Fichier endommagé** : au chargement, chaque réglage est ramené dans sa plage (instrument 0-9, octave 0-7,
  volume 0-127…) et chaque note jouée aussi : la puce ne reçoit jamais une valeur hors limites.
- **TRACKER.SRC 0.1 n'a pas de morceau** : à l'enregistrement, la position unique du morceau désigne le pattern
  affiché. PLAYER et Studio rejouent donc ce que tu entendais, pattern long compris.

## Premier lancement

1. Copie `TRACKER2.SRC` (ou `TRACKER.SRC`) et `PLAYER.SRC` sur la carte SD, par exemple dans `/TOOLS/TRACKER`.
2. Sur la machine : `cd /tools/tracker`, puis `compile tracker2.src` et `run tracker2.bin`. Le tracker charge ses 10
   instruments par défaut depuis `/tools/sounded/sounds` (la banque FM de la machine est vide après `ymInit`).
3. **Notes.** Les vrais programmes de la carte SD (`EXAMPLES/AUDIO/SIMPLSND`, `DEMOS/ORGAN`) jouent le La 440 Hz avec
   la **note 69** sur une **voix d'octave 5**, comme un numéro MIDI ; le tracker part de là.
4. TRACKER2 occupe 84 % de la zone de code (32 Ko, réglage par défaut `data 0xB000`), TRACKER 60 %.
5. **Instruments FM** : sur SETUP, `L` charge un `.INS` dans l'emplacement de la piste (par exemple
   `/demos/organ/sounds/pad01.ins`), `S` enregistre l'instrument édité dans un `.INS`. Ils sont aussi gardés avec le
   projet (`NOM.M8S`).
6. Pour rejouer un projet sans le tracker : `compile player.src`, `run player.bin` dans le dossier des `.M8S`,
   puis choisir le morceau dans la liste.

## Les 8 pistes

| Piste | Voix matérielle | Usage |
|---|---|---|
| FM1 à FM6 | 6 voix du YM2612 | mélodies, basses, accords ; banque de 10 instruments |
| DRM | voix 0 du synthé numérique | batterie (20 sons DPCM) ou oscillateur (carré, dents de scie, bruit) |
| SYN | voix 1 du synthé numérique | oscillateur (ou batterie) |

Les deux voix du synthé partagent un filtre, deux enveloppes et un LFO (réglages dans SETUP).

## TRACKER3.SRC (0.3)

Un seul écran : barre d'outils (transport, BPM, octave, pas d'édition, fichiers), pistes (muet/solo, oscilloscope par piste : la forme d'onde seule, sans barre de niveau), vue centrale à onglets
(PATTERN, SONG, SETUP, FILE, HELP ; `TAB`), puis clavier, instruments et état
toujours visibles.

- **Retirés pour tenir sur la machine** (2026-10-03 et 04) : la souris, la mini-vue du morceau, le panneau des
  enveloppes, le rythme euclidien, l'écran ABOUT (le nom de l'auteur est sur le bord haut de la fenêtre) et les barres
  de niveau à côté des oscilloscopes. Le compilateur de la machine refuse plus de 254 fonctions + paramètres, et le code ne peut pas dépasser
  36 Ko (`data 0xC000`). TRACKER3 en était à 409 et environ 35,5 Ko ; il est à 235 et environ 32,5 Ko (estimation).
- **Valeur de la case** : `FCTN` ou `CTRL` + haut/bas change la note (demi-ton, `SHIFT` : octave), l'instrument ou la
  vélocité sous le curseur ; la note s'entend à l'arrêt.
- **Répétition** : une flèche gardée enfoncée se répète (après 0,4 s, toutes les 70 ms).
- Les **oscilloscopes** sont dessinés d'après les notes jouées : la machine ne relit pas le son.

### PATTERN

Grille de 8 pistes, 16 lignes à la fois. Case : **note**, **instrument** (0-9), **vélocité** (hexadécimal, 01 à 7F).
Un pattern fait 16, 64, 128 ou 256 pas (4, 8 ou 16 blocs de 16 pas qui se suivent).

| Touche | Action |
|---|---|
| flèches | déplacer le curseur (gauche/droite : note, instrument, vélocité, puis piste suivante) |
| `SHIFT+haut` / `SHIFT+bas` | page précédente / suivante d'un pattern long |
| `Z S X D C V G B H N J M` | notes Do à Si de l'octave courante |
| `Q 2 W 3 E R 5 T 6 Y 7 U I` | mêmes notes une octave plus haut |
| `1` | note off |
| `Retour arrière` | efface la case |
| `-` / `=` | octave -/+ (champ note) ; vélocité -/+ (champ vélocité, `SHIFT` : pas de 16) |
| `[` / `]` | instrument par défaut -/+ |
| `;` / `'` | vélocité par défaut -/+ |
| `,` / `.` | pattern précédent / suivant ; pendant la lecture d'un pattern, il joue tout de suite, au même pas de la mesure |
| `0-9` | sur le champ instrument : instrument de la case |
| `FCTN+C` / `FCTN+V` / `FCTN+K` | copier / coller / effacer le pattern (tous ses blocs) |
| `FCTN+L` | longueur du pattern : 16, 64, 128, 256 pas |
| `FCTN+-` / `FCTN+=` | pas d'édition (0 à 16) |
| `FCTN+1` à `FCTN+8` | piste muette / audible, tout de suite : sa note en cours est coupée (enregistré dans le projet) |
| `FCTN+SHIFT+1` à `8` | piste en solo (pas enregistré) |
| `FCTN+F` | suivre la lecture ou non |
| `FCTN+gauche` / `FCTN+droite` | transposer la piste courante du pattern (pas la batterie) |

Sur la piste DRM, les touches de notes choisissent un des 20 sons de la batterie (`Z` = grosse caisse acoustique…).
Allonger un pattern prend les blocs suivants (le tracker demande `Y`/`O` s'ils ont des notes) ; raccourcir garde les
notes des blocs libérés.

### SONG

Vue arrangement : une bande de blocs, un par entrée, colorés par pattern ; l'entrée choisie en surbrillance, celle
qui joue en rouge.

| Touche | Action |
|---|---|
| haut / bas | choisir une entrée |
| gauche / droite | pattern précédent / suivant |
| `-` / `=` | transposition -/+ |
| `A` | insérer une copie de l'entrée |
| `Retour arrière` | supprimer l'entrée |
| `L` | boucle du morceau on/off |
| clic / glisser un bloc | choisir / déplacer l'entrée |

### SETUP

26 réglages : piste sélectionnée (instrument, octave de voix, panoramique, volume, muet), formes d'onde du synthé,
enveloppes, filtre, LFO, **NOTE MODE** et **NOTE OFFSET**. Haut/bas : choisir ; gauche/droite (ou `-` `=`) : changer
(`SHIFT` : pas de 8). `L` : charger un `.INS` dans l'emplacement de la piste ; `S` : enregistrer
cet instrument dans un `.INS`.

### FILE

`S` sauvegarde (`NOM.M8S`, un seul fichier), `L` charge, `R` renomme le projet (8 caractères), `N` nouveau.

### Touches communes

| Touche | Action |
|---|---|
| `Espace` | lecture / arrêt du pattern courant (en boucle) |
| `Entrée` | lecture / arrêt du morceau à partir de l'entrée choisie |
| `9` / `0` | tempo -/+ 1 (`SHIFT` : 10), de 40 à 255 BPM |
| `Échap` | arrêt |
| `FCTN+Échap` | quitter |

Le tempo est en noires ; un pas est une double-croche (`15000 / BPM` millisecondes). **Clavier MIDI** (port MIDI IN)
: chaque note joue sur la piste courante ; à l'arrêt, sur PATTERN, elle s'écrit sous le curseur.

## TRACKER.SRC (0.1)

Le prototype, volontairement réduit : pas de morceau (un pattern joue en boucle), pas de solo, de rythme
euclidien, d'entrée MIDI ni de panneau d'enveloppe. Écrans PATTERN, SETUP, FILE, HELP, ABOUT au clavier.

Touches : celles de PATTERN ci-dessus sans `FCTN+SHIFT+1..8` ni `FCTN+E` ; `FCTN` ou `CTRL` +
haut/bas change la valeur de la case ou du réglage ; sur SETUP, `CTRL` + gauche/droite change la valeur de 2, `L`
charge un `.INS`. Communes : `Espace`, `9`/`0`, `TAB`, `FCTN+Échap`. Mêmes fichiers que la 0.3, instruments compris.

## PLAYER.SRC

Liste les morceaux `.M8S` de son dossier (haut/bas pour choisir, `Entrée` pour jouer, `Échap` pour revenir à la
liste, `Échap` encore pour quitter) et rejoue celui choisi avec ses instruments : morceau avec transpositions et boucle, pistes
muettes respectées, et pour un morceau d'une seule position (projet 0.1) le pattern entier en boucle. Pendant la
lecture, tout de suite : `1` à `8` rend une piste muette ou audible (sa note en cours est coupée), gauche/droite
saute à la position précédente/suivante au même pas. Une vingtaine
de petites fonctions, sans code d'édition : à copier dans un jeu ou une démo.

## Correspondance avec le tracker JavaScript

| JS | Micro-8 |
|---|---|
| 4 voix (lead, basse, arpège, batterie) | 8 pistes (6 FM + 2 synthé) |
| ADSR par voix, formes d'onde | instruments FM (YM2612) et enveloppes du synthé |
| filtre par voix, panoramique, volume | panoramique et volume par piste ; un filtre partagé pour le synthé |
| delay, saturation, compresseur, bitcrusher | **non repris** (le matériel n'a pas ces effets) |
| export WAV / stems | dans Micro-8 Studio |
| arrangement, transposition par pattern | écran SONG |

## À vérifier au premier lancement

1. **Compilation** des trois programmes après la correction des décalages (voir plus haut).
2. **`_fError` après `ymLoad`/`ymSave`/`import`** : supposé mis à jour comme les autres fonctions fichier.
3. **`ymInit`** : les instruments du projet sont chargés après `ymInit`, au cas où il remettrait la banque à zéro.
4. **Codes de touches** (annexe B.2) : `FCTN`+chiffre donne-t-il bien le code du chiffre ?
5. **Vitesse de la VM** : à tempo très élevé, la lecture pourrait sauter des pas. Micro-8 Studio a un mode
   « Hardware Speed (X1) » pour s'en faire une idée ; à calibrer avec `micro8_lofi/dev/X1BENCH.SRC`.
6. **Zone mémoire des patterns** (page `0x90`, de `0x24000` à `0x2B800` avec le presse-papier) : à éviter en double
   buffer bitmap.

## Prochaines étapes possibles

- Effets par case (arpège, glissando, coupure) : il faudrait un fichier de plus et un moteur de lecture plus fin ;
  TRACKER3 n'a plus qu'environ 3 Ko de code de marge (davantage avec `data 0xC000`). Micro-8 Studio les a déjà
  (« Studio FX »), dans Studio seulement.
- Visualisation du son avec l'ADC (`adcSampleL`) : l'émulateur ne simule pas l'ADC, à essayer sur la machine.
