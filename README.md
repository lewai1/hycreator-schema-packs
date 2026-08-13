# HyCreator schema packs

The registry of **schema packs** — a mod's custom asset kinds as shareable data.
Install one in [HyCreator](https://github.com/lewai1/HyCreator) and that mod's assets get
typed forms, docs, dropdowns and New-Asset wizard cards, **without the mod's sources**.
Pure data: a pack cannot execute anything.

## Install

In HyCreator: **Settings → Sources → Schema packs → Browse packs…** — every pack in this
repo, one click to install, update or uninstall.

Someone sent you a `.schemapack.json` directly? **Add from file…** in the same place.

## Publish your mod's pack

1. Open your mod (the one whose Java registers asset stores) in HyCreator.
2. `Ctrl+Shift+P` → **Export schema pack…** (also in the editor's ⋯ menu).
3. PR this repo: add `packs/<ModId>/<ModId>.schemapack.json` and an entry in `index.json`.

That's it — once merged, every HyCreator user sees it in Browse packs.

## Format

A pack is one JSON file:

| field | meaning |
|---|---|
| `formatVersion` | The exporter's format. Readers refuse packs **newer** than they understand, by name — bumping this is how the format evolves safely. |
| `mod.id` / `mod.name` / `mod.version` | What the pack describes. Re-installing a pack with the same `mod.id` replaces the previous one — that is the update path. |
| `mod.gameVersions` | The manifest's server range, e.g. `>=0.6.0-pre.11 <0.7.0`. Informational: shown so a stale pack is visible at a glance. |
| `kinds[]` | Each kind ties a `Server/`-relative store folder to typed fields (`className`, `path`, `ext`, `fields`). |
| `defs{}` | Nested classes the fields reference (`into` / `iinto` / `map`), as form rows — arrays of objects stay fully typed. |

Field rows carry `name`, `vtype` (`string` / `number` / `integer` / `boolean` / `array` / `object`),
`required`, optional `doc`, enum `vals`, and nesting refs. Docs travel clean — HyCreator adds
provenance (`from X.java in Mod`) at install time.

`index.json` lists every pack for the in-app browser: `id`, `name`, `description`, `modUrl`
(the mod's page — GitHub, CurseForge, Modrinth, Modtale…), `modVersion`, `gameVersions`,
`kinds` (count) and `path` (repo-relative pack file).

## Versioning contract

- **Pack format** (`formatVersion`): owned by HyCreator's exporter. Old packs stay readable forever;
  a pack from a newer format asks the user to update the app instead of mangling.
- **Mod version** (`mod.version`): bump when you re-export after changing your mod's codecs.
  Browse packs shows an Update button when the registry carries a newer version than the installed pack.
