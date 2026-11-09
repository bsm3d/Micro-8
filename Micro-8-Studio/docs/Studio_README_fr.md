# Micro-8 Studio

## Sommaire

- [1. Aperçu](#1-aperçu)
- [2. Éditeur](#2-éditeur)
- [3. Exécution et débogage](#3-exécution-et-débogage)
- [4. Convertisseur d'images](#4-convertisseur-dimages)
- [5. Éditeurs d'assets](#5-éditeurs-dassets)
- [6. Glyphes et fenêtres étroites](#6-glyphes-et-fenêtres-étroites)
- [7. Structure du code](#7-structure-du-code)
- [8. Tests](#8-tests)
- [9. Ce qui est validé](#9-ce-qui-est-validé)
- [10. Limites connues](#10-limites-connues)

---

## 1. Aperçu

Un IDE pour le Micro-8 et son langage Lofi : éditeur, analyseur en direct, émulateur avec débogueur, convertisseur
d'images. Même apparence que Spectra Workbench : thèmes **sombre** et **clair** (View > Dark / Light Theme, ou
Preferences), splash à étapes réelles au lancement (`--no-splash` pour le supprimer) et boîte **About** (Help >
About) au style de la suite.

**Version portable** (`Micro8Studio-<version>-win64-portable.zip`, faite par `python build.py`) : décompresser
n'importe où (disque, clé USB) et lancer `Micro8Studio.exe`, sans rien installer. Le dossier contient
`Examples\` (le mode d'emploi du Micro-8 et `OS\`, la carte SD par défaut) et `runtime\` ; Studio garde ses réglages
dans `data\` à côté de l'`.exe` (rien dans le registre), et le dossier se copie tel quel sur un autre PC. Le cœur
de l'émulateur est compilé pour le processeur : x86-64, AVX2 ou AVX-512, choisi au lancement (affiché dans About).

Depuis les sources :

    run_studio.bat                       # ou : python studio/main.py [FICHIER[:LIGNE]] [--root DOSSIER_SD]

Prérequis : Python 3.11+ avec `pip install -r requirements.txt` (dans `micro8_studio/`) (PySide6, Pillow, numpy).
Le dossier racine par défaut est la carte SD `Examples/OS` (à côté de l'appli, avec le mode d'emploi dans
`Examples/`) (`Root...` dans le panneau Files pour en choisir un autre) : c'est le système de fichiers de la machine émulée (`import`, `export`, `fOpen`…). Le moteur d'émulation
(analyseur, interpréteur, machine, vidéo, audio, débogueur) est le paquet `lofi/`, documenté séparément dans
`../README.md` avec son fonctionnement interne détaillé (mémoire, vidéo, audio, tuiles, sprites, interruptions
raster...).

## 2. Éditeur

- Onglets, numéros de ligne, coloration Lofi (types, mots-clés, API, variables `_xxx`, nombres, textes,
  commentaires), copier / couper / coller / annuler / rétablir, ligne courante, parenthèses appariées, occurrences
  du mot sous le curseur.

- Recherche `Ctrl+F` (compteur, casse, mot entier, regex), remplacement `Ctrl+H`, `F3` / `Maj+F3`, recherche dans
  les fichiers `Ctrl+Maj+F`, aller à la ligne `Ctrl+G`, signets `F2` / `Ctrl+F2`.

- Auto-indentation, fermeture automatique des `( [ { "`, `Tab` / `Maj+Tab` sur une sélection, commentaire `Ctrl+/`,
  dupliquer `Ctrl+D`, supprimer la ligne `Ctrl+Maj+K`, déplacer les lignes `Alt+↑/↓`.

- **Complétion** (à la frappe, ou `Ctrl+Espace`) : vos fonctions, variables et types, toute l'API (avec la
  signature du manuel), variables intégrées, mots-clés, **membres** après `variable.` ou `tableau[i].` pour vos
  types personnalisés, et **snippets** (`for`, `while`, `if`, `ifelse`, `main`, `func`, `type`, `loop`, `sprite`,
  `import` : nom + `Tab`, ou depuis la liste). Choisir une fonction ajoute les parenthèses. **Aide aux
  paramètres** : dans un appel, la signature s'affiche avec le paramètre courant en gras (API et fonctions du
  programme).

- **Navigation et refactoring** : aller à la définition `F12` ou `Ctrl+clic` (fonction, type, variable ; pour
  l'API, la signature), renommer un symbole `Ctrl+R` (occurrences entières hors textes et commentaires, une seule
  annulation), formater le document `Ctrl+Alt+L` (indentation par accolades, style du fichier : les sources
  réelles utilisent des tabulations), zoom `Ctrl+molette`.

- **Préférences** (`Ctrl+,`) : thème, **langue** (English / Français, redémarrer pour appliquer), police et taille,
  largeur de tabulation ou tabulations, fermeture automatique des paires, complétion et aides, délai d'analyse,
  mise en forme à l'enregistrement. L'interface (menus, barre d'outils, Préférences) est traduite via le mécanisme
  Qt Linguist standard (`self.tr()`, `.ts`/`.qm` dans `studio/i18n/`, voir `i18n.py`).

- **Analyse en direct** (0,5 s après la frappe) : erreurs et avertissements soulignés, marqueur dans la marge,
  panneau Problems, plan du programme (Outline). Un programme avec erreurs ne démarre pas. L'analyse connaît aussi
  les règles du vrai compilateur trouvées sur la machine (un décalage `<<` `>>` se fait d'un `byte`, jamais d'un
  `int`) et estime la taille du bytecode (modèle ajusté sur les 74 `.BIN` de la carte SD, à 5 % près) : affichée
  dans la carte mémoire, avec un avertissement au-delà des 32 Ko du segment de code.

- **Octets préservés** : un fichier est enregistré octet pour octet (fins de ligne CR LF ou LF conservées). Les
  codes de contrôle du Micro-8 dans les textes (nouvelle ligne 0x0D, couleurs…) s'affichent comme des « control
  pictures » (␍ ␐…) ; menu Edit > Insert Control Glyph pour en insérer.

## 3. Exécution et débogage

`F5` lance le programme sur le Micro-8 émulé (écran dans le dock *Micro-8 screen* : cliquez dessus pour lui donner
le clavier ; souris ; manette : flèches, Z=A, X=B, Entrée=Start, Tab=Select). Le son est joué en direct (FM par
Nuked-OPN2). Une erreur d'exécution arrête le programme sur sa ligne avec un message clair, y compris quand elle
vient d'un argument impossible (`5 >> -1`, `sqrt(-1.0)`, un pow() sans résultat réel, une adresse hors limites) ;
un réel passé là où la machine attend un entier est tronqué comme par `int()`.

| Action | Touche |
|---|---|
| Lancer / continuer | `F5` (lancer sans points d'arrêt : `Ctrl+F5`) |
| Arrêter / relancer | `Maj+F5` / `Ctrl+Maj+F5` |
| Pause | `F6` |
| Pas à pas sortant / entrant / sur place | `Maj+F11` / `F11` / `F10` |
| Point d'arrêt | `F9` ou clic dans la marge (clic droit : condition, ex. `i==2`) |
| Point d'arrêt mémoire | Debug > Ajouter un point d'arrêt mémoire, ou clic droit sur un octet dans Memory : le programme s'arrête dès que cet octet (ou cette plage, RAM ou SRAM externe) change, quel que soit l'auteur (affectation, `poke`, fonction de l'API, interruption raster) ; Output donne l'adresse, l'ancienne -> la nouvelle valeur et la ligne qui a écrit |

Panneaux : **Variables** (pile d'appels, locaux et globaux, valeurs modifiées en orange), **Watch** (expressions
Lofi évaluées à l'arrêt, ex. `data[i]*10`), **Breakpoints** (lignes et mémoire), **Memory** (dump hexadécimal de la RAM ou de la SRAM
externe, octets modifiés en surbrillance, raccourcis vers les zones utiles), **Hardware** (couches, défilement,
pages, raster, palettes, sprites, registres du synthé).

### 3.1. Vitesse d'émulation

- **Par défaut** : le programme tourne aussi vite que le PC le permet ; le temps machine suit l'horloge réelle (hors
  temps passé à l'arrêt).

- **Run > Hardware Speed (X1)** : le programme tourne à la vitesse du vrai Micro-8. Chaque opération du VM (un nœud
  d'expression, un test de boucle, un saut) coûte un temps fixe ; le temps machine est la somme de ces coûts, et
  l'émulateur attend l'horloge réelle quand il est en avance. Un programme fait donc autant de travail par image
  que sur la machine. Réglage mémorisé, modifiable pendant l'exécution ; la barre d'état affiche `Running (X1)`.

- **Vitesse** : Preferences > Emulation > Hardware speed (X1), 1000 kHz par défaut (VM ≈ 1 MHz). Calibrage :
  lancer `dev/X1BENCH.SRC` sur la vraie machine, puis `python dev/x1_calibrate.py 1000 TOTAL_MESURÉ` donne la
  valeur à saisir.

- **PC trop lent** : si le PC ne suit pas, toute l'exécution ralentit (horloge et son compris) au lieu de sauter du
  temps machine ; la barre d'état l'indique.

Limite : c'est un modèle de coût, pas une émulation au cycle près de l'ATmega128 et du firmware, les appels d'API
(copies mémoire, affichage) ont un coût fixe quelle que soit leur taille.

Menu Tools : contrôle des indices de tableau (désactivé comme sur la vraie machine / avertir dans Output /
arrêter), son, convention de notes du synthé, capture d'écran PNG, export du son de la session en WAV, **Hardware
Link...** (voir §5.8).

## 4. Convertisseur d'images

`Ctrl+Maj+I` ouvre la fenêtre *Image tools* : glisser-déposer une image (PNG, JPG, GIF, TGA…).

- **Bitmap 400×240** → `.BMP` (48000 octets, 4 bits/pixel) + `.PAL` (48 octets, 3 bits par composante).

- **Sprites 24×21** → `.SPR` (252 octets de pixels + boîte de collision AABB calculée) + `.PAL`.

- **Tuiles 16×16** → `.TLS` + `.PAL` ; **tuiles + carte** : image découpée, tuiles identiques fusionnées, `.TLS` +
  `.LVL` (carte d'un octet par case) + `.PAL`. Avertissement au-delà de 256 tuiles / 256 sprites.

- Ajustement (fit / fill / stretch / 1:1), rééchantillonnage, luminosité / contraste / saturation, palette
  optimisée pour l'image, ou celle d'un `.PAL`, ou la palette éditée à la main (clic sur une couleur : 512
  couleurs), tramage (aucun, Floyd-Steinberg, Bayer 4×4), transparence de l'indice 0, comparaison
  original / converti, zoom.

- Export dans le dossier choisi et **code Lofi prêt à coller** (`import(...)`, adresses de palette, activation des
  couches).

- Onglet **Asset viewer** : ouvre les `.BMP`, `.SPR`, `.TLS` existants avec leur palette (retrouvée
  automatiquement), boîtes de collision en magenta, export PNG. Les formats ont été vérifiés sur les ressources de
  la carte SD : le `.BMP` de MOUNTAIN se reconvertit à l'identique, les sprites de même.

## 5. Éditeurs d'assets

Équivalents des outils de la carte SD (SPRITED, MAPED, IMAGED, SOUNDED, SCREENED, le tracker embarqué).

Chaque éditeur s'ouvre dans un onglet, à côté du code : barre d'outils, Tools ou File > New Asset, double-clic sur
un fichier de l'arborescence, ou glisser-déposer. Enregistrement `Ctrl+S`, annuler/rétablir/copier/coller par le
menu Edit. Tous relisent puis réécrivent **octet pour octet** les fichiers réels de la carte SD (vérifié par les
tests).

### 5.1. Sprite editor

`.SPR` + `.PLA` / `.PLB` : jeu de sprites 24×21, jusqu'à 256.

- Liste de vignettes (nouveau, dupliquer, supprimer, déplacer).
- Dessin pixel par pixel : crayon, gomme, ligne, rectangle, ovale, remplissage, pipette, sélection avec
  déplacement / copier / coller, miroir X/Y, pinceau 1-4, zoom molette, grille, papier d'oignon (le sprite
  précédent en transparence).
- Annulation par sprite, éditeur de palette (512 couleurs, palettes A et B).
- **Boîte de collision AABB** (champs, ajustement automatique), retournements, remplacement / échange de couleurs.
- **Aperçu d'animation** (plage, images par seconde), import d'une planche PNG, export PNG.

### 5.2. Tile / map editor

`.TLS` + `.PAL` + `.LVL` : tuiles 16×16, jusqu'à 256.

- Tuiles éditées pixel par pixel, carte peinte (peindre, remplir, pipette, sélection copier / coller).
- Redimensionnement de la carte, cadre d'écran 25×15, grille, zoom.
- Supprimer une tuile renumérote la carte automatiquement.
- Import d'une image en tuiles + carte (tuiles identiques fusionnées), export PNG.
- Formats : `.TLS` brut (+ `.PAL`) ou forme de l'éditeur de tuiles (palette de 48 octets devant) ; `.LVL` avec
  en-tête de 4 octets (largeur, hauteur) ou brut (largeur demandée, comme le niveau 80×24 de la démo TILES).

### 5.3. Bitmap editor

`.BMP` 400×240 + `.PAL` : les mêmes outils que le sprite editor, appliqués à toute l'image.

- Palette de 16 couleurs, grille guide 16×16.
- Import d'une image avec ajustement, tramage et palette optimisée.
- Coller une image du presse-papiers, export PNG.

### 5.4. Sound editor

`.INS` 54 octets, `.SYN` 16 octets: banque de 10 instruments YM2612.

- Les 54 paramètres (4 opérateurs : detune, multiplicateur, niveau total, rate scaling, attaque, AM, decay,
  sustain rate, sustain level, release).
- Algorithme avec **schéma** (porteuses en orange), feedback, LFO, opérateurs actifs.
- **Courbe d'enveloppe** par opérateur, **clavier de test** (souris ou touches Z S X D C V… / Q 2 W 3 E R…) joué
  en direct par Nuked-OPN2, forme d'onde et spectre du son.
- Copier / coller / initialiser / hasard / mutation, charger un `.INS` dans un emplacement, banque de 10 fichiers.
- Onglet **synthé numérique** : oscillateurs, batterie DPCM (20 sons), enveloppes, LFO, filtre SVF, gains et
  panoramique, les 16 registres de 0x1110. Le préréglage s'enregistre en `.SYN` (16 octets, format du studio).

### 5.5. Wave editor

`.wav` : forme d'onde avec sélection, zoom, défilement, spectre.

- Couper / copier / coller / supprimer / rogner, silence, inverser, gain, normaliser, fondus, retirer la
  composante continue, mono.
- Ré-échantillonnage (8-48 kHz), écrêtage en N bits (la batterie DPCM du synthé est en 4 bits), génération de
  signaux, lecture avec boucle, export WAV.
- « Send to wave editor » (éditeur de sons) et Tools > Sound of this Run l'alimentent avec les sons du synthé ou
  d'un programme.

### 5.6. Tracker

Un seul fichier `.M8S` compressé comme sur C64 (pistes en listes d'événements, pistes répétées ou transposées
stockées en référence : 782 octets pour SID RUSH ; format dans `micro8_tracker/README.md`) : le séquenceur musical
intégré du Micro-8.

- 8 pistes : 6 voix FM, une piste batterie/oscillateur DRM, une piste SYN.
- Cellules note/instrument/vélocité, patterns de 16 à 256 pas (blocs réclamés/libérés).
- Morceau (séquence de patterns transposables, boucle), vue page ou continue. Pendant la lecture du morceau,
  la grille suit le pattern qui joue ; en choisir un autre à la main l'affiche jusqu'à l'entrée suivante.
- **Bandeau SONG** : une carte par entrée, 2 rangées de 16 (comme les banques Elektron). Le pattern s'y lit en
  banque (`A01`…`D16`, 16 patterns par banque) avec son numéro en hexadécimal ; une entrée qui se répète est une
  seule carte « ×N » (le REP d'Elektron : `Ctrl`+molette ou le bouton de copie pour en ajouter ou retirer, clic
  droit pour le nombre exact), et la carte qui joue indique la répétition en cours (« 2/3 »). Au-delà de 32 cartes,
  des boutons de page (« 0-31 », « 32-63 »…) ; la page suit la lecture, et survoler un bouton de page en glissant
  une carte l'ouvre. Molette : pattern, `Maj`+molette : transposition, glisser : déplacer (avec ses répétitions),
  double-clic : jouer d'ici.
- Édition façon OctaMED : sélection Maj+flèches ou souris, transposer, copier/couper/coller une plage, inverser,
  étendre/réduire, vélocité fixe/rampe/aléatoire.
- Un défilement par piste, mute (clic) / solo (double-clic), **instantanés** : la note en cours est coupée.
- Pendant la lecture d'un pattern, en choisir un autre le joue **tout de suite**, au même pas de la mesure (le saut
  direct des Elektron), au lieu d'attendre la fin du pattern. TRACKER, TRACKER2 et PLAYER font de même sur la
  machine.

**Générateurs** (`trackergen.py`, `trackertools.py`) : rythmes euclidiens, polyrythmies, 20 préréglages batterie,
presets basse/pad/arpège/aléatoire, 12 presets FM, n'écrivent que des notes, donc 100 % compatibles avec la
machine réelle.

**Effets Studio** (arpège, retrigger, delay, cut, probabilité, LFO YM, swing, longueurs de piste) : uniquement pour
l'aperçu dans Studio, désactivables, la lecture sur machine reste celle de TRACKER.SRC. Export MIDI, WAV, pistes
séparées (stems). `.EXT.json` : fichier annexe pour les longueurs de pattern et les effets, pas lu par la machine.

Testé byte-exact contre TRACKER.SRC/TRACKER2.SRC (les deux versions du tracker embarqué, voir
`../micro8_tracker/`) : un projet fait ici s'ouvre inchangé sur la machine réelle et inversement.

### 5.7. Screen designer

`.SCR`, `screendesign.py` : éditeur générique d'écran texte 80×40 (glyphes + couleurs), dans le style des outils
système (SPRITED/SOUNDED/MAPED).

- Palette de glyphes (les 256 de la police réelle), barre de couleurs papier/encre (palette texte par défaut de la
  machine).
- Outils peindre / cadre / cercle / remplir / écrire (machine à écrire : clic place un curseur, on tape), pipette,
  zoom, annuler/rétablir.
- `Ctrl+←/→` fait défiler la couleur d'encre, `Ctrl+↑/↓` la couleur de papier.

Fichier `.SCR` = 6400 octets (3200 glyphes puis 3200 couleurs, exactement la disposition de la mémoire écran
0x1200/0x1E80 : un simple `import(nom,0x1200,6400)` l'affiche depuis n'importe quel programme Lofi), plus 2 octets
de somme de contrôle Fletcher-16 optionnelle, ignorés par ce même `import`, vérifiés au chargement ici et dans
l'outil équivalent sur machine, `micro8_tracker/SCREENED.SRC` (source retirée du dépôt). Un fichier corrompu est refusé au chargement, un
fichier plus ancien sans somme se charge quand même.

### 5.8. Hardware Link (expérimental)

Tools > Hardware Link... (`hwlink.py`, `hwlink_panel.py`) : fondations pour un futur lien USB-MIDI vers un vrai
boîtier Micro-8, via une interface USB-MIDI branchée au PC.

- Encapsule des octets bruts dans des trames SysEx (ID fabricant réservé non-commercial `0x7D`), versionnées et
  numérotées (appariement requête/réponse), sommées (somme de contrôle simple sur 8 bits).
- Ping et Identify pour valider un câble de bouclage.
- L'envoi tourne sur un fil dédié pour ne jamais geler l'interface.

**Le firmware réel du Micro-8 ne parle pas ce protocole**, et le SysEx n'atteint pas un programme Lofi : pour
échanger des fichiers avec la machine, voir §5.11 (Transfer with Machine), qui marche avec le firmware actuel.

### 5.9. Bibliothèque Micro-8

Edit > Insert Micro-8 Library... colle dans le programme courant des fonctions Lofi prêtes à l'emploi pour relire
et écrire, **sur la machine**, les fichiers produits par les éditeurs du Studio (`lib/M8LIB.SRC`, `m8lib.py`).
Lofi n'a pas de `#include` et compile en une passe : les blocs cochés, plus ceux dont ils ont besoin, sont insérés
après les commentaires d'en-tête, dans l'ordre de la bibliothèque. Un bloc déjà présent n'est pas recopié, et une
seule annulation retire l'insertion.

| Bloc | Fonctions | Fichier |
|---|---|---|
| `pal` | `loadPal` / `savePal` | `.PAL` 48 octets (16 R, 16 V, 16 B, 0-7) vers une adresse de palette |
| `bitmap` | `loadBitmap` / `saveBitmap` | `.BMP` 48000 octets, page bitmap affichée (0x2F05) |
| `sprites` | `loadSprites` / `saveSprites` | `.SPR`, 256 octets par sprite, à partir d'un numéro |
| `tiles` | `loadTiles` / `saveTiles` | `.TLS`, 128 octets par tuile 16x16 |
| `level` | `loadLevel` / `saveLevel` | `.LVL`, un numéro de tuile par case, dans un tableau d'octets |
| `screen` | `loadScreen` / `saveScreen` | `.SCR` 3200 glyphes + 3200 couleurs + somme Fletcher-16 (celle du Screen designer) |
| `song` | `songLoad` / `songStart` / `songTick` / `songStop` | projet tracker `.M8S`, joué par le coeur du PLAYER |

Toutes renvoient `true` en cas de succès (`_fError` à 0). Un `.SCR` abîmé est refusé, un `.SCR` ancien sans somme
est accepté. La fin de `M8LIB.SRC` (`//@main`) est un auto-test à lancer tel quel (F5) : il écrit puis relit
chaque type de fichier et joue `NEWSONG.M8S` s'il existe.

### 5.10. Projets

File > Project (`project.py`) : un projet est **un dossier de la carte SD** qui contient un programme et ses assets,
décrit par un petit manifeste JSON `NOM.M8P` (nom 8.3, il peut voyager sur la carte sans gêner). Le manifeste garde
le programme principal, les fichiers du projet, les onglets ouverts, l'onglet courant et le réglage X1.

- **New Project...** : choisir le dossier et le nom (1 à 8 lettres, chiffres ou `_`) ; crée `NOM.SRC` s'il n'existe
  pas, et les assets déjà présents dans le dossier rejoignent le projet. **Open Project...** (`Ctrl+Maj+O`) rouvre
  les onglets et le mode X1 ; Studio rouvre au démarrage le projet ouvert à la fermeture.
- **Save Project** (`Ctrl+Alt+Maj+S`) enregistre tous les fichiers ouverts et le manifeste ; un fichier ouvert depuis
  le dossier du projet y est ajouté (ou **Add Current File to Project**). Un nouveau fichier est proposé par défaut
  dans le dossier du projet. **Close Project** ferme seulement les onglets du projet.
- **F5** avec un éditeur d'asset au premier plan lance le programme du projet.
- **Check Project** : ce que la vraie machine refuserait (noms non 8.3 de la FAT32, erreurs de compilation, code
  au-delà du segment de 32 Ko, fichiers du projet manquants) et les fichiers que le programme nomme dans ses textes
  mais qui ne sont pas dans le dossier (simple note : le programme peut les écrire lui-même).
- **Export Project to SD Card...** : copie le manifeste, le programme et ses fichiers dans `CARTE/DOSSIER/`
  (demande avant de remplacer). **Send Project to Machine...** : le même envoi par MIDI (§5.11).
- **Tools > Write System to SD Card...** : écrit la carte livrée avec Studio (fichiers système, exemples et leurs
  programmes compilés) sur une vraie carte SD pour la machine. Les `.BIN`, livrés chiffrés, sont déchiffrés par le
  cœur C++ directement sur la carte, qui doit être un lecteur amovible (jamais un disque fixe ni le dossier de Studio) ; un résumé annonce les
  fichiers nouveaux, remplacés et identiques avant d'écrire, et le presse-papiers ou l'annulation de la machine
  déjà présents sur la carte sont conservés. Les mises à jour du système (`SYSTEM/UPDATES`) ne sont pas livrées.
- Le dossier du projet doit être sous la racine de l'arbre Files (la carte émulée) pour que les chemins du
  programme se résolvent comme sur la machine : si ce n'est pas le cas, la racine remonte au dossier parent.
- Un manifeste hostile ou abîmé ne peut rien atteindre hors de son dossier (`..`, lecteur, chemin absolu ignorés).

### 5.11. Transfert avec la machine (MIDI)

Tools > Transfer with Machine... (`m8link.py`, `linkpanel.py`, `lib/LINK.SRC`) : envoyer, récupérer et lister les
fichiers de la carte SD d'un vrai Micro-8, par ses prises MIDI et n'importe quelle interface USB-MIDI.

1. Copier `LINK.SRC` sur la carte (bouton **Copy LINK.SRC to the SD Card**, ou à la main), le lancer sur la machine.
2. Câbles : MIDI OUT du PC vers MIDI IN du Micro-8, MIDI OUT du Micro-8 vers MIDI IN du PC.
3. Choisir le port de sortie et le port d'entrée, **Test Connection**, puis **List** (dossier de la carte, double-clic
   pour entrer dans un dossier), **Send Files...**, **Send Project** (dans `/DOSSIER` du projet, sous-dossiers
   compris) ou **Fetch Selected...**.

Sans machine : choisir **Emulator**, **Start LINK in the Emulator** ; tout marche de la même façon contre l'émulateur.

Pourquoi ce protocole : un programme Lofi ne voit le MIDI qu'à travers `midiUpdt()`, une file de 16 événements
analysés par le firmware, et le SysEx ne lui parvient pas. Tout passe donc en messages Note : deux octets par message
(note, vélocité et leurs bits hauts dans le canal), des blocs de 128 octets avec somme de contrôle, accusés de
réception, renvois et ralentissement automatique si la file de la machine déborde. Un fichier reçu va d'abord dans
`LINK.TMP` et ne remplace la cible qu'une fois complet : une coupure ne l'abîme jamais. Débit : environ 1,3 Ko/s
(31,25 kbit/s), soit une image de 48 000 octets en 40 s environ. Limites : fichiers de 64 Ko au plus (`fSize()` est
un `nat`), noms 8.3.

L'émulateur modélise l'entrée MIDI du firmware : 960 µs par message sur le câble, file de 16 événements qui perd
l'excédent, `midiSend()` qui attend l'UART (320 µs par octet). Le clavier MIDI choisi dans Tools > MIDI Input joue
aussi dans le programme en cours, par `midiUpdt()`, comme sur la machine.

## 6. Glyphes et fenêtres étroites

- **Icônes dessinées** (`icons.py`), au style de Spectra Workbench : glyphes au trait sur une grille 24×24 (trait
  1,6 px, bouts arrondis), encrés selon le thème (gris clair sur le sombre, presque noir sur le clair), états
  normal / survol / désactivé, aucun fichier image et aucun symbole Unicode dans les boutons. Barre d'outils,
  outils de dessin, éditeurs, onglets (un point dessiné marque un onglet modifié), problèmes et plan du programme
  les utilisent ; ils suivent le changement de thème.
- **Fenêtre étroite** : chaque éditeur défile au lieu d'être coupé, les barres d'outils passent sur plusieurs
  lignes (`flow.py`), l'éditeur de sons empile ses zones (voix, opérateurs, clavier, oscilloscope) selon la
  largeur, les panneaux du bas défilent. `Ctrl+Maj+M` (Focus Mode) masque tous les panneaux.

## 7. Structure du code

`main.py` (entrée) · `splash.py` / `about.py` / `version.py` / `credits.json` (identité) · `prefs.py` ·
`intellisense.py` (symboles, complétion, snippets, renommage, formatage) · `window.py` (fenêtre, menus, exécution,
débogage, enregistrement des outils) · `editor.py` (éditeur, marge, recherche) · `lofi_syntax.py` (coloration,
conversion octets ↔ éditeur) · `panels.py` (Problems, Output, Outline, Files, Search, Variables, Watch,
Breakpoints, Memory, Hardware) · `icons.py` (glyphes) / `flow.py` (barres qui passent à la ligne) ·
`pixeledit.py` (noyau pixel : outils, annulation, canevas, palette), `sprited.py` / `mapedit.py` / `bmpedit.py` /
`soundedit.py` / `waveedit.py` / `datedit.py` / `screendesign.py` / `editorbase.py` (éditeurs d'assets) ·
`tracker.py` (éditeur tracker) / `trackergen.py` (générateurs, sans Qt) / `trackertools.py` (dialogue Generate,
export) · `hwlink.py` (protocole SysEx, expérimental) / `hwlink_panel.py` (son panneau) · `midiin.py` /
`midiout.py` (MIDI réel, winmm : notes courtes et SysEx) · `manual.py` (visionneuse Markdown, manuel matériel et
cette doc) · `screen.py` (écran émulé) · `session.py` (programme en cours : threads, horloge, son) ·
`imgconv.py` / `imgtool.py` (conversion, interface) · `m8lib.py` (bibliothèque Micro-8) ·
`project.py` (projets) · `m8link.py` / `linkpanel.py` (transfert MIDI avec la machine, `../lib/LINK.SRC` côté machine) ·
`theme.py`.

Le moteur est le paquet `lofi/` (analyseur, interpréteur, machine, vidéo, audio, débogueur : voir `../README.md`).

## 8. Tests

    python tests/run_studio_tests.py <python avec PySide6>     # 47 scripts hors écran : éditeurs, projets, transfert, fuites...
    python tests/run_tests.py                                # moteur : 73 programmes, checker, audio, matériel, débogueur

## 9. Ce qui est validé

- **Moteur Lofi** (`tests/run_tests.py`) : les 73 programmes réels de la carte SD compilent et tournent sans
  erreur ; robustesse du compilateur (`lofi/checker.py`) testée par mutation (des milliers d'essais sur les
  sources réelles) et cas limites adversariaux (imbrication extrême, littéraux énormes...), une entrée
  pathologique rapporte une erreur propre, ne plante jamais l'analyse en direct de l'éditeur.
- **Éditeurs d'assets** (`tests/studio_assets_test.py` et dédiés) : ronds-trips octet-exacts contre les vraies
  ressources de la carte SD (sprites, tuiles, bitmap) et contre les sources Lofi de référence (TRACKER.SRC,
  TRACKER2.SRC) pour le tracker et le screen designer.
- **Fuzzing aléatoire** : séquences de touches/souris aléatoires (avec modificateurs FCTN/SHIFT/CTRL) rejouées sur
  les trois programmes Lofi (TRACKER, TRACKER2, PLAYER) et sur les outils Studio (Screen Designer,
  Hardware Link), des centaines d'essais sans crash à ce jour.
- **Fuites** (`tests/studio_leak_test.py`) : cycles lancer/arrêter (avec et sans son) sans thread, handle ni
  mémoire qui restent ; sessions libérées ; 30 onglets et 60 éditeurs d'assets (tous les types, dont Screen
  Designer) ouverts et fermés sans qu'aucun ne survive ; threads d'envoi Hardware Link qui ne s'accumulent pas et
  dialogue bien libéré ; cycles d'ouverture/fermeture de ports MIDI réels sans fuite de handles (si un port est
  présent sur la machine). Le pilote audio (winmm) est libéré à chaque arrêt.
- **Réactivité** : l'envoi MIDI SysEx (Hardware Link) tourne sur un fil dédié, jamais sur le fil GUI, un port lent
  ou débranché ne gèle plus l'interface. L'analyse en direct fait une seule passe (erreurs, plan, carte mémoire) :
  ~50 ms sur les plus gros sources de la carte SD (TRACKER2, HOOGY), au lieu de trois.

- **Fichiers corrompus** : chaque format d'asset (12) est rechargé tronqué, vide, doublé, aléatoire et muté
  (des centaines de variantes), puis ré-affiché et ré-enregistré : erreur propre ou chargement, jamais de crash.

- **Application compilée** : dans la version livrée, tout le code de Studio (tracker compris) et du moteur est
  compilé en code natif (61 modules) ; toutes les suites passent aussi sur cette version compilée, et chaque
  construction lance l'application livrée pour vérifier qu'elle joue un morceau et exécute un programme.
- **Cœur natif (C++)** : l'interpréteur Lofi et la machine sont en C++, vérifiés valeur pour valeur contre l'ancien
  émulateur Python lors du portage. Trois compilations (x86-64, AVX2, AVX-512) donnent les mêmes images et le même
  son au bit près (`tests/native_core_test.py`). Une version de contrôle qui s'arrête au moindre comportement non
  défini du C++ (débordement, décalage invalide, accès hors limites) passe toute la suite et des cas extrêmes
  (`tests/native_edge_test.py` : entiers et réels énormes, tailles négatives, coordonnées très loin de l'écran,
  `rnd()` sur toute l'étendue 64 bits identique à Python). Endurance : des centaines d'exécutions, d'erreurs,
  d'arrêts, de sessions F5 avec le vrai son et de lectures du tracker sans croissance de mémoire, de handles ni
  de threads.
- **Émulateur** : même comportement, ~23 % plus rapide (chemin arithmétique entier, variables, lexer) ; le mode X1
  est testé réglé sur l'horloge murale (1690 ms machine = 1,7 s réelles pour `X1BENCH.SRC`).
- **Sauvegardes** : le format `.SCR` (Screen Designer) porte une somme de contrôle Fletcher-16 vérifiée au
  chargement, identique bit à bit des deux côtés (Studio et `micro8_tracker/SCREENED.SRC` (source retirée du dépôt)) ; un fichier corrompu
  est refusé, un fichier antérieur sans somme se charge quand même. Voir `SCREENED.SRC` (retiré du dépôt) pour le
  détail du format.

- **Matériel (code du FPGA)** : le synthé numérique est rendu par un modèle du HDL dont la version rapide est
  vérifiée échantillon par échantillon contre une version ligne à ligne (`tests/digisynth_test.py`) ; l'image
  800×480, les priorités, le défilement, les sprites, le pointeur, les registres d'état et la cadence 59,92 Hz sont
  vérifiés par `tests/fpga_video_test.py` ; la police est identique au bit près à la ROM du FPGA.

## 10. Limites connues

Voir `../README.md` (conventions devinées côté firmware ; le matériel suit le code source du FPGA, voir « Fidélité au matériel »). Le
débogueur travaille à la ligne (une instruction à la fois) ; `for(a;b;c)` ne s'arrête pas sur ses trois parties
séparément. Hardware Link n'a encore aucun vrai firmware en face : testé avec de vrais ports MIDI et un protocole
solide côté Studio, mais sans round-trip réel (ping/pong) tant qu'aucun boîtier Micro-8 ou câble de bouclage MIDI
n'est disponible. Le transfert MIDI (§5.11) est validé contre le modèle de l'entrée MIDI du firmware ; sur la
vraie machine, deux inconnues sont prises en charge (canaux comptés de 0 ou de 1, boucle Lofi plus lente que le
câble : renvois et ralentissement) mais pas encore mesurées.
