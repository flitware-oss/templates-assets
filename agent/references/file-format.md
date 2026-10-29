# `.flittemplate` file format (v1)

JSON envelope. Record identity, tenant, audit, status and manifest never
travel — the importer and backend hooks re-derive them.

```jsonc
{
  "format": "flitware-template",
  "formatVersion": 1,
  "exportedAt": "2026-10-29T00:00:00.000Z",
  "engine": { "name": "pdfme", "version": "6.2.1" },
  "source": { "rootCollection": "fleets" },
  // optional: portable custom-field definitions the variables need
  "customFields": [
    { "key": "capacidad", "label": "Capacidad del vehículo",
      "description": "Ej.: \"19 pasajeros\"",
      "type": "short_text", "config": { "max_length": 60 } }
  ],
  "template": {
    "name": "Carta de aceptación de vehículo",
    "description": "…",
    "root_collection": "fleets",
    "template_type": "PDF",
    "definition": {
      "basePdf": { "width": 215.9, "height": 279.4, "padding": [10, 10, 10, 10] },
      "schemas": [[ /* one page = array of schemas */ ]]
    },
    "settings": {
      "filenamePattern": "carta-aceptacion-$collection.plate",
      "pageSize": "Letter",
      "orientation": "portrait",
      "templateFormatVersion": 1,
      "engine": "pdfme",
      "engineVersion": "6.2.1",
      "allowPrint": true,
      "allowDownload": true,
      // optional: $input declarations (see input-variables.md)
      "inputs": [
        { "key": "ciudad", "label": "Ciudad de expedición",
          "type": "short_text", "required": true,
          "config": { "max_length": 100 } }
      ]
    }
  }
}
```

## Schema elements

One array entry per page in `definition.schemas` (multi-page allowed).
Sizes in **mm**. Every schema needs a unique `name` and finite
`width`/`height`. Allowed types: `text`, `line`, `list`, `table`,
`image` (Data URL only — prefer none), `signature`, `qrcode`. Unknown
`type` rejects the file; `readOnly` expressions are rejected;
`fontName` other than default warns. Full catalog with verified
examples: [components.md](components.md).

Page geometry: `A4` (210 × 297) or `Letter` (215.9 × 279.4), portrait or
landscape via `basePdf` dimensions; `settings.pageSize` + `orientation`
must match. See [layout-system.md](layout-system.md).

## Custom fields (envelope `customFields`)

Portable subset per referenced `$collection.custom_fields.<key>`: `key`,
`label`, `description?`, `type`, `required?`, `config?` (constraints +
static options). The importer offers to create missing ones (opt-in).
Never include `catalog_id` or `source: "master_catalog"`.

## Limits (fail-closed)

- Whole file ≤ 768 KB; `definition` ≤ 512 KB; `settings` ≤ 16 KB.
- `customFields` ≤ 50 entries; `settings.inputs` ≤ 50 declarations.
- Labels ≤ 120 chars, input descriptions ≤ 500, option codes
  `^[A-Za-z0-9_-]{1,64}$`, ≤ 200 static options per select.
- Declaration keys `^[a-z][a-z0-9_]{1,63}$` (same for custom-field keys).
