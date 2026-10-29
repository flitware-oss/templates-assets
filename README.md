# Flitware Templates Assets

Plantillas PDF portables (`.flittemplate`) listas para importar en Flitware,
más el skill de agente que sabe generarlas. **Este README es el entrypoint:
léelo y conocerás todo el repo sin abrir nada más** (cada sección enlaza la
fuente de verdad cuando necesites detalle).

> All example documents use **fictional data** (companies, people, plates,
> IDs, phones). They are layout + variable references, never real records.

## Empieza aquí

| Quieres… | Ve a… |
|---|---|
| Crear o revisar una plantilla | [agent/SKILL.md](agent/SKILL.md) (skill + 10 referencias) |
| Ver qué ejemplos hay | [Índice de ejemplos](#índice-de-ejemplos) abajo, o [examples/colombia/README.md](examples/colombia/README.md) |
| Importar una plantilla en Flitware | [Cómo usar una plantilla](#cómo-usar-una-plantilla) |
| Entender el formato/variables | [Contratos de un vistazo](#contratos-de-un-vistazo) |

## Mapa del repo (completo)

```text
README.md                    # ← estás aquí: entrypoint de discovery
LICENSE                      # Licencia del repo
examples/
  colombia/
    README.md                # Índice CO: tabla por colección + convenciones $input
    fleets/                  # root_collection = fleets (9 cartas oficiales, Letter)
      carta-aceptacion.flittemplate
      certificado-disponibilidad-capacidad-transportadora.flittemplate
      certificado-ingresos.flittemplate            # Sin tabla; $input valor_*; sin custom fields
      desistimiento-capacidad-transportadora.flittemplate  # + sección ANEXOS
      desvinculacion-vehiculo-mutuo-acuerdo.flittemplate   # Placa en asunto; doble firma
      paz-y-salvo.flittemplate                     # $input.motivo (defecto TRASPASO)
      solicitud-visto-bueno.flittemplate
      solicitud-tarjeta-operacion-inicial.flittemplate
      solicitud-tarjeta-operacion-cambio-empresa.flittemplate
    drivers/                 # root_collection = drivers (1 ejemplo)
      carta-laboral.flittemplate                   # Sin $input; 5 custom fields
  peru/README.md             # Placeholder: misma segmentación por colección
  brazil/README.md           # Placeholder (em breve)
agent/
  SKILL.md                   # Skill flitware-template-author: flujo general, cualquier tipo
  references/
    file-format.md           # Envelope .flittemplate v1, settings, límites
    variables.md             # $collection/$tenant/$system/$input + reglas de mapeo
    input-variables.md       # Declaraciones $input: tipos, defaults, convenciones CO
    components.md            # Catálogo verificado: text/line/list/table/image/qrcode/signature
    layout-system.md         # Tamaños, grilla, math de desborde, multi-página
    recipe-official-letters.md  # Esqueleto cartas oficiales CO (coordenadas)
    recipe-certificates.md   # Certificados centrados + marco + QR
    recipe-reports.md        # Reportes tabulares y listas
    sanitization.md          # Política de datos ficticios + grep pre-publish
    colombia-fleets.md       # Los 9 docs fleets mapeados (deltas)
    qa-checklist.md          # Verificación pre-publish
```

Regla de organización: `examples/<país>/<colección>/`; el directorio es el
`root_collection` de la plantilla (`fleets`, `drivers`, …).

## Contratos de un vistazo

**Archivo** (`flitware-template` v1, motor `pdfme 6.2.1`): envelope con
`format`, `formatVersion: 1`, `exportedAt`, `engine`, `source.rootCollection`,
`customFields?` (definiciones portables) y `template {name, description,
root_collection, template_type: "PDF", definition, settings}`. Identidad,
tenant, auditoría, estado y manifiesto nunca viajan. Detalle:
[agent/references/file-format.md](agent/references/file-format.md).

**Variables** (tokens `$namespace.path` dentro del texto):

| Namespace | Fuente | Declaración |
|---|---|---|
| `$collection.*` | Registro impreso (campos, 1 salto de relación, `custom_fields.*`) | No (schema vivo) |
| `$tenant.*` | Empresa (nombre, NIT, representante, contactos) | No |
| `$system.date`, `$system.datetime` | Fecha de generación | No |
| `$input.<key>` | Respuesta al generar/previsualizar | Sí, en `settings.inputs` |

**`$input`**: tipos = custom fields (`short/long_text`, `number`, `date`,
`datetime`, `boolean`, `single/multi_select`), solo opciones estáticas
(catálogos rechazados). Efecto = respuesta sobre `default_value`; requerido
sin respuesta = gap. Nunca se almacena ni se loguea. Funciona también en
`filenamePattern`. Detalle:
[agent/references/input-variables.md](agent/references/input-variables.md).

**Páginas**: `A4` (210 × 297) o `Letter` (215.9 × 279.4), portrait o
landscape; Colombia = Letter salvo reportes anchos. Componentes permitidos:
`text`, `line`, `list`, `table`, `image` (solo Data URL), `signature`,
`qrcode`. Detalle:
[agent/references/components.md](agent/references/components.md),
[agent/references/layout-system.md](agent/references/layout-system.md).

**Límites**: archivo ≤ 768 KB, `definition` ≤ 512 KB, `settings` ≤ 16 KB,
≤ 50 `customFields`, ≤ 50 `inputs`, etiquetas ≤ 120, descripciones ≤ 500,
códigos de opción `^[A-Za-z0-9_-]{1,64}$`.

## Índice de ejemplos

| Archivo | País | Colección | Tipo | `$input.*` | Custom fields |
|---|---|---|---|---|---|
| `fleets/carta-aceptacion` | CO | fleets | Carta oficial | ciudad, destinatario | capacidad |
| `fleets/certificado-disponibilidad-capacidad-transportadora` | CO | fleets | Carta oficial | ciudad, destinatario | capacidad |
| `fleets/certificado-ingresos` | CO | fleets | Carta oficial | ciudad, destinatario, valor_ingresos, valor_letras | — |
| `fleets/desistimiento-capacidad-transportadora` | CO | fleets | Carta oficial + anexos | ciudad, destinatario | capacidad |
| `fleets/desvinculacion-vehiculo-mutuo-acuerdo` | CO | fleets | Carta oficial, doble firma | ciudad, destinatario | capacidad |
| `fleets/paz-y-salvo` | CO | fleets | Carta oficial | ciudad, destinatario, motivo | capacidad |
| `fleets/solicitud-visto-bueno` | CO | fleets | Carta oficial | ciudad, destinatario | capacidad |
| `fleets/solicitud-tarjeta-operacion-inicial` | CO | fleets | Carta oficial | ciudad, destinatario | capacidad |
| `fleets/solicitud-tarjeta-operacion-cambio-empresa` | CO | fleets | Carta oficial | ciudad, destinatario | capacidad |
| `drivers/carta-laboral` | CO | drivers | Certificación | — | cargo, fecha_ingreso, salario, tipo_contrato, ciudad |

(Todos `.flittemplate` bajo `examples/colombia/<colección>/`.)

## Cómo usar una plantilla

1. Descarga el `.flittemplate` que necesites.
2. En Flitware ve a **Plantillas → Importar plantilla** y selecciona el archivo.
3. Si la plantilla usa campos personalizados que tu empresa no tiene, el
   importador te preguntará si deseas crearlos.
4. Revisa la plantilla en el Diseñador (llega **inactiva** a propósito),
   ajusta textos y logo, y actívala.
5. Al generar o previsualizar, las variables `$input.*` se preguntan en
   pantalla: no edites la plantilla para cada documento.

## Cómo aportar una plantilla

1. Lee [agent/SKILL.md](agent/SKILL.md) y sigue su flujo (tipo → componentes
   → mapeo → layout → archivo → sanitización → QA).
2. Nómbrala `<slug-del-documento>.flittemplate` en
   `examples/<país>/<colección>/` y agrega su fila al índice del país.
3. Verifica el [qa-checklist](agent/references/qa-checklist.md) completo,
   incluido el grep de sanitización.

## Convenciones

- Datos 100% ficticios en ejemplos y docs; nada real viaja jamás (ver
  [sanitization.md](agent/references/sanitization.md)).
- Sin logos ni arte cliente en los archivos: el tenant pone su imagen en
  el Diseñador (anótalo en `description` si el original la tenía).
- Claves `$input` estables por país (`ciudad`, `destinatario`, …) para que
  los tenants las aprendan una vez.
- `filenamePattern` por registro (`…-$collection.plate`,
  `…-$collection.document_number`); slugs únicos por país.
