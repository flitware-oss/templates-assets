# Recipe: certificates

Centered single-purpose documents (course completion, affiliation,
insurance, vehicle roadworthiness…). Letter portrait unless the design
asks for landscape. Centered column: `x = 30`, `width = 155.9`.

| Block | y | h | size | align | content |
|---|---|---|---|---|---|
| frame | 12 | — | — | — | 4 `line`s: top/bottom `w=191.9` at `x=12`, sides via `w=0.5, height=255.4` |
| issuer | 30 | 8 | 10 | center | `$tenant.name` |
| title | 45 | 16 | 16 | center | e.g. `CERTIFICADO DE AFILIACIÓN` |
| preamble | 70 | 12 | 11 | center | e.g. `La empresa certifica que:` |
| recipient | 85 | 20 | 14 | center | `$collection.name` (or owner/driver) |
| body | 110 | 40 | 11 | justify | What is certified + record facts + `$input.*` specifics |
| expedition | 160 | 8 | 10 | center | `$input.ciudad, $system.date` |
| signatures | 200 | — | — | — | Two `line`s (`w=60` at `x=35` / `x=120`) + name 10 / role 9 |
| qr | 232 | 30 | — | — | `qrcode` (`x=93 w=30 h=30`) + caption 8: `$input.verification_url` |

Notes:

- Vertical side lines: `line` with small `width` and large `height`
  (e.g. `position {x: 12, y: 12}, width: 0.5, height: 255.4`).
- The QR carries per-document verification: declare
  `$input.verification_url` (`short_text`, required) and print the same
  value as caption so the code stays useful on paper.
- `$input` candidates: motive, validity period (`date` renders long
  form), serial/radicado number (`short_text`).
- Keep ornaments minimal (rules only): no background images in examples.
