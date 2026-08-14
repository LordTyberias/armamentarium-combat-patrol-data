# armamentarium-combat-patrol-data

BattleScribe catalogue data for the fixed Combat Patrol rosters of Warhammer 40,000, read by the
[Armamentarium](https://github.com/LordTyberias/Armamentarium) project.

- `Combat Patrol.gst` — the game system: profile types, the categories, one force entry.
- 34 `.cat` files — one per faction, together holding **301 unit entries** across **68 patrols**.

## What this is, and what it is not

A Combat Patrol is a fixed roster: it is chosen, not built. That shapes the data in three ways that
differ from an ordinary BattleScribe catalogue, and they are deliberate:

- **No points.** Combat Patrol datasheets do not carry point values, so no `costs` element is written.
  Inventing zeros would be inventing data.
- **The model count sits on the entry.** There is no `selectionEntryGroup` to pick a number from,
  because nothing is picked — each unit entry states `min = max = <model count>` directly.
- **Profiles keep their printed order.** The order models are listed in is content, not incidental.

Each unit row is its own root `selectionEntry`; which patrol it belongs to is a category, alongside its
faction, its Imperium/Chaos/Xenos grouping and — where applicable — a `Legacy` marker.

## The category ids are a contract

The reader tells those four kinds of membership apart by the **id prefix**, never by the name — three of
them share names with unit keywords. Six Space Marine catalogues carry the same `cp-faction::` id, and
79 of the 301 entries name "Space Marines" as their group and "Imperium" as a keyword at the same time.

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

Nothing in this repository enforces any of it. A prefix or an id that drifts produces no build error and
no failing file — it produces an empty patrol list in the application, which is why it is written down
here rather than left to be inferred from the data.

## This is not meant for other BattleScribe tools

The data is read losslessly by the Armamentarium importer; that is measured, over all 297 entries.

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

## Licence

MIT, see `LICENSE`.

Warhammer 40,000 and Combat Patrol are trademarks of Games Workshop Limited. This repository is
unofficial and unaffiliated; it contains no Games Workshop artwork and claims no rights in their
intellectual property.
