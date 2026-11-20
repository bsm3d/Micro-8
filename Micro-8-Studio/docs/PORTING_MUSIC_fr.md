# Porter une musique du tracker Micro-8 vers un autre système

Pour un développeur qui compose dans le tracker de Micro-8 Studio et veut la même musique dans un jeu ou une démo sur un
autre système : Mega Drive, NES, C64, Game Boy, Amiga, un moteur PC comme Unity ou Godot, ou son propre lecteur.

Le tracker est un séquenceur à pas qui pilote les puces sonores du Micro-8. Tout ce qu'il faut pour un portage est soit dans
le fichier du morceau, soit dans un des exports ci-dessous. Cette page dit ce que contient chaque export, comment le morceau
est cadencé, comment une note devient un son sur la puce, et donne une recette par cible.

## 1. Ce que le tracker fournit

| Quoi | Où | Contient | Utile pour |
|---|---|---|---|
| **VGM** | tracker, menu Export, *Export VGM* | chaque écriture de registre du YM2612 avec son instant (VGM 1.50, avec point de boucle) | Mega Drive et tout lecteur de VGM : le son FM est exact |
| **Événements de notes, JSON** | *Export note events (JSON...)* | tempo, pistes, instruments FM en champs nommés, chaque note avec instant, durée, note MIDI et hauteur en Hz | votre lecteur ou convertisseur, dans n'importe quel langage |
| **Événements de notes, CSV** | *Export note events (CSV)* | les mêmes notes, une ligne chacune | tableurs, scripts rapides |
| **MIDI** (un fichier, ou un par piste) | *Export MIDI...* | notes, tempo, un canal MIDI par piste, batterie sur le canal 10 | DAW, éditeurs MIDI, importeurs et convertisseurs MIDI des trackers d'autres systèmes |
| **WAV** (un fichier, ou des stems, un par piste) | *Export WAV...* | le morceau tel que le Studio le joue, 44,1 kHz stéréo | musique en streaming, formats échantillonnés (MOD, XM), moteurs qui lisent des fichiers audio |
| **`.M8S`** | le morceau lui-même | le morceau tel que le Micro-8 le lit (§3) | lire le morceau sans Studio |
| **`.INS`** | un instrument | 54 octets, un instrument FM (§4) | réutiliser un son dans un autre lecteur YM2612 ou YM2151 |

Les effets du Studio (arpège, retrigger, délai, coupure, probabilité, swing, glide, vibrato et trémolo) font partie de
l'export : arpèges, retriggers et glides sont écrits comme des notes ordinaires, un portage n'a donc jamais à les connaître.
Le vibrato et le trémolo arrivent dans le VGM comme écritures de la puce, et dans le JSON sous `studio_fx`.

## 2. Comment le morceau est cadencé

- Un **pas** est une double croche. Le tempo est en battements par minute et un battement fait quatre pas : un pas dure
  `15 / tempo` secondes (120 BPM : 0,125 s). Le JSON le donne en `step_seconds`.
- Un **pattern** a 16 pas et contient 8 pistes. Un morceau est une liste de 128 positions au plus, chacune désignant un
  pattern (0 à 63) et une transposition en demi-tons. Un *pattern long* (16 à 256 pas) est fait de plusieurs patterns
  réservés les uns à la suite des autres.
- Les effets utilisent **6 ticks par pas**, de sorte que des notes tombent entre deux pas. Les instants des exports en
  tiennent déjà compte, swing compris.
- Chaque piste est **monophonique**, comme le canal de la puce : une nouvelle note termine la précédente. `duration_s` dans
  le JSON est le temps jusqu'au prochain événement de la piste, une extinction de note, ou la fin du morceau.
- Le morceau boucle quand `loops` est vrai. Le VGM porte le point de boucle (le début du morceau).

### Les 8 pistes

| Piste | Puce | Genre |
|---|---|---|
| FM1 à FM6 | YM2612 (6 canaux FM à 4 opérateurs) | mélodique, chacune joue un des 10 instruments |
| DRM | synthé numérique, voix 0 | 20 batteries échantillonnées (ou une voix de synthé) |
| SYN | synthé numérique, voix 1 | voix de synthé : carré, dent de scie, bruit, ou les 20 batteries |

Le YM2612 est la puce FM de la Mega Drive : un VGM des six pistes FM se joue tel quel sur une Mega Drive, un émulateur ou un
lecteur de VGM. DRM et SYN sont le synthé numérique propre au Micro-8 : ils ne sont pas dans le VGM, prenez-les dans le MIDI
ou le JSON.

## 3. Les notes

Une note dans un pattern est une valeur de 1 à 120 (255 est une extinction), avec un instrument (0 à 9) et une vélocité (1 à 127).

- `midi_note = valeur - 1 + décalage`, où le `décalage` est le décalage de tonalité du morceau (`transpose_offset` dans le
  JSON, en général 0). Les noms de notes du Studio comptent une octave de plus que le MIDI : son La-3 est le la de 110 Hz.
- Chaque piste FM a aussi un réglage d'**octave**. La hauteur que la puce joue est
  `hz = 440 * 2^((midi_note - 69) / 12 + (octave - 5))`. Le JSON donne cette fréquence en `hz` pour les pistes FM : un
  portage n'a pas besoin de la formule ; les exports MIDI envoient la note elle-même.
- **Batterie** (DRM et SYN en mode batterie) : la valeur 1 à 20 choisit le son. Le JSON la donne en `drum`, avec la note de
  percussion General MIDI en `gm_drum_note` : 1 grosse caisse acoustique (35), 2 électro (36), 3 techno (36), 4 caisse claire
  acoustique (38), 5 électro (40), 6 techno (40), 7 rim shot (37), 8 clap (39), 9 charleston fermé (42), 10 charleston électro
  fermé (42), 11 charleston ouvert (46), 12 électro ouvert (46), 13 charleston pied (44), 14 tambourin (54), 15 ride (51),
  16 crash (49), 17 tom (47), 18 clave (75), 19 conga (63), 20 cloche à vache (56).
- La **vélocité** baisse le niveau total des opérateurs porteurs (ceux de sortie) sur la puce, avec le volume de la voix et le
  volume général : `gain = volume/127 * général/127 * vélocité/128`, puis le niveau total des porteurs monte de
  `-20 * log10(gain) / 0,75`, plafonné à 127 (un pas de niveau total fait 0,75 dB).

## 4. Les instruments FM

Un fichier `.INS` ou un emplacement du morceau fait 54 octets. Le JSON les nomme (`fm_instruments`, par emplacement) :

| Octets | Contenu |
|---|---|
| 0 à 39 | les quatre opérateurs, 10 octets chacun, dans l'ordre opérateur 1, 2, 3, 4 du YM2612 |
| 40 | feedback (0 à 7) |
| 41 | algorithme (0 à 7) |
| 42 | profondeur d'amplitude du LFO (AMS, 0 à 3) |
| 43 | profondeur de fréquence du LFO (PMS, 0 à 7) |
| 44 | opérateurs actifs, un masque de 4 bits (0 est lu comme les quatre) |
| 45 | mode spécial (0 ou 1, le mode du canal 3 du YM2612 ; Studio ne s'en sert pas) |
| 46 à 53 | non utilisés par Studio |

Par opérateur, dans l'ordre : detune (0-7), multiple (0-15), niveau total (0-127), rate scaling (0-3), attack rate (0-31),
AM enable (0-1), decay rate (0-31), sustain rate (0-31), sustain level (0-15), release rate (0-15). Ce sont les valeurs du
YM2612 lui-même : rien à convertir sur une Mega Drive. Où elles tombent dans les registres du canal *c* de la banque *b*
(les décalages de registre des opérateurs 1 à 4 sont 0x00, 0x08, 0x04, 0x0C) :

| Registre | Valeur |
|---|---|
| 0x30 + décalage | detune << 4 \| multiple |
| 0x40 + décalage | niveau total (les porteurs reçoivent en plus l'atténuation de vélocité) |
| 0x50 + décalage | rate scaling << 6 \| attack rate |
| 0x60 + décalage | AM enable << 7 \| decay rate |
| 0x70 + décalage | sustain rate |
| 0x80 + décalage | sustain level << 4 \| release rate |
| 0xB0 + c | feedback << 3 \| algorithme |
| 0xB4 + c | gauche << 7 \| droite << 6 \| AMS << 4 \| PMS |
| 0xA4 + c, 0xA0 + c | bloc << 3 \| fnum haut, puis fnum bas |
| 0x28 | note on ou off : le masque d'opérateurs << 4, puis le canal (0 à 2 et 4 à 6) |

Le numéro de fréquence est `fnum = hz * 144 * 2^(21 - bloc) / horloge`, avec le plus petit bloc qui le garde sous 2048 et
l'horloge de la puce de la Mega Drive, 7 670 454 Hz (NTSC). Une console PAL (7 600 489 Hz) joue tout environ 1 % plus bas.

## 5. Recettes

**Mega Drive / Genesis.** Exportez le VGM. Jouez-le avec n'importe quel lecteur de VGM pour entendre ce que la console jouera.
Avec SGDK, donnez le VGM au compilateur de ressources (`XGM nom "morceau.vgm"` dans un fichier `.res` ; `xgmtool` fait la même
conversion à la main), puis appelez `XGM_startPlay(nom)`. La batterie et la piste SYN ne sont pas dans le VGM : laissez-les au
PSG et au DAC de votre jeu, ou remplacez-les par une batterie PCM jouée par le pilote XGM. Pour reconstruire le morceau dans un
tracker Mega Drive, lisez les instruments dans le JSON (les champs sont ceux du YM2612) et les notes dans le MIDI ou le JSON.

**NES.** Exportez le MIDI, un fichier par piste, et le JSON. FM1 et FM2 font de bons canaux carrés, FM3 le triangle, DRM le
bruit et le DPCM. Saisissez les parties dans votre tracker NES (ou scriptez à partir du JSON) et refaites les sons : les
instruments FM n'ont pas d'équivalent NES, utilisez `hz`, `velocity` et `duration_s` pour régler la hauteur et l'enveloppe de
volume de chaque note.

**C64.** Exportez le MIDI par piste, et le JSON. Le SID a 3 voix : choisissez les trois pistes qui portent le morceau (basse,
mélodie, batterie) et repliez le reste en arpèges (les notes d'arpège sont déjà dans les événements).

**Game Boy.** Exportez le MIDI par piste, et le JSON. Quatre canaux : deux carrés (mélodies), une onde (basse), du bruit
(batterie).

**Amiga et trackers MOD / XM.** Exportez les stems WAV (un par piste). Chargez-les comme échantillons, ou découpez une note de
chaque instrument dans le stem et rejouez-la aux notes du JSON.

**Unity, Godot, tout moteur qui lit de l'audio.** Exportez le WAV (ou les stems pour une musique adaptative) et bouclez-le.
`step_seconds` et `length_seconds` du JSON donnent la grille de battements pour caler le jeu sur la musique ; `loops` et la
durée disent où elle se répète.

**Votre propre lecteur.** Lisez le JSON. Un lecteur minimal qui transforme les événements en un enregistrement par note sur un
compteur de 60 images par seconde :

```python
import json
doc = json.load(open('song.events.json'))
FPS = 60
records = []
for e in doc['events']:
    if e['type'] != 'on':
        continue
    frame = round(e['time_s'] * FPS)
    length = round(e.get('duration_s', 0.1) * FPS)
    note = e.get('midi_note', e.get('gm_drum_note'))
    records.append((frame, e['track'], note, e['velocity'], length))
```

Triez par image, stockez les écarts entre images et écrivez un octet par champ : un lecteur 8 bits typique est une boucle qui
attend l'image suivante et démarre la note sur le canal du système. Pour les pistes FM, gardez `hz` et l'emplacement
d'instrument pour jouer le même son sur un pilote YM2612 ou YM2151.

## 6. Vérifier un portage

- Jouez le **WAV** à côté de votre portage : la durée, le tempo et le point de boucle du morceau doivent correspondre à
  `length_seconds`, `tempo_bpm` et `loops`.
- Le **VGM** est la référence du son FM : si un lecteur de VGM et votre portage ne s'accordent pas, c'est le portage qui a tort.
- Comparez les nombres de notes : le JSON a un événement `on` par note du morceau, le MIDI un note on par événement `on` de la
  même piste.
- Les effets sont développés dans tous les exports. Si une note manque dans votre portage, regardez si le morceau utilise la
  probabilité (effet `P`) : ses notes dépendent de la graine aléatoire du morceau, et un export garde le tirage qu'il a fait.
