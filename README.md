# armamentarium-combat-patrol-data

BattleScribe catalogue data for the fixed Combat Patrol rosters of Warhammer 40,000, read by the
[Armamentarium](https://github.com/LordTyberias/Armamentarium) project.

- `Combat Patrol.gst` — the game system: profile types, the categories, one force entry.
- 34 `.cat` files — one per faction, together holding **328 unit entries** and **74 rules rows**
  across **74 patrols**.

## What this is, and what it is not

A Combat Patrol is a fixed roster: it is chosen, not built. That shapes the data in four ways that
differ from an ordinary BattleScribe catalogue, and they are deliberate:

- **No points.** Combat Patrol datasheets do not carry point values, so no `costs` element is written.
  Inventing zeros would be inventing data.
- **The model count sits on the entry.** There is no `selectionEntryGroup` to pick a number from,
  because nothing is picked — each unit entry states `min = max = <model count>` directly.
- **Profiles keep their printed order.** The order models are listed in is content, not incidental.
- **Each patrol opens with a rules row that holds no models.** A printed Combat Patrol sheet carries
  three Stratagems and its Secondary Objective alongside the datasheets, and those belong to the patrol
  rather than to any one unit. There is no other carrier for them, so the patrol gets one entry stating
  `min = max = 0` — the same construction Spearhead uses for its Battle Traits (ADR 0042). It is not a
  unit and must not be counted as one: a reader that gives it a floor of one model invents a model that
  does not exist.

Each unit row is its own root `selectionEntry`; which patrol it belongs to is a category, alongside its
faction, its Imperium/Chaos/Xenos grouping and — where applicable — a `Legacy` marker.

## The category ids are a contract

The reader tells those four kinds of membership apart by the **id prefix**, never by the name — three of
them share names with unit keywords. Six Space Marine catalogues carry the same `cp-faction::` id, and
79 of the 317 unit entries name "Space Marines" as their group and "Imperium" as a keyword at once.

| Prefix | Meaning |
|---|---|
| `cp-cat::<patrol-slug>` | the patrol the entry belongs to; the slug repeats in the entry id |
| `cp-faction::<slug>` | the faction the units belong to — shared between catalogues of one faction |
| `cp-group::<slug>` | Imperium / Space Marines / Chaos / Xenos |
| `cp-flag::legacy` | the one flag, not a prefix: the patrol is no longer supported |
| `cp-kw::<slug>` | a unit keyword — everything the datasheet prints under "Keywords" |

Unit entry ids are `cp::<patrol-slug>::NN-<unit-slug>`, and a fixed wargear line is a mandatory child
entry under `<entry-id>::<slug>`. **Those ids are stored in users' saved army lists**, so changing one
strands the list that carries it.

The rules row is `cp::<patrol-slug>::0-patrol-rules`. The single digit is not a typo and not free
choice: the reader sorts a patrol's entries by id with an **ordinal** comparison to recover the printed
order, unit rows already start at `00-`, and renumbering them is what the sentence above forbids. `0-`
sorts ahead of `00-` because `-` (0x2D) precedes `0` (0x30). It carries the patrol, faction, group and
`Legacy` links like a unit row — the reader reads those four from the *first* entry, and this is now the
first — but **no `cp-kw::` keywords**, because it is not a unit and its keywords would be printed on a
datasheet. Its two profile types live in the `.gst` beside the other four:

| Profile type | Characteristics |
|---|---|
| `Stratagem` | `CP`, `When`, `Target`, `Effect` |
| `Secondary Objective` | `Type` (`Default` / `Optional` / `Only`), `Effect` |
| `Army Rule` | `Description` |
| `Detachment` | `DP`, `Rule`, `Force Disposition` |
| `Enhancement` | `Restriction`, `Effect` |

The last three arrived with the two 11th-edition patrols and are described under their provenance
entry below. A patrol carries the profile types its own printed sheet carries and no others: the 72
older patrols have `Stratagem` and `Secondary Objective`, the two newer ones have `Army Rule`,
`Detachment`, `Enhancement` and `Stratagem`.

`Only` is not a third state invented for tidiness. Three patrols have one objective and no alternative,
and both Knight sheets say so in as many words: "nor will you have a choice of secondary objectives".

Nothing in this repository enforces any of it. A prefix or an id that drifts produces no build error and
no failing file — it produces an empty patrol list in the application, which is why it is written down
here rather than left to be inferred from the data.

## This is not meant for other BattleScribe tools

The data is read losslessly by the Armamentarium importer; that is measured, over all 389 entries.

**It is not expected to be usable in the BattleScribe Data Editor, in New Recruit, or in any other
roster builder, and that is deliberate rather than an oversight.** The reasons are the three above: a
self-invented game system, entries without costs, and no selection groups to choose from. An editor
built for assembling a roster will load these files and find nothing to assemble.

What the format buys is not interoperability. It is that the data lives outside the application, in its
own repository, in plain XML that any text or XML tool can edit, on its own release cadence. If that is
what you need, help yourself — at your own risk.

## Provenance

The files were generated once, from the Armamentarium project's own JSON data, by a throwaway tool at
`tools/cp-migrate.cs`. **That tool and the data it read are both gone** — deleted together in ARMAM-70,
when the application stopped shipping its own copy and started reading this repository instead. There is
nothing left to regenerate from: these files are the source, and they are edited directly.

Everything added since then carries its own source, named here.

### Death Korps Combat Platoon (ARMAM-140, added 2026-08-14)

Transcribed from screenshots of the Warhammer 40,000 app's datasheets, then checked value by value
against a public Combat Patrol reference — no deviation over roughly eighteen weapon lines. The model
counts have a third, independent confirmation in the English Warhammer Community announcement.

### Masters of Terror, Remorseless Reavers, Warpsmith's Gauntlet, Preybane War Party (ARMAM-141, added 2026-08-14)

**These four have no official source, and that is stated here rather than glossed over.** Every route
was checked and every one came back empty: `warhammer40000.com/combat-patrol/` still carries the 2023
set of 24 faction PDFs; Wahapedia does not list them; neither of the two collected Combat Patrol rules
PDFs contains them; the German and English Warhammer Community announcements give box contents but no
datasheets; and Games Workshop does not currently offer these patrols in the Warhammer 40,000 app.

Their values therefore come from the public reference at
`akinnane.github.io/40kcheatsheet/combat-patrols-web.html`, which carries no provenance statement of
its own. What makes it usable here is a measurement rather than a claim: when ARMAM-140 transcribed a
patrol from official app screenshots, that same reference matched it exactly across every weapon line.
Its model counts were cross-checked again here against the German Warhammer Community announcement,
which agrees for the three patrols it covers; the Iron Warriors box is not in that article and has no
second source.

Three things about that reference are worth knowing before editing these four. Its **datasheet cards
are sorted alphabetically**, so the row order here comes from its header line, which is the printed
one. Its **prose inflects four model names differently from its own statline tables** (`7 Fellgor
Beastman`, `1 Red Corsairs Raiders`); the statline form was taken, matching this catalogue's convention
of a singular label for one model and a plural above that. And it writes **"Every X is equipped with"**
for a sub-group of several models — the model count for such a line comes from the unit composition
above it, not from the word "every".

Nothing was derived from regular-edition datasheets, and nothing was invented. If Games Workshop
publishes these four, the official values replace these.

### Five weapon values, and the Stratagems and Secondary Objectives (ARMAM-278, added 2026-09-02)

**The corrections.** A comparison against the official PDFs found one transcription error; a structural
check for values the rules cannot express — armour penetration is never positive, damage is never
negative — found four more that the PDF comparison could not see, because the PDFs print them the same
way. Each correction has a witness in the source itself:

| Catalogue | Weapon | | was | now | witness |
|---|---|---|---|---|---|
| Black Templars | Multi-melta | D | `6` | `D6` | the sheet prints `D6`; Salamanders carries it twice |
| Imperial Fists | Chainfist | D | `-2` | `2` | the catalogue holds it 17× with `2` |
| Imperial Fists | Power fist (Terminator) | D | `-2` | `2` | Captain Torreus, two pages earlier, has `D 2` |
| Iron Hands | Servo-arm | AP | `2` | `-2` | Adeptus Mechanicus carries the same arm at `-2` |
| White Scars | Bellicatus – Icarus | AP | `1` | `-1` | the `krak` line directly beneath keeps its minus |

Only the first is this repository's error. The other four reproduce a typesetting slip in the official
PDF, and they are corrected against the rules rather than kept faithful, because a damage of `-2` is not
a value the game can resolve. Four torrent weapons were also moved from `-` to `N/A`, the spelling the
other 49 use; all four sat in the patrols transcribed from the public reference and carried its habit.

**The Stratagems and Secondary Objectives.** 216 Stratagems (three per patrol) and 141 Secondary
Objectives now sit in the rules row described above. They were extracted from the same public reference
as the ARMAM-141 patrols and then checked against the official PDFs, which win wherever the two differ.

Of the 72 patrols, 52 could be checked mechanically against a PDF text layer and 8 more were read on the
rendered page or on an official app screenshot. That found two errors in the reference, both corrected
here: `Prescribed Excoriatian` is spelled `Prescribed Excoriation` on the sheet, and the Stratagem the
reference calls `Overwhelming Force` is headed `Duty and Honour` — that name appears in the sheet's
flavour text, not as its title, and its effect is written out where the reference abbreviates it. It
also found a gap: the reference lists only Default/Optional *pairs* and therefore carries nothing at all
for the three patrols that have a single objective and no choice. Those three were read off the source
and added.

**What is not checked.** The wording of the remaining 20 patrols rests on the reference alone — 12 of
their PDFs have no text layer, 4 have no official source at all (the four above), and the rest are image
sets. Six further PDFs are OCR scans whose text layer drops the CP figures and mangles the headings;
their Stratagems were confirmed on the rendered page instead. `Phase` and `Turn`, which the reference
carries in addition, are deliberately left out: the sheets do not print them as fields, three of the 216
are empty, and every characteristic becomes a column in the rendered datasheet.

## Licence

MIT, see `LICENSE`.

Warhammer 40,000 and Combat Patrol are trademarks of Games Workshop Limited. This repository is
unofficial and unaffiliated; it contains no Games Workshop artwork and claims no rights in their
intellectual property.

### 'Ardmob and Assault Force (ARMAM-341, added 2026-09-08)

The two Combat Patrols that came with the 11th-edition launch box *Armageddon*. Games Workshop names
them in as many words: *"you already have two Combat Patrols, in the form of the Ork 'Ardmob and the
Space Marines Assault Force"*. They also ship in the *Getting Started* sets and the *Warhammer
40,000 Starter Set*, so they are not tied to the launch box.

**Transcribed from the Warhammer 40,000 app**, from screen recordings of every datasheet with all
sections expanded, plus single screenshots for the four sections a recording had missed. The
Armageddon datasheet-card PDFs that Games Workshop published for free were used as a cross-check and
then set aside: **they are not these datasheets.** Measured on the Intercessors, the Combat Patrol
sheet has 5 models where the box sheet has 10, OC 3 against 2, a different ability, and different
grenade launcher profiles. A Combat Patrol datasheet of this edition is its own, simplified sheet.

Nothing else carries them. The Games Workshop download index was read in full (1789 entries, all
languages): its 128 Combat Patrol downloads are the 2024 set and stop there. The public reference
that ARMAM-141 used lists 209 patrol titles and neither of these two. Wahapedia carries 11th-edition
unit datasheets but no Combat Patrol sheets.

**These two are the first patrols of the 11th edition, and their sheet is shaped differently.** It
carries no Secondary Objective. In its place stand an army rule, a detachment with a points cost and
a force disposition, two enhancements and three stratagems — which is why the rules row grew three
profile types. Everything else about the format is unchanged.

Two things are worth knowing before editing them. The Ork patrol has **two datasheets of the same
name** (`'Ardmob Boyz`), told apart by their wargear and by their entry id, exactly as the app
presents them; renaming one would invent a name Games Workshop does not use. And their `Shoota`
profiles **differ**: the second sheet marks it [RAPID FIRE 1], the first marks nothing. That was
read twice from separate screenshots. It is carried as it stands — whether it is deliberate or an
error on their side is not something this data can decide.
