# Layout system (any kind)

## Page sizes

| `pageSize` | portrait `basePdf` | landscape `basePdf` |
|---|---|---|
| `Letter` | 215.9 × 279.4 | 279.4 × 215.9 |
| `A4` | 210 × 297 | 297 × 210 |

`settings.pageSize` + `settings.orientation` must describe the same
geometry as `definition.basePdf`. Colombia examples stay Letter unless
the kind demands otherwise (wide tabular reports may use landscape).
`padding: [10, 10, 10, 10]` on every file.

## Grid

Work inside 20 mm side margins unless the kind needs full bleed:

- Letter portrait content column: `x = 20`, `width = 175.9`.
- A4 portrait content column: `x = 20`, `width = 170`.
- Landscape: mirror the same margins on the long axis.
- Keep ≥ 10 mm bottom margin; signature/footer blocks end above
  y = 268 (portrait) or the equivalent.

Stack blocks top-down with ≥ 4 mm rhythm gaps; reserve ≥ 8 mm above
signature rules.

## Overflow math (mandatory per text box)

pdfme does not grow boxes (default `overflow: "visible"`): estimate
lines and fit height with margin.

```text
chars_per_line ≈ width_mm / (fontSize_pt × 0.19)
lines = Σ over "\n"-parts of max(1, ceil(len / chars_per_line))
needed_mm = lines × fontSize_pt × 0.45
require: box_height ≥ needed_mm × 1.2
```

(175.9 mm at 11 pt ≈ 84 chars/line ≈ 5 mm/line.) Count variables at
their *rendered* length for long values (company names, multiline
recipients). If the estimate fails, raise the height or split the
paragraph — never ship a tight box. `overflow: "expand"` is allowed
only as the last block of a page (it grows downward and overlaps
whatever follows).

## Fonts and color

Roboto default everywhere: no `fontName` (it warns on import), no bold
(emphasis via size and caps). Body 10–11 pt, titles 12–16 pt, footers
8–9 pt. Colors as hex (`fontColor`, `backgroundColor`); keep body text
near-black for print.

## Multi-page

One array entry per page in `definition.schemas`. Repeat the header
block (title + record key, e.g. plate) on every page — pdfme does not
repeat content for you. Tables do not flow across pages: split them
explicitly. Import validation counts pages and schemas; keep totals
small (dozens of schemas, not hundreds).

## Picking components

- Flowing prose / labeled values → `text` (justify body, left labels).
- Label/value pairs (≤ ~15 rows) → `table` with `showHead`, or a text
  grid when cells would carry quotes.
- Repeating rows from a fixed set → `table` (2D JSON body).
- Annexes, requirements, item lists → `list` (JSON array or lines).
- Rules, frames, signature lines → `line`.
- Verification links, record URLs → `qrcode` + printed URL in `text`.
- Tenant artwork → omit; note it in `description`.

When a table or an intricate grid gets complex, build it in the
Designer, export the `.flittemplate`, then sanitize the JSON by hand.
