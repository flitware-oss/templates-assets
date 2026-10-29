# Variable namespaces

Tokens are bare `$namespace.path` variables embedded in text. A token
needs at least one dot; `$name` alone is not a variable. Unknown
namespaces and undeclared paths fail validation (`INVALID` on import,
`400`/`422` from the API).

## `$collection.*` — the record being printed

Root collection fields plus one relation hop (`$collection.brand.name`).
Relations expose the target's safe fields (`name`, `code`, ...).
`tenant`, audit and internal fields are blocked.

Colombia `fleets` fields you will use most:

| Doc label | Variable |
|---|---|
| PLACA | `$collection.plate` |
| CLASE | `$collection.type.name` |
| MARCA | `$collection.brand.name` |
| MODELO | `$collection.model` |
| No. MOTOR | `$collection.engine_number` |
| No. CHASIS | `$collection.vin_number` |
| SERVICIO | `$collection.service_type.name` |
| COMBUSTIBLE | `$collection.fuel_type.name` |
| PROPIETARIO | `$collection.owner` |
| CC/NIT propietario | `$collection.owner_document_number` |
| Tipo doc. propietario | `$collection.owner_document_type.name` |

Colombia `drivers` fields: `$collection.name`, `$collection.document_number`,
`$collection.document_type.name`, `$collection.email`, plus custom fields.

Missing schema data (e.g. vehicle `capacidad`) →
`$collection.custom_fields.<key>` + a portable definition in envelope
`customFields` (see file-format.md). The importer asks to create it.

## `$tenant.*` — the company

`name`, `legal_tax_id` (NIT), `legal_representative`, `address`, `phone`,
`email`, plus `company.*` (→ `transport_companies`: `name`, `cif_nif`,
`document_number`, `legal_representative`). Use for letterhead data,
"NIT …" lines and signature blocks. `logo` is a file field: do not
reference it as a variable — tenants place their own logo image in the
Designer.

## `$system.*` — generation time

`$system.date` (e.g. "29 de octubre de 2026" in `es`) and
`$system.datetime`. No declaration needed. Date lines read
`"$input.ciudad, $system.date"`.

## `$input.*` — asked at generate time

`$input.<key>` (exactly two segments), declared in `settings.inputs`.
Use when the answer changes per document: recipient, city, bank, amounts,
motive. Required inputs block generation until answered; unanswered
required inputs render as gaps. Full guide: [input-variables.md](input-variables.md).

## Mapping rules

1. Prefer record/company data over `$input`: only what truly varies per
   document becomes an input.
2. Prefer `$input` over custom fields when the data is not worth storing
   on the record (recipient of one letter vs. the vehicle's capacity).
3. Keep `$input` keys stable across the country family (`ciudad`,
   `destinatario`) so tenants learn them once.
4. Legal boilerplate (decree citations, fixed paragraphs) stays static;
   normalize obvious source typos.
