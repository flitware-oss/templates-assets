# Letter layout recipe (Colombia)

Page: `basePdf {width: 215.9, height: 279.4, padding: [10,10,10,10]}`.
Content column: `x = 20`, `width = 175.9`. Font: Roboto default
(no `fontName`, no bold — emphasis via size and caps).

## Official-letters skeleton (all 9 fleets docs)

| Block | y | h | size | align | content |
|---|---|---|---|---|---|
| date | 15 | 8 | 10 | left | `$input.ciudad, $system.date` |
| recipient | 26 | 26 | 10 | left | `Señores\n$input.destinatario` (`Señor` for personal addressees) |
| subject | 55 | 12 | 12 | left | `ASUNTO: …` (may embed `$collection.plate`) |
| greeting | 70 | 8 | 10 | left | `Cordial saludo.` |
| body | 80 | 30–48 | 11 | justify | Company + record + input prose |
| table title? | — | 8 | 11 | left | e.g. `DESCRIPCIÓN DEL VEHÍCULO:` |
| vehicle rows | — | 7 each | 10 | left | Label `x=20 w=50` + value `x=72 w=123.9` |
| closing | — | 8 | 10 | left | `Cordialmente,` |
| signature | 240 | — | — | — | `line` (`x=20 w=80`) + name 11 / role 10 / company 10 |

Keep ≥ 8 mm between the last block and the signature rule, and end the
signature block above y = 268. Double signature (desvinculación): second
column at `x = 120` (`line w=75` + owner name + "Propietario del vehículo").

Vehicle table (11 rows, pitch 7 mm — keep this order everywhere):
PLACA `$collection.plate`, CLASE `$collection.type.name`, MARCA
`$collection.brand.name`, MODELO `$collection.model`, CAPACIDAD
`$collection.custom_fields.capacidad`, No. MOTOR
`$collection.engine_number`, No. CHASIS `$collection.vin_number`,
SERVICIO `$collection.service_type.name`, COMBUSTIBLE
`$collection.fuel_type.name`, PROPIETARIO `$collection.owner`, CC/NIT
`$collection.owner_document_number`.

## Overflow math (mandatory per text box)

pdfme does not grow boxes: estimate lines and fit height with margin.

```text
chars_per_line ≈ width_mm / (fontSize_pt × 0.19)
lines = Σ over "\n"-parts of max(1, ceil(len / chars_per_line))
needed_mm = lines × fontSize_pt × 0.45
require: box_height ≥ needed_mm × 1.2
```

(175.9 mm at 11 pt ≈ 84 chars/line ≈ 5 mm/line.) Count variable
tokens at their *rendered* length, not the `$…` length, when the value
is long (company names, multiline recipients). If the estimate fails,
raise the height or split the paragraph — never ship a tight box.

## Logo

Source letters show the company logo top-right. Templates ship **without**
it (no client artwork travels): the tenant adds their own `image` in the
Designer. Note it in the file `description` when the original had one.
