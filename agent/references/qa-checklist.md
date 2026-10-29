# QA checklist (every file, every publish)

- [ ] Parses as `COMPATIBLE` with zero errors and zero warnings
      (`parseTemplateImportFile`: envelope, versions, schemas, settings).
- [ ] `root_collection` == containing directory; `pageSize` + `orientation`
      match `basePdf` geometry (`A4` 210 × 297, `Letter` 215.9 × 279.4).
- [ ] Every `$input.<key>` referenced is declared in `settings.inputs`;
      every `$collection.custom_fields.<key>` ships in `customFields`.
- [ ] No `master_catalog`, no `catalog_id`, no `readOnly`, no remote
      images, no `fontName`, no unknown schema `type`.
- [ ] Overflow math passes on every text box (layout-system.md);
      `overflow: "expand"` only on last-in-page blocks.
- [ ] Schema `name`s unique per page; positions/widths finite and inside
      margins; signature/footer blocks clear of the bottom edge.
- [ ] `table` content parses as JSON after sample substitution;
      `headWidthPercentages` sums to 100; `list` content is a JSON array
      or `\n` lines; `qrcode` content non-empty; images are Data URLs.
- [ ] Multi-page files repeat the header block (title + record key) and
      number pages statically.
- [ ] Sanitization grep clean (sanitization.md); `description` notes any
      tenant-placed artwork the original had.
- [ ] `filenamePattern` resolves per record (`…-$collection.plate` /
      `…-$collection.document_number` / `…-$input.<key>`); no two examples
      share a slug.
- [ ] Country index updated (`examples/<country>/README.md` table row +
      inputs/custom-fields columns).
- [ ] Round-trip: export-shaped JSON re-parses identically
      (`buildTemplateExport` → `parseTemplateImportFile`).
