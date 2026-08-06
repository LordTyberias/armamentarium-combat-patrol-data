# armamentarium-combat-patrol-data

BattleScribe catalogue data for the fixed Combat Patrol rosters of Warhammer 40,000, generated from the
data files of the [Armamentarium](https://github.com/LordTyberias/Armamentarium) project.

- `Combat Patrol.gst` — the game system: profile types, the categories, one force entry.
- 34 `.cat` files — one per faction, together holding **297 unit entries** across **67 patrols**.

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

## This is not meant for other BattleScribe tools

The data is read losslessly by the Armamentarium importer; that is measured, over all 297 entries,
against the rendered datasheets of the source data.

**It is not expected to be usable in the BattleScribe Data Editor, in New Recruit, or in any other
roster builder, and that is deliberate rather than an oversight.** The reasons are the three above: a
self-invented game system, entries without costs, and no selection groups to choose from. An editor
built for assembling a roster will load these files and find nothing to assemble.

What the format buys is not interoperability. It is that the data lives outside the application, in its
own repository, in plain XML that any text or XML tool can edit, on its own release cadence. If that is
what you need, help yourself — at your own risk.

## Provenance and regeneration

The files are generated, not hand-written. The generator lives in the Armamentarium repository at
`tools/cp-migrate.cs` and is deterministic — identifiers are hashes over stable strings, files are
written in a fixed order — so a regeneration that produces different bytes means the source data
changed, not the run.

## Licence

MIT, see `LICENSE`.

Warhammer 40,000 and Combat Patrol are trademarks of Games Workshop Limited. This repository is
unofficial and unaffiliated; it contains no Games Workshop artwork and claims no rights in their
intellectual property.
