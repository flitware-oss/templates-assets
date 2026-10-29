# Colombia `fleets` — the 9 documents, mapped

All share the official-letters skeleton (letter-layout.md), the 11-row
vehicle table, and `ciudad` + `destinatario` inputs. Deltas only:

1. **carta-aceptacion** — `Señores` + multiline destinatario (the source
   rendered a literal `null` here: required `$input` fixes that class of
   bug). Body: acceptance into the fleet.
2. **certificado-disponibilidad-capacidad-transportadora** — personal
   address (`Señor`). Body: requests the capacity certificate.
3. **certificado-ingresos** — no vehicle table. Owner-focused paragraph
   (`$collection.owner`, `$collection.owner_document_type.name`,
   `$collection.owner_document_number`, `$collection.type.name`,
   `$collection.plate`) + `$input.valor_ingresos` (number) and
   `$input.valor_letras` + validation paragraph with `$tenant.phone` /
   `$tenant.email`. No custom fields.
4. **desistimiento-capacidad-transportadora** — adds an `ANEXOS` title +
   static bullet paragraph (`▪ …`; rendered as text, not a `list`
   schema, for predictable layout).
5. **desvinculacion-vehiculo-mutuo-acuerdo** — subject embeds the plate;
   body names the owner + ID; **double signature** (company + owner).
6. **paz-y-salvo** — body ends `válido para $input.motivo` (default
   `"TRASPASO"`); has the `DESCRIPCIÓN DEL VEHÍCULO:` title.
7. **solicitud-visto-bueno** — vista bueno for initial registration.
8. **solicitud-tarjeta-operacion-inicial** — body opens with
   `$tenant.legal_representative` + Decree 1079/2015 & 431/2017
   citations; has the vehicle-description title.
9. **solicitud-tarjeta-operacion-cambio-empresa** — same body as (8),
   different subject.

Shared custom field: `capacidad` (`short_text`, `Ej.: "19 pasajeros"`).
Shared filename shape: `<slug>-$collection.plate`.
