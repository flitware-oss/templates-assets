---
name: flitware-template-author
description: Author sanitized .flittemplate examples of any document kind for the Flitware templates-assets repo (components, variables, layout, QA).
---

# Flitware Template Author

Write portable Flitware template files (`.flittemplate`) for `examples/`,
for **any document kind**: official letters, certificates, tabular
reports, forms, labels, receipts, QR passes — whatever the source
requires. Every file must import cleanly (`COMPATIBLE`, zero diagnostics),
render without clipped text, and contain **zero real data**.

## When to use this skill

- Creating a new example from a source document (scan, photo, PDF, text).
- Extending an example (new country, new collection, new `$input`, new kind).
- Reviewing a `.flittemplate` before publish.

## Inputs you need

1. The source document (layout + fixed texts).
2. Country (`colombia` today; `peru`, `brazil` are placeholders).
3. Root collection (directory name = `root_collection`, e.g. `fleets`).
4. Document kind (letter, certificate, report, form, label, mixed…).

## Workflow

1. **Identify the kind and components.** Read the source and pick from the
   verified catalog ([components.md](references/components.md)): `text`
   blocks, `table` or text grids, `list` annexes, `line` rules/frames,
   `image` art, `qrcode` verification, `signature` placeholders.
2. **Map the data** ([variables.md](references/variables.md)). Record data
   → `$collection.*`; company data → `$tenant.*`; generation date →
   `$system.*`; per-document answers → `$input.*` with declarations
   ([input-variables.md](references/input-variables.md)); fixed legal
   prose → static text (normalize obvious typos).
3. **Lay out the page** ([layout-system.md](references/layout-system.md)):
   size + orientation, margins grid, then the kind recipe when one fits
   (`recipe-*.md`) or a custom grid otherwise. Run the overflow math on
   every text box.
4. **Write the file** per [file-format.md](references/file-format.md):
   `examples/<country>/<collection>/<slug>.flittemplate`.
5. **Sanitize** per [sanitization.md](references/sanitization.md): the file
   must read as a blank form — variables and static prose only.
6. **QA** per [qa-checklist.md](references/qa-checklist.md), then update the
   country index (`examples/<country>/README.md`).

## Hard rules

- Page size matches the file: `A4` (210 × 297) or `Letter` (215.9 × 279.4),
  `portrait` or `landscape`; Colombia examples stay Letter unless the kind
  demands otherwise (wide reports may use landscape).
- `$input.*` and `$collection.custom_fields.*` references must be covered:
  every `$input` key declared in `settings.inputs`, every custom key shipped
  in envelope `customFields`.
- Static select options only. Never a `master_catalog` source or a
  `catalog_id` anywhere in the file.
- No remote artwork, no real logos, no custom fonts (Roboto default).
- Table `content` must be valid JSON after substitution; list content a
  JSON array or newline lines; `qrcode` content a non-empty string.
- Overflow math passes on every text box; multi-page files repeat their
  header block per page.

## References

- [file-format.md](references/file-format.md) — envelope, settings, limits.
- [variables.md](references/variables.md) — the four namespaces + mapping rules.
- [input-variables.md](references/input-variables.md) — declarations, types, defaults.
- [components.md](references/components.md) — verified schema catalog + examples.
- [layout-system.md](references/layout-system.md) — sizes, grid, overflow math, multi-page.
- [recipe-official-letters.md](references/recipe-official-letters.md) — Colombia letters skeleton.
- [recipe-certificates.md](references/recipe-certificates.md) — centered certificates + QR.
- [recipe-reports.md](references/recipe-reports.md) — tabular reports and lists.
- [sanitization.md](references/sanitization.md) — fictional-data policy.
- [colombia-fleets.md](references/colombia-fleets.md) — the 9 fleets docs, mapped.
- [qa-checklist.md](references/qa-checklist.md) — pre-publish verification.
