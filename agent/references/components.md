# Schema components (verified)

Every schema needs a unique `name`, a known `type`, `position: {x, y}`
and finite `width`/`height` (mm). Unknown types and `readOnly`
expressions reject the file. All shapes below were verified by rendering
with `@pdfme/generator` 6.2.1 (see `/tmp/pdfme-probe.mjs` in the authoring
session); `$variables` substitute inside any string content.

## `text` — paragraphs, labels, values

```jsonc
{ "name": "body", "type": "text",
  "content": "El vehículo $collection.plate …\nSegunda línea.",
  "position": { "x": 20, "y": 80 }, "width": 175.9, "height": 34,
  "fontSize": 11, "alignment": "justify" }
```

Multiline via `\n`. Useful optionals: `verticalAlignment`
(`top`|`middle`|`bottom`), `lineHeight` (default 1),
`characterSpacing`, `fontColor` / `backgroundColor` (hex),
`borderColor` + `borderWidth`, `padding`, `underline`, `strikethrough`,
`overflow: "expand"` (grows downward past `height` — only as the last
block or with room below, else it overlaps the next box).
`fontName` other than default warns on import: leave it out.

## `line` — rules, dividers, signature lines, frames

```jsonc
{ "name": "sig_line", "type": "line",
  "position": { "x": 20, "y": 240 }, "width": 80, "height": 0.5 }
```

No `content`. Frames are four lines. Keep inside page margins.

## `list` — bulleted / numbered annexes

```jsonc
{ "name": "anexos", "type": "list",
  "content": "[\"Certificado A.\", \"Certificado B.\"]",
  "position": { "x": 20, "y": 203 }, "width": 175.9, "height": 20,
  "fontSize": 10, "listStyle": "bullet",
  "markerWidth": 6, "markerGap": 2, "itemSpacing": 1 }
```

`content` is a JSON string array **or** newline-separated lines;
`\t` prefixes nest (up to 8 levels). `listStyle`: `bullet` (•) or
`ordered` (1. 2.). Accepts the text props (`fontSize`, colors…).
Prefer `list` over hand-drawn `▪` paragraphs for real annexes.

## `table` — grids with head + body

`content` is a **JSON string of a 2D string array** (rows of cells).
Variables may sit inside cells, but a substituted value containing `"`
breaks the JSON: keep variable cells quote-free (IDs, plates, names,
codes) and put free prose in `text` blocks instead.

Minimal verified shape:

```jsonc
{ "name": "vehiculo", "type": "table",
  "position": { "x": 20, "y": 120 }, "width": 175.9, "height": 60,
  "content": "[[\"PLACA\",\"$collection.plate\"],[\"MARCA\",\"$collection.brand.name\"]]",
  "showHead": true, "head": ["Campo", "Valor"],
  "headWidthPercentages": [50, 50],
  "tableStyles": { "borderColor": "#000000", "borderWidth": 0.3 },
  "headStyles": { "alignment": "left", "verticalAlignment": "top",
    "fontSize": 10, "lineHeight": 1, "characterSpacing": 0,
    "fontColor": "#ffffff", "backgroundColor": "#333333",
    "borderColor": "#000000",
    "borderWidth": { "top": 0.3, "right": 0.3, "bottom": 0.3, "left": 0.3 },
    "padding": { "top": 2, "right": 2, "bottom": 2, "left": 2 } },
  "bodyStyles": { /* same keys + */ "alternateBackgroundColor": "#f5f5f5" },
  "columnStyles": {} }
```

`headWidthPercentages` should sum to 100. For label/value pairs a
two-column table reads better than 22 text boxes; for flowing prose use
`text`. When a table gets complex, build it in the Designer, export,
then sanitize the JSON.

## `image` — logos, photos, stamps

`content` is a **Data URL** (`data:image/png;base64,…` —JPEG/GIF/WebP
also allowed). Remote URLs are rejected. Examples ship **without**
artwork (no client logos travel): tenants place their own image in the
Designer. An empty `content: ""` renders nothing and is a valid
placeholder, but prefer omitting the schema and noting it in the file
`description`.

## `qrcode` — verification codes, links

```jsonc
{ "name": "verify", "type": "qrcode",
  "content": "$input.verification_url",
  "position": { "x": 165, "y": 240 }, "width": 30, "height": 30 }
```

`content` is the encoded string (keep it non-empty). Variables work:
declare `$input.verification_url` (or a record field) for per-document
codes. Optionals: `backgroundColor`, `barColor`.

## `signature` — handwritten signature placeholder

Same shape as `image` (Data URL content, empty renders nothing).
Use one per signer next to their printed name/role; the signer draws or
uploads at signing time outside Flitware.
