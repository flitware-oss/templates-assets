# Colombia — Plantillas de ejemplo

Tamaño carta (`Letter`), `root_collection` = directorio contenedor.
Todos los datos visibles son ficticios.

## `fleets/`

| Archivo | Documento | `$input.*` | Custom fields |
|---|---|---|---|
| `carta-aceptacion.flittemplate` | Carta de aceptación de vehículo al parque automotor | `ciudad`, `destinatario` | `capacidad` |
| `certificado-disponibilidad-capacidad-transportadora.flittemplate` | Solicitud de certificado de disponibilidad de capacidad transportadora | `ciudad`, `destinatario` | `capacidad` |
| `certificado-ingresos.flittemplate` | Certificado de ingresos del propietario/afiliado | `ciudad`, `destinatario`, `valor_ingresos`, `valor_letras` | — |
| `desistimiento-capacidad-transportadora.flittemplate` | Desistimiento del trámite de capacidad transportadora (+ anexos) | `ciudad`, `destinatario` | `capacidad` |
| `desvinculacion-vehiculo-mutuo-acuerdo.flittemplate` | Desvinculación por mutuo acuerdo (doble firma) | `ciudad`, `destinatario` | `capacidad` |
| `paz-y-salvo.flittemplate` | Paz y salvo por todo concepto | `ciudad`, `destinatario`, `motivo` | `capacidad` |
| `solicitud-visto-bueno.flittemplate` | Solicitud de visto bueno para matrícula inicial | `ciudad`, `destinatario` | `capacidad` |
| `solicitud-tarjeta-operacion-inicial.flittemplate` | Solicitud de tarjeta de operación inicial | `ciudad`, `destinatario` | `capacidad` |
| `solicitud-tarjeta-operacion-cambio-empresa.flittemplate` | Solicitud de tarjeta de operación por cambio de empresa | `ciudad`, `destinatario` | `capacidad` |

Custom field portable incluido en cada archivo que lo necesita:

- `capacidad` (`short_text`, ej. `"19 pasajeros"`): el esquema base de
  `fleets` no trae capacidad; al importar se ofrece crearlo.

## `drivers/`

| Archivo | Documento | `$input.*` | Custom fields |
|---|---|---|---|
| `carta-laboral.flittemplate` | Certificación laboral para conductores | — | `cargo`, `fecha_ingreso`, `salario`, `tipo_contrato`, `ciudad` |

## Convenciones de `$input`

- `ciudad` (`short_text`, requerido): ciudad de expedición, va junto a `$system.date`.
- `destinatario` (`long_text`, requerido): bloque del destinatario, una línea
  por renglón (nombre, cargo, entidad, ciudad).
- `motivo` (`short_text`, defecto `"TRASPASO"`): para qué es válido el paz y salvo.
- `valor_ingresos` (`number`, requerido) + `valor_letras` (`long_text`,
  requerido): cifra y su escritura en letras.
