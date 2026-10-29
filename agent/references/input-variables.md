# `$input` variables — declarations guide

Every `$input.<key>` used in the definition or `filenamePattern` must be
declared in `settings.inputs`. Undeclared keys invalidate the file.

## Declaration shape

```jsonc
{ "key": "destinatario",
  "label": "Destinatario",
  "description": "Una línea por renglón: nombre, cargo, entidad, ciudad.",
  "type": "long_text",
  "required": true,
  "config": { "max_length": 500 } }
```

- `key`: `^[a-z][a-z0-9_]{1,63}$`, unique per template.
- `label`: required, ≤ 120 chars. `description`: ≤ 500 chars.
- `required`: default `false`. Required inputs block generation until
  answered (this fixes the classic "Señores null" bug: never let a
  recipient render empty — declare it required).
- `default_value`: optional pre-fill, validated against the type
  (a bad default rejects the file). Example: `motivo` defaults to
  `"TRASPASO"`.

## Types (same as custom fields)

`short_text`, `long_text` (`max_length`, `pattern`), `number` (`min`,
`max`, `decimals`), `date` (renders "29 de octubre de 2026"),
`datetime`, `boolean` (renders Sí/No), `single_select`, `multi_select`
(`source: "static"`, `options: [{code, label, active}]`, `max_items`).

Selects render the option **code**, and codes must match
`^[A-Za-z0-9_-]{1,64}$` (no spaces/accents). Prefer `short_text` with a
description of examples when the rendered phrase needs spaces
(see `tipo_contrato` in the drivers carta-laboral). Never
`source: "master_catalog"` or `catalog_id`: catalog links cannot travel
and reject the file.

## Conventions (Colombia)

| key | type | required | notes |
|---|---|---|---|
| `ciudad` | `short_text` (≤100) | yes | Expedition city, with `$system.date` |
| `destinatario` | `long_text` (≤500) | yes | One line per row: name, title, entity, city |
| `motivo` | `short_text` (≤100) | no, default `"TRASPASO"` | Paz y salvo validity |
| `valor_ingresos` | `number` (`min` 0, `decimals` 0) | yes | Renders raw (`1500000`) |
| `valor_letras` | `long_text` (≤500) | yes | Amount spelled out |

## Semantics (for accurate docs)

Effective value = provided answer over `default_value`. Missing required
→ unresolved gap; missing optional → `""` (no gap). Bad values degrade
to gaps, never failed renders. Values apply to one generation and are
never stored or logged. `$input.*` also works in `filenamePattern`.
MCP resolve validates declarations but cannot pass answers yet
(defaults resolve, missing required surface as warnings).
