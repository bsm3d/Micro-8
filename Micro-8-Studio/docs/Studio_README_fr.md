# Micro-8 Studio

## Sommaire

- [1. Aperçu](#1-aperçu)
- [2. Éditeur](#2-éditeur)
- [3. Exécution et débogage](#3-exécution-et-débogage)
- [4. Convertisseur d'images](#4-convertisseur-dimages)
- [5. Éditeurs d'assets](#5-éditeurs-dassets)
- [6. Petites fenêtres](#6-petites-fenêtres)
- [7. Limites connues](#7-limites-connues)

---

## 1. Aperçu

Un IDE pour le Micro-8 et son langage Lofi : éditeur, analyseur en direct, émulateur avec débogueur, convertisseur
d'images. Thèmes **sombre** et **clair** (View > Dark / Light Theme, ou Preferences), splash à étapes réelles au
lancement (`--no-splash` pour le supprimer) et boîte **About** (Help > About).

Décompressez la version portable n'importe où (disque, clé USB) et lancez `Micro8Studio.exe`, sans rien installer ;
l'installeur place les mêmes fichiers dans Program Files ou le dossier de votre choix. Le dossier contient
`Examples\` (le mode d'emploi du Micro-8 et `OS\`, la carte SD par défaut) et `runtime\` ; Studio garde ses réglages
dans `data\` à côté de l'`.exe` (rien dans le registre), et le dossier se copie tel quel sur un autre PC. Le cœur
de l'émulateur est compilé pour le processeur : x86-64, AVX2 ou AVX-512, choisi au lancement (affiché dans About).

Le dossier racine par défaut est la carte SD `Examples/OS` (à côté de l'appli, avec le mode d'emploi dans
`Examples/`) (`Root...` dans le panneau Files pour en choisir un autre) : c'est le système de fichiers de la machine
émulée (`import`, `export`, `fOpen`…).

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
  Qt Linguist standard.

- **Analyse en direct** (0,5 s après la frappe) : erreurs et avertissements soulignés, marqueur dans la marge,
  panneau Problems, plan du programme (Outline). Un programme avec erreurs ne démarre pas. L'analyse connaît aussi
  les règles du vrai compilateur trouvées sur la machine (un décalage `<<` `>>` se fait d'un `byte`, jamais d'un
  `int`) et estime la taille du bytecode (modèle ajusté sur les `.BIN` de la carte SD, à 5 % près) : affichée
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
  lancer `dev/X1BENCH.SRC` sur la vraie machine, puis saisir le total mesuré dans Preferences pour obtenir la
  valeur à utiliser.

- **PC trop lent** : si le PC ne suit pas, toute l'exécution ralentit (horloge et son compris) au lieu de sauter du
  temps machine ; la barre d'état l'indique.

Limite : c'est un modèle de coût, pas une émulation au cycle près de l'ATmega128 et du firmware, les appels d'API
(copies mémoire, affichage) ont un coût fixe quelle que soit leur taille.

Menu Tools : contrôle des indices de tableau (désactivé comme sur la vraie machine / avertir dans Output /
arrêter), son, convention de notes du synthé, capture d'écran PNG, export du son de la session en WAV.

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
stockées en référence) : le séquenceur musical
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
  direct des Elektron), au lieu d'attendre la fin du pattern. Les trackers et le lecteur embarqués font de même sur la
  machine.

**Générateurs** : rythmes euclidiens, polyrythmies, 20 préréglages batterie,
presets basse/pad/arpège/aléatoire, 12 presets FM, n'écrivent que des notes, donc 100 % compatibles avec la
machine réelle.

**Effets Studio** (arpège, retrigger, delay, cut, probabilité, LFO YM, swing, longueurs de piste) : uniquement pour
l'aperçu dans Studio, désactivables, la lecture sur machine reste celle du tracker embarqué. Export MIDI, WAV, pistes
séparées (stems). `.EXT.json` : fichier annexe pour les longueurs de pattern et les effets, pas lu par la machine.

Testé octet pour octet contre les trackers embarqués (les deux versions et le lecteur) : un projet fait ici s'ouvre inchangé sur la machine réelle et inversement.

### 5.7. Screen designer

Fichiers `.SCR` : éditeur générique d'écran texte 80×40 (glyphes + couleurs), dans le style des outils
système (SPRITED/SOUNDED/MAPED).

- Palette de glyphes (les 256 de la police réelle), barre de couleurs papier/encre (palette texte par défaut de la
  machine).
- Outils peindre / cadre / cercle / remplir / écrire (machine à écrire : clic place un curseur, on tape), pipette,
  zoom, annuler/rétablir.
- `Ctrl+←/→` fait défiler la couleur d'encre, `Ctrl+↑/↓` la couleur de papier.

Fichier `.SCR` = 6400 octets (3200 glyphes puis 3200 couleurs). Un simple `import(nom,0x1200,6400)` l'affiche
depuis n'importe quel programme Lofi. Un fichier corrompu est refusé au chargement.

### 5.8. Bibliothèque Micro-8

Edit > Insert Micro-8 Library... colle dans le programme courant des fonctions Lofi prêtes à l'emploi pour relire
et écrire, **sur la machine**, les fichiers produits par les éditeurs du Studio (`lib/M8LIB.SRC`).
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
| `screen` | `loadScreen` / `saveScreen` | `.SCR` 3200 glyphes + 3200 couleurs (celle du Screen designer) |
| `song` | `songLoad` / `songStart` / `songTick` / `songStop` | projet tracker `.M8S`, joué par le coeur du PLAYER |

Toutes renvoient `true` en cas de succès (`_fError` à 0). Un `.SCR` abîmé est refusé. La fin de `M8LIB.SRC` (`//@main`) est un auto-test à lancer tel quel (F5) : il écrit puis relit
chaque type de fichier et joue `NEWSONG.M8S` s'il existe.

### 5.9. Projets

File > Project : un projet est **un dossier de la carte SD** qui contient un programme et ses assets,
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
  (demande avant de remplacer). **Send Project to Machine...** : le même envoi par MIDI (§5.10).
- **Tools > Write System to SD Card...** : écrit la carte livrée avec Studio (fichiers système, exemples et leurs
  programmes compilés) sur une vraie carte SD pour la machine. Un résumé annonce les fichiers nouveaux, remplacés et
  identiques avant d'écrire, et le presse-papiers ou l'annulation de la machine déjà présents sur la carte sont
  conservés. La cible doit être un lecteur amovible (jamais un disque fixe ni le dossier de Studio).
- Le dossier du projet doit être sous la racine de l'arbre Files (la carte émulée) pour que les chemins du
  programme se résolvent comme sur la machine : si ce n'est pas le cas, la racine remonte au dossier parent.

### 5.10. Transfert avec la machine (MIDI)

Tools > Transfer with Machine... (`lib/LINK.SRC`) : envoyer, récupérer et lister les
fichiers de la carte SD d'un vrai Micro-8, par ses prises MIDI et n'importe quelle interface USB-MIDI.

1. Copier `LINK.SRC` sur la carte (bouton **Copy LINK.SRC to the SD Card**, ou à la main), le lancer sur la machine.
2. Câbles : MIDI OUT du PC vers MIDI IN du Micro-8, MIDI OUT du Micro-8 vers MIDI IN du PC.
3. Choisir le port de sortie et le port d'entrée, **Test Connection**, puis **List** (dossier de la carte, double-clic
   pour entrer dans un dossier), **Send Files...**, **Send Project** (dans `/DOSSIER` du projet, sous-dossiers
   compris) ou **Fetch Selected...**.

Sans machine : choisir **Emulator**, **Start LINK in the Emulator** ; tout marche de la même façon contre l'émulateur.

Le débit est d'environ 1,3 Ko/s : une image de 48 000 octets prend 40 s environ. Fichiers de 64 Ko au plus, noms
8.3. Un fichier reçu ne remplace la cible qu'une fois complet : une coupure ne l'abîme jamais.

Le clavier MIDI choisi dans Tools > MIDI Input joue aussi dans le programme en cours, comme sur la machine.

## 6. Petites fenêtres

Chaque éditeur et chaque panneau s'adapte à une fenêtre étroite au lieu d'être coupé : les barres d'outils passent sur
plusieurs lignes, l'éditeur de sons empile ses zones (voix, opérateurs, clavier, oscilloscope) selon la largeur, et
les panneaux du bas défilent. `Ctrl+Maj+M` (Focus Mode) masque tous les panneaux pour gagner de la place.

## 7. Limites connues

Le débogueur travaille à la ligne (une instruction à la fois) ; `for(a;b;c)` ne s'arrête pas sur ses trois parties
séparément.
