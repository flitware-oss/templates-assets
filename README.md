# Flitware Templates Assets

Plantillas PDF portables (`.flittemplate`) listas para importar en Flitware,
más el skill de agente que sabe generarlas.

> All example documents use **fictional data** (companies, people, plates,
> IDs, phones). They are layout + variable references, never real records.

## Estructura

```text
examples/
  colombia/            # Único país con ejemplos por ahora
    fleets/            # Plantillas con root_collection = fleets
    drivers/           # Plantillas con root_collection = drivers
  peru/                # Próximamente
  brazil/              # Próximamente (em breve)
agent/                 # Skill para agentes que generan plantillas
```

Cada país se segmenta por **colección**: el directorio es el
`root_collection` de la plantilla (`fleets`, `drivers`, ...). Ver el índice
en [examples/colombia/README.md](examples/colombia/README.md).

## Cómo usar una plantilla

1. Descarga el `.flittemplate` que necesites.
2. En Flitware ve a **Plantillas → Importar plantilla** y selecciona el archivo.
3. Si la plantilla usa campos personalizados que tu empresa no tiene, el
   importador te preguntará si deseas crearlos.
4. Revisa la plantilla en el Diseñador (llega **inactiva** a propósito),
   ajusta textos y logo, y actívala.
5. Al generar o previsualizar, las variables `$input.*` (destinatario,
   ciudad, valores puntuales) se preguntan en pantalla: no edites la
   plantilla para cada documento.

Todas las plantillas de Colombia usan tamaño **carta** (`Letter`,
215.9 × 279.4 mm), motor `pdfme 6.2.1` y formato `flitware-template` v1.

## Datos ficticios

Ningún ejemplo contiene información real: empresas, NIT, personas, placas,
teléfonos y correos son inventados y cualquier parecido es coincidencia.
Si aportas una plantilla nueva, sanitízala antes de subirla (ver el skill).

## Generar plantillas nuevas

Lee [agent/SKILL.md](agent/SKILL.md): formato del archivo, catálogo de
variables (`$collection`, `$tenant`, `$system`, `$input`), receta de
diagramación carta, reglas de sanitización y checklist de calidad.
