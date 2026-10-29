---
name: flitware-template-author
description: Author sanitized, Letter-size .flittemplate examples for the Flitware templates-assets repo (variables, $input, layout, QA).
---

# Flitware Template Author

Write portable Flitware template files (`.flittemplate`) for `examples/`.
Every file you produce must import cleanly (`COMPATIBLE`, zero diagnostics),
render on **Letter** (215.9 × 279.4 mm) without clipped text, and contain
**zero real data** (all companies, people, plates, IDs, phones are fictional).

## When to use this skill

- Creating a new example from a source document (scan, photo, PDF, text).
- Extending an existing example (new country, new collection, new `$input`).
- Reviewing a `.flittemplate` before publish.

## Inputs you need

1. The source document (what the paper looks like + its fixed texts).
2. Country (`colombia` today; `peru`, `brazil` are placeholders).
3. Root collection (directory name = `root_collection`, e.g. `fleets`).

## Workflow

1. **Map the document.** Read the source top to bottom. For every data
   point decide its kind (see [variables.md](references/variables.md)):
   - Record data → `$collection.*` (fields, relations, `custom_fields.*`).
   - Company data → `$tenant.*` (name, NIT, representative, contacts).
   - Generation date → `$system.date`.
   - Per-document answers (recipient, city, amounts, motive) → `$input.*`
     with a declaration (see [input-variables.md](references/input-variables.md)).
   - Fixed legal prose → static text (normalize obvious typos).
2. **Lay out Letter.** Follow the coordinate recipe in
   [letter-layout.md](references/letter-layout.md). Run the overflow math
   on every text box; never ship a box the estimate overflows.
3. **Write the file** per [file-format.md](references/file-format.md):
   `examples/<country>/<collection>/<slug>.flittemplate`.
4. **Sanitize** per [sanitization.md](references/sanitization.md). The file
   must read as a blank form: variables and static prose only, no sample
   persons/companies/plates/phones.
5. **QA** per [qa-checklist.md](references/qa-checklist.md), then update the
   country index (`examples/<country>/README.md`).

## Hard rules

- Letter only for Colombia (`basePdf` 215.9 × 279.4, `pageSize: "Letter"`).
- `$input.*` and `$collection.custom_fields.*` references must be covered:
  every `$input` key declared in `settings.inputs`, every custom key shipped
  in envelope `customFields`.
- Static select options only. Never a `master_catalog` source or a
  `catalog_id` anywhere in the file.
- No logos, no remote images, no custom fonts (Roboto default).
- Keep the shared vehicle-table mapping for Colombia fleets docs (see
  [colombia-fleets.md](references/colombia-fleets.md)) so the family stays
  consistent.

## References

- [file-format.md](references/file-format.md) — envelope, settings, limits.
- [variables.md](references/variables.md) — the four namespaces + mapping rules.
- [input-variables.md](references/input-variables.md) — declarations, types, defaults.
- [letter-layout.md](references/letter-layout.md) — coordinates, fonts, overflow math.
- [sanitization.md](references/sanitization.md) — fictional-data policy.
- [colombia-fleets.md](references/colombia-fleets.md) — the 9 fleets docs, mapped.
- [qa-checklist.md](references/qa-checklist.md) — pre-publish verification.
