# Recipe: tabular reports and lists

Use for inventories, settlements, checklists printouts, annex-heavy
filings. Prefer `landscape` when columns exceed ~110 mm of content.

## Table or text grid?

- `table` when the source is a real grid (head + aligned rows). Cells
  are JSON strings: keep variable cells quote-free (IDs, plates, names,
  codes, dates) and move free prose to `text`.
- Text grid (label/value `text` pairs) when cells may carry quotes or
  long wrapping prose, or when rows ≤ ~6 and styling is trivial.
- `list` for annexes and requirements (JSON array or `\n` lines,
  `\t` nesting, `bullet` or `ordered`).

## Minimal report skeleton (Letter portrait)

| Block | y | h | notes |
|---|---|---|---|
| header | 15 | 14 | Title 14 left + `$input.periodo` / `$system.date` 10 right (two boxes) |
| filters | 30 | 10 | 9 pt gray: record key + `$input.*` scope (city, date range) |
| table | 45 | 150–180 | `table` with `showHead`; widths sum to 100 |
| totals | — | 12 | Right-aligned `text`: totals row (values via `$input.*` or record fields) |
| footer | 255 | 10 | 8 pt center: issuer + page note (`Página 1 de N` static per page) |

## Rules

- `headWidthPercentages` sums to 100; keep ≤ 6 columns on portrait.
- `content` must parse as JSON **after** substitution: validate by
  substituting sample values mentally (no `"` inside values).
- Tables do not flow across pages: split long bodies explicitly and
  repeat the header block (title + record key) per page.
- `bodyStyles.alternateBackgroundColor` (`#f5f5f5`) for readability;
  head with dark `backgroundColor` + white `fontColor`.
- Totals/observations as `text` below the table, never as magic rows.
- For dynamic row counts the author fixes the rows at authoring time
  (templates are static layouts): document the expected row range in
  the file `description`.
