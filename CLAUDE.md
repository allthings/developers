# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A **public**, Markdown-only repository holding specifications we give to people
outside Allthings. It is the organisation's only public docs repo — every other
Allthings repo is private, so anything an external reader must be able to open
has to live here.

There is no code, no build, no generator, no CI, no website. GitHub's own
Markdown rendering *is* the publishing mechanism. Do not add a static site
generator, a `package.json`, or a Pages workflow without an explicit decision to
do so — the previous incarnation (a Gatsby 1.x site in the now-archived
`allthings/documentation`) died precisely because nobody could run the build.

## Structure

Flat, by design. One file per specification, at the repository root.

```
README.md                            index; keep the contents table current
CLAUDE.md                            this file
erp-data-exchange-specification.md   the spec
```

Rules for adding a document:

- **Root level, `kebab-case.md`.** No `docs/` directory, no per-topic
  subdirectories — the file paths are public URLs and shallow ones age better.
- **Only if the audience is external.** Internal documentation belongs in the
  repo that owns the code it describes (e.g. `connect/docs/`, `nisaba/doc/`).
  A doc here that only Allthings engineers read is a doc in the wrong place.
- **Add a row to the README contents table**, naming the audience and the
  service that implements it.
- Prefer one long document with stable heading anchors over several small
  files. Splitting a spec breaks deep links (see below).

## Paths and anchors are frozen

File paths and heading anchors in this repo are load-bearing public URLs.
`erp-data-exchange-specification.md` in particular is linked from:

- `nisaba/src/notification/channels/failureEmail.ts` — a **customer-facing
  email** sent to external data providers when an import fails
- `nisaba/doc/nisaba-config.md`, `nisaba/doc/nisaba-new-import.md`,
  `nisaba/doc/garaio-rabbitmq-testing.md`
- `nisaba/src/tools/exporter/grem/mapper.ts` and its test

Those links depend on the anchors `#data-types`, `#manifestjson` and
`#examples`. Do not rename the file, do not rename or reorder those headings,
and do not split the document. If a rename is genuinely required, update every
inbound reference in `nisaba` in the same change.

## Keeping the ERP spec honest

The spec must match what `nisaba` actually accepts. The canonical list of
importable files is `VALID_IMPORT_FILENAMES` in `nisaba/src/config.ts`, and the
resource enum is `VALID_UUID_REMAPPING_RESOURCES` in the same file. When either
changes, the spec changes in the same pull request.

The spec previously advertised 14 CSV files including `collections.csv` and
`collectionAssignments.csv`, which `nisaba` never implemented. That drift was
removed in 2026-08; do not reintroduce documentation for unimplemented files.
