# QA checklist (every file, every publish)

- [ ] Parses as `COMPATIBLE` with zero errors and zero warnings
      (`parseTemplateImportFile`: envelope, versions, schemas, settings).
- [ ] `root_collection` == containing directory; `pageSize: "Letter"`;
      `basePdf` 215.9 × 279.4; engine `pdfme 6.2.1`.
- [ ] Every `$input.<key>` referenced is declared in `settings.inputs`;
      every `$collection.custom_fields.<key>` ships in `customFields`.
- [ ] No `master_catalog`, no `catalog_id`, no `readOnly`, no remote/empty
      images, no `fontName`, no unknown schema `type`.
- [ ] Overflow math passes on every text box (letter-layout.md).
- [ ] Schema `name`s unique per page; positions/widths finite; signature
      block above y = 268.
- [ ] Sanitization grep clean (sanitization.md); `description` mentions
      the tenant adds their own logo when the original had one.
- [ ] `filenamePattern` resolves per record (`…-$collection.plate` /
      `…-$collection.document_number`); no two examples share a slug.
- [ ] Country index updated (`examples/<country>/README.md` table row +
      inputs/custom-fields columns).
- [ ] Round-trip: export-shaped JSON re-parses identically
      (`buildTemplateExport` → `parseTemplateImportFile`).
